+++
title = "缓存"
description = "Kubeflow Pipelines 步骤缓存入门"
weight = 40
                    
+++
{{% alpha-status
feedbacklink="https://github.com/kubeflow/pipelines/issues" %}}

从 Kubeflow Pipelines 0.4 开始，Kubeflow Pipelines 在独立部署和 AI Platform Pipelines 中都支持步骤缓存功能。

## 在你开始之前

本指南告诉您 Kubeflow Pipelines 步骤缓存的基本概念以及如何使用它。
本指南假定您已经安装了 Kubeflow Pipelines 或想要使用 [Kubeflow Pipelines 部署指南](/docs/components/pipelines/installation/) 中的 独立部署或 AI Platform Pipelines 选项来部署 Kubeflow Pipelines。

## 什么是分步缓存？

Kubeflow Pipelines 缓存提供步进级输出缓存。
所有通过 KFP 后端和 UI 提交的管道默认启用缓存。
使用 TFX SDK 创作的管道例外，它有自己的缓存机制。
缓存键计算基于组件（基本镜像、命令行、代码）、传递给组件的参数（值或工件）以及任何其他自定义。
如果组件完全相同并且参数与之前的某些执行完全相同，则可以跳过任务并使用旧步骤的输出。
可以控制缓存重用行为，并且管道作者可以指定考虑重用的缓存数据的最大陈旧度。
用缓存后，系统可以跳过已经执行的步骤，从而节省时间和金钱。

## 缓存开闭

Kubeflow Pipelines 0.4 之后默认启用缓存。
这些是有关禁用和启用缓存服务的说明：

### 配置对 Kubeflow 集群的访问

使用以下说明配置 `kubectl` 访问 Kubeflow 集群。

1.  以下命令检测 `kubectl` 是否安装：

    ```bash
    which kubectl
    ```

    响应应该是这样的：

    ```bash
    /usr/bin/kubectl
    ```

    如果未安装 `kubectl`，按照
    [kubectl 按照和设置][kubectl-install]指引操作。

2.  按照 [Kubernetes 集群的访问][kubectl-access]指南配置访问。 

### 在 Kubeflow Pipelines 部署中禁用缓存：

1. 确保 `mutatingwebhookconfiguration` 存在于您的集群中：

    ```
    export NAMESPACE=<Namespace where KFP is installed>
    kubectl get mutatingwebhookconfiguration cache-webhook-${NAMESPACE}
    ```
2. 修改 `mutatingwebhookconfiguration` 规则：

    ```
    kubectl patch mutatingwebhookconfiguration cache-webhook-${NAMESPACE} --type='json' -p='[{"op":"replace", "path": "/webhooks/0/rules/0/operations/0", "value": "DELETE"}]'
    ```

### 启用缓存

1. 确保 `mutatingwebhookconfiguration` 存在于您的集群中：

    ```
    export NAMESPACE=<Namespace where KFP is installed>
    kubectl get mutatingwebhookconfiguration cache-webhook-${NAMESPACE}
    ```
2. 修改回 `mutatingwebhookconfiguration` 规则：

    ```
    kubectl patch mutatingwebhookconfiguration cache-webhook-${NAMESPACE} --type='json' -p='[{"op":"replace", "path": "/webhooks/0/rules/0/operations/0", "value": "CREATE"}]'
    ```

## 管理缓存陈旧性

缓存默认启用，如果您曾经使用相同的参数执行过相同的组件，则将跳过该组件的任何新执行，并且将从缓存中获取输出。
在某些情况下，某些组件的缓存输出数据可能会在一段时间后变得太陈旧而无法使用。
要控制重用缓存数据的最大陈旧度，您可以设置步骤的 `max_cache_staleness` 参数。
`max_cache_staleness` 采用 [RFC3339 Duration](https://www.ietf.org/rfc/rfc3339.txt) 格式 (所以 30 days = "P30D")。
默认情况下， `max_cache_staleness` 置为无穷大，因此任何旧的缓存数据都将被重用。

步骤设置 `max_cache_staleness` 为 30 天：

```
def some_pipeline():
      # task is a target step in a pipeline
      task = some_op()
      task.execution_options.caching_strategy.max_cache_staleness = "P30D"
```

理想情况下，组件代码应该是纯粹的和确定性的，因为它在给定相同输入的情况下产生相同的输出。
如果您的组件不是确定性的 (比如，它在每次调用时返回不同的随机数) 您可能希望通过设置 `max_cache_staleness` 为 0 来禁用从此组件创建的任务的缓存：

```
def some_pipeline():
      # task is a target step in a pipeline
      task_never_use_cache = some_op()
      task_never_use_cache.execution_options.caching_strategy.max_cache_staleness = "P0D"
```
更好的解决方案是使组件具有确定性。如果组件使用随机数生成，您可以将 RNG 种子公开为组件输入。如果组件获取一些变化的数据，您可以添加时间戳或日期输入。

[kubectl-access]: https://kubernetes.io/docs/reference/access-authn-authz/authentication/
[kubectl-install]: https://kubernetes.io/docs/tasks/tools/install-kubectl/