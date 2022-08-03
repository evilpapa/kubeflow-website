+++
title = "获取支持"
description = "从哪里获取 Kubeflow 支持"
weight = 80
+++

本页面描述了当您遇到问题、有疑问或想
对 Kubeflow 提出建议时可以
探索的 Kubeflow 资源和支持选项。

<a id="application-status"></a>
## 应用状态

从 Kubeflow v1.0 发布开始，Kubeflow 社区
将*稳定状态*归于那些满足定义的
稳定性、可支持性和可升级性级别的应用程序和组件。

当您将 Kubeflow 部署到 Kubernetes 集群时，您的部署
包括许多应用程序。应用程序版本控制
独立于 Kubeflow 版本控制。
当应用程序在稳定性、可升级性以及日志记录和监控等服务的提供方面
满足某些[标准](https://github.com/kubeflow/community/blob/master/guidelines/application_requirements.md)时，
应用程序将迁移到 1.0 版。

当应用程序迁移到 1.0 版本时，Kubeflow 社区将决定
是否在 Kubeflow 的下一个主要或次要版本中
将该应用程序版本标记为*稳定*。

Kubeflow 的应用状态指标：

* **稳定** 意味着应用程序符合 达到应用程序版本 1.0 的
  [标准](https://github.com/kubeflow/community/blob/master/guidelines/application_requirements.md)，
  并且 Kubeflow 社区已经认为该应用程序对于这个版本的 Kubeflow 是稳定的。 
* **Beta** 意味着该应用程序正在向 1.0 版发布，
  并且其维护者已经传达了满足稳定状态标准的时间表。
* **Alpha** 意味着应用程序处于开发和/或集成
  到 Kubeflow 的早期阶段。

<a id="levels-of-support"></a>
## 支持级别

下表描述了您可以根据应用程序状态获得的支持级别：

<div class="table-responsive">
  <table class="table table-bordered">
    <thead class="thead-light">
      <tr>
        <th>应用状态</th>
        <th>支持等级</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>稳定</td>
        <td>Kubeflow 社区为稳定的应用程序提供 <i>最大努力</i>的支持。
          查看下面的
          <a href="#community-support">社区支持</a>部分，
          了解尽力支持的定义以及您可以报告和讨论问题的社区渠道。
          您还可以考虑向 
          <a href="#provider-support">Kubeflow community 提供商</a>或您的
          <a href="#cloud-support">云提供商</a>请求支持。
        </td>
      </tr>
      <tr>
        <td>Beta</td>
        <td>Kubeflow 社区为 beta 的应用程序提供 <i>最大努力</i>的支持。
          查看下面的
          <a href="#community-support">社区支持</a>，
          了解尽力支持的定义以及您可以报告和讨论问题的社区渠道。
        </td>
      </tr>
      <tr>
        <td>Alpha</td>
        <td>在 alpha 状态下，
          每个应用程序的响应都不同，
          具体取决于该应用程序的社区规模和该应用程序的当前积极开发水平。
        </td>
      </tr>
    </tbody>
  </table>
</div>

<a id="community-support"></a>
## 来自 Kubeflow 社区的支持

Kubeflow 拥有一个活跃且乐于助人的用户和贡献者社区。

Kubeflow 社区尽最大努力为稳定和 beta 应用程序提供支持。
**尽力而为的支持** 意味着没有正式的协议或承诺来解决问题，
但社区认识到尽快解决问题的重要性。
如果满足以下所有条件，
社区将致力于帮助您诊断和解决问题：

* 原因属于 Kubeflow 控制的技术框架。例如，
  如果问题是由组织内的特定网络配置引起的，
  Kubeflow 社区可能无法提供帮助。
* 社区成员可以重现该问题。
* 问题的报告者可以帮助进行进一步的诊断和故障排除。

您可以在以下地方提出问题和建议：

* **Slack** 用于在线聊天和消息传递。查看 Kubeflow 的 
  [Slack 工作空间和频道](/docs/about/community/#slack).
* **Kubeflow 讨论** 用于基于电子邮件的小组讨论。加入
  [kubeflow-discuss](/docs/about/community/#mailing-list) 组。
* 用于概述和操作指南的 **Kubeflow 文档**。通常，
  排除问题时，尤其要参考以下文档：

  * [Kubeflow 安装及设置](/docs/started/getting-started/)
  * [Kubeflow 组件](/docs/components/)
  * [进一步设置和故障排除](/docs/other-guides/)

* 用于已知问题、问题和功能请求的 **Kubeflow 问题追踪**。
  搜索未解决的问题以查看其他人是否已经记录了您遇到的问题
  并了解迄今为止的任何解决方法。
  如果没有人记录您的问题，请创建一个新问题来描述问题。

    每个 Kubeflow 应用程序在 [GitHub 上的 Kubeflow 组织](https://github.com/kubeflow)内
    都有自己的问题跟踪器。
    为了帮助您入门，以下是主要的问题跟踪器：

  * [Kubeflow 核心](https://github.com/kubeflow/kubeflow/issues)
  * [kfctl 命令行工具](https://github.com/kubeflow/kfctl/issues)
  * [Kustomize 清单](https://github.com/kubeflow/manifests/issues)
  * [Kubeflow Pipelines](https://github.com/kubeflow/pipelines/issues)
  * [Katib AutoML](https://github.com/kubeflow/katib/issues)
  * [Metadata](https://github.com/kubeflow/metadata/issues)
  * [Fairing notebook SDK](https://github.com/kubeflow/fairing/issues)
  * [Kubeflow Training (TFJob, PyTorchJob, MXJob, XGBoostJob)](https://github.com/kubeflow/training-operator/issues)
  * [KFServing](https://github.com/kubeflow/kfserving/issues)
  * [示例](https://github.com/kubeflow/examples/issues)
  * [文档](https://github.com/kubeflow/website/issues)

<a id="provider-support"></a>
## 来自 Kubeflow 生态系统中的供应商的支持

以下组织为 Kubeflow 部署提供建议和支持：

<div class="table-responsive">
  <table class="table table-bordered">
    <thead class="thead-light">
      <tr>
        <th>Organization</th>
        <th>Points of contact</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><a href="https://www.arrikto.com">Arrikto</a></td>
        <td><a href="mailto:sales@arrikto.com">sales@arrikto.com</a></td>
      </tr>
      <tr>
        <td><a href="https://www.ubuntu.com">Canonical</a></td>
        <td><a href="https://ubuntu.com/kubeflow#get-in-touch">保持联系</a></td>
      </tr>
      <tr>
        <td><a href="https://www.pattersonconsultingtn.com/">Patterson Consulting</a></td>
        <td> 
        <a href="http://www.pattersonconsultingtn.com/offerings/managed_kubeflow.html">托管 Kubeflow</a></td>
      </tr>
      <tr>
        <td><a href="https://www.seldon.io/">Seldon</a></td>
        <td> 
        <a href="https://github.com/SeldonIO/seldon-core/issues">问题 
        追踪</a></td>
      </tr>    
    </tbody>
  </table>
</div>

<a id="cloud-support"></a>
## 来自云平台提供商的支持

如果您使用云提供商的服务来托管 Kubeflow，云提供商
可能能够帮助您诊断和解决问题。

请参阅您正在使用的云服务或平台的支持页面：

* [亚马逊网络服务 (AWS)](https://aws.amazon.com/contact-us/)
* [Canonical Ubuntu](https://ubuntu.com/kubeflow#get-in-touch)
* [谷歌云平台 (GCP)](https://cloud.google.com/support-hub/)
* [IBM Cloud](https://www.ibm.com/cloud/support)
* [Microsoft Azure](https://azure.microsoft.com/en-au/support/options/)
* [Red Hat OpenShift](https://help.openshift.com/)

## 其他可以提问的地方

您也可以尝试在 Stack Overflow 上搜索答案或提问。
请查看 [带有
“kubeflow” 标签的问题](https://stackoverflow.com/questions/tagged/kubeflow)。

## 参与 Kubeflow 社区

您可以通过多种方式参与 Kubeflow。例如，
您可以为 Kubeflow 代码或文档做出贡献。
您可以加入社区会议，与维护者讨论特定主题。有关更多信息，请参阅
[Kubeflow community page](/docs/about/community/) 获取更多信息。

## 关注新闻

跟踪 Kubeflow 新闻：

* [Kubeflow 博客](https://blog.kubeflow.org/) 是发布
  新版本、活动和技术演练的主要渠道。
* 关注 [Kubeflow Twitter](https://twitter.com/kubeflow) 以获取
  共享的技术提示。
* 发行说明详细介绍了每个 Kubeflow 应用程序的最新更新。

    每个 Kubeflow 应用程序在 [GitHub 上的 Kubeflow 组织](https://github.com/kubeflow)内都有自己的存储库。
    一些应用程序发布发行说明。
    为了帮助您入门，以下是主要应用程序的发行说明：

  * [Kubeflow 核心](https://github.com/kubeflow/kubeflow/releases)
  * [Kubeflow Pipelines](https://github.com/kubeflow/pipelines/releases)
  * [Katib AutoML](https://github.com/kubeflow/katib/releases)
  * [Metadata](https://github.com/kubeflow/metadata/releases)
  * [Fairing notebook SDK](https://github.com/kubeflow/fairing/releases)
  * [Kubeflow Training 控制器](https://github.com/kubeflow/training-operator/releases)
  * [KFServing](https://github.com/kubeflow/kfserving/releases)
  
