+++
title = "容器镜像"
description = "关于 Kubeflow Notebooks 的容器镜像"
weight = 30
                    
+++
Kubeflow Notebooks 原生支持三种类型的 notebook，[JupyterLab](https://github.com/jupyterlab/jupyterlab)、[RStudio](https://github.com/rstudio/rstudio)、[Visual Studio Code (code-server)](https://github.com/cdr/code-server)，任何基于 Web 的 IDE 都可以使用。
Notebook 服务器作为 Kubernetes Pod 容器运行，这意味着 IDE 的类型（以及安装的软件包）取决于您为服务器选择的 Docker 映像。

## 镜像

我们提供了很多 [容器镜像示例](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers) 来帮你入门。

### 基础镜像

这些镜像为 Kubeflow Notebook 容器提供了一个共同的起点。
查看 [自定义镜像](#custom-images) 以了解如何使用您自己的包来扩展它们。

Dockerfile | Registry | Notes
--- | --- | ---
[base](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/base) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/base:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/base) | 公共基础镜像
[codeserver](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/codeserver) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/codeserver:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/codeserver) | [code-server](https://github.com/cdr/code-server) (Visual Studio Code) 基础镜像
[jupyter](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter) | [JupyterLab](https://github.com/jupyterlab/jupyterlab) 基础镜像
[rstudio](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/rstudio) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/rstudio:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/rstudio) | [RStudio](https://github.com/rstudio/rstudio) 基础镜像

### 完整镜像

这些镜像扩展自 [基础镜像](#base-images) 并使用数据科学家和 ML 工程师使用的通用包。

Dockerfile | Registry | Notes
--- | --- | ---
[codeserver-python](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/codeserver-python) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/codeserver-python:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/codeserver-python) | code-server (Visual Studio Code) + Conda Python
[jupyter-pytorch (CPU)](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-pytorch) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-pytorch:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-pytorch) | JupyterLab + PyTorch (CPU)
[jupyter-pytorch (CUDA)](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-pytorch) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-pytorch-cuda:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-pytorch-cuda) | JupyterLab + PyTorch (CUDA)
[jupyter-pytorch-full (CPU)](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-pytorch-full) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-pytorch-full:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-pytorch-full) | JupyterLab + PyTorch (CPU) + [common](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-pytorch-full/requirements.txt) packages
[jupyter-pytorch-full (CUDA)](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-pytorch-full) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-pytorch-cuda-full:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-pytorch-cuda-full) | JupyterLab + PyTorch (CUDA) + [common](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-pytorch-full/requirements.txt) packages
[jupyter-scipy](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-scipy) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-scipy:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-scipy) | JupyterLab + [SciPy](https://www.scipy.org/) packages
[jupyter-tensorflow (CPU)](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-tensorflow) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-tensorflow:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-tensorflow) | JupyterLab + TensorFlow (CPU)
[jupyter-tensorflow (CUDA)](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-tensorflow) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-tensorflow-cuda:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-tensorflow-cuda) | JupyterLab + TensorFlow (CUDA)
[jupyter-tensorflow-full (CPU)](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-tensorflow-full) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-tensorflow-full:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-tensorflow-full) | JupyterLab + TensorFlow (CPU) + [common](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-tensorflow-full/requirements.txt) packages
[jupyter-tensorflow-full (CUDA)](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-tensorflow-full) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-tensorflow-cuda-full:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/jupyter-tensorflow-cuda-full) | JupyterLab + TensorFlow (CUDA) + [common](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/jupyter-tensorflow-full/requirements.txt) packages
[rstudio-tidyverse](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers/rstudio-tidyverse) | [`public.ecr.aws/j1r0q0g6/notebooks/notebook-servers/rstudio-tidyverse:{TAG}`](https://gallery.ecr.aws/j1r0q0g6/notebooks/notebook-servers/rstudio-tidyverse) | RStudio + [Tidyverse](https://www.tidyverse.org/) packages

### 镜像依赖图

此流程图显示了我们的笔记本容器映像如何相互依赖。

<img src="/docs/images/notebook-container-image-chart.png" 
     alt="A flow-chart showing how notebook container images depend on each other"  
     class="mt-3 mb-3 border border-info rounded">

## 自定义镜像

用户在生成 Kubeflow Notebook 后 __生产__ 的包只会持续 pod 的生命周期（除非安装到 PVC 支持的目录中）。

为确保在 Pod 重新启动期间保留包，用户需要：
1. [构建包含它们的自定义镜像](https://github.com/kubeflow/kubeflow/tree/master/components/example-notebook-servers#custom-images)，或者
2. 确保它们安装在 PVC 支持的目录中

### 镜像要求

要让 Kubeflow Notebooks 使用容器镜像，该镜像必须：
- 在端口上公开一个 HTTP 接口 `8888`:
  - kubeflow 在运行时设置一个 `NB_PREFIX` 环境变量来让容器能够监听该 URL 路径
  - kubeflow 使用 IFrames，所以确保应用在 HTTP 响应头设置 `Access-Control-Allow-Origin: *`
- 以 `jovyan` 用户运行：
  - `jovyan` 主目录应为 `/home/jovyan`
  - `jovyan` UID 应为 `1000`
- 能成功启动挂载在 `/home/jovyan` 的空 PVC：
  - kubeflow 将 PVC 挂载在 `/home/jovyan` 来是 Pod 重启之后能够保持状态
  
## 下一步

- 通过在生成笔记本服务器时指定容器映像来使用它。
  (查看 [快速指南](/docs/components/notebooks/quickstart-guide/)。)