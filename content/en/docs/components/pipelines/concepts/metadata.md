+++
title = "ML 元数据"
description = "关于 Kubeflow Pipelines 中元数据的概念"
weight = 90
                    
+++

**注意:** Kubeflow Pipelines 元数据依赖已从使用 [kubeflow/metadata](https://github.com/kubeflow/metadata)
转变为使用 [google/ml-metadata](https://github.com/google/ml-metadata)。

Kubeflow Pipelines 后端存储在元数据存储中运行的管道的运行时信息。
运行时信息包括任务的状态、工件的可用性、与执行或工件相关的自定义属性等。
了解更多 [ML Metadata 入门](https://github.com/google/ml-metadata/blob/master/g3doc/get_started.md)信息。

如果一个工件被不同运行中的多个执行使用，
您可以跨管道运行查看工件和 run 之间的连接。
这种连接可视化称为 *Lineage Graph*。

## 下一步

* 了解 [output Aritfact](/docs/components/pipelines/concepts/output-artifact)。
