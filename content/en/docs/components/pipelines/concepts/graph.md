+++
title = "图"
description = "Kubeflow Pipelines 中图的概念"
weight = 30
                    
+++

*graph*  Kubeflow Pipelines UI 中管道运行时执行的
图形表示。该图显示了管道运行已执行或正在执行的步骤，
箭头指示每个步骤表示的管道组件之间的父/子关系。
运行开始后即可查看图表。
图中的每个节点对应于管道中的一个步骤，并相应地进行标记。

下面的屏幕截图显示了管道图的示例：

<img src="/docs/images/pipelines-xgboost-graph.png" 
  alt="XGBoost results on the pipelines UI"
  class="mt-3 mb-3 border border-info rounded">

每个节点的右上角都有一个图标，
指示其状态：正在运行、成功、失败或已跳过。
（当其父节点包含条件子句时，可以跳过节点。）

## 下一步

* 阅读 [Kubeflow Pipelines 概述](/docs/components/pipelines/introduction/).
* 参考 [pipelines 快速指引](/docs/components/pipelines/overview/quickstart/) 
  发布 Kubeflow 并直接通过 UI 运行简单的 管道 示例。