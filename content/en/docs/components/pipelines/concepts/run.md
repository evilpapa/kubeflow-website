+++
title = "运行和重复运行"
description = "Kubeflow Pipelines 中运行的概念"
weight = 50
                    
+++

*run* 是管道的单次执行。 运行包含您尝试的所有实验的不可变日志，
并且设计为独立的以允许可重复性。您可以通过查看 Kubeflow Pipelines UI 上
的详细信息页面来跟踪运行的进度，
您可以在其中查看运行中每个步骤的运行时图、输出工件和日志。

<a id=recurring-run></a>
*recurring run*，或 Kubeflow Pipelines [backend APIs](https://github.com/kubeflow/pipelines/tree/06e4dc660498ce10793d566ca50b8d0425b39981/backend/api/go_http_client/job_client) 中的 job，即时
一个可重复运行管道。 重复运行的配置包括具有指定所有参数值的管道副本和[run trigger](/docs/components/pipelines/concepts/run-trigger/)。
您可以在任何实验中开始重复运行，它会定期启动运行配置的新副本。
。您可以从 Kubeflow Pipelines UI 启用/禁用重复运行。
您还可以指定最大并发运行数，以限制并行启动的运行数。
如果预计管道将运行很长时间并被触发频繁运行，这将很有帮助。

## 下一步

* 阅读 [Kubeflow Pipelines 概述](/docs/components/pipelines/introduction/).
* 参阅 [pipelines 快速开始](/docs/components/pipelines/overview/quickstart/)
  部署 Kubeflow 并直接从 Kubeflow Pipelines UI 运行示例管道。
