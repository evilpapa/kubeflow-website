+++
title = "Pipelines 多用户隔离"
description = "Kubeflow Pipelines 多用户隔离入门"
weight = 30
+++

Kubeflow Pipelines 多用户隔离由 [Kubeflow multi-user isolation](/docs/components/multi-tenancy/) 集成。

常见的 Kubeflow 多用户操作请参考 [Multi-user 隔离入门](/docs/components/multi-tenancy/getting-started/)，
包括以下内容：

* [授予用户最小 Kubernetes 集群访问权限](/docs/components/multi-tenancy/getting-started/#pre-requisites-grant-user-minimal-kubernetes-cluster-access)
* [通过 Kubeflow UI 管理贡献者](/docs/components/multi-tenancy/getting-started/#managing-contributors-through-the-kubeflow-ui)
* 对于 Google Cloud：[从 Kubeflow 到 Google Cloud 的集群内身份验证](/docs/gke/authentication/#in-cluster-authentication)

注意，**当前**仅在从 Kubeflow v1.1 开始除 OpenShift 之外所有平台的
[Kubeflow 完整部署](/docs/components/pipelines/installation/overview/#full-kubeflow-deployment)中
支持 Kubeflow Pipelines 多用户隔离，针对最新的平台支持，请参考 [kubeflow/manifests#1364](https://github.com/kubeflow/manifests/issues/1364#issuecomment-668415871)。

另请注意，Kubeflow 中的隔离支持
不提供任何硬性安全保证，以防止用户
恶意尝试渗透其他用户的配置文件。
 
## 资源如何分离？

Kubeflow Pipelines 通过 Kubernetes 命名空间（Kubeflow 配置文件）分离其资源。

实验直接属于命名空间，不再有默认
实验。运行和重复运行属于其父实验的命名空间。

管道运行在用户命名空间中执行，
因此用户可以利用 Kubernetes 命名空间隔离。
例如，他们可以为不同命名空间中的其他服务配置不同的密钥。

其他用户未经许可无法查看您命名空间中的资源，因为
Kubeflow Pipelines API 服务器拒绝对当前用户
无权访问的命名空间的请求。

请注意，目前管道定义没有多用户隔离。有关详细信息，
请参阅 [Current Limitations](#current-limitations) 获取更多信息。

### 何时使用 UI

当您从 Kubeflow 仪表板访问 Kubeflow Pipelines UI 时，它仅显示
您选择的命名空间中的实验、运行和重复运行。同样，
当您从 UI 创建资源时，它们也属于您选择的命名空间。

您可以选择不同的命名空间来查看其他命名空间中的资源。

### 何时使用 SDK

首先，您需要使用 SDK 连接到 Kubeflow Pipelines 公共端点。
对于 Google Cloud，请按照 [说明](/docs/gke/pipelines/authentication-sdk/#connecting-to-kubeflow-pipelines-in-a-full-kubeflow-deployment) 操作。

调用 SDK 方法进行实验时，需要提供额外的
命名空间参数。运行，重复运行归实验所有。它们
与父实验位于相同的命名空间中，因此您可以像以前一样调用
它们的 SDK 方法。

例如：

```python
import kfp
client = kfp.Client(...) # Refer to documentation above for detailed arguments.

client.create_experiment(name='<Your experiment name>', namespace='<Your namespace>')
print(client.list_experiments(namespace='<Your namespace>'))
client.run_pipeline(
    experiment_id='<Your experiment ID>', # Experiment determines namespace.
    job_name='<Your job ID>',
    pipeline_id='<Your pipeline ID>')
print(client.list_runs(experiment_id='<Your experiment ID>'))
print(client.list_runs(namespace='<Your namespace>'))
```

要将您的用户命名空间存储为默认上下文，请使用
[`set_user_namespace`](https://kubeflow-pipelines.readthedocs.io/en/stable/source/kfp.client.html#kfp.Client.set_user_namespace)
方法。此方法将您的用户命名空间存储在配置文件
`$HOME/.config/kfp/context.json`。设置默认命名空间后，
如果未提供命名空间参数，SDK 方法默认使用此命名空间。

```python
# Note, this saves the namespace in `$HOME/.config/kfp/context.json`. Therefore,
# You only need to call this once. The saved namespace context will be picked up
# by other clients you use later.
client.set_user_namespace(namespace='<Your namespace>')
print(client.get_user_namespace())

client.create_experiment(name='<Your experiment name>')
print(client.list_experiments())
client.run_pipeline(
    experiment_id='<Your experiment ID>', # Experiment determines namespace.
    job_name='<Your job name>',
    pipeline_id='<Your pipeline ID>')
print(client.list_runs())

# Specifying a different namespace will override the default context.
print(client.list_runs(namespace='<Your other namespace>'))
```

请注意，不再可以直接从集群内工作负载
访问 Kubeflow Pipelines API 服务，请阅读 [当前限制部分](#current-limitations)
获取更多信息。

Kubeflow Pipelines SDK 的详细文档可以在
[Kubeflow Pipelines SDK Reference](https://kubeflow-pipelines.readthedocs.io/en/stable/source/kfp.client.html) 找到。

### 何时使用 REST API 或生成的 Python API 客户端

同样，在调用 [REST API endpoints](/docs/components/pipelines/reference/api/kubeflow-pipeline-api-spec/)
或使用 [生成的 Python API client](https://kubeflow-pipelines.readthedocs.io/en/stable/source/kfp.server_api.html)，
实验 API 需要命名空间参数。请注意，命名空间是
使用资源引用来引用的。
资源引用 **类型** 是 `NAMESPACE`，资源引用 **key id** 是命名空间名称。

以下示例演示了如何在多用户环境中使用 [生成的 Python API 客户端 (kf-server-api)](https://kubeflow-pipelines.readthedocs.io/en/stable/source/kfp.server_api.html)。

```python
from kfp_server_api import ApiRun, ApiPipelineSpec, \
    ApiExperiment, ApiResourceType, ApiRelationship, \
    ApiResourceReference, ApiResourceKey
# or you can also do the following instead
# from kfp_server_api import *

experiment=client.experiments.create_experiment(body=ApiExperiment(
    name='test-experiment-1234',
    resource_references=[ApiResourceReference(
        key=ApiResourceKey(
            id='<namespace>', # Replace with your own namespace.
            type=ApiResourceType.NAMESPACE,
        ),
        relationship=ApiRelationship.OWNER,
    )],
))
print(experiment)
pipeline = client.pipelines.list_pipelines().pipelines[0]
print(pipeline)
client.runs.create_run(body=ApiRun(
    name='test-run-1234',
    pipeline_spec=ApiPipelineSpec(
        pipeline_id=pipeline.id,
    ),
    resource_references=[ApiResourceReference(
        key=ApiResourceKey(
            id=experiment.id,
            type=ApiResourceType.EXPERIMENT,
        ),
        relationship=ApiRelationship.OWNER,
    )],
))
runs=client.runs.list_runs(
    resource_reference_key_type=ApiResourceType.EXPERIMENT,
    resource_reference_key_id=experiment.id,
)
print(runs)
```

## 当前限制

### 未隔离的资源

以下资源目前不支持隔离，并且在
没有访问控制的情况下共享：

* Pipelines (Pipeline 定义).
* [机器学习元数据 (MLMD)](https://www.tensorflow.org/tfx/guide/mlmd)中的工件、执行和其他元数据实体。
* [Minio artifact 存储](https://min.io/)，其中包含管道运行的输入/输出工件。

## 集群内 API 请求认证

有关详细信息，请参阅 [从同一集群连接到 Kubeflow 管道](/docs/components/pipelines/sdk/connect-api/#connect-to-kubeflow-pipelines-from-the-same-cluster)。

或者，Jupyter notebooks 或 cron 任务等集群内工作负载也可以通过公共端点访问 Kubeflow Pipelines API。此选项是特定于平台的，并在 
[从集群外部连接到 Kubeflow 管道](/docs/components/pipelines/sdk/connect-api/#connect-to-kubeflow-pipelines-from-outside-your-cluster)进行了说明。
