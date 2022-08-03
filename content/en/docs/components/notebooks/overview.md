+++
title = "概述"
description = "Kubeflow 笔记本概述"
weight = 5
                    
+++
{{% stable-status %}}

## 什么是 Kubeflow 笔记本？

Kubeflow Notebooks 提供了一种在 Kubernetes 集群中运行基于 Web 的开发环境的方法，方法是在 Pod 中运行它们。

一些关键功能包括：
- 对 [JupyterLab](https://github.com/jupyterlab/jupyterlab)、[RStudio](https://github.com/jupyterlab/jupyterlab)、[Visual Studio Code (code-server)](https://github.com/cdr/code-server) 的原生支持。
- 用户可以直接在集群中创建笔记本容器，而不是在他们的工作站本地创建。
- 管理员可以为其组织提供标准笔记本图像，并预先安装所需的软件包。
- 访问控制由 Kubeflow 的 RBAC 管理，可以更轻松地在整个组织内共享笔记本。

## 下一步

- 使用[快速入门指南](/docs/components/notebooks/quickstart-guide/)开始使用 Kubeflow 笔记本。
- 了解如何创建自己的 [容器镜像](/docs/components/notebooks/container-images/)。
