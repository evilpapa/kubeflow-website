+++
title = "示例"
description = "演示使用 Kubeflow 进行机器学习的示例"
weight = 99
+++

{{% alert title="提醒" color="warning" %}}
[kubeflow/examples](https://github.com/kubeflow/examples) 存储库中的一些示例尚未使用较新版本的 Kubeflow 进行测试。请参阅您选择的示例的自述文件。
{{% /alert %}}


{{% blocks/sample-section title="MNIST 图像分类"
  kfctl="v1.0.0"
  url="https://github.com/kubeflow/examples/tree/master/mnist"
  api="https://api.github.com/repos/kubeflow/examples/commits?path=mnist&sha=master" %}}
使用 MNIST 数据集训练和提供图像分类模型。
本教程采用在您的 Kubeflow 集群中运行的 Jupyter notebook 的形式。
您可以选择在各种云上部署 Kubeflow 并训练模型，
包括 Amazon Web Services (AWS)、Google Cloud Platform (GCP)、IBM Cloud、Microsoft Azure 和本地。
使用 TensorFlow Serving 为模型提供服务。
{{% /blocks/sample-section %}}

{{% blocks/sample-section title="金融时间序列"
  kfctl="v0.7"
  url="https://github.com/kubeflow/examples/tree/master/financial_time_series"
  api="https://api.github.com/repos/kubeflow/examples/commits?path=financial_time_series&sha=master" %}}
在 Google Cloud Platform (GCP) 上使用 TensorFlow 训练和分析
用于金融时间序列的模型。
使用 Kubeflow Pipelines SDK 自动化工作流。
{{% /blocks/sample-section %}}

## 下一步

完成
[Kubeflow Pipelines 示例](/docs/components/pipelines/tutorials/build-pipeline/)。
