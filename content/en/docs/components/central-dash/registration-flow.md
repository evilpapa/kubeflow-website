+++
title = "注册流程"
description = "在 Kubeflow 设置命名空间"
weight = 10
                    
+++
{{% alert title="已过时" color="warning" %}}
本指南包含有关 Kubeflow 1.0 的过时信息。本指南需要
针对 Kubeflow 1.1 进行更新。
{{% /alert %}}

本指南适用于首次登录 Kubeflow 的 Kubeflow 用户。
用户可能是部署 Kubeflow 的人，
也可能是其他有权访问 Kubeflow 集群并使用 Kubeflow 的人。

## 命名空间简介

根据您的 Kubeflow 集群的设置，您可能需要
在首次登录 Kubeflow 时创建 **命名空间**。
命名空间有时称为 **配置文件**或**工作组**。

Kubeflow 在以下情况下会提示您创建命名空间：

* 对于支持多用户隔离的 Kubeflow 部署：
  您的用户名还没有与角色绑定相关的命名空间，
  这些角色绑定可以让您对命名空间进行管理（所有者）访问。
* 对于支持单用户隔离的 Kubeflow 部署：
  Kubeflow 集群没有命名空间角色绑定。

如果 Kubeflow 没有提示您创建命名空间，那么
您的 Kubeflow 管理员可能已经为您创建了命名空间。
您应该能够看到 Kubeflow 中央仪表板并开始使用 Kubeflow。

## 要求

您的 Kubeflow 管理员必须执行以下步骤：

* 按照 [Kubeflow 入门指南 ](/docs/started/getting-started/)
  将 Kubeflow部署到 Kubernetes 集群。
* 授予您对 Kubernetes 集群的访问权限。参阅 [多租户
  指南](/docs/components/multi-tenancy/getting-started/#onboarding-a-new-user).

## 创建空间

如果您还没有与您的用户名关联的合适命名空间，
那么在您首次登录 Kubeflow 时会显示以下屏幕：

<img src="/docs/images/auto-profile1.png" 
  alt="Profile creation step 1"
  class="mt-3 mb-3 border border-info rounded">

单击 **Start Setup** 并按照屏幕上的说明设置命名空间。
您命名空间的默认名称是您的用户名。

创建命名空间后，您应该会看到 Kubeflow 看板中心，
您的命名空间在屏幕顶部的下拉列表中可用：

<img src="/docs/images/central-ui.png"
  alt="Kubeflow central UI"
  class="mt-3 mb-3 border border-info rounded">

## 下一步

* 在 Kubeflow [设置 Jupyter notebook](/docs/components/notebooks/setup/)。
* 阅读更多 [Kubeflow 多租户](/docs/components/multi-tenancy/)内容。
