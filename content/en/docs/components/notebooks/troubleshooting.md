+++
title = "故障排除"
description = "Kubeflow Notebooks 常见问题的问题及解决方案"
weight = 100
                    
+++

## ISSUE: notebook 无法启动

### 解决方案: 检查笔记本的事件

运行以下命令，然后检查 `events` 部分以确保没有错误：

```shell
kubectl describe notebooks "${MY_NOTEBOOK_NAME}" --namespace "${MY_PROFILE_NAMESPACE}"
```

### 解决方案: 检查笔记本 pod 的事件

运行以下命令，然后检查 `events` 部分以确保没有错误：

```shell
kubectl describe pod "${MY_NOTEBOOK_NAME}-0" --namespace "${MY_PROFILE_NAMESPACE}"
```

### 解决方案: 检查 pod 的 YAML

运行以下命令并检查 Pod YAML 是否符合预期：

```shell
kubectl get pod "${MY_NOTEBOOK_NAME}-0" --namespace "${MY_PROFILE_NAMESPACE}" -o yaml
```

### 解决方案: 检查 Pod 日志

运行以下命令从 Pod 获取日志：

```shell
kubectl logs "${MY_NOTEBOOK_NAME}-0" --namespace "${MY_PROFILE_NAMESPACE}"
```

## 问题: 手动删除 notebook

### 解决方案: 使用 kubectl 删除 Notebook 资源

运行以下命令手动删除 Notebook 资源：

```shell
kubectl delete notebook "${MY_NOTEBOOK_NAME}" --namespace "${MY_PROFILE_NAMESPACE}"
```