+++
title = "升级说明"
description = "升级 Kubeflow Pipelines Backend 时的通知和重大变更"
weight = 90
+++

页介绍升级 Kubeflow Pipelines Backend 时需要了解的注意事项和重大变更。

有关升级说明，请参阅特定于发行版的文档：

* [在 Google Cloud 升级 Kubeflow Pipelines](/docs/distributions/gke/pipelines/upgrade/)

## 升级到 [v1.7]

[v1.7]: https://github.com/kubeflow/pipelines/releases/tag/1.7.0

* **重大变化**: 元数据 UI 和可视化与 TensorFlow Extended (TFX) <= v1.0.0 不兼容。升级到 v1.2.0 或以上，参考 [Kubeflow Pipelines Backend 及 TensorFlow Extended (TFX) 兼容矩阵](/docs/components/pipelines/installation/compatibility-matrix/)。

* **注意**: Emissary executor (Alpha)，一个新的 argo 工作流执行器作为一个选项提供。 由于 [Kubernetes 在 v1.20 之后启用 Docker 容器运行时](https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/)，在不久的将来，emissary 可能会成为 Kubeflow Pipelines 的默认工作流执行器。

    例如，当前默认的 docker 执行器不能在开箱即用的 Google Kubernetes Engine (GKE) 1.19+ 上运行。要使用 docker 执行器，您的集群节点映像必须配置为使用 docker（已弃用）作为容器运行时。

    或者，使用 emissary executor (Alpha) 消除了对容器运行时的限制，但请注意您的某些管道可能需要手动迁移。Kubeflow Pipelines 团队欢迎您在 [Emissary Executor github 问题反馈](https://github.com/kubeflow/pipelines/issues/6249) 提交反馈。

  有关这两个选项的详细配置和迁移说明，请参阅 [Argo Workflow Executors](https://www.kubeflow.org/docs/components/pipelines/installation/choose-executor/)。

 * **注意**: [Kubeflow Pipelines SDK v2 兼容模式](/docs/components/pipelines/sdk-v2/v2-compatibility/) (Beta) 最近已发布。新模式增加了对使用 ML 元数据跟踪管道运行和工件的支持。在 v1.7 后端，新增了对 v2 兼容模式的完整 UI 支持和缓存功能。我们欢迎您就您遇到的积极经验或问题提供任何 [反馈](https://github.com/kubeflow/pipelines/issues/6451)。
