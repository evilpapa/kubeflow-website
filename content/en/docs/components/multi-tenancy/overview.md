+++
title = "多用户隔离介绍"
description = "Kubeflow 管理员为什么需要多用户隔离？"
weight = 10
                    
+++

{{% stable-status %}}

在 Kubeflow 集群中，通常需要将用户隔离到一个组中，其中一个组包含一个或多个用户。此外，用户可能需要属于多个组。Kubeflow 的多用户隔离简化了用户操作，因为每个用户只查看和编辑其配置中定义的 Kubeflow 组件和模型工件。用户的视图不会被不在其配置中的组件或模型工件弄乱。这种隔离还提供了高效的基础设施和操作，即单个集群支持多个隔离用户，并且不需要管理员操作不同的集群来隔离用户。

## 关键概念

**Administrator**: 管理员是创建和维护 Kubeflow 集群的人。此人为其他用户配置权限（即查看、编辑）。

**User**: 用户是有权访问集群中某些资源集的人。用户需要被管理员授予访问权限。

**Profile**: 配置文件是用户的唯一配置，它决定了他们的访问权限并由管理员定义。

**Isolation**: 隔离使用 Kubernetes 命名空间。命名空间隔离用户或一组用户，即 Bob 的命名空间或 Bob 和 Sara 共享的 ML Eng 命名空间。

**Authentication**: 身份验证由 Istio 和 OIDC 的集成提供，并由 mTLS 保护。更多细节可以在 [这里找到](https://journal.arrikto.com/kubeflow-authentication-with-istio-dex-5eafdfac4782) 

**Authorization**: 授权由与 Kubernetes RBAC 的集成提供。

Kubeflow 多用户隔离由 Kubeflow 管理员配置。管理员为每个用户配置 Kubeflow 用户配置文件。创建并应用配置后，用户只能访问管理员为其配置的 Kubeflow 组件。该配置限制未经授权的 UI 用户查看或意外删除模型工件。

通过多用户隔离，对用户进行身份验证和授权，然后提供基于时间的令牌，即 json Web 令牌 (JWT)。访问令牌作为用户请求中的 Web 标头携带，并授权用户访问其配置文件中配置的资源。Profile 配置了几个项目，包括用户的命名空间、RBAC RoleBinding、Istio ServiceRole 和 ServiceRoleBindings 以及资源配额和自定义插件。可以在 [此处](https://github.com/kubeflow/kubeflow/blob/master/components/profile-controller/README.md)找到有关配置文件定义和相关 CRD 的更多信息。


## 现有集成

这些 Kubeflow 组件可以支持多用户隔离：Central Dashboard、Notebooks、Pipelines、AutoML (Katib)、KFServing。此外，笔记本创建的资源（例如，培训作业和部署）也继承相同的访问权限。

重要说明：多用户隔离有几个可配置的依赖关系，尤其是与 Kubeflow 如何与底层 Kubernetes 集群的身份管理系统配置相关的依赖关系。此外，Kubeflow 多用户隔离不提供硬性安全保证，防止恶意企图渗透其他用户的配置文件。

在配置多用户隔离以及您的安全和身份管理要求时，建议您咨询您的 [分发提供商](https://www.kubeflow.org/docs/distributions/)。 KubeCon [演示文稿](https://www.youtube.com/watch?v=U8yWOKOhzes) 提供了对架构和实现的详细回顾。对于本地部署，Kubeflow 使用 Dex 作为联合 OpenID 连接提供程序，并且可以与 LDAP 或 Active Directory 集成以提供身份验证和身份服务。这可以是高级配置，建议您咨询分发提供商或为本地 Kubeflow 提供高级技术支持的团队。

## 下一步

* 了解有关多[用户隔离设计](/docs/components/multi-tenancy/design/)的更多信息。
