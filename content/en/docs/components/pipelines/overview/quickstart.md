+++
title = "快速开始"
description = "Kubeflow 管道入门"
weight = 10

+++                 
{{% stable-status %}}

如果您想了解 Kubeflow Piplines 用户界面 (UI) 并快速运行简单的管道，请使用本指南。 

本快速入门指南的目标是展示如何使用 Kubeflow Pipelines 安装附带的两个示例，
这些示例在 Kubeflow Pipelines UI 上可见。
您可以将本指南用作 
Kubeflow Pipelines UI 的介绍。

## 部署 Kubeflow 并打开 Kubeflow Pipelines UI

[部署 Kubeflow Pipelines](/docs/components/pipelines/installation/overview/)有多种选择，请选择最适合您需求的选项。如果您不确定并且只想尝试 kubeflow 管道，建议您从[独立部署](/docs/components/pipelines/installation/standalone-deployment/)开始。

部署 Kubeflow Pipelines 后，请确保您可以访问 UI。访问 UI 的步骤因您用于部署 Kubeflow Pipelines 的方法而异。

## 运行一个基本管道

Kubeflow Pipelines 提供了一些示例，您可以使用
这些示例快速试用 Kubeflow Pipelines。
以下步骤向您展示了如何运行包含一些 Python 操作
但不包含机器学习 (ML) 工作负载的基本示例：

1. 在管道 UI 上单击示例名称，**[Tutorial] Data passing in python components**：
  <img src="/docs/images/click-pipeline-sample.png" 
    alt="Pipelines UI"
    class="mt-3 mb-3 border border-info rounded">

1. 点击 **Create experiment**:
  <img src="/docs/images/pipelines-start-experiment.png" 
    alt="Creating an experiment on the pipelines UI"
    class="mt-3 mb-3 border border-info rounded">

1. 按照提示创建 **experiment** ，然后创建 **run**。
   该示例为您需要的所有参数提供默认值。以下
   幕截图假设您已经创建了一个名为
  _My experiment_ 的实验，现在正在创建一个名为 _My first run_ 的运行：
  <img src="/docs/images/pipelines-start-run.png" 
    alt="Creating a run on the pipelines UI"
    class="mt-3 mb-3 border border-info rounded">

1. 点击 **Start** 执行管道。
1. 单击实验仪表板上的运行名称：
  <img src="/docs/images/pipelines-experiments-dashboard.png" 
    alt="Experiments dashboard on the pipelines UI"
    class="mt-3 mb-3 border border-info rounded">

1. 通过单击图表的组件和其他 UI 元素来探索图表
   和执行的其他方面：
  <img src="/docs/images/pipelines-basic-run.png" 
    alt="Run results on the pipelines UI"
    class="mt-3 mb-3 border border-info rounded">

您可以在 Kubeflow Pipelines repo中找到 [**Data passing in python components** 教程源代码](https://github.com/kubeflow/pipelines/tree/master/samples/tutorials/Data%20passing%20in%20python%20components)。

## 运行 ML 管道

本节向您展示如何运行管道 UI 中可用的 XGBoost 示例。
与上述基本示例不同，
XGBoost 示例包含了 ML 组件。

按照以下步骤运行示例：

1. 在管道 UI 上单击示例 
  **[Demo] XGBoost - Iterative model training**：
  <img src="/docs/images/click-xgboost-sample.png" 
    alt="XGBoost sample on the pipelines UI"
    class="mt-3 mb-3 border border-info rounded">

1. 点击 **Create experiment**.
1. 按照提示创建 **experiment**，然后创建 **run**.

   以下屏幕截图显示了运行详细信息：
    <img src="/docs/images/pipelines-start-xgboost-run.png" 
      alt="Starting the XGBoost run on the pipelines UI"
      class="mt-3 mb-3 border border-info rounded">

1. 点击 **Start** 创建执行。
1. 单击实验仪表板上的运行名称。
1. 通过单击图表的组件和其他 UI 元素来
   探索图表和运行的其他方面。以下屏幕截图
   显示了管道完成运行时的部分图表：
    <img src="/docs/images/pipelines-xgboost-graph.png" 
      alt="XGBoost results on the pipelines UI"
      class="mt-3 mb-3 border border-info rounded">

你可以在 Kubeflow Pipelines repo 中找到 [**XGBoost - Iterative model training** 演示源代码](https://github.com/kubeflow/pipelines/tree/master/samples/core/xgboost_training_cm)。

## 下一步

* 详细了解 Kubeflow Pipelines 中的
  [主要概念](/docs/pipelines/overview/concepts/)。
* 本页向您展示了如何运行 Kubeflow Pipelines UI 中提供的一些示例。
  接下来，您可能希望从笔记本运行管道，或者从代码编译和运行示例。请参阅使用 
  [Kubeflow Pipelines 示例](/docs/components/pipelines/tutorials/build-pipeline/)进行试验的指南。
* 使用 [Kubeflow Pipelines 
  SDK](/docs/components/pipelines/sdk/sdk-overview/) 构建机器学习管道。
