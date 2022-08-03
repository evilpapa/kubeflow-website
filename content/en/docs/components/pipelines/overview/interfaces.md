+++
title = "Pipelines 接口"
description = "与 Kubeflow Pipelines 系统交互的方式"
weight = 20
                    
+++

本页面介绍了可用于通过 Kubeflow Pipelines 构建和运行
机器学习 (ML) 工作流的接口。

## 用户界面 (UI)

通过点击 Kubeflow UI 中的 **Pipeline Dashboard** 访问 Kubeflow Pipelines UI。
Kubeflow Pipelines UI 看起来是这样：
  <img src="/docs/images/pipelines/pipelines-ui.png" 
    alt="Pipelines UI"
    class="mt-3 mb-3 border border-info rounded">

在 Kubeflow Pipelines UI 中，您可以执行以下任务：

* 运行一个或多个预加载示例以快速试用管道。
* 将管道作为压缩文件上传。管道可以是您
  构建的管道（请参阅如何[构建管道](/docs/components/pipelines/sdk/build-pipeline/)）
  或有人与您共享的管道。
* 创建一个 *实验* 来对一个或多个管道运行进行分组。
  请参阅 [实验的定义](/docs/components/pipelines/concepts/experiment/)。
* 在实验中创建并开始 *run* 。run 是管道的
  单次执行。参阅 [run 的
  定义](/docs/components/pipelines/concepts/run/)。
* 探索管道运行的配置、图以及输出。
* 比较实验中一次或多次运行的结果。
* 通过创建重复 run 来安排运行。

有关访问 Kubeflow Pipelines UI 和运行示例的更多信息，
请参阅 [快速指引](/docs/components/pipelines/overview/quickstart/)。

构建管道组件时，您可以写出信息以在 UI 中显示。
参阅 [导出 
指标](/docs/components/pipelines/sdk/pipelines-metrics/) 及 [UI 
可视化结果](/docs/components/pipelines/sdk/output-viewer/)。

## Python SDK

Kubeflow Pipelines SDK 提供了一组 Python 包，可用于
指定和运行 ML 工作流。

参阅 [Kubeflow Pipelines SDK
简介](/docs/components/pipelines/sdk/sdk-overview/) ，
了解使用 SDK 构建管道组件和管道的方式的概述。

## REST API

Kubeflow Pipelines API 对于持续集成/部署系统
很有用，例如，您希望将管道执行合并
到 shell 脚本或其他系统中。
例如，您可能希望在新数据进入时触发管道运行。

参阅 [Kubeflow Pipelines API 参考 
文档](/docs/components/pipelines/reference/api/kubeflow-pipeline-api-spec/)。
