+++
title = "架构"
description = "Kubeflow 架构简介"
weight = 10
+++

<!--
Note for authors: The source of the diagrams is held in Google Slides decks,
in the "Doc diagrams" folder in the public Kubeflow shared drive.
-->

本指南介绍了 Kubeflow 作为开发和部署机器学习（ML）
系统的平台。

Kubeflow 是一个数据科学家构建和实验 
ML 工作流的平台。Kubeflow 还适用于希望将 ML 系统部署
到各种环境以进行开发、测试和生产级服务的 ML 工程师
和操作团队。

## 概念概述

Kubeflow 是 *Kubernetes的机器学习工具包*。

T下图显示了 Kubeflow 作为一个平台，用于
在 Kubernetes 之上安排 ML 系统的组件：

<img src="/docs/images/kubeflow-overview-platform-diagram.svg" 
  alt="An architectural overview of Kubeflow on Kubernetes"
  class="mt-3 mb-3 border border-info rounded">

Kubeflow 构建在 [Kubernetes](https://kubernetes.io/) 作为一个
部署、扩展和管理的复杂系统。

使用 Kubeflow 配置界面（见[下文](#interfaces)），您可以
指定工作流程所需的 ML 工具。 然后，您可以将
工作流部署到各种云、本地和本地平台，以供试验
和生产使用。

## ML 工作流介绍

当您开发和部署 ML 系统时，ML 工作流通常包含几个
阶段。开发 ML 系统是一个迭代过程。
您需要评估 ML 工作流程各个阶段的输出，并在
必要时对模型和参数进行更改，以确保模型不断
产生您需要的结果。

为简单起见，下图按顺序
显示了工作流阶段。 工作流末尾的箭头指向流程，
以指示流程的迭代性质：

<img src="/docs/images/kubeflow-overview-workflow-diagram-1.svg" 
  alt="A typical machine learning workflow"
  class="mt-3 mb-3 border border-info rounded">

更详细地查看各个阶段：

* 在实验阶段，您根据
  初始假设开发模型，
  并迭代地测试和更新模型以产生您正在寻找的结果：

  * 确定您希望 ML 系统解决的问题。
  * 收集和分析训练 ML 模型所需的数据。
  * 选择 ML 框架和算法，并对模型的初始版本进行编码。
  * 试验数据并训练你的模型。
  * 调整模型超参数以确保最有效的处理和最准确的结果。

* 在生产阶段，您部署一个执行以下过程的系统：

  * 将数据转换为训练系统所需的格式。
    为了确保您的模型在训练和预测期间表现一致，
    转换过程在实验和生产阶段必须相同。
  * 训练 ML 模型。
  * 为模型提供在线预测或以批处理模式运行。
  * 监控模型的性能，并将结果输入您的流程以调整或重新训练模型。

## ML 工作流中的 Kubeflow 组件

下图将 Kubeflow 添加到工作流中，显示了各个阶段使用了
哪些 Kubeflow 组件：

<img src="/docs/images/kubeflow-overview-workflow-diagram-2.svg" 
  alt="Where Kubeflow fits into a typical machine learning workflow"
  class="mt-3 mb-3 border border-info rounded">

要了解更多信息，请阅读以下 Kubeflow 组件指南：

* Kubeflow 包括用于创建和管理
  [Jupyter notebooks](/docs/components/notebooks/) 的服务。使用笔记本进行交互式数据科学
  和 ML 工作流程试验。

* [Kubeflow Pipelines](/docs/components/pipelines/) 是一个基于 Docker 容器
  构建、部署和管理多步 ML 工作流的平台。

* Kubeflow 提供了几个 [组件](/docs/components/) ，您可以使用它们来
  构建您的 ML 训练、超参数调整以及
  跨多个平台提供工作负载。

## 特定 ML 工作流程的示例

下图显示了特定 ML 工作流的简单示例，
您可以使用该工作流来训练和服务在 MNIST 数据集上训练的模型：

<img src="/docs/images/kubeflow-gcp-e2e-tutorial-simplified.svg" 
  alt="ML workflow for training and serving an MNIST model"
  class="mt-3 mb-3 border border-info rounded">

有关工作流程的详细信息和自己运行系统，请参阅
[end-to-end tutorial for Kubeflow on GCP](https://github.com/kubeflow/examples/tree/master/mnist#mnist-on-kubeflow-on-gcp).

<a id="interfaces"></a>
## Kubeflow 接口

本节介绍可用于与 Kubeflow 交互以及在 Kubeflow 上
构建和运行 ML 工作流的接口。

### Kubeflow 用户界面 (UI) 

Kubeflow 用户界面如下所示：

<img src="/docs/images/central-ui.png" 
  alt="The Kubeflow UI"
  class="mt-3 mb-3 border border-info rounded">

UI 提供了一个中央仪表板，您可以使用它来访问 Kubeflow 
部署的组件。阅读
[如何访问中央仪表板](/docs/components/central-dash/overview/).

## Kubeflow API 和 SDK

Kubeflow 的各种组件都提供 API 和 Python SDK。
请参阅以下参考文档集：

* [Pipelines 参考手册](/docs/components/pipelines/reference/) 包括 Kubeflow
  Pipelines API 和 SDK，以及包括 Kubeflow Pipelines 领域
  特定语言 (DSL)。
* [Fairing 参考手册](/docs/external-add-ons/fairing/reference/) Kubeflow Fairing
  SDK。

## 下一步

* 按照 [Installing Kubeflow](/docs/started/installing-kubeflow/) 设置环境并安装 Kubeflow。
