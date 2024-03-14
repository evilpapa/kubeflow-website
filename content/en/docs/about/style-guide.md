+++
title =  "文档样式指引"
description = "编写 Kubeflow 文档的风格指南"
weight = 90
+++

此风格指南是针对 [Kubeflow 文档的](/docs/)。
文档指南帮助贡献者撰写能让读者更快速且正确理解的文档。

Kubeflow 文档目标：

- 在样式和术语上保持一致，以便读者可以以特定的结构和约定预期。
  读者不必反复学习如何使用文档或者质疑自己是否正确理解了某些内容。
- 简明、清晰的写作更能让读者快速找到并理解他们需要的信息。

## 使用标准美式拼写

Use American spelling rather than Commonwealth or British spelling.
Refer to [Merriam-Webster's Collegiate Dictionary, Eleventh Edition](http://www.merriam-webster.com/).

## 谨慎使用大写字母

一些提示：

- 仅将页面中每个标题的第一个字母大写。 （即使用句子大小写。）
- 大写（几乎）页面标题中的每个单词。 （也就是说，使用标题大小写。）
  “and”、“in”等小词没有大写字母。
- 在页内文章，为品牌名，如 Kubeflow、Kubernetes 等品牌名使用大写。
  参考[如下](#use-full-correct-brand-names)品牌名。
- 不要为强调词使用大小写。

## 拼全首次使用的缩写和首字母缩略词

当你第一次在页面上使用缩写或首字母缩写时，一定要把它的全名拼出来。
不要以为人们知道缩写或首字母缩写的意思，即使这看起来像是常识。

示例： “在虚拟机（VM）中本地化运行 Kubernetes”

## 如果你想使用缩写的话

比如：使用 "it's" 替代 "it is" 的写法是可以的。

<a id="brand-names"></a>

## 使用完整、正确的品牌名称

当提到产品或品牌时，请使用全名。
如产品所有者在产品文档中所做的那样，将名称大写。
不要使用缩写，即使它们是常用的，除非产品所有者批准了缩写。

<div class="table-responsive">
  <table class="table table-bordered">
    <thead class="thead-light">
      <tr>
        <th>使用如下</th>
        <th>替代如下</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Kubeflow</td>
        <td>kubeflow</td>
      </tr>
      <tr>
        <td>Kubernetes</td>
        <td>k8s</td>
      </tr>
      <tr>
        <td>ksonnet</td>
        <td>Ksonnet</td>
      </tr>
    </tbody>
  </table>
</div>

## 与标点符号一致

在页面中始终使用标点符号。
例如，如果在列表中的每个项目后使用句点（句号），则在页面上的所有其他列表上使用句点。

如果您不确定某个特定的约定，请查看其他页面。

示例：

- Kubeflow文档中的大多数页面在每个列表项的末尾都使用句点。
- 页面副标题的末尾没有句号，并且副标题不需要是一个完整的句子。
  （副标题来自于每页首页的“描述”。）

## 使用主动语音而不是被动语音

被动语态通常令人困惑，因为不清楚谁应该执行动作。

<div class="table-responsive">
  <table class="table table-bordered">
    <thead class="thead-light">
      <tr>
        <th>使用主动语音</th>
        <th>替代被动语音</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>你可以配置 Kubeflow 为</td>
        <td>Kubeflow 可配置为</td>
      </tr>
      <tr>
        <td>添加文件夹到目录</td>
        <td>文件夹应添加到你的目录</td>
      </tr>
    </tbody>
  </table>
</div>

## 使用简单现在时

避免将来时（“will”）和复杂的语法，如连词语气（“would”，“should”）。

<div class="table-responsive">
  <table class="table table-bordered">
    <thead class="thead-light">
      <tr>
        <th>使用简单现在时</th>
        <th>代替将来时或复杂语法</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>以下命令提供一个虚拟机</td>
        <td>以下命令将提供一个虚拟机</td>
      </tr>
      <tr>
        <td>如果添加以下配置元素，系统打开网络</td>
        <td>如果以下元素被添加，系统将打开网络</td>
      </tr>
    </tbody>
  </table>
</div>

**Exception:** 如果有必要传达正确的意思，请使用将来时态。这种要求很少见。

## 直接向听众讲话

在句子中使用“我们”可能会令人困惑，因为读者可能不知道他们是否是你所描述的“我们”的一部分。

例如，比较以下两种语句：

- “在这个版本中，我们添加了许多新功能。”
- “在本教程中，我们制作了一个飞碟。”

“开发人员”或“用户”这两个词可能有歧义。

例如如果读者正在构建一个也有用户的产品，
那么读者就不知道你指的是读者还是他们产品的用户。

<div class="table-responsive">
  <table class="table table-bordered">
    <thead class="thead-light">
      <tr>
        <th>直接向读者致辞</th>
        <th>而不是“我们”、“用户”或“开发人员”</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>将文件夹包含到路径</td>
        <td>用户需要确保文件夹包含到他们的路径
        </td>
      </tr>
      <tr>
        <td>在本教程中，您将构建一个飞碟</td>
        <td>在本教程中，我们将构建一个飞碟</td>
      </tr>
    </tbody>
  </table>
</div>

## 使用简短的句子

保持句子简短。短句比长句更容易阅读。
下面是一些写短句的技巧。

<div class="table-responsive">
  <table class="table table-bordered">
    <thead class="thead-light">
      <tr>
        <th colspan="2">少用单词，不要用很多表达相同意思的单词</th>
      </tr>
      <tr>
        <th>使用以下</th>
        <th>代替以下</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>您可以使用</td>
        <td>也可以使用</td>
      </tr>
      <tr>
        <td>You can</td>
        <td>You are able to</td>
      </tr>
    </tbody>
  </table>
</div>

<div class="table-responsive">
  <table class="table table-bordered">
    <thead class="thead-light">
      <tr>
        <th colspan="2">把一个长句分成两个或两个以上的短句</th>
      </tr>
      <tr>
        <th>使用以下</th>
        <th>代替以下</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>您不需要运行GKE集群。部署过程
          为您创建一个集群</td>
        <td>您不需要运行GKE集群，因为部署流程为您创建一个集群</td>
      </tr>
    </tbody>
  </table>
</div>
<div class="table-responsive">
  <table class="table table-bordered">
    <thead class="thead-light">
      <tr>
        <th colspan="2">使用列表而不是长句来显示各种选项</th>
      </tr>
      <tr>
        <th>使用以下</th>
        <th>代替以下</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>
          <p>来训练一个模型：</p>
          <ol>
            <li>打包应用为Kubernetes 容器。</li>
            <li>上传融到到在线仓库。</li>
            <li>提交训练工作。</li>
          </ol>
        </td>
        <td>要训练模型，必须将程序打包到Kubernetes中
          容器，将容器上传到在线注册表，然后提交您的
          培训工作。</td>
      </tr>
    </tbody>
  </table>
</div>


## 避免过多的文本样式

当渲染 UI 控件或其他 UI 元素时使用 **粗体文本**。

为以下内容使用 `代码样式`：

- 文件名，文件夹，以及路径
- 行内代码及命令
- 对象字段名

避免使用粗体或大写字母进行强调。 
如果页面上有太多的文本高亮，它会变得混乱甚至让人感到反感。

## 使用尖括号表示占位符

比如：

- `export KUBEFLOW_USERNAME=<your username>`
- `--email <your email address>`

## 自定义图片样式

Kubeflow文档使用 Bootstrap 类来为图像和其他内容添加样式。

以下代码片段展示了使图像在页面上显示得漂亮的典型样式：

```
<img src="/docs/images/my-image.png"
  alt="My image"
  class="mt-3 mb-3 p-3 border border-info rounded">
```

要查看一些图片样式示例，请移步到 [OAuth 设置页](/docs/gke/deploy/oauth-setup/)。

更多帮助，请查看[Bootstrap 图片样式](https://getbootstrap.com/docs/4.6/content/images/) 指引以及 Bootstrap 工具，如 [边框](https://getbootstrap.com/docs/4.6/utilities/borders/)。

## 更详细的样式指引

[Google 开发文档样式指引](https://developers.google.com/style/) 包含了有关为开发人员受众编写清晰、易读、简洁文档的具体方面的详细信息。

## 下一步

- 参考 [文档 README](https://github.com/kubeflow/website/blob/master/README.md) 引导来为 Kubeflow 文档进行贡献。
