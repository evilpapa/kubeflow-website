+++
title = "安装选项"
description = "Kubeflow Pipelines 部署方式概览"
weight = 10
                    
+++

Kubeflow Pipelines 提供了一些安装选项。本页介绍了每个选项可用的选项和功能：

* [Kubeflow Pipelines 独立安装](#kubeflow-pipelines-standalone) 是最小的便携式安装，仅包含 Kubeflow Pipelines。
* Kubeflow Pipelines 作为 [完整 Kubeflow 部署的一部分](#full-kubeflow-deployment)，
  Kubeflow Pipelines提供了所有 Kubeflow 组件以及与每个平台的更多集成。
* **Beta**: [Google Cloud AI Platform Pipelines](#google-cloud-ai-platform-pipelines) 通过 Google Cloud [Google Cloud Console](https://console.cloud.google.com/ai-platform/pipelines/clusters) 的管理界面可以更容器的安装和使用 Kubeflow Pipelines。
* [本地](/docs/components/pipelines/installation/localcluster-deployment) Kubeflow Pipelines 部署用于测试目的。

## 选择安装选项

1. 除了 Pipelines 之外，您还想使用其他 Kubeflow 组件吗？

   如果是，请选择 [完整的 Kubeflow 部署](#full-kubeflow-deployment)。
1. 您可以使用云/本地 Kubernetes 集群吗？

   如果不能，您应该按照在 [本地集群上部署 Kubeflow Pipelines 中](/docs/components/pipelines/installation/localcluster-deployment)的步骤尝试在本地 Kubernetes 集群上使用 Kubeflow Pipelines 进行学习和测试。
1. 你想使用 [multi-user 支持](https://github.com/kubeflow/pipelines/issues/1223) 的 Kubeflow Pipelines 吗？

   如果是，请选择版本 >= v1.1的 [完整 Kubeflow 部署](#full-kubeflow-deployment)。
1. 您是否在 Google Cloud 上部署？

   如果是，请部署 [Kubeflow Pipelines Standalone](#kubeflow-pipelines-standalone)。您还可以使用
   [Google Cloud AI Platform Pipelines](#google-cloud-ai-platform-pipelines) 通过用户界面部署 Kubeflow Pipelines，
   但在可定制性和可升级性方面存在限制。
   详情请阅读相应章节。
1. 您部署在其他平台上。

   请在做出决定之前将您的平台特定的 [full Kubeflow](#full-kubeflow-deployment) 与
    [Kubeflow Pipelines Standalone](#kubeflow-pipelines-standalone) 进行对比。

**警告:** 请谨慎选择您的安装选项， 当前
不支持在不同安装选项之间迁移数据。如果这对比很重要，请
创建 [一个 GitHub issue](https://github.com/kubeflow/pipelines/issues/new/choose)。


<a id="standalone"></a>
## 独立 Kubeflow Pipelines

使用此选项将 Kubeflow Pipelines 部署到本地、云甚至
本地 Kubernetes 集群，而无需 Kubeflow 的其他组件。
独立部署 Kubeflow Pipelines，您只能使用 kustomize 清单。此过程使您可以更轻松地自定义部署并将 Kubeflow Pipelines 
集成到现有的 Kubernetes 集群中。

安装指引
: [Kubeflow Pipelines 独立部署
  指南](/docs/components/pipelines/installation/standalone-deployment/)

接口
:
  * Kubeflow Pipelines UI
  * Kubeflow Pipelines SDK
  * Kubeflow Pipelines API
  * Kubeflow Pipelines endpoint **仅为 Google 云自动配置**

  如果您希望在其他平台上部署 Kubeflow Pipelines，您可以通过
  `kubectl port-forward` 访问它，或配置平台启用身份验证来访问端点。

发布时间表
: Kubeflow Pipelines 独立版在所有 Kubeflow Pipelines 版本都可用。
你将可以访问最新的功能。

升级支持 (**Beta**)
: 了解 [升级独立 Kubeflow Pipelines Standalone](/docs/components/pipelines/installation/standalone-deployment/#upgrading-kubeflow-pipelines)

Google Cloud 集成
:
  * Kubeflow Pipelines 公共端点支持 **auto-configured** 身份验证。
  * Open the Kubeflow Pipelines UI via the **Open Pipelines Dashboard** link in [the AI Platform Pipelines dashboard of Cloud Console](https://console.cloud.google.com/ai-platform/pipelines/clusters).
  * (Optional) You can choose to persist your data in Google Cloud managed storage (Cloud SQL and Cloud Storage).
  * [All options to authenticate to Google Cloud](/docs/gke/pipelines/authentication-pipelines/) are supported.

具体功能说明：
:
  * After deployment, your Kubernetes cluster contains Kubeflow Pipelines only.
  It does not include the other Kubeflow components.
  For example, to use a Jupyter Notebook, you must use a local notebook or a
  hosted notebook in a cloud service such as the [AI Platform
  Notebooks](https://cloud.google.com/ai-platform/notebooks/docs/).
  * Kubeflow Pipelines multi-user support is **not available** in standalone, because
  multi-user support depends on other Kubeflow components.

<a id="full-kubeflow"></a>
## 完整的 Kubeflow 部署

Use this option to deploy Kubeflow Pipelines to your local machine, on-premises,
or to a cloud, as part of a full Kubeflow installation.

安装指引
: [Kubeflow installation guide](/docs/started/getting-started/)

接口
:
  * Kubeflow UI
  * Kubeflow Pipelines UI within or outside the Kubeflow UI
  * Kubeflow Pipelines SDK
  * Kubeflow Pipelines API
  * Other Kubeflow APIs
  * Kubeflow Pipelines endpoint is auto-configured with auth support for each platform

发布时间表
: The full Kubeflow is released quarterly. It has significant delay in receiving
Kubeflow Pipelines updates.

| Kubeflow Version       | Kubeflow Pipelines Version |
|------------------------|----------------------------|
| 0.7.0                  | 0.1.31                     |
| 1.0.0                  | 0.2.0                      |
| 1.0.2                  | 0.2.5                      |
| 1.1.0                  | 1.0.0                      |
| 1.2.0                  | 1.0.4                      |
| 1.3.0                  | 1.5.0                      |
| 1.4.0                  | 1.7.0                      |

Note: Google Cloud, AWS, and IBM Cloud have supported Kubeflow Pipelines 1.0.0 with multi-user separation. Other platforms might not be up-to-date for now, refer to [this GitHub issue](https://github.com/kubeflow/manifests/issues/1364#issuecomment-668415871) for status.

升级支持
:
Refer to [the full Kubeflow section of upgrading Kubeflow Pipelines on Google Cloud](/docs/gke/pipelines/upgrade/#full-kubeflow) guide.

Google Cloud 集成
:
  * A Kubeflow Pipelines public endpoint with auth support is **auto-configured** for you using [Cloud Identity-Aware Proxy](https://cloud.google.com/iap).
  * There's no current support for persisting your data in Google Cloud managed storage (Cloud SQL and Cloud Storage). Refer to [this GitHub issue](https://github.com/kubeflow/pipelines/issues/4356) for the latest status.
  * You can [authenticate to Google Cloud with Workload Identity](/docs/gke/pipelines/authentication-pipelines/#workload-identity).

Notes on specific features
:
  * After deployment, your Kubernetes cluster includes all the
  [Kubeflow components](/docs/components/).
  For example, you can use the Jupyter notebook services
  [deployed with Kubeflow](/docs/components/notebooks/) to create one or more notebook
  servers in your Kubeflow cluster.
  * Kubeflow Pipelines multi-user support is **only available** in full Kubeflow. It supports
  using a single Kubeflow Pipelines control plane to orchestrate user pipeline
  runs in multiple user namespaces with authorization.
  * Latest features and bug fixes may not be available soon because release
  cadence is long.

<a id="marketplace"></a>
## Google Cloud AI Platform Pipelines

{{% alert title="Beta release" color="warning" %}}
<p>Google Cloud AI Platform Pipelines is currently in <b>Beta</b> with
  limited support. The Kubeflow Pipelines team is interested in any feedback you may have,
  in particular on the usability of the feature.

  You can raise any issues or discussion items in the
  <a href="https://github.com/kubeflow/pipelines/issues">Kubeflow Pipelines
  issue tracker</a>.</p>
{{% /alert %}}

Use this option to deploy Kubeflow Pipelines to Google Kubernetes Engine (GKE)
from Google Cloud Marketplace. You can deploy Kubeflow Pipelines to an existing or new
GKE cluster and manage your cluster within Google Cloud.

安装指引
: [Google Cloud AI Platform Pipelines documentation](https://cloud.google.com/ai-platform/pipelines/docs)

接口
:
  * Google Cloud Console for managing the Kubeflow Pipelines cluster and other Google Cloud
    services
  * Kubeflow Pipelines UI via the **Open Pipelines Dashboard** link in the
    Google Cloud Console
  * Kubeflow Pipelines SDK in Cloud Notebooks
  * Kubeflow Pipelines endpoint of your instance is auto-configured for you

发布时间表
: AI Platform Pipelines is available for a chosen set of stable Kubeflow
Pipelines releases. You will receive updates slightly slower than Kubeflow
Pipelines Standalone.

升级支持 (**Alpha**)
: An in-place upgrade is not supported.

To upgrade AI Platform Pipelines by reinstalling it (with existing data), refer to the [Upgrading AI Platform Pipelines](/docs/gke/pipelines/upgrade/#ai-platform-pipelines) guide.

Google Cloud 集成
:
  * You can deploy AI Platform Pipelines on [Cloud Console UI](https://console.cloud.google.com/marketplace/details/google-cloud-ai-platform/kubeflow-pipelines).
  * A Kubeflow Pipelines public endpoint with auth support is **auto-configured** for you.
  * (Optional) You can choose to persist your data in Google Cloud managed storage services (Cloud SQL and Cloud Storage).
  * You can [authenticate to Google Cloud with the Compute Engine default service account](/docs/gke/pipelines/authentication-pipelines/#compute-engine-default-service-account). However, this method may not be suitable if you need workload permission separation.
  * You can deploy AI Platform Pipelines on both public and private GKE clusters as long as the cluster [has sufficient resources for AI Platform Pipelines](https://cloud.google.com/ai-platform/pipelines/docs/configure-gke-cluster#ensure).

具体功能说明：
:
  * After deployment, your Kubernetes cluster contains Kubeflow Pipelines only.
  It does not include the other Kubeflow components.
  For example, to use a Jupyter Notebook, you can use [AI Platform
  Notebooks](https://cloud.google.com/ai-platform/notebooks/docs/).
  * Kubeflow Pipelines multi-user support is **not available** in AI Platform Pipelines, because
  multi-user support depends on other Kubeflow components.
