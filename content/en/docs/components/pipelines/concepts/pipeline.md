+++
title = "Pipeline"
description = "Kubeflow Pipelines 中管道的概念"
weight = 10
                    
+++

*pipeline* 对机器学习 (ML) 工作流的描述，包括工作流中的所有
[组件](/docs/components/pipelines/concepts/component/) 以及这些组件如何以
[graph](/docs/components/pipelines/concepts/graph/) 的方式相互关联。pipeline
配置包括运行管道所需的输入（参数）的定义
以及每个组件的输入和输出。

当您运行管道时，系统会启动一个或多个与您的工作流程（管道）中
[steps](/docs/components/pipelines/concepts/step/) (组件) 对应的 Kubernetes Pod。
Pod 启动 Docker 容器，容器依次启动您的程序。

开发管道后，您可以使用 Kubeflow Pipelines UI 或 Kubeflow Pipelines SDK 上传您的管道。

## 下一步
* 阅读 [Kubeflow Pipelines 概述](/docs/components/pipelines/introduction/).
* 参考 [pipelines 快速指引](/docs/components/pipelines/overview/quickstart/)
  部署 Kubeflow 并直接从 Kubeflow Pipelines UI 运行示例管道。
