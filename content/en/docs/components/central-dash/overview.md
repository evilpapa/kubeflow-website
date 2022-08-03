+++
title = "看板中心"
description = "Kubeflow 用户界面 (UI) 简介"
weight = 10

+++

{{% stable-status %}}

您的 Kubeflow 部署包括一个看板中心，
可让您快速访问集群中部署的 Kubeflow 组件。
仪表板包括以下功能：

- 特定操作的快捷方式、最近的管道和笔记本列表以及指标，
  让您在一个视图中概览您的作业和集群。
- 集群中运行的组件的 UI 的外壳，包括 
  **Pipelines**、**Katib**、**Notebooks** 等。
- 必要时提示新用户设置其命名空间的 [registration 流程](/docs/components/central-dash/registration-flow/)。

## Kubeflow UI 概览

Kubeflow UI 包括以下内容：

* **主页**: 主页，是访问最新资源、活跃实验和
  有用文档的中心枢纽。
* **Notebook Servers**: 用来管理 [Notebook servers](/docs/components/notebooks/)。
* **TensorBoards**: 用来管理 TensorBoard servers。
* **Models**: 管理部署的 [KFServing models](/docs/components/kfserving/kfserving/)。
* **Volumes**: 管理集群数据卷。
* **Experiments (AutoML)**: 管理 [Katib](/docs/components/katib/) 实验。
* **Experiments (KFP)**: 管理 [Kubeflow Pipelines (KFP)](/docs/components/pipelines/) 实验。
* **Pipelines**: 管理 KFP pipelines。
* **Runs**: 管理 KFP runs。
* **Recurring Runs**: 管理 KFP recurring runs。
* **Artifacts**: 跟踪 ML Metadata (MLMD) 工件。
* **Executions**: 跟踪 MLMD 中各种组件的运行。
* **管理贡献者**: 在 Kubeflow 部署中配置跨命名空间的用户访问共享。

看板中心如下所示：

<img src="/docs/images/central-ui.png"
  alt="Kubeflow central UI"
  class="mt-3 mb-3 border border-info rounded">

## 访问看板中心

要访问看板中心，您需要连接到提供给 Kubeflow 的
[Istio 网关](https://istio.io/docs/concepts/traffic-management/#gateways)，它提供了接入
Kubeflow 的[服务网格](https://istio.io/docs/concepts/what-is-istio/#what-is-a-service-mesh)。

访问 Istio 网关的方式取决于您的配置方式。

## 使用 Google Cloud Platform (GCP) 的 URL 模式

果您按照指南 [在 GCP 上配置 Kubeflow](/docs/gke/deploy/)，
则可以通过以下模式的 URL 访问 Kubeflow 看板中心 UI：

```
https://<application-name>.endpoints.<project-id>.cloud.goog/
```

该 URL 会显示上图所示的仪表板。

如果您使用 Cloud Identity-Aware Proxy (IAP) 部署 Kubeflow，Kubeflow 使用 
[Let's Encrypt](https://letsencrypt.org/) 服务为 Kubeflow UI 提供 SSL 证书。
如需对证书问题进行故障排除，请参阅[监控 Cloud IAP 设置](/docs/gke/deploy/monitor-iap-setup/)的指南。

## 使用 kubectl 和端口转发

如果您没有将 Kubeflow 配置为与身份提供者集成，
那么您可以直接端口转发到 Istio 网关。

如果以下任何一项为真，端口转发通常不起作用：

  * 您已经使用 [CLI 部署](/docs/gke/deploy/deploy-cli/)的默认设置
  * 在 GCP 上部署了 Kubeflow。

  * 您已将 Istio 入口配置为仅
    接受特定域或 IP 地址上的 HTTPS 流量。

  * 您已将 Istio 入口配置为执行授权检查
    （例如，使用 Cloud IAP 或 [Dex](https://github.com/dexidp/dex)）。


您可以通过 `kubectl` 端口转发访问 Kubeflow，如下所示：

1. 如果您还没有这样做，请安装 `kubectl`：

    * 如果您在 GCP 上使用 Kubeflow，请在命令行上运行以下命令：
    `gcloud components install kubectl`。
    * 或者，按照 [`kubectl`
    安装指南](https://kubernetes.io/docs/tasks/tools/install-kubectl/)进行操作。

1. 使用以下命令设置到
  [Istio 网关](https://istio.io/docs/tasks/traffic-management/ingress/ingress-control/)的端口转发。

    {{% code-webui-port-forward %}}

1. 访问看板中心：

    ```
    http://localhost:8080/
    ```

    根据您配置 Kubeflow 的方式，并非所有 UI 都在端口转发
    到反向代理之后工作。

    对于某些 Web 应用程序，您需要配置应用程序
    所服务的基本 URL。

    例如，如果您将 Kubeflow 部署并使用入口服务
    `https://example.mydomain.com` 并将应用程序
    配置服务在 URL `https://example.mydomain.com/myapp` ，则
    应用程序在服务时可能无法运行，
    因为路径`https://localhost:8080/myapp` 不匹配。

## 下一步

* 探索 [贡献管理
  选项](/docs/components/multi-tenancy/)，可以在其中为
  共享部署设置单个命名空间或
  为 Kubeflow 部署配置多租户。
* 为 Kubeflow [设置 Jupyter notebooks](/docs/components/notebooks/setup/)。
