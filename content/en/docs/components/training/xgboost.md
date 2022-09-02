+++
title = "XGBoost 训练 (XGBoostJob)"
description = "使用 XGBoostJob 训练 XGBoost 模型"
weight = 30
                    
+++

{{% stable-status %}}

本页描述 `XGBoostJob` 来训练 [XGBoost](https://github.com/dmlc/xgboost) 机器学习模型。

`XGBoostJob` 是一个 Kubernetes
[自定义资源](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)
用于在 Kubernetes 上运行 XGBoost 训练工作。 Kubeflow 的
`XGBoostJob` 实现是在 [`training-operator`](https://github.com/kubeflow/training-operator)。

## 安装 XGBoost Operator

如果已经完成，请跟随 [开始指引](/docs/started/getting-started/) 来部署 Kubeflow。

> 默认的，XGBoost Operator 会作为训练 operator 的控制器部署。

如果想要安装 Kubeflow 之外独立版本的训练 operator，
查看 [kubeflow/training-operator's README](https://github.com/kubeflow/training-operator#installation).

## 验证 Kubeflow 部署包含 XGBoost 支持

检查 XGboost 自定义资源已经安装

```
kubectl get crd
```

输出应包含 `xgboostjobs.kubeflow.org`

```
NAME                                           AGE
...
xgboostjobs.kubeflow.org                       4d
...
```

检测训练 operator 通过如下运行：

```
kubectl get pods -n kubeflow
```

输出结果应包含 `training-operator-xxx` 如下：

```
NAME                                READY   STATUS    RESTARTS   AGE
training-operator-d466b46bc-xbqvs   1/1     Running   0          4m37s
```

## 创建一个 XGBoost 训练工作

你可以通过定义一个 `XGboostJob` 配置文件创建一个训练任务。参考
[IRIS 示例](https://github.com/kubeflow/training-operator/blob/master/examples/xgboost/xgboostjob.yaml)清单。
你可以在此基础上按照你的需求修改配置文件。比如：添加 `CleanPodPolicy`
节点 `None` 来为在 job 终止时保持 pod。

```
cat xgboostjob.yaml
```

部署 `XGBoostJob` 资源开启训练：

```
kubectl create -f xgboostjob.yaml
```

现在你可以看到已创建的指定副本数量的 pod。

```
kubectl get pods -l job-name=xgboost-dist-iris-test-train
```

训练在 cpu 集群上花费 5-10 分钟。可以检查日志以查看其训练进度。

```
PODNAME=$(kubectl get pods -l job-name=xgboost-dist-iris-test-train,replica-type=master,replica-index=0 -o name)
kubectl logs -f ${PODNAME}
```

## 监控 XGBoostJob

```
kubectl get -o yaml xgboostjobs xgboost-dist-iris-test-train
```

查看 status 节点来监控 job 状态。 以下时一个成功完成 job 的输出示例。

```yaml
apiVersion: kubeflow.org/v1
kind: XGBoostJob
metadata:
  creationTimestamp: "2021-09-06T18:34:06Z"
  generation: 1
  name: xgboost-dist-iris-test-train
  namespace: default
  resourceVersion: "5844304"
  selfLink: /apis/kubeflow.org/v1/namespaces/default/xgboostjobs/xgboost-dist-iris-test-train
  uid: a1ea6675-3cb5-482b-95dd-68b2c99b8adc
spec:
  runPolicy:
    cleanPodPolicy: None
  xgbReplicaSpecs:
    Master:
      replicas: 1
      restartPolicy: Never
      template:
        spec:
          containers:
            - args:
                - --job_type=Train
                - --xgboost_parameter=objective:multi:softprob,num_class:3
                - --n_estimators=10
                - --learning_rate=0.1
                - --model_path=/tmp/xgboost-model
                - --model_storage_type=local
              image: docker.io/merlintang/xgboost-dist-iris:1.1
              imagePullPolicy: Always
              name: xgboost
              ports:
                - containerPort: 9991
                  name: xgboostjob-port
                  protocol: TCP
    Worker:
      replicas: 2
      restartPolicy: ExitCode
      template:
        spec:
          containers:
            - args:
                - --job_type=Train
                - --xgboost_parameter="objective:multi:softprob,num_class:3"
                - --n_estimators=10
                - --learning_rate=0.1
              image: docker.io/merlintang/xgboost-dist-iris:1.1
              imagePullPolicy: Always
              name: xgboost
              ports:
                - containerPort: 9991
                  name: xgboostjob-port
                  protocol: TCP
status:
  completionTime: "2021-09-06T18:34:23Z"
  conditions:
    - lastTransitionTime: "2021-09-06T18:34:06Z"
      lastUpdateTime: "2021-09-06T18:34:06Z"
      message: xgboostJob xgboost-dist-iris-test-train is created.
      reason: XGBoostJobCreated
      status: "True"
      type: Created
    - lastTransitionTime: "2021-09-06T18:34:06Z"
      lastUpdateTime: "2021-09-06T18:34:06Z"
      message: XGBoostJob xgboost-dist-iris-test-train is running.
      reason: XGBoostJobRunning
      status: "False"
      type: Running
    - lastTransitionTime: "2021-09-06T18:34:23Z"
      lastUpdateTime: "2021-09-06T18:34:23Z"
      message: XGBoostJob xgboost-dist-iris-test-train is successfully completed.
      reason: XGBoostJobSucceeded
      status: "True"
      type: Succeeded
  replicaStatuses:
    Master:
      succeeded: 1
    Worker:
      succeeded: 2
```
