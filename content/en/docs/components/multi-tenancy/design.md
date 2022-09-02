+++
title = "多用户隔离设计"
description = "支持多用户隔离的深度设计"
weight = 20
                    
+++

{{% stable-status %}}

## 设计概述

Kubeflow 多租户目前是围绕 *user namespaces* 构建的。
具体来说，Kubeflow 定义了用户特定的命名空间，并使用 Kubernetes
[role-based access control (RBAC) policies ](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) [基于角色的访问控制 (RBAC) 策略](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) 来管理用户访问。

此功能使用户能够共享对其工作区的访问。
工作区所有者可以通过 Kubeflow UI 与其他用户共享/撤销工作区访问权限。
被邀请后，用户有权编辑工作区和操作 Kubeflow 自定义资源。

Kubeflow 多租户是自助服务的——新用户可以通过 UI 自行注册以创建和拥有自己的工作空间。

Kubeflow 使用 Istio 来控制集群内流量。默认情况下，除非 Istio RBAC 允许，
否则对用户工作区的请求会被拒绝。
入站用户请求使用身份提供程序（例如，Google Cloud 上的 Identity Aware Proxy (IAP) 或用于本地部署的 Dex）进行识别，然后通过 Istio RBAC 规则进行验证。

在内部，Kubeflow 使用 *Profile* 自定义资源来控制所有涉及的策略、角色和绑定，并保证一致性。Kubeflow 还提供了一个插件接口来管理 Kubernetes 之外的外部资源/策略，
例如与 Amazon Web Services API 接口以进行身份管理。

下图说明了具有两条用户访问路由的 Kubeflow 多租户集群：通过 Kubeflow 中央仪表板和通过 kubectl 命令行界面 (CLI)。

<img src="/docs/images/multi-tenancy-cluster.png"
  alt="multi tenancy cluster "
  class="mt-3 mb-3 border border-info rounded">

## 功能要求
- Kubeflow 使用 [Istio](https://istio.io/) 对集群内流量进行访问控制。
- Kubeflow 配置文件控制器需要`cluster admin` 权限。
- Kubeflow UI 需要在身份感知代理之后提供。身份感知代理和 Kubernetes 主服务器应该共享相同的身份管理。
- Google Cloud 上的 Kubeflow 安装使用 [GKE](https://cloud.google.com/kubernetes-engine) 和 [IAP](https://cloud.google.com/iap/docs/concepts-overview)。
- Kubeflow 的本地安装使用 [Dex](https://github.com/dexidp/dex)，提供灵活的 OpenID Connect (OIDC) 服务。

## 支持的平台
* 再 GCP 上使用 [IAP](/docs/gke/deploy) 安装 Kubeflow 默认开启多租户。

## 下一步

* 了解 [如何使用多用户隔离和配置文件](/docs/components/multi-tenancy/getting-started/)。
*  Kubeflow 中阅读[有关 Istio ](/docs/external-add-ons/istio/istio-in-kubeflow/)的更多信息。
