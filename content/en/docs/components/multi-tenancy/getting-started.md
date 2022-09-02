+++
title = "多用户隔离入门"
description = "如何使用多用户隔离属性配置"
weight = 30

+++

{{% stable-status %}}

## 使用概览

安装和配置 Kubeflow 后，您将默认
访问您的 *主配置文件（primary profile）*。 *profile* 配置文件拥有同名的 Kubernetes 命名空间
以及 Kubernetes 资源的集合。 用户可以查看
和修改他们的主要配置文件。您可以与系统中的其他用户
共享对您的个人资料的访问权限。 与其他用户共享对配置文件
的访问权限时，您可以选择是仅提供读取访问权限还是提供读取/修改访问
权限。 在通过 Kubeflow 看板中心工作时，
出于所有实际目的，活动命名空间直接与
活动配置文件相关联。 

## 使用示例

您可以从 Kubeflow 看板中心的顶部栏中选择您的活动
配置文件。请注意，您只能查看
您拥有查看或修改权限的配置文件。

<img src="/docs/images/select-profile.png"
  alt="Select active profile "
  class="mt-3 mb-3 border border-info rounded">

本指南说明了使用 Jupyter 笔记本服务的
用户隔离功能，这是系统中
第一个与多用户隔离功能完全集成的服务。

选择活动配置文件后，笔记本服务器 UI 仅显示
当前所选配置文件中的活动笔记本服务器。
所有其他笔记本服务器仍然对您隐藏。如果您切换活动配置文件，
视图会适当地切换活动笔记本列表。
您可以连接到任何列出的笔记本服务器，
并查看和修改服务器中可用的现有 Jupyter 笔记本。

例如，下图显示了用户主要配置文件中
可用的笔记本服务器列表：

<img src="/docs/images/notebooks-in-profile.png"
  alt="List of notebooks in active profile "
  class="mt-3 mb-3 border border-info rounded">

当未经授权的用户访问此配置文件中的笔记本时，他们会看到
一个错误：

<img src="/docs/images/notebook-access-error.png"
  alt="Error listing notebooks in inacessible profile"
  class="mt-3 mb-3 border border-info rounded">

当您从笔记本服务器 UI 创建 Jupyter 笔记本服务器时，
会在您的活动配置文件中创建笔记本 pod。
如果您没有对活动配置文件的修改访问权限，
则只能浏览当前活动的笔记本服务器并访问现有的笔记本，
但不能在该配置文件中创建新的笔记本服务器。
您可以在您具有查看和修改访问权限的主要配置文件中创建笔记本服务器。

## 新用户入职

**administrator** 可以为Kubeflow集群中的任何用户手动创建配置文件。
这里的管理员是 在 Kubernetes 集群中具有 [*cluster-admin*](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#user-facing-roles) 角色。
此人有权在集群中创建和修改 Kubernetes 资源。
例如，部署 Kubeflow 的人将在集群中拥有管理权限。

我们推荐这种方法，因为它鼓励采用 GitOps 流程来处理配置文件的创建。

Kubeflow {{% kf-latest-version %}} 可选择在首次登录时为经过身份验证的用户提供自动配置文件创建工作流程。

### 先决条件：授予用户最小 Kubernetes 集群访问权限

您必须为每个用户授予允许他们连接到 Kubernetes 集群的最小权限范围。

例如，对于 Google Cloud 用户，您应该授予以下 Cloud Identity and Access Management (IAM) 角色。
在以下命令中，替换 `[PROJECT]` 为您的 Google Cloud 项目并替换 `[EMAIL]` 为用户的电子邮件地址：

* 要访问 Kubernetes 集群，用户需要 [Kubernetes Engine
  Cluster Viewer](https://cloud.google.com/kubernetes-engine/docs/how-to/iam)
  角色：

    ```
    gcloud projects add-iam-policy-binding [PROJECT] --member=user:[EMAIL] --role=roles/container.clusterViewer
    ```

* 要通过 IAP 访问 Kubeflow UI，用户需要
  [IAP-secured Web App User](https://cloud.google.com/iap/docs/managing-access)
  角色:

    ```
    gcloud projects add-iam-policy-binding [PROJECT] --member=user:[EMAIL] --role=roles/iap.httpsResourceAccessor
    ```

    **注意：** 你需要分配 `IAP-secured Web App User` 角色
    给到用户，即使该用户已经是项目的拥有者或者编辑人员。 `IAP-secured Web App User` 角色不只是 `Project Owner` 或 `Project Editor` 角色。

* 为了能够运行 `gcloud get credentials` 并在 Cloud Logging
  (formerly Stackdriver) 查看日志，用户需要项目的 viewer access 权限：

    ```
    gcloud projects add-iam-policy-binding [PROJECT] --member=user:[EMAIL] --role=roles/viewer
    ```

### 手动创建配置文件

管理员可以手动为用户创建配置文件，如下所述。

在本地机器上创建一个包含以下内容的 `profile.yaml` 文件：

```
apiVersion: kubeflow.org/v1beta1
kind: Profile
metadata:
  name: profileName   # 替换为所需配置文件的名称，这将是用户的命名空间名称
spec:
  owner:
    kind: User
    name: userid@email.com   # 替换为用户的电子邮件

  resourceQuotaSpec:    # 可以选择设置资源配额
   hard:
     cpu: "2"
     memory: 2Gi
     requests.nvidia.com/gpu: "1"
     persistentvolumeclaims: "1"
     requests.storage: "5Gi"
```
运行以下命令创建相应的配置文件资源：

```
kubectl create -f profile.yaml

kubectl apply -f profile.yaml  #if you are modifying the profile
```

上面的命令创建了一个名为 *profileName* 的属性文件。配置文件所有者是
*userid@email.com* 并且具有查看和修改该配置文件的访问权限。
以下资源是作为配置文件创建的一部分创建的：

  - 与相应配置文件同名的 Kubernetes 命名空间。
  - Kubernetes RBAC ([Role-based access control](https://kubernetes.io/docs/reference/access-authn-authz/rbac/))
    角色绑定角色绑定命名空间： *Admin*。这使配置文件所有者成为命名空间管理员，
    从而使他们能够使用 kubectl（通过 Kubernetes API）访问命名空间。
  - Istio 命名空间范围内的  AuthorizationPolicy: *user-userid-email-com-clusterrole-edit*.
    这允许 `user` 访问属于创建 AuthorizationPolicy 的命名空间的数据
  - 命名空间范围内的服务帐户 *default-editor* 和 *default-viewer* 
    供命名空间中用户创建的 pod 使用。
  - 将设置命名空间范围内的资源配额限制。

**注意：**: 由于配置文件与 Kubernetes 命名空间是一一对应的，术语 *profile* 和 *namespace* 有时在文档中可以互换使用。

### 批量创建用户配置文件

管理员可能希望为多个用户批量创建配置文件。您可以通过本地计算机上创建一个包含多个配置文件描述部分的 `profile.yaml` 来执行此操作
如下所示：

```
apiVersion: kubeflow.org/v1beta1
kind: Profile
metadata:
  name: profileName1   # 替换为所需配置文件的名称
spec:
  owner:
    kind: User
    name: userid1@email.com   # 替换为用户的电子邮件
---
apiVersion: kubeflow.org/v1beta1
kind: Profile
metadata:
  name: profileName2   # 替换为所需配置文件的名称
spec:
  owner:
    kind: User
    name: userid2@email.com   # 替换为用户的电子邮件
```

运行以下命令将命名空间应用到 Kubernetes 集群：
```
kubectl create -f profile.yaml

kubectl apply -f profile.yaml  #if you are modifying the profiles
```

这将创建多个配置文件，每个配置文件列于
 `profile.yaml` 文件。

### 自动配置文件创建

Kubeflow {{% kf-latest-version %}} 提供自动配置文件创建：

  - 默认情况下，自动配置文件创建未激活，需要明确包含在部署中。在部署期间打开自动用户配置文件创建后，会在首次登录时为经过身份验证的用户创建一个新的用户配置文件。用户将能够在 Kubeflow 看板中心的下拉列表中看到他们的新配置文件。
  - 在部署过程中启用自动配置文件通过设置 `CD_REGISTRATION_FLOW` 环境变量为 `true`。修改 `<manifests-path>/apps/centraldashboard/upstream/base/params.env` 注册变量为 `true`

   ```
   CD_CLUSTER_DOMAIN=cluster.local
   CD_USERID_HEADER=kubeflow-userid
   CD_USERID_PREFIX=
   CD_REGISTRATION_FLOW=true
   ```

  - 当经过身份验证的用户首次登录系统并访问中央仪表板时，他们会自动触发配置文件创建。
      - 一条简短的消息介绍了配置文件：<img
        src="/docs/images/auto-profile1.png" alt="Automatic profile creation
        step 1" class="mt-3 mb-3 border border-info rounded">
      - 用户可以命名他们的个人资料并单击 *Finish*:  <img
        src="/docs/images/auto-profile2.png" alt="Automatic profile creation
        step 2" class="mt-3 mb-3 border border-info rounded">
      - 这会将用户重定向到仪表板，他们可以在其中查看并在下拉列表中选择他们的个人资料。

## 列出和描述配置文件

管理员可以列出系统中的现有配置文件：
```
$ kubectl get profiles
```
并使用以下方式描述特定配置文件：
```
$ kubectl describe profile profileName
```

## 删除现有配置文件

管理员可以使用以下方法删除现有配置文件：
```
$ kubectl delete profile profileName
```

这将删除配置文件、相应的命名空间以及与配置文件关联的任何 Kubernetes 资源。
配置文件的所有者或有权访问配置文件的其他用户将不再有权访问配置文件，
并且不会在中央仪表板的下拉列表中看到它。


## 通过 Kubeflow UI 管理贡献者

Kubeflow {{% kf-latest-version %}} 允许与系统中的其他用户共享配置文件。配置文件的拥有者可以使用通过看板使用
**Manage Contributors** tab 页来共享他人对其个人属性文件的访问权限。

<img src="/docs/images/multi-user-contributors.png"
  alt="Manage Contributors in Profiles"
  class="mt-3 mb-3 border border-info rounded">

以下是管理贡献者选项卡视图的示例：

<img src="/docs/images/manage-contributors.png"
  alt="Manage Contributors in Profiles"
  class="mt-3 mb-3 border border-info rounded">

请注意，在上面的视图中，与配置文件关联的帐户是集群管理员（*Cluster Admin*），
该帐户用于部署 Kubeflow。该视图列出了用户可访问的配置文件以及与该配置文件关联的角色。

要添加或删除贡献者，请将用户的电子邮件地址或用户标识符
填入 **Contributors to your namespace** 选项。

<img src="/docs/images/add-contributors.png"
  alt="Add Contributors"
  class="mt-3 mb-3 border border-info rounded">

管理贡献者选项卡显示名称空间所有者添加的贡献者。
请注意，集群管理员可以查看系统中的所有配置文件及其贡献者。

<img src="/docs/images/view-contributors.png"
  alt="View Contributors"
  class="mt-3 mb-3 border border-info rounded">


贡献者可以访问命名空间中的所有 Kubernetes 资源，并且可以创建笔记本服务器以及访问现有的笔记本。

## 手动管理贡献者

管理员可以手动将参与者添加到现有配置文件，如下所述。

在本地机器上创建一个包含以下内容的 `rolebinding.yaml` 文件：

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  annotations:
    role: edit
    user: userid@email.com   # replace with the email of the user from your Active Directory case sensitive
  name: user-userid-email-com-clusterrole-edit
  # Ex: if the user email is lalith.vaka@kp.org the name should be user-lalith-vaka-kp-org-clusterrole-edit
  # Note: if the user email is Lalith.Vaka@kp.org from your Active Directory, the name should be user-lalith-vaka-kp-org-clusterrole-edit
  namespace: profileName # replace with the namespace/profile name that you are adding contributors to
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: kubeflow-edit
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: userid@email.com   # replace with the email of the user from your Active Directory case sensitive
```

在本地机器上创建一个包含以下内容的 authorizationpolicy.yaml 文件：

```
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  annotations:
    role: edit
    user: userid@email.com # replace with the email of the user from your Active Directory case sensitive
  name: user-userid-email-com-clusterrole-edit
  namespace: profileName # replace with the namespace/profile name that you are adding contributors to
spec:
  action: ALLOW
  rules:
  - when:
    - key: request.headers[kubeflow-userid] # for GCP, use x-goog-authenticated-user-email instead of kubeflow-userid for authentication purpose
      values:
      - accounts.google.com:userid@email.com   # replace with the email of the user from your Active Directory case sensitive
```

运行以下命令创建对应的贡献者资源：

```
kubectl create -f rolebinding.yaml
kubectl create -f authorizationpolicy.yaml
```

上面的命令将贡献者 *userid@email.com* 添加到名为 *profileName* 的配置文件中。贡献者
*userid@email.com* 具有查看和修改该个人资料的权限。
