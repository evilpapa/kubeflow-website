+++
title = "介绍"
description = "Kubeflow 介绍"
weight = 1
+++

Kubeflow 项目致力于使在 Kubernetes 上部署机器学习 (ML) 工作流
变得简单、可移植和可扩展。我们的目标
不是重新创建其他服务，而是提供一种直接的方式来将用于 ML 的同类
最佳开源系统部署到不同的基础设施。在任何
运行 Kubernetes 的地方，都应该能够运行 Kubeflow。

## Kubeflow 入门

阅读 [architecture 概述](/docs/started/architecture/)，
了解 Kubeflow 架构的介绍，
并了解如何使用 Kubeflow 管理您的 ML 工作流。

按照 [安装 Kubeflow](/docs/started/installing-kubeflow/) 来设置
并安装您的 Kubeflow 环境。

观看以下视频，其中介绍了 Kubeflow。

{{< youtube id="cTZArDgbIWw" title="Introduction to Kubeflow">}}

## 什么是 Kubeflow？

Kubeflow 是 _Kubernetes的机器学习工具包_。

要使用 Kubeflow，基本的工作流程是：

- 下载并运行 Kubeflow 部署二进制文件。
- 自定义生成的配置文件。
- 运行指定的脚本以将容器部署到您的
  特定环境。

您可以调整配置以选择要用于 ML 工作流的
每个阶段的平台和服务：

1. 数据准备
2. 模型训练，
3. 预估服务
4. 服务管理

您可以选择在本地、本地或云环境中
部署 Kubernetes 工作负载。

## Kubeflow 职责

我们的目标是通过让 Kubernetes 做它擅长的事情，
使扩展机器学习 (ML) 模型并将它们部署到生产环境中尽可能简单：

- 在各种基础设施上轻松、可重复、可移植的部署
  （例如，在笔记本电脑上进行试验，然后迁移到
  本地集群或云）
- 部署和管理松散耦合的微服务
- 根据需求扩展

由于 ML 从业者使用多种工具，其中一个关键目标是
根据用户需求（在合理范围内）自定义堆栈，并让
系统处理“无聊的事情”。
虽然我们从一组狭窄的技术开始，但我们正在与许多
不同的项目合作，以包括额外的工具。

最终，我们希望拥有一组简单的清单，在_任何_ Kubernetes 运行的地方
为您提供易于使用的 ML 堆栈，并且可以根据其部署到的集群
进行自我配置。

## 历史

Kubeflow 最初是谷歌内部运行 [TensorFlow](https://www.tensorflow.org/) 的开源方式 [TensorFlow Extended](https://www.tensorflow.org/tfx/)。
它最初只是在 Kubernetes 上运行 TensorFlow 作业的一种更简单的方法，但后来扩展为用于运行端到端机器学习工作流的多架构、多云框架。

## 路线图

要了解未来版本的 Kubeflow 中会出现什么，请参阅 [Kubeflow 路线图](https://github.com/kubeflow/kubeflow/blob/master/ROADMAP.md)。

以下组件也有路线图：

- [Kubeflow 管道](https://github.com/kubeflow/pipelines/blob/master/ROADMAP.md)
- [KF Serving](https://github.com/kubeflow/kfserving/blob/master/ROADMAP.md)
- [Katib](https://github.com/kubeflow/katib/blob/master/ROADMAP.md)
- [Training 控制器](https://github.com/kubeflow/common/blob/master/ROADMAP.md)

## 深入了解

有很多方法可以为 Kubeflow 做出贡献，我们欢迎贡献！

阅读 [贡献者指南](/docs/about/contributing/) 以开始使用代码，并在 [社区页面](/docs/about/community/) 了解社区。
