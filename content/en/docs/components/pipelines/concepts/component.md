+++
title = "组件"
description = "Kubeflow Pipelines 中组件的概念"
weight = 20
                    
+++

*pipeline component* 是一组自包含的代码，它
执行 ML 工作流（管道）中的一个步骤，例如数据预处理、数据转换、
模型训练等。组件类似于函数，因为它具有名称、参数、返回值和主体。

## 组件代码

每个组件的代码包括以下内容：

* **客户端代码:** 与端点对话以提交作业的代码。例如，
  与 Google Dataproc API 对话以提交 Spark 作业的代码。

* **运行时代码:** 执行实际工作并且通常在集群中运行的代码。例如，
  将原始数据转换为预处理数据的 Spark 代码。

请注意客户端代码和运行时代码的命名约定——对于名为“mytask”的任务：

* `mytask.py` 程序包含客户端代码。
* `mytask` 目录包含所有运行时代码。

## 组件定义

YAML 格式的组件规范描述了 Kubeflow Pipelines 系统的组件。
组件定义包含以下部分：

* **Metadata:** 名称、描述等。
* **Interface:** 输入/输出规范（名称、类型、描述、默认值等）。
* **Implementation:** 在给定组件输入的一组参数值的情况下，
  如何运行组件的规范。
  实现部分还描述了如何在组件完成运行后从组件中获取输出值。

有关组件的完整定义，请参阅
[component 规范](/docs/components/pipelines/reference/component-spec/)。

## 组件容器化

必须将组件打包为
[Docker 镜像](https://docs.docker.com/get-started/)。组件
代表容器内的特定程序或入口点。

管道中的每个组件都是独立执行的。组件不在同一个
进程中运行，不能直接共享内存中的数据。您必须通过序列化（到字符串或文件）在组件之间
传递的所有数据片段，以便数据可以通过分布式网络传输。
然后，您必须反序列化数据以在下游组件中使用。

## 下一步

* 阅读 [Kubeflow Pipelines 概述](/docs/components/pipelines/introduction/).
* 参阅 [pipelines 快速开始](/docs/components/pipelines/overview/quickstart/)
  部署 Kubeflow 并直接从 Kubeflow Pipelines UI 运行示例管道。
* 建您自己的
  [组件和管道](/docs/components/pipelines/sdk/build-component/).
* 构建 [可复用组件](/docs/components/pipelines/sdk/component-development/)
  在多个管道中复用。