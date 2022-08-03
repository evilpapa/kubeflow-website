+++
title = "安装 Kubeflow"
description = "Kubeflow 开发配置"
weight = 20

+++

Kubeflow 是 Kubernetes 的端到端机器学习 (ML) 平台，它为 ML 生命周期中的每个阶段提供组件，从探索到训练和部署。
运营商可以选择最适合他们的用户，不需要部署每个组件。如
需详细了解 Kubeflow 的组件和架构，请参阅 <a href="/docs/started/architecture/">Kubeflow 架构</a>页面。

有两种方法可以启动和运行 Kubeflow，您可以：
1. 使用[打包发行版](#packaged-distributions)
1. 使用[manifests](#manifests) (高级)

<a id="packaged-distributions"></a>
## 安装打包的 Kubeflow 发行版

{{% alert color="warning" %}}
打包发行版由各自的维护者开发和支持，Kubeflow 社区目前不认可或认证任何发行版。
{{% /alert %}}

<b>有关选项列表和文档链接，请参见下表：</b>

<div class="table-responsive">
  <table class="table table-bordered">
    <thead class="thead-light">
      <tr>
        <th>名称</th>
        <th>维护者</th>
        <th>平台</th>
        <th>版本</th>
        <th>文档</th>
        <th>网站</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>AWS 上的 Kubeflow</td>
        <td>Amazon Web Services (AWS)</td>
        <td>亚马逊弹性 Kubernetes 服务 (EKS)</td>
        <td>1.3</td>
        <td><a href="/docs/distributions/aws/">文档</a></td>
        <td></td>
      </tr>
      <tr>
        <td>Azure 上的 Kubeflow</td>
        <td>Microsoft Azure</td>
        <td>Azure Kubernetes 服务 (AKS)</td>
        <td>1.2</td>
        <td><a href="/docs/distributions/azure/">文档</a></td>
        <td></td>
      </tr>
      <tr>
        <td>谷歌云上的 Kubeflow</td>
        <td>谷歌云</td>
        <td>Google Kubernetes Engine (GKE)</td>
        <td>{{% gke/latest-version %}}</td>
        <td><a href="/docs/distributions/gke/">文档</a></td>
        <td></td>
      </tr>
      <tr>
        <td>IBM Cloud 上的 Kubeflow</td>
        <td>IBM Cloud</td>
        <td>IBM Cloud Kubernetes 服务 (IKS) </td>
        <td>1.5</td>
        <td><a href="/docs/distributions/ibm/">文档</a></td>
        <td><a href="https://github.com/IBM/manifests/tree/v1.5-branch">外部网站</a></td>
      </tr>
      <tr>
        <td>Nutanix 上的 Kubeflow</td>
        <td>Nutanix</td>
        <td>Nutanix Karbon</td>
        <td>1.4</td>
        <td><a href="/docs/distributions/nutanix/">文档</a></td>
        <td></td>
      </tr>
      <tr>
        <td>OpenShift 上的 Kubeflow</td>
        <td>Red Hat</td>
        <td>OpenShift</td>
        <td>1.3</td>
        <td><a href="/docs/distributions/openshift/">文档</a></td>
        <td><a href="https://opendatahub.io/docs/kubeflow.html">外部网站</a></td>
      </tr>
      <tr>
        <td>Argoflow</td>
        <td>Argoflow 社区</td>
        <td>Conformant Kubernetes</td>
        <td>1.3</td>
        <td>N/A</td>
        <td><a href="https://github.com/argoflow/argoflow">外部网站</a></td>
      </tr>
      <tr>
        <td>Arrikto MiniKF</td>
        <td>Arrikto</td>
        <td>AWS 市场、GCP 市场</td>
        <td>1.4</td>
        <td><a href="/docs/distributions/minikf/">文档</a></td>
        <td><a href="https://www.arrikto.com/get-started/">外部网站</a></td>
      </tr>
      <tr>
        <td>Arrikto Enterprise Kubeflow</td>
        <td>Arrikto</td>
        <td>EKS,
            AKS,
            GKE
        </td>
        <td>1.4</td>
        <td>
          <a href="/docs/distributions/ekf/">文档</a>
        </td>
        <td>
          <a href="https://www.arrikto.com/enterprise-kubeflow/">外部网站</a>
        </td>
      </tr>
      <tr>
        <td>Charmed Kubeflow</td>
        <td>Canonical</td>
        <td>Conformant Kubernetes</td>
        <td>1.4</td>
        <td><a href="/docs/distributions/charmed/">文档</a></td>
        <td><a href="https://charmed-kubeflow.io/docs/quickstart">外部网站</a></td>
      </tr>
    </tbody>
  </table>
</div>

<a id="manifests"></a>
## 手动安装 Kubeflow 清单

{{% alert color="warning" %}}
此方法适用于高级用户。Kubeflow 社区将不支持特定于环境的问题。如果您需要支持，请考虑使用 Kubeflow 的[打包发行版](#packaged-distributions)。
{{% /alert %}}

<a href="https://github.com/kubeflow/community/tree/master/wg-manifests">清单工作组</a>负责汇总每个官方 Kubeflow 组件的权威清单。
虽然这些清单旨在作为打包发行版的基础，但高级用户可以选择按照 <a href="https://github.com/kubeflow/manifests#installation">这些说明</a>直接安装它们。

<a id="next-steps"></a>
## 下一步

* 查看 Kubeflow <a href="/docs/components/">组件文档</a>
* 探索 <a href="/docs/components/pipelines/sdk/">Kubeflow Pipelines SDK</a>
