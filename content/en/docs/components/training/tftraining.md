+++
title = "TensorFlow 训练 (TFJob)"
description = "使用 TFJob 训练 Tensorflow 模型"
weight = 10
                    
+++

{{% stable-status %}}

本页描述使用 `TFJob` 训练 [TensorFlow](https://www.tensorflow.org/) 机器学习模型。

## 什么是 TFJob？

`TFJob` 是一个 Kubernetes [自定义资源](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) 用于在 Kubernetes 运行 TensorFlow 训练任务。Kubeflow 关于 `TFJob` 的实现在 [`training-operator`](https://github.com/kubeflow/training-operator)。

**注意**: `TFJob` 由于 Istio 的 [自动边车注入](https://istio.io/v1.3/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection)原因而不能运行在默认的命名空间。为了让 `TFJob` 能跑起来，需要为 `TFJob` pod 添加 `sidecar.istio.io/inject: "false"` 禁用注解。

`TFJob` 资源的 YAML 表示类似如下（编辑并使用你自己的镜像及命令来实现你的训练代码）：

```yaml
apiVersion: kubeflow.org/v1
kind: TFJob
metadata:
  generateName: tfjob
  namespace: your-user-namespace
spec:
  tfReplicaSpecs:
    PS:
      replicas: 1
      restartPolicy: OnFailure
      template:
        metadata:
          annotations:
            sidecar.istio.io/inject: "false"
        spec:
          containers:
            - name: tensorflow
              image: gcr.io/your-project/your-image
              command:
                - python
                - -m
                - trainer.task
                - --batch_size=32
                - --training_steps=1000
    Worker:
      replicas: 3
      restartPolicy: OnFailure
      template:
        metadata:
          annotations:
            sidecar.istio.io/inject: "false"
        spec:
          containers:
            - name: tensorflow
              image: gcr.io/your-project/your-image
              command:
                - python
                - -m
                - trainer.task
                - --batch_size=32
                - --training_steps=1000
```

若要给 `TFJob` 赋予接入密钥的能力，比如通过 GKE 安装的 Kubeflow 自动接入 GCP 证书，可以如以下示例挂在和使用密钥：

```yaml
apiVersion: kubeflow.org/v1
kind: TFJob
metadata:
  generateName: tfjob
  namespace: your-user-namespace
spec:
  tfReplicaSpecs:
    PS:
      replicas: 1
      restartPolicy: OnFailure
      template:
        metadata:
          annotations:
            sidecar.istio.io/inject: "false"
        spec:
          containers:
            - name: tensorflow
              image: gcr.io/your-project/your-image
              command:
                - python
                - -m
                - trainer.task
                - --batch_size=32
                - --training_steps=1000
              env:
                - name: GOOGLE_APPLICATION_CREDENTIALS
                  value: "/etc/secrets/user-gcp-sa.json"
              volumeMounts:
                - name: sa
                  mountPath: "/etc/secrets"
                  readOnly: true
          volumes:
            - name: sa
              secret:
                secretName: user-gcp-sa
    Worker:
      replicas: 1
      restartPolicy: OnFailure
      template:
        metadata:
          annotations:
            sidecar.istio.io/inject: "false"
        spec:
          containers:
            - name: tensorflow
              image: gcr.io/your-project/your-image
              command:
                - python
                - -m
                - trainer.task
                - --batch_size=32
                - --training_steps=1000
              env:
                - name: GOOGLE_APPLICATION_CREDENTIALS
                  value: "/etc/secrets/user-gcp-sa.json"
              volumeMounts:
                - name: sa
                  mountPath: "/etc/secrets"
                  readOnly: true
          volumes:
            - name: sa
              secret:
                secretName: user-gcp-sa
```

如果你不熟悉 Kubernetes 资源，请移步 [Understanding Kubernetes Objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) 页面。

`TFJob` 与内置[控制器](https://kubernetes.io/docs/concepts/workloads/controllers/)的不同之处在于 `TFJob` 规范旨在管理[分布式 TensorFlow 训练作业](https://www.tensorflow.org/guide/distributed_training)。

一个分发的 TensorFlow 任务通常包含 0 或者多个以下处理器

- **Chief** chief 负责编排训练和执行任务，例如为模型设置检查点。
- **Ps** ps 是参数服务器； 这些服务器为模型参数提供分布式数据存储。
- **Worker** worker 进行模型训练的实际工作。 在某些情况下，worker 0 也可能担任 chief。
- **Evaluator** 评估器可用于在训练模型时计算评估指标。

`TFJob` 规范中的字段 **tfReplicaSpecs** 包含从副本类型（如上所列）到该副本的 **TFReplicaSpec** 的映射。 **TFReplicaSpec** 由 3 个字段组成

- **replicas** 为此 `TFJob` 生成的此类副本的数量。

- **template** [PodTemplateSpec](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.11/#podtemplatespec-v1-core) 描述要为每个副本创建的 pod。

  - **pod 必须包含名为 `tensorflow` 的容器**.

- **restartPolicy** 确定 pod 在退出时是否会重新启动。 允许的值如下

  - **Always** 意味着 pod 将始终重新启动。 此策略适用于参数服务器，因为它们永远不会退出，并且应始终在发生故障时重新启动。

  - **OnFailure** 表示如果 Pod 因故障退出，Pod 将重新启动。

    - 非零退出代码表示失败。
    - 退出代码 0 表示成功，Pod 不会重新启动。
    - 这个策略对 chief 和 worker 都有好处。

  - **ExitCode** 表示重启行为取决于`tensorflow`容器的退出代码，如下所示：

    - 退出代码 `0` 表示该过程已成功完成并且不会重新启动。
    - 以下退出代码表示永久错误，容器不会重新启动：
      - `1`: 一般错误
      - `2`: 滥用 shell 内置函数
      - `126`: 调用的命令无法执行
      - `127`: 找不到相关命令
      - `128`: 退出参数无效
      - `139`:  由 SIGSEGV 终止的容器（无效的内存引用）
    - 以下退出代码表示可重试错误，容器将重新启动：
      - `130`: 由 SIGINT 终止的容器（键盘 Control-C）
      - `137`: 容器收到 SIGKILL
      - `143`: 容器收到一个 SIGTERM
    - 退出代码`138`对应于 SIGUSR1，并为用户指定的可重试错误保留。
    - 其他退出代码未定义，并且无法保证行为。

    有关退出代码的背景信息，请参阅[终止信号的 GNU 指南](https://www.gnu.org/software/libc/manual/html_node/Termination-Signals.html) 和[Linux 文档项目](http://tldp.org/LDP/abs/html/exitcodes.html)。

  - **Never** 意味着终止的 pod 永远不会重新启动。应该很少使用此策略，因为 Kubernetes 会因多种原因终止 Pod（例如节点变得不健康），这将阻止作业恢复。

## 安装 TensorFlow Operator

如果您还没有这样做，请按照[入门指南](https://www.kubeflow.org/docs/started/getting-started/)部署 Kubeflow。

> 默认情况下，`TFJob`Operator 将被部署为训练 Operator 中的控制器。

如果您想在没有 Kubeflow 的情况下安装独立版本的训练 Operator，请参阅[kubeflow/training-operator 的自述文件](https://github.com/kubeflow/training-operator#installation)。

### 验证您的 Kubeflow 部署中是否包含 TFJob 支持

检查是否安装了 TensorFlow 自定义资源：

```fallback
kubectl get crd
```

输出应包括以下 `tfjobs.kubeflow.org` 内容：

```fallback
NAME                                             CREATED AT
...
tfjobs.kubeflow.org                         2021-09-06T18:33:58Z
...
```

通过以下方式检查 Training Operator 是否正在运行：

```fallback
kubectl get pods -n kubeflow
```

输出应包括以下 `training-operaror-xxx` ：

```fallback
NAME                                READY   STATUS    RESTARTS   AGE
training-operator-d466b46bc-xbqvs   1/1     Running   0          4m37s
```

## 运行 Mnist 示例

请参阅[分布式 MNIST 示例](https://github.com/kubeflow/training-operator/blob/master/examples/tensorflow/simple.yaml)清单。您可以根据您的要求更改配置文件。

部署`TFJob`资源开始训练：

```fallback
kubectl create -f https://raw.githubusercontent.com/kubeflow/training-operator/master/examples/tensorflow/simple.yaml
```

监控作业（请参阅[下面的详细指南](https://www.kubeflow.org/docs/components/training/tftraining/#monitoring-your-job)）：

```fallback
kubectl -n kubeflow get tfjob tfjob-simple -o yaml
```

删除它

```fallback
kubectl -n kubeflow delete tfjob tfjob-simple
```

## 自定义 TFJob

通常，您可以在`TFJob` yaml 文件中更改以下值：

1. 将镜像更改为指向包含您的代码的 docker 镜像
2. 更改副本的数量和类型
3. 更改分配给每个资源的资源（请求和限制值）
4. 设置任何环境变量
  - 例如，您可能需要配置各种环境变量来与 GCS 或 S3 等数据存储桶通信
5. 如果要使用 PV 进行存储，请附加 PV。

## 使用 GPUs

要使用 GPU，您的集群必须配置为可以调度 GPU。

- 节点必须连接 GPU。
- Kubernetes 集群必须识别 `nvidia.com/gpu` 资源类型。
- 必须在集群上安装 GPU 驱动程序。
- 了解更多信息：
  - [用于调度 GPU 的 Kubernetes 说明](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/)
  - [GKE 说明](https://cloud.google.com/kubernetes-engine/docs/concepts/gpus)
  - [EKS 指令](https://docs.aws.amazon.com/eks/latest/userguide/gpu-ami.html)

要附加 GPU，请在应包含 GPU 的副本中指定容器上的 GPU 资源；例如。

```yaml
apiVersion: "kubeflow.org/v1"
kind: "TFJob"
metadata:
  name: "tf-smoke-gpu"
spec:
  tfReplicaSpecs:
    PS:
      replicas: 1
      template:
        metadata:
          creationTimestamp: null
        spec:
          containers:
            - args:
                - python
                - tf_cnn_benchmarks.py
                - --batch_size=32
                - --model=resnet50
                - --variable_update=parameter_server
                - --flush_stdout=true
                - --num_gpus=1
                - --local_parameter_device=cpu
                - --device=cpu
                - --data_format=NHWC
              image: gcr.io/kubeflow/tf-benchmarks-cpu:v20171202-bdab599-dirty-284af3
              name: tensorflow
              ports:
                - containerPort: 2222
                  name: tfjob-port
              resources:
                limits:
                  cpu: "1"
              workingDir: /opt/tf-benchmarks/scripts/tf_cnn_benchmarks
          restartPolicy: OnFailure
    Worker:
      replicas: 1
      template:
        metadata:
          creationTimestamp: null
        spec:
          containers:
            - args:
                - python
                - tf_cnn_benchmarks.py
                - --batch_size=32
                - --model=resnet50
                - --variable_update=parameter_server
                - --flush_stdout=true
                - --num_gpus=1
                - --local_parameter_device=cpu
                - --device=gpu
                - --data_format=NHWC
              image: gcr.io/kubeflow/tf-benchmarks-gpu:v20171202-bdab599-dirty-284af3
              name: tensorflow
              ports:
                - containerPort: 2222
                  name: tfjob-port
              resources:
                limits:
                  nvidia.com/gpu: 1
              workingDir: /opt/tf-benchmarks/scripts/tf_cnn_benchmarks
          restartPolicy: OnFailure
```

按照 TensorFlow 的[说明](https://www.tensorflow.org/guide/gpu) 使用 GPU。

## 监控你的工作

获取您的任务状态

```bash
kubectl -n kubeflow get -o yaml tfjobs tfjob-simple
```

这是示例作业的示例输出

```yaml
apiVersion: kubeflow.org/v1
kind: TFJob
metadata:
  creationTimestamp: "2021-09-06T11:48:09Z"
  generation: 1
  name: tfjob-simple
  namespace: kubeflow
  resourceVersion: "5764004"
  selfLink: /apis/kubeflow.org/v1/namespaces/kubeflow/tfjobs/tfjob-simple
  uid: 3a67a9a9-cb89-4c1f-a189-f49f0b581e29
spec:
  tfReplicaSpecs:
    Worker:
      replicas: 2
      restartPolicy: OnFailure
      template:
        spec:
          containers:
            - command:
                - python
                - /var/tf_mnist/mnist_with_summaries.py
              image: gcr.io/kubeflow-ci/tf-mnist-with-summaries:1.0
              name: tensorflow
status:
  completionTime: "2021-09-06T11:49:30Z"
  conditions:
    - lastTransitionTime: "2021-09-06T11:48:09Z"
      lastUpdateTime: "2021-09-06T11:48:09Z"
      message: TFJob tfjob-simple is created.
      reason: TFJobCreated
      status: "True"
      type: Created
    - lastTransitionTime: "2021-09-06T11:48:12Z"
      lastUpdateTime: "2021-09-06T11:48:12Z"
      message: TFJob kubeflow/tfjob-simple is running.
      reason: TFJobRunning
      status: "False"
      type: Running
    - lastTransitionTime: "2021-09-06T11:49:30Z"
      lastUpdateTime: "2021-09-06T11:49:30Z"
      message: TFJob kubeflow/tfjob-simple successfully completed.
      reason: TFJobSucceeded
      status: "True"
      type: Succeeded
  replicaStatuses:
    Worker:
      succeeded: 2
  startTime: "2021-09-06T11:48:10Z"
```

### 条件

A`TFJob`有一个`TFJobStatus`，它有一个数组，`TFJobConditions`其中`TFJob`有或没有通过。数组的每个元素`TFJobCondition`都有六个可能的字段：

- **lastUpdateTime **字段提供上次更新此条件的时间。
- **lastTransitionTime** 字段提供最后一次从一种状态转换到另一种状态的时间。
- **message** 字段是人类可读的消息，指示有关转换的详细信息。
- **reason** 字段是条件最后一次转换的唯一、单字、驼峰式格式原因。
- **status** 字段是一个字符串，可能值为“True”、“False”和“Unknown”。
- **type** 字段是具有以下可能值的字符串 ：
  - **TFJobCreated** 表示`TFJob`已被系统接受，但其中一个或多个 pod/service 尚未启动。
  - **TFJobRunning** 表示这个的所有子资源（例如services/pods）`TFJob` 已经被成功调度和启动，并且job正在运行。
  - **TFJobRestarting** 表示其中的一个或多个子资源（例如服务/pod）`TFJob`出现问题并正在重新启动。
  - **TFJobSucceeded** 表示作业成功完成。
  - **TFJobFailed** 表示作业失败。

一个任务的成败决定如下

- 如果一个任务有 **chief**，成败取决于 chief 状态值。

- 如果一个任务没有 chief，成败取决于 workers。

- 如果都有，`TFJob` 如果被监视的进程以退出代码 0 退出，则成功。

- 在非零退出代码的情况下，行为由副本的 restartPolicy 确定。

- 如果 restartPolicy 允许重新启动，则该进程将重新启动并`TFJob`继续执行。

  - 对于 restartPolicy ExitCode，行为取决于退出代码。
- 如果 restartPolicy 不允许重新启动，则非零退出代码被视为永久失败，并且作业被标记为失败。

### tfReplicaStatuses

tfReplicaStatuses 提供了一个映射，指示每个副本在给定状态下的 pod 数量。有三种可能的状态

- **Active** 是当前正在运行的 pod 的数量。
- **Succeeded **是成功完成的 pod 数。
- **Failed** 是完成但出现错误的 pod 数量。

### 活动事件

在执行期间，`TFJob`将发出事件以指示正在发生的事情，例如创建/删除 pod 和服务。默认情况下，Kubernetes 不会保留超过 1 小时的事件。查看作业运行的最近事件

```fallback
kubectl -n kubeflow describe tfjobs tfjob-simple
```

这将产生类似的输出

```fallback
Name:         tfjob-simple
Namespace:    kubeflow
Labels:       <none>
Annotations:  <none>
API Version:  kubeflow.org/v1
Kind:         TFJob
Metadata:
  Creation Timestamp:  2021-09-06T11:48:09Z
  Generation:          1
  Managed Fields:
    API Version:  kubeflow.org/v1
    Fields Type:  FieldsV1
    fieldsV1:
      f:spec:
        .:
        f:tfReplicaSpecs:
          .:
          f:Worker:
            .:
            f:replicas:
            f:restartPolicy:
            f:template:
              .:
              f:spec:
    Manager:      kubectl-create
    Operation:    Update
    Time:         2021-09-06T11:48:09Z
    API Version:  kubeflow.org/v1
    Fields Type:  FieldsV1
    fieldsV1:
      f:spec:
        f:runPolicy:
          .:
          f:cleanPodPolicy:
        f:successPolicy:
        f:tfReplicaSpecs:
          f:Worker:
            f:template:
              f:metadata:
              f:spec:
                f:containers:
      f:status:
        .:
        f:completionTime:
        f:conditions:
        f:replicaStatuses:
          .:
          f:Worker:
            .:
            f:succeeded:
        f:startTime:
    Manager:         manager
    Operation:       Update
    Time:            2021-09-06T11:49:30Z
  Resource Version:  5764004
  Self Link:         /apis/kubeflow.org/v1/namespaces/kubeflow/tfjobs/tfjob-simple
  UID:               3a67a9a9-cb89-4c1f-a189-f49f0b581e29
Spec:
  Tf Replica Specs:
    Worker:
      Replicas:        2
      Restart Policy:  OnFailure
      Template:
        Spec:
          Containers:
            Command:
              python
              /var/tf_mnist/mnist_with_summaries.py
            Image:  gcr.io/kubeflow-ci/tf-mnist-with-summaries:1.0
            Name:   tensorflow
Status:
  Completion Time:  2021-09-06T11:49:30Z
  Conditions:
    Last Transition Time:  2021-09-06T11:48:09Z
    Last Update Time:      2021-09-06T11:48:09Z
    Message:               TFJob tfjob-simple is created.
    Reason:                TFJobCreated
    Status:                True
    Type:                  Created
    Last Transition Time:  2021-09-06T11:48:12Z
    Last Update Time:      2021-09-06T11:48:12Z
    Message:               TFJob kubeflow/tfjob-simple is running.
    Reason:                TFJobRunning
    Status:                False
    Type:                  Running
    Last Transition Time:  2021-09-06T11:49:30Z
    Last Update Time:      2021-09-06T11:49:30Z
    Message:               TFJob kubeflow/tfjob-simple successfully completed.
    Reason:                TFJobSucceeded
    Status:                True
    Type:                  Succeeded
  Replica Statuses:
    Worker:
      Succeeded:  2
  Start Time:     2021-09-06T11:48:10Z
Events:
  Type    Reason                   Age                    From              Message
  ----    ------                   ----                   ----              -------
  Normal  SuccessfulCreatePod      7m9s                   tfjob-controller  Created pod: tfjob-simple-worker-0
  Normal  SuccessfulCreatePod      7m8s                   tfjob-controller  Created pod: tfjob-simple-worker-1
  Normal  SuccessfulCreateService  7m8s                   tfjob-controller  Created service: tfjob-simple-worker-0
  Normal  SuccessfulCreateService  7m8s                   tfjob-controller  Created service: tfjob-simple-worker-1
  Normal  ExitedWithCode           5m48s (x3 over 5m48s)  tfjob-controller  Pod: kubeflow.tfjob-simple-worker-1 exited with code 0
  Normal  ExitedWithCode           5m48s                  tfjob-controller  Pod: kubeflow.tfjob-simple-worker-0 exited with code 0
  Normal  TFJobSucceeded           5m48s                  tfjob-controller  TFJob kubeflow/tfjob-simple successfully completed.
```

这里的事件表明 pod 和服务已成功创建。

## TensorFlow 日志[ ](https://www.kubeflow.org/docs/components/training/tftraining/#tensorflow-logs)

日志记录遵循标准的 K8s 日志记录实践。

您可以使用 kubectl 为尚未**删除**的任何 pod 获取标准输出/错误。

首先找到作业控制器为感兴趣的副本创建的 pod。Pod 将被命名

```fallback
${JOBNAME}-${REPLICA-TYPE}-${INDEX}
```

确定 pod 后，您可以使用 kubectl 获取日志。

```fallback
kubectl logs ${PODNAME}
```

The **CleanPodPolicy** in the `TFJob` spec controls deletion of pods when a job terminates. The policy can be one of the following values

`TFJob` 规范中的**CleanPodPolicy控制在作业终止时删除 pod。该策略可以是以下值之一

- **Running **策略意味着只有在作业完成时仍在运行的 Pod（例如参数服务器）才会被立即删除；已完成的 pod 不会被删除，以便保留日志。这是默认值。
- **All** 策略意味着当作业完成时，所有 pod 甚至已完成的 pod 都将被立即删除。
- **None** 策略意味着作业完成时不会删除任何 pod 。

如果您的集群利用了 Kubernetes [集群日志记录](https://kubernetes.io/docs/concepts/cluster-administration/logging/) ，那么您的日志也可能会被发送到适当的数据存储以进行进一步分析。

### GKE 上的 Stackdriver

有关使用 Stackdriver 获取日志的说明，请参阅[日志记录和监控](https://www.kubeflow.org/docs/gke/monitoring/)指南。

如 [日志和监控](https://www.kubeflow.org/docs/gke/monitoring/#filter-with-labels)指南中所述，可以根据 pod 标签获取特定副本的日志。

使用 Stackdriver UI，您可以使用类似的查询

```fallback
resource.type="k8s_container"
resource.labels.cluster_name="${CLUSTER}"
metadata.userLabels.job-name="${JOB_NAME}"
metadata.userLabels.replica-type="${TYPE}"
metadata.userLabels.replica-index="${INDEX}"
```

或者使用 gcloud

```fallback
QUERY="resource.type=\"k8s_container\" "
QUERY="${QUERY} resource.labels.cluster_name=\"${CLUSTER}\" "
QUERY="${QUERY} metadata.userLabels.job-name=\"${JOB_NAME}\" "
QUERY="${QUERY} metadata.userLabels.replica-type=\"${TYPE}\" "
QUERY="${QUERY} metadata.userLabels.replica-index=\"${INDEX}\" "
gcloud --project=${PROJECT} logging read  \
     --freshness=24h \
     --order asc  ${QUERY}
```

## 故障排除

以下是解决您的工作问题的一些步骤

1. 您的工作是否存在状态？运行命令

   ```fallback
   kubectl -n ${USER_NAMESPACE} get tfjobs -o yaml ${JOB_NAME}
   ```

  - **USER_NAMESPACE** 是为您的用户配置文件创建的命名空间。

  - 如果结果输出不包含您的作业的状态，则这通常表明作业规范无效。

  - 如果`TFJob`规范无效，则 tf 操作员日志中应该有一条日志消息

    ```fallback
    kubectl -n ${KUBEFLOW_NAMESPACE} logs `kubectl get pods --selector=name=tf-job-operator -o jsonpath='{.items[0].metadata.name}'`
    ```

    - **KUBEFLOW_NAMESPACE**是您在其中部署`TFJob`操作员的命名空间。

2. 检查您的作业的事件以查看是否创建了 pod

  - 有多种获取事件的方法；如果您的任务不到**1 小时** ，那么您可以

    ```fallback
    kubectl -n ${USER_NAMESPACE} describe tfjobs ${JOB_NAME}
    ```

  - 输出的底部应包含作业发出的事件列表；例如

    ```yaml
    Events:
     Type    Reason                   Age   From              Message
     ----    ------                   ----  ----              -------
     Normal  SuccessfulCreatePod      90s   tfjob-controller  Created pod: tfjob2-worker-0
     Normal  SuccessfulCreatePod      90s   tfjob-controller  Created pod: tfjob2-ps-0
     Normal  SuccessfulCreateService  90s   tfjob-controller  Created service: tfjob2-worker-0
     Normal  SuccessfulCreateService  90s   tfjob-controller  Created service: tfjob2-ps-0
    ```

  - Kubernetes 仅将事件保留**1 小时**（参见[kubernetes/kubernetes#52521](https://github.com/kubernetes/kubernetes/issues/52521)）

    - 根据您的集群设置，事件可能会持久保存到外部存储并且可以访问更长时间
    - 在 GKE 上，事件保存在 stackdriver 中，可以使用上一节中的说明进行访问。

  - 如果 pod 和服务没有被创建，那么这表明它们`TFJob`没有被处理；常见的原因是

    - `TFJob`规格无效（见上文）
    - `TFJob` operator 未运行

3. 检查 pod 的事件以确保它们已调度好。

  - 有多种获取事件的方法；如果您的 pod 不到**1 小时** ，那么您可以这样做

    ```fallback
     kubectl -n ${USER_NAMESPACE} describe pods ${POD_NAME}
    ```

  - 输出的底部应包含如下事件

    ```fallback
    Events:
    Type    Reason                 Age   From                                                  Message
    ----    ------                 ----  ----                                                  -------
    Normal  Scheduled              18s   default-scheduler                                     Successfully assigned tfjob2-ps-0 to gke-jl-kf-v0-2-2-default-pool-347936c1-1qkt
    Normal  SuccessfulMountVolume  17s   kubelet, gke-jl-kf-v0-2-2-default-pool-347936c1-1qkt  MountVolume.SetUp succeeded for volume "default-token-h8rnv"
    Normal  Pulled                 17s   kubelet, gke-jl-kf-v0-2-2-default-pool-347936c1-1qkt  Container image "gcr.io/kubeflow/tf-benchmarks-cpu:v20171202-bdab599-dirty-284af3" already present on machine
    Normal  Created                17s   kubelet, gke-jl-kf-v0-2-2-default-pool-347936c1-1qkt  Created container
    Normal  Started                16s   kubelet, gke-jl-kf-v0-2-2-default-pool-347936c1-1qkt  Started container
    ```

  - 一些可以阻止容器启动的常见问题是

    - 资源不足，无法调度 pod
    - pod 尝试挂载不存在或不可用的卷（或密钥）
    - docker 镜像不存在或无法访问（例如由于权限问题）

4. 如果容器启动；按照上一节中的说明检查容器的日志。


## 更多信息

- 浏览[`TFJob`参考文档](https://www.kubeflow.org/docs/reference/tfjob)。
- 了解如何[使用 gang-scheduling 运行作业](https://www.kubeflow.org/docs/use-cases/job-scheduling#running-jobs-with-gang-scheduling)。