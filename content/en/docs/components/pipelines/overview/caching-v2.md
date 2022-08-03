+++
title = "缓存 v2"
description = "Kubeflow Pipelines 步骤缓存 v2"
weight = 41

+++
{{% beta-status
feedbacklink="https://github.com/kubeflow/pipelines/issues" %}}

从 [Kubeflow Pipelines SDK v2](https://www.kubeflow.org/docs/components/pipelines/sdk-v2/) 和 Kubeflow Pipelines 1.7.0 开始，Kubeflow Pipelines 在 [独立部署](https://www.kubeflow.org/docs/components/pipelines/installation/standalone-deployment/) 和 [AI Platform Pipelines](https://cloud.google.com/ai-platform/pipelines/docs) 中都支持步骤缓存功能。

## 在你开始之前
本指南告诉您 Kubeflow Pipelines 缓存的基本概念以及如何使用它。
本指南假定您已经安装了 Kubeflow Pipelines，或者想要使用 [Kubeflow Pipelines 部署
指南](/docs/components/pipelines/installation/) 的独立部署 或者 AI Platform Pipelines 方式发布 Kubeflow Pipelines。

## 什么是分步缓存？

Kubeflow Pipelines 缓存提供步进级输出缓存，该过程通过跳过在先前管道运行中完成的计算来帮助降低成本。
使用 [Kubeflow Pipelines SDK v2](https://www.kubeflow.org/docs/components/pipelines/sdk-v2/) 中 `kfp.dsl.PipelineExecutionMode.V2_COMPATIBLE`模式创建的所有管道任务默认都是启用缓存的。
当 Kubeflow Pipeline 运行管道时，它会通过每个管道任务的接口
检查 Kubeflow Pipeline 中是否存在执行。
任务的接口定义为管道任务规范（基础镜像、命令、参数）、管道任务的输入（工件的名称和 id、参数的名称和值），
以及管道任务的输出规范（工件和参数）。
注意：如果生成工件的生产者任务没有缓存，那么生产者任务将生成一个具有不同 ID 的新工件，使用生产者任务生成的工件的下游任务不会命中缓存。

如果 Kubeflow Pipelines 中有匹配的执行，则使用该执行的输出，并跳过该任务。缓存被命中的示例：

<img src="/docs/images/pipelines/v2/cacheicon.png" 
  alt="Cache is hit on KFPv2 pipelines"
  class="mt-3 mb-3 border border-info rounded">


## 缓存开闭

[Kubeflow Pipelines SDK v2](https://www.kubeflow.org/docs/components/pipelines/sdk-v2/) 的 `kfp.dsl.PipelineExecutionMode.V2_COMPATIBLE` 模式默认开启缓存。

您可以为使用 Python 创建的管道运行关闭执行缓存。当管道使用 [create_run_from_pipeline_func](https://kubeflow-pipelines.readthedocs.io/en/latest/source/kfp.client.html#kfp.Client.create_run_from_pipeline_func) 或 [create_run_from_pipeline_package](https://kubeflow-pipelines.readthedocs.io/en/latest/source/kfp.client.html#kfp.Client.create_run_from_pipeline_package) 再或 [run_pipeline](https://kubeflow-pipelines.readthedocs.io/en/latest/source/kfp.client.html#kfp.Client.run_pipeline,) 时，你可以设置 `enable_caching` 参数来指定此管道运行是否使用缓存。
