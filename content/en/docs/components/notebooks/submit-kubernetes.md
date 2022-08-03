+++
title = "提交 Kubernetes 资源"
description = "从 Notebook 提交 Kubernetes 资源"
weight = 40
                    
+++

## Notebook Pod ServiceAccount

Kubeflow 将 Kubernetes ServiceAccount `default-editor` 分配给 Notebook Pod。
Kubernetes `default-editor` ServiceAccount 绑定到 `kubeflow-edit` ClusterRole，它对许多 Kubernetes 资源具有命名空间范围的权限。

你可以使用如下命令获取所有 `ClusterRole/kubeflow-edit` RBAC 的信息：
```
kubectl describe clusterrole kubeflow-edit
```

## Notebook Pod 中的 Kubectl

因为每个 Notebook Pod 都绑定了高权限的 Kubernetes ServiceAccount `default-editor`，所以您可在其中运行 `kubectl` 而无需提供额外的身份验证。

例如，以下命令将创建中定义的资源 `test.yaml`：

```shell
kubectl create -f "test.yaml" --namespace "MY_PROFILE_NAMESPACE"
```

## 下一步

- 参阅 Kubeflow Notebook [入门指南](/docs/components/notebooks/quickstart-guide/)。
- 探索其他 [Kubeflow 组件](/docs/components/)。
