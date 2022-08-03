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

10. *(可选)* 指定一个或多个 __"data volumes"__ 作为 PVC 卷挂载。

11. *(可选)* 指定一个或多个附加 __"configurations"__
    - These correspond to [PodDefault resources](https://github.com/kubeflow/kubeflow/blob/master/components/admission-webhook/README.md) which exit in your profile namespace.
    - Kubeflow matches the labels in the __"configurations"__ field against the properties specified in the PodDefault manifest.
    - For example, select the label `add-gcp-secret` in the __"configurations"__ field to match to a PodDefault manifest containing the following configuration:
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

12. *(Optional)* Specify any __"GPUs"__ that your notebook server will request.
    - Kubeflow uses "limits" in Pod requests to provision GPUs onto the notebook Pods
      (Details about scheduling GPUs can be found in the [Kubernetes Documentation](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).)

13. *(Optional)* Specify the setting for __"enable shared memory"__.
    - Some libraries like PyTorch use shared memory for multiprocessing.
    - Currently, there is no implementation in Kubernetes to activate shared memory.
    - As a workaround, Kubeflow mounts an empty directory volume at `/dev/shm`.

14. Click __"LAUNCH"__ to create a new Notebook CRD with your specified settings.
    - You should see an entry for your new notebook server on the __"Notebook Servers"__ page
    - There should be a spinning indicator in the __"Status"__ column.
    - It can take a few minutes for kubernetes to provision the notebook server pod.
    - You can check the status of your Pod by hovering your mouse cursor over the icon in the __"Status"__ column.

15. Click __"CONNECT"__ to view the web interface exposed by your notebook server.

    <img src="/docs/images/notebook-servers.png"
    alt="Opening notebooks from the Kubeflow UI"
    class="mt-3 mb-3 border border-info rounded">

## Next steps

- Learn how to create your own [container images](/docs/components/notebooks/container-images/).
- Review examples of using [jupyter and tensorflow](/docs/components/notebooks/jupyter-tensorflow-examples/).
- Visit the [troubleshooting guide](/docs/components/notebooks/troubleshooting) to fix common errors.
