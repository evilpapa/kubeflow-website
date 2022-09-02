+++
title = "Katib 简介"
description = "用于超参数调整和神经架构搜索的 Katib 概述"
weight = 10
                    
+++

{{% beta-status
  feedbacklink="https://github.com/kubeflow/katib/issues" %}}

本指南介绍了超参数调优、神经
架构搜索以及作为 Kubeflow 组件的 Katib 系统的概念。

Katib 是一个用于自动化机器学习 (AutoML) 的 Kubernetes 原生项目。
Katib 支持超参数调整、提前停止和
神经架构搜索 (NAS)。
在 [fast.ai](https://www.fast.ai/2018/07/16/auto-ml2/)、
[Google Cloud](https://cloud.google.com/automl)、
[Microsoft Azure](https://docs.microsoft.com/en-us/azure/machine-learning/concept-automated-ml#automl-in-azure-machine-learning) 或
[Amazon SageMaker](https://aws.amazon.com/blogs/aws/amazon-sagemaker-autopilot-fully-managed-automatic-machine-learning/) 上
了解更多有关 AutoML 的内容。

Katib 是一个与机器学习 (ML) 框架无关的项目。
它可以调整以用户选择的任何语言编写的应用程序的超参数，
并原生支持许多 ML 框架，
例如 TensorFlow、MXNet、PyTorch、XGBoost 等。

Katib 支持多种 AutoML 算法，例如
[贝叶斯优化](https://arxiv.org/pdf/1012.2599.pdf)、
[Tree of Parzen Estimators](https://papers.nips.cc/paper/2011/file/86e8f7ab32cfd12577bc2619bc635690-Paper.pdf)、
[Random Search](https://en.wikipedia.org/wiki/Hyperparameter_optimization#Random_search)、
[Covariance Matrix Adaptation Evolution Strategy](https://en.wikipedia.org/wiki/CMA-ES)、
[Hyperband](https://arxiv.org/pdf/1603.06560.pdf)、
[Efficient Neural Architecture Search](https://arxiv.org/abs/1802.03268)、
[Differentiable Architecture Search](https://arxiv.org/abs/1806.09055) 等。
其他算法支持即将推出。

[Katib 项目](https://github.com/kubeflow/katib) 是开源的。
[开发者指引](https://github.com/kubeflow/katib/blob/master/docs/developer-guide.md)
对于想要为项目做出贡献的 开发人员来说是一个很好的起点。

## 超参数和超参数调优

_Hyperparameters_ 控制模型训练过程的变量。
他们包括：

- 学习率。
- 神经网络中的层数。
- 每层的节点数。

Hyperparameter values are not _learned_. In other words, in contrast to the
node weights and other training _parameters_, the model training process does
not adjust the hyperparameter values.

_Hyperparameter tuning_ is the process of optimizing the hyperparameter values
to maximize the predictive accuracy of the model. If you don't use Katib or a
similar system for hyperparameter tuning, you need to run many training jobs
yourself, manually adjusting the hyperparameters to find the optimal values.

Automated hyperparameter tuning works by optimizing a target variable,
also called the _objective metric_, that you specify in the configuration for
the hyperparameter tuning job. A common metric is the model's accuracy
in the validation pass of the training job (_validation-accuracy_). You also
specify whether you want the hyperparameter tuning job to _maximize_ or
_minimize_ the metric.

For example, the following graph from Katib shows the level of validation accuracy
for various combinations of hyperparameter values (the learning rate, the number of
layers, and the optimizer):

<img src="/docs/components/katib/images/random-example-graph.png"
  alt="Graph produced by the random example"
  class="mt-3 mb-3 border border-info rounded">

_(To run the example that produced this graph, follow the [getting-started
guide](/docs/components/katib/hyperparameter/).)_

Katib runs several training jobs (known as _trials_) within each
hyperparameter tuning job (_experiment_). Each trial tests a different set of
hyperparameter configurations. At the end of the experiment, Katib outputs
the optimized values for the hyperparameters.

You can improve your hyperparameter tunning experiments by using
[early stopping](https://en.wikipedia.org/wiki/Early_stopping) techniques.
Follow the [early stopping guide](/docs/components/katib/early-stopping/)
for the details.

## 神经网络搜索

{{% alert title="Alpha version" color="warning" %}}
NAS is currently in <b>alpha</b> with limited support. The Kubeflow team is
interested in any feedback you may have, in particular with regards to usability
of the feature. You can log issues and comments in
the [Katib issue tracker](https://github.com/kubeflow/katib/issues).
{{% /alert %}}

In addition to hyperparameter tuning, Katib offers a _neural architecture
search_ feature. You can use the NAS to design
your artificial neural network, with a goal of maximizing the predictive
accuracy and performance of your model.

NAS is closely related to hyperparameter tuning. Both are subsets of AutoML.
While hyperparameter tuning optimizes the model's hyperparameters, a NAS system
optimizes the model's structure, node weights and hyperparameters.

NAS technology in general uses various techniques to find the optimal neural
network design.

You can submit Katib jobs from the command line or from the UI. (Learn more
about the Katib interfaces later on this page.) The following screenshot shows
part of the form for submitting a NAS job from the Katib UI:

<img src="/docs/components/katib/images/nas-parameters.png"
  alt="Submitting a neural architecture search from the Katib UI"
  class="mt-3 mb-3 border border-info rounded">

## Katib 接口

You can use the following interfaces to interact with Katib:

- A web UI that you can use to submit experiments and to monitor your results.
  Check the [getting-started
  guide](/docs/components/katib/hyperparameter/#katib-ui)
  for information on how to access the UI.
  The Katib home page within Kubeflow looks like this:

  <img src="/docs/components/katib/images/home-page.png"
    alt="The Katib home page within the Kubeflow UI"
    class="mt-3 mb-3 border border-info rounded">

- A gRPC API. Check the [API reference on GitHub](https://github.com/kubeflow/katib/blob/master/pkg/apis/manager/v1beta1/gen-doc/api.md).

- Command-line interfaces (CLIs):

  - The Kubernetes CLI, **kubectl**, is useful for running commands against your
    Kubeflow cluster. Learn about kubectl in the [Kubernetes
    documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

- Katib Python SDK. Check the [Katib Python SDK documentation on GitHub](https://github.com/kubeflow/katib/tree/master/sdk/python/v1beta1).

## Katib concepts

This section describes the terms used in Katib.

### Experiment

An _experiment_ is a single tuning run, also called an optimization run.

You specify configuration settings to define the experiment. The following are
the main configurations:

- **Objective**: What you want to optimize. This is the objective metric, also
  called the target variable. A common metric is the model's accuracy
  in the validation pass of the training job (_validation-accuracy_). You also
  specify whether you want the hyperparameter tuning job to _maximize_ or
  _minimize_ the metric.

- **Search space**: The set of all possible hyperparameter values that the
  hyperparameter tuning job should consider for optimization, and the
  constraints for each hyperparameter. Other names for search space include
  _feasible set_ and _solution space_. For example, you may provide the
  names of the hyperparameters that you want to optimize. For each
  hyperparameter, you may provide a _minimum_ and _maximum_ value or a _list_
  of allowable values.

- **Search algorithm**: The algorithm to use when searching for the optimal
  hyperparameter values.

Katib experiment is defined as a
[Kubernetes CRD](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) .

For details of how to define your experiment, follow the guide to [running an
experiment](/docs/components/katib/experiment/).

### Suggestion

A _suggestion_ is a set of hyperparameter values that the hyperparameter
tuning process has proposed. Katib creates a trial to evaluate the suggested
set of values.

Katib suggestion is defined as a
[Kubernetes CRD](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) .

### Trial

A _trial_ is one iteration of the hyperparameter tuning process. A trial
corresponds to one worker job instance with a list of parameter assignments.
The list of parameter assignments corresponds to a suggestion.

Each experiment runs several trials. The experiment runs the trials until it
reaches either the objective or the configured maximum number of trials.

Katib trial is defined as a
[Kubernetes CRD](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) .

### Worker 工作

_worker job_ 是运行以评估试验并计算其
目标值的过程。

The worker job can be any type of Kubernetes resource or
[Kubernetes CRD](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/).
Follow the
[trial template guide](/docs/components/katib/trial-template/#custom-resource)
to check how to support your own Kubernetes resource in Katib.

Katib 在上游有这些 CRD 示例：

- [Kubernetes `Job`](https://kubernetes.io/docs/concepts/workloads/controllers/job/)

- [Kubeflow `TFJob`](/docs/components/training/tftraining/)

- [Kubeflow `PyTorchJob`](/docs/components/training/pytorch/)

- [Kubeflow `MXJob`](/docs/components/training/mxnet)

- [Kubeflow `XGBoostJob`](/docs/components/training/xgboost)

- [Kubeflow `MPIJob`](/docs/components/training/mpi)

- [Tekton `Pipelines`](https://github.com/kubeflow/katib/tree/master/examples/v1beta1/tekton)

- [Argo `Workflows`](https://github.com/kubeflow/katib/tree/master/examples/v1beta1/argo)

通过提供上述工作类型，Katib 支持多种 ML 框架

## 下一步

查看[入门指引](/docs/components/katib/hyperparameter/)
来设置 Katib 及运行一些超参转换示例。
