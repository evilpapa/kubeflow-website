+++
title = "快速指引"
description = "Kubeflow 笔记本入门"
weight = 10
                    
+++

## 概括

1. 按照 [入门 - 安装 Kubeflow](/docs/started/installing-kubeflow/) 进行安装。
2. 在浏览器打开 Kubeflow [看板中心](/docs/components/central-dash/)。
3. 在左侧面板点击 __"Notebooks"__ 
4. 点击 __"New Server"__ 创建一个新的笔记本服务器。
5. 指定笔记本服务器的配置。
6. 配置笔记本后点击 __"CONNECT"__

## 详细步骤

1. 在浏览器打开 Kubeflow [看板中心](/docs/components/central-dash/)。

2. 选择一个空间：
    - 单击命名空间下拉菜单以查看可用命名空间的列表。
    - 选择与您的 Kubeflow 配置文件对应的命名空间。
      （有关配置文件的更多信息，请参阅[多用户隔离](/docs/components/multi-tenancy/)页面。）

   <img src="/docs/images/notebooks-namespace.png"
   alt="Selecting a Kubeflow namespace"
   class="mt-3 mb-3 border border-info rounded">

3. 在左侧面板点击 __"Notebooks"__

   <img src="/docs/images/jupyterlink.png"
   alt="Opening notebooks from the Kubeflow UI"
   class="mt-3 mb-3 border border-info rounded">

4. 点击 __"Notebook Servers"__ 页面的 __"New Server"__：

   <img src="/docs/images/add-notebook-server.png"
   alt="The Kubeflow notebook servers page"
   class="mt-3 mb-3 border border-info rounded">

5. 为笔记本服务输入 __"Name"__。
    - 名称可以包含字母和数字，但不能包含空格。
    - 比如，`my-first-notebook`。

   <img src="/docs/images/new-notebook-server.png"
   alt="Form for adding a Kubeflow notebook server"
   class="mt-3 mb-3 border border-info rounded">

6. 为您的笔记本服务器选择一个 Docker __"镜像"__ 
    - __自定义镜像__: 如果您选择自定义选项，则必须以 `registry/image:tag` 格式指定 Docker 镜像。
      (请参阅 [容器镜像](/docs/components/notebooks/container-images/)。)
    - __标准镜像__: 点击 __"Image"__ 下拉菜单以查看可用图像列表。
     （您可以从 Kubeflow 管理员配置的列表中选择）

7. S指定笔记本服务器将请求的 __“CPU”__ 数量。

8. 指定笔记本服务器将请求的 __“RAM”__ 数量。

9. 指定一个 __“工作空间卷”__ 作为 PVC 卷安装在您的主文件夹上。

10. *（可选）* 指定一个或多个 __"data volumes"__ 作为 PVC 卷挂载。

11. *（可选）* 指定一个或多个附加 __"configurations"__
    - 这些对应存在与你的配置属性相同空间的 [PodDefault 资源](https://github.com/kubeflow/kubeflow/blob/master/components/admission-webhook/README.md)。
    - Kubeflow 将 __"configurations"__ 字段中的标签与 PodDefault 清单中指定的属性进行匹配。
    - 比如，选择 __"configurations"__ 字段中 `add-gcp-secret` 标签来匹配包含如下配置的 PodDefault 清单：
    ```yaml
    apiVersion: kubeflow.org/v1alpha1
    kind: PodDefault
    metadata:
      name: add-gcp-secret
      namespace: MY_PROFILE_NAMESPACE
    spec:
     selector:
      matchLabels:
        add-gcp-secret: "true"
     desc: "add gcp credential"
     volumeMounts:
     - name: secret-volume
       mountPath: /secret/gcp
     volumes:
     - name: secret-volume
       secret:
        secretName: gcp-secret
    ```

12. *（可选）* 指定您的笔记本服务器要请求的任何 __"GPUs"__ 。
    - Kubeflow 在 Pod 资源请求中使用 "limits" 限制笔记本 Pod 使用资源
      （调度 GPU 资源可在 [Kubernetes 文档](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/)查看细节。）

13. *（可选）* 指定 __"开启内存分享"__ 设置。
    - PyTorch 等一些库使用共享内存进行批处理。
    - 目前，Kubernetes 中没有实现共享内存的激活。
    - 作为一种解决方法，Kubeflow 在 `/dev/shm` 挂载了一个空目录。

14. 点击 __"LAUNCH"__ 创建一个新的指定配置的 Notebook CRD。
    - 您应该会在 __“笔记本服务器”__ 页面上看到新笔记本服务器的条目
    - __“状态”__ 栏中应该有一个旋转指示器。
    - Kubernetes 可能需要几分钟来配置 notebook 服务器 pod。
    - 您可以通过将鼠标光标悬停在 __“状态”__ 列中的图标上来检查 Pod 的状态。

15. 点击 __"CONNECT"__ 以查看笔记本服务器公开的 Web 界面。

    <img src="/docs/images/notebook-servers.png"
    alt="Opening notebooks from the Kubeflow UI"
    class="mt-3 mb-3 border border-info rounded">

## 下一步

- 了解如何创建自己的 [容器镜像](/docs/components/notebooks/container-images/)。
- 查看使用 [jupyter 和 tensorflow](/docs/components/notebooks/jupyter-tensorflow-examples/)的示例。
- 访问 [故障排除指南](/docs/components/notebooks/troubleshooting) 以修复常见错误。
