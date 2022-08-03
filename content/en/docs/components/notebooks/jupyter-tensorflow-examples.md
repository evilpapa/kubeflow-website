+++
title = "Jupyter TensorFlow 示例"
description = "在 Kubeflow Notebooks 中使用 Jupyter 和 TensorFlow 的示例"
weight = 40

+++

## Mnist 示例

(改编自 [tensorflow/tensorflow - mnist_softmax.py](https://github.com/tensorflow/tensorflow/blob/r1.4/tensorflow/examples/tutorials/mnist/mnist_softmax.py))

1. 创建笔记本服务器时，请选择安装了 Jupyter 和 TensorFlow的 [容器镜像](/docs/components/notebooks/container-images/)。

2. 使用 Jupyter 的界面创建一个新的 **Python 3** 笔记本。

3. 复制以下代码并将其粘贴到您的笔记本中：

    ```python
    from tensorflow.examples.tutorials.mnist import input_data
    mnist = input_data.read_data_sets("MNIST_data/", one_hot=True)

    import tensorflow as tf

    x = tf.placeholder(tf.float32, [None, 784])

    W = tf.Variable(tf.zeros([784, 10]))
    b = tf.Variable(tf.zeros([10]))

    y = tf.nn.softmax(tf.matmul(x, W) + b)

    y_ = tf.placeholder(tf.float32, [None, 10])
    cross_entropy = tf.reduce_mean(-tf.reduce_sum(y_ * tf.log(y), reduction_indices=[1]))

    train_step = tf.train.GradientDescentOptimizer(0.05).minimize(cross_entropy)

    sess = tf.InteractiveSession()
    tf.global_variables_initializer().run()

    for _ in range(1000):
      batch_xs, batch_ys = mnist.train.next_batch(100)
      sess.run(train_step, feed_dict={x: batch_xs, y_: batch_ys})

    correct_prediction = tf.equal(tf.argmax(y,1), tf.argmax(y_,1))
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
    print("Accuracy: ", sess.run(accuracy, feed_dict={x: mnist.test.images, y_: mnist.test.labels}))
    ```

4. 运行代码。您应该会看到很多来自 Tensorflow 的 `WARNING` 信息，然后是显示训练准确性的一行，如下所示：

    ```
    Accuracy:  0.9012
    ```

## 下一步

- 请参阅在 Jupyter 笔记本中创建 Kubeflow 管道的 [简单示例](https://github.com/kubeflow/examples/tree/master/pipelines/simple-notebook-pipeline)。
- 使用 [Kubeflow Pipelines SDK](/docs/components/pipelines/sdk/sdk-overview/) 构建机器学习工作流。
- 了解 Kubeflow Notebook 提供的高级功能，例如 [提交 Kubernetes 资源](/docs/components/notebooks/submit-kubernetes/) 或 [构建 Docker 镜像](/docs/components/notebooks/custom-notebook/)。 
