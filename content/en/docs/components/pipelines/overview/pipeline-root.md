+++
title = "Pipeline Root"
description = "Kubeflow Pipelines 管道 Root 入门"
weight = 50

+++
{{% beta-status
feedbacklink="https://github.com/kubeflow/pipelines/issues" %}}

从 [Kubeflow Pipelines SDK v2](https://www.kubeflow.org/docs/components/pipelines/sdk-v2/) 和 Kubeflow Pipelines 1.7.0 开始，Kubeflow Pipelines 的 pipeline root 在 [standalone deployment](https://www.kubeflow.org/docs/components/pipelines/installation/standalone-deployment/) 和 [AI Platform Pipelines](https://cloud.google.com/ai-platform/pipelines/docs) 都支持新的中间工件存储库功能：

## 在你开始之前
本指南告诉您 Kubeflow Pipelines 管道根的基本概念以及如何使用它。
本指南假定您已经安装了 Kubeflow Pipelines，或者想要使用 [Kubeflow Pipelines 部署
指南](/docs/components/pipelines/installation/) 的独立部署 或者 AI Platform Pipelines 方式发布 Kubeflow Pipelines。

## 什么时pipeline root?

Pipeline root 表示一个工件存储库，Kubeflow Pipelines 在其中存储管道的工件。
此特性支持 MinIO、S3、GCS 原生 [Go CDK](https://github.com/google/go-cloud)。将 Kubeflow Pipelines 与其他系统集成时，可以更容易地在 S3 和 GCS 中访问工件。

**注意:** 对于 MinIO，您无法更改 MinIO 实例。Kubeflow Pipelines 只能使用自己部署的 Minio 实例。
(如果您需要指定 Minio 实例，请点赞这个 [GitHub Issue](https://github.com/kubeflow/pipelines/issues/6517)。)

## 如何配置管道根身份验证
#### MinIO
您不需要通过 MinIO 的身份验证。
Kubeflow Pipelines 配置了自己部署的 MinIO 实例的身份验证。

#### GCS
如果要指定 `pipeline root` 到 GCS：

查看 [authentication-pipelines](https://www.kubeflow.org/docs/distributions/gke/pipelines/authentication-pipelines/)

#### S3
如果要指定 `pipeline root` 到 S3，请选择以下选项之一：

* 通过 [AWS IRSA](https://aws.amazon.com/blogs/containers/cross-account-iam-roles-for-kubernetes-service-accounts/):

* 通过 kfp sdk:
`dsl.get_pipeline_conf().add_op_transformer(aws.use_aws_secret('xxx', ‘xxx’, ‘xxx’))`
  
**参考资料**:
* [add-op-transformer](https://kubeflow-pipelines.readthedocs.io/en/latest/source/kfp.dsl.html#kfp.dsl.PipelineConf.add_op_transformer)
* [use-aws-secret](https://kubeflow-pipelines.readthedocs.io/en/latest/source/kfp.extensions.html#kfp.aws.use_aws_secret)

## 如何配置 pipeline root

#### 通过 ConfigMaps

通过修改在 Kubernetes 空间的 `kfp-launcher` ConfigMap 的 `defaultPipelineRoot` 条目来配置默认的 管道根。

```shell
kubectl edit configMap kfp-launcher -n ${namespace}
```
此管道根将是 Kubernetes 命名空间中运行的所有管道的默认管道根，除非您使用以下选项之一覆盖它：

####  通过构建 Pipelines
可以通过 `kfp.dsl.pipeline` 注解在 [构建 pipelines](https://www.kubeflow.org/docs/components/pipelines/sdk-v2/build-pipeline/#build-your-pipeline) 时配置

####  通过 SDK 提交 Pipeline
提交管道时，使用如下任一方式可以配置 `pipeline_root` 参数：
* [create_run_from_pipeline_func](https://kubeflow-pipelines.readthedocs.io/en/latest/source/kfp.client.html#kfp.Client.create_run_from_pipeline_func)
* [create_run_from_pipeline_package](https://kubeflow-pipelines.readthedocs.io/en/latest/source/kfp.client.html#kfp.Client.create_run_from_pipeline_package) 
* [run_pipeline](https://kubeflow-pipelines.readthedocs.io/en/latest/source/kfp.client.html#kfp.Client.run_pipeline).

####  通过 UI 提交 Pipeline Run
通过 UI 提交 pipeline run 配置 `pipeline_root` run 参数
<img src="/docs/images/pipelines/v2/pipelines-ui-pipelineroot.png"
alt="Configure pipeline root on the pipelines UI"
class="mt-3 mb-3 border border-info rounded">
