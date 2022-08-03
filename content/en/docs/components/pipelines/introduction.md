+++
title = "介绍"
description = "Kubeflow Pipelines 的目标和主要概念介绍"
weight = 10
                    
+++

{{% stable-status %}}

Kubeflow Pipelines 是一个基于 Docker 容器构建和部署可移植、
可扩展的机器学习 (ML) 工作流的平台。

## 快速开始

按照[管道快速入门指南](/docs/components/pipelines/overview/quickstart)
运行您的第一个管道。

## 什么是 Kubeflow 管道？

Kubeflow Pipelines 平台包括：

* 用于管理和跟踪实验、作业和运行的用户界面 (UI)。
* 用于调度多步 ML 工作流的引擎。
* 用于定义和操作管道和组件的 SDK。
* 使用 SDK 与系统交互的笔记本。

以下是 Kubeflow Pipelines 的目标：

* 端到端编排：启用和简化机器学习管道的编排。
* 简单的实验：让您轻松尝试多种想法和技术并管理您的各种试验/实验。
* 易于重用：使您能够重用组件和管道以快速创建端到端解决方案，而无需每次都重新构建。

Kubeflow Pipelines 可作为 Kubeflow 的核心组件或作为独立安装使用。

* [了解有关安装 Kubeflow的更多信息](/docs/started/getting-started/)。
* [了解有关独立安装 Kubeflow Pipelines 的更多信息](/docs/components/pipelines/installation/overview/)。

{{% pipelines-compatibility %}}

## 什么是管道？

_管道_ 是对 ML 工作流的描述，包括工作流中的所有组件
以及它们如何以图形的形式组合。 （请参阅
下面显示管道图示例的屏幕截图。）管道
包括运行管道所需的输入（参数）的定义以及
每个组件的输入和输出。

开发管道后，您可以在 Kubeflow Pipelines UI 上
上传和共享它。

_pipeline 组件_ 是一组自包含的用户代码，打包为
[Docker 镜像](https://docs.docker.com/get-started/)，在
管道中执行一个步骤。例如，一个组件
可以负责数据预处理、数据转换、模型训练等。

请参阅[管道](/docs/components/pipelines/concepts/pipeline/)
和[组件](/docs/components/pipelines/concepts/component/)的概念指南。

## 管道示例

下面的屏幕截图和代码展示了 `xgboost-training-cm.py` 使用
CSV 格式的结构化数据创建 XGBoost 模型的管道。
。您可以在 [GitHub](https://github.com/kubeflow/pipelines/tree/master/samples/core/xgboost_training_cm) 上查看有关管道的源代码和其他信息 。

### 管道的运行时执行图

下面的屏幕截图显示了 Kubeflow Pipelines UI 中示例管道的
运行时执行图：

<img src="/docs/images/pipelines-xgboost-graph.png" 
  alt="XGBoost results on the pipelines UI"
  class="mt-3 mb-3 border border-info rounded">

### 表示管道的 Python 代码

下面是 `xgboost-training-cm.py` 定义管道的 Python 代码的摘录。
你可以在
[GitHub](https://github.com/kubeflow/pipelines/tree/master/samples/core/xgboost_training_cm) 查看完整代码。

```python
@dsl.pipeline(
    name='XGBoost Trainer',
    description='A trainer that does end-to-end distributed training for XGBoost models.'
)
def xgb_train_pipeline(
    output='gs://your-gcs-bucket',
    project='your-gcp-project',
    cluster_name='xgb-%s' % dsl.RUN_ID_PLACEHOLDER,
    region='us-central1',
    train_data='gs://ml-pipeline-playground/sfpd/train.csv',
    eval_data='gs://ml-pipeline-playground/sfpd/eval.csv',
    schema='gs://ml-pipeline-playground/sfpd/schema.json',
    target='resolution',
    rounds=200,
    workers=2,
    true_label='ACTION',
):
    output_template = str(output) + '/' + dsl.RUN_ID_PLACEHOLDER + '/data'

    # Current GCP pyspark/spark op do not provide outputs as return values, instead,
    # we need to use strings to pass the uri around.
    analyze_output = output_template
    transform_output_train = os.path.join(output_template, 'train', 'part-*')
    transform_output_eval = os.path.join(output_template, 'eval', 'part-*')
    train_output = os.path.join(output_template, 'train_output')
    predict_output = os.path.join(output_template, 'predict_output')

    with dsl.ExitHandler(exit_op=dataproc_delete_cluster_op(
        project_id=project,
        region=region,
        name=cluster_name
    )):
        _create_cluster_op = dataproc_create_cluster_op(
            project_id=project,
            region=region,
            name=cluster_name,
            initialization_actions=[
              os.path.join(_PYSRC_PREFIX,
                           'initialization_actions.sh'),
            ],
            image_version='1.2'
        )

        _analyze_op = dataproc_analyze_op(
            project=project,
            region=region,
            cluster_name=cluster_name,
            schema=schema,
            train_data=train_data,
            output=output_template
        ).after(_create_cluster_op).set_display_name('Analyzer')

        _transform_op = dataproc_transform_op(
            project=project,
            region=region,
            cluster_name=cluster_name,
            train_data=train_data,
            eval_data=eval_data,
            target=target,
            analysis=analyze_output,
            output=output_template
        ).after(_analyze_op).set_display_name('Transformer')

        _train_op = dataproc_train_op(
            project=project,
            region=region,
            cluster_name=cluster_name,
            train_data=transform_output_train,
            eval_data=transform_output_eval,
            target=target,
            analysis=analyze_output,
            workers=workers,
            rounds=rounds,
            output=train_output
        ).after(_transform_op).set_display_name('Trainer')

        _predict_op = dataproc_predict_op(
            project=project,
            region=region,
            cluster_name=cluster_name,
            data=transform_output_eval,
            model=train_output,
            target=target,
            analysis=analyze_output,
            output=predict_output
        ).after(_train_op).set_display_name('Predictor')

        _cm_op = confusion_matrix_op(
            predictions=os.path.join(predict_output, 'part-*.csv'),
            output_dir=output_template
        ).after(_predict_op)

        _roc_op = roc_op(
            predictions_dir=os.path.join(predict_output, 'part-*.csv'),
            true_class=true_label,
            true_score_column=true_label,
            output_dir=output_template
        ).after(_predict_op)

    dsl.get_pipeline_conf().add_op_transformer(
        gcp.use_gcp_secret('user-gcp-sa'))
```

### Kubeflow Pipelines UI 上的管道输入数据

下面的部分屏幕截图显示了用于启动管道运行的 Kubeflow Pipelines UI。
代码中的管道定义决定了哪些参数出现在 UI 表单中。
管道定义还可以为参数设置默认值：

<img src="/docs/images/pipelines-start-xgboost-run.png" 
  alt="Starting the XGBoost run on the pipelines UI"
  class="mt-3 mb-3 border border-info rounded">

### 管道的输出

以下屏幕截图显示了 Kubeflow Pipelines UI 上可见的管道输出示例。

预测结果：

<img src="/docs/images/predict.png" 
  alt="Prediction output"
  class="mt-3 mb-3 p-3 border border-info rounded">

混淆矩阵：

<img src="/docs/images/cm.png" 
  alt="Confusion matrix"
  class="mt-3 mb-3 p-3 border border-info rounded">

受试者工作特征 (ROC) 曲线：

<img src="/docs/images/roc.png" 
  alt="ROC"
  class="mt-3 mb-3 p-3 border border-info rounded">

## 架构概述

<img src="/docs/images/pipelines-architecture.png" 
  alt="Pipelines architectural diagram"
  class="mt-3 mb-3 p-3 border border-info rounded">

在高层次上，管道的执行过程如下：

* **Python SDK**: 您可以使用 Kubeflow Pipelines 
  领域特定语言 ( [DSL](https://github.com/kubeflow/pipelines/tree/master/sdk/python/kfp/dsl) )
  创建组件或指定管道。
* **DSL 编译器**: The
  [DSL 编译器](https://github.com/kubeflow/pipelines/tree/master/sdk/python/kfp/compiler)
  将管道的 Python 代码转换为静态配置 (YAML)。
* **管道服务**: 您调用管道服务以从
  静态配置创建管道运行。
* **Kubernetes 资源**: 管道服务调用 Kubernetes API 服务器
  来创建必要的 Kubernetes 资源 ( [CRDs](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) ) 
  来运行管道。
* **编排控制器**: 一组编排控制器
  执行完成管道所需的容器。
  容器在虚拟机上的 Kubernetes Pod 中执行。
  一个示例控制器是
  **[Argo Workflow](https://github.com/argoproj/argo-workflows)** 控制器，
  它协调任务驱动的工作流。
* **Artifact 存储**: Pod 存储两种数据：

  * **Metadata:** 实验、作业、管道运行和单个标量指标。
    度量数据被聚合用于排序和过滤。
    Kubeflow Pipelines 将元数据存储在 MySQL 数据库中。
  * **Artifacts:** 管道包、视图和大规模指标（时间序列）。
    使用大规模指标来调试管道运行或调查单个运行的性能。
    Kubeflow Pipelines 将工件存储在像 [Minio 服务器](https://docs.minio.io/)
    或 [Cloud Storage](https://cloud.google.com/storage/docs/)这样的工件存储中。 

    MySQL 数据库和 Minio 服务器均由 Kubernetes
    [PersistentVolume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#types-of-persistent-volumes)
    子系统。

* **持久性代理和 ML 元数据**: 管道持久性代理
  监视由管道服务创建的 Kubernetes 资源，并将这些资源的状态
  持久化在 ML 元数据服务中。Pipeline Persistence Agent 记录
  执行的容器集及其输入和输出。
  输入/输出由容器参数或数据工件 URI 组成。
* **Pipeline web 服务器**: 管道网络服务器从各种服务收集数据
  以展示相关视图：当前运行的管道列表、
  管道执行历史、数据工件列表、有关单个管道运行的调试信息、
  有关单个管道运行的执行状态。

## 下一步

* 按照  
  [pipelines 快速指南](/docs/components/pipelines/overview/quickstart) 来
  部署 Kubeflow 并直接从 Kubeflow Pipelines UI 运行
  示例管道。
* 使用 [Kubeflow Pipelines 
  SDK](/docs/components/pipelines/sdk/sdk-overview/) 构建机器学习管道。
* 按照完整指南来试验
  [Kubeflow Pipelines 示例](/docs/components/pipelines/tutorials/build-pipeline/)。
