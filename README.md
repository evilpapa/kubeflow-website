# Kubeflow 网站

欢迎来到 Kubeflow 公共站点的 GitHub 仓库！

文档部署在 https://www.kubeflow.org。

我们在 [Hugo](https://gohugo.io/) 利用 [google/docsy](https://github.com/google/docsy)
组织网站样式，并使用 [Netlify](https://www.netlify.com/) 管理网站发布。

## 快速开始

这是一个文档更新快速指引文档:

1. 派生 Github [kubeflow/website 仓库](https://github.com/kubeflow/website)。

2. 修改并发送推送请求 (PR)。

3. 如果您还没有准备好进行审核，请在 PR 名称中添加“WIP”以表明它正在进行中。
   或者，您使用 `/hold` [prow 命令](https://prow.k8s.io/command-help) 在评论中将 PR 标记为未准备好合并。

4. 等待自动化 PR 工作流程进行一些检查。
   准备好后，您应该会看到如下评论：`deploy/netlify — Deploy preview ready!`

5. 点击 **详情** 右侧“准备好部署审查”查看更新的预览。

6. 继续更新您的文档并推送您的更改，直到您对内容感到满意为止。

7. 当您准备好进行审核时，向 PR 添加评论，删除任何保留或“WIP”标记，并分配审核者/批准者。
   查看 [Kubeflow 贡献者指引](https://www.kubeflow.org/docs/about/contributing/).

如果您在 GitHub 工作流程方面需要更多帮助，请关注
这篇 [标准 GitHub 工作流指引](https://github.com/kubeflow/website/blob/master/quick-github-guide.md)。

## 本地开发

本节展示如何通过在本地运行 Hugo 服务来进行本地开发。

### 安装 Hugo

要安装 Hugo，关注[适用于您的操作系统类型的说明](https://gohugo.io/getting-started/installing/)。

**注意:** 我们推荐使用 Hugo 版本 `0.92.0`，它时当前我们发布到 Netlify 的版本。

比如，使用 homebrew 在 macOS 或 Linux 安装 hugo：

```bash
# WARNING: 这可能会安装大于 `0.92.0` 的最新版本
brew install hugo
```

### 安装 Node 包

如果您打算更改网站样式，你需要安装一些 **node 类库**。
（查看 [Docsy 设置指引](https://www.docsy.dev/docs/getting-started/#install-postcss) 获取更多信息）

你可以通过下述安装我们在 Netlify 使用的相同版本（定义在 `package.json`）：

```bash
npm install -D
```

### 运行 Hugo 本地服务器

使用可视化 GitHub 工作流 fork GitHub 仓库并克隆到本地机器。

1. 在 GitHub UI 中 **Fork** [kubeflow/website 仓库](https://github.com/kubeflow/website)。

2. 克隆 fork 的分支到本地：

    ```bash
    git clone git@github.com:<your-github-username>/website.git
    cd website/
    ```

3. 递归下载子模块（docsy）：

    ```bash
    git submodule update --init --recursive
    ```

4. 开启本地 Hugo 服务器：

    ```bash
    hugo server -D
    ```

5. 通过 [http://localhost:1313/](http://localhost:1313/) 访问本地网站。

### 有用的文档

* [Docsy 主题用户指引](https://www.docsy.dev/docs/getting-started/)
* [Hugo 安装指引](https://gohugo.io/getting-started/installing/)
* [Hugo 基本用法](https://gohugo.io/getting-started/usage/)
* [Hugo 站点目录结构](https://gohugo.io/getting-started/directory-structure/)
* [hugo 服务器参考](https://gohugo.io/commands/hugo_server/)

## 菜单结构

站点主题有个 Hugo 菜单 (`main`)，它定义了顶部导航栏。您可以找到并
通过修改 [站点配置文件](https://github.com/kubeflow/website/blob/master/config.toml) 调整定义菜单。

左侧导航面板由 [`docs`](https://github.com/kubeflow/website/tree/master/content/en/docs)目录结构定义。

每个 _页面前面_ 的 `weight` 属性确定页面相对于同一目录中其他页面的位置。 权重越低，页面出现在该部分中的时间就越早。

下面是个 `_index.md` 文件示例：

```md
+++
title = "开始使用 Kubeflow"
description = "Overview"
weight = 1
+++
```

## Docsy 主题

我们为站点使用 [Docsy](https://www.docsy.dev/) 主题。 
主题文件通过再 `themes/docsy` 文件夹的 [git 子模块](https://git-scm.com/book/en/v2/Git-Tools-Submodules) 管理。

**请勿修改这些文件**，他们实际上并不再仓库中，但却是 [google/docsy](https://github.com/google/docsy) 仓库的一部分。

要更新 docsy 新提交的特性，在根目录运行如下命令：

```bash
git submodule update --remote
```

## 文档主题指引

有关编写有效文档的指导，请参阅
[Kubeflow 文档的样式指南](https://kubeflow.org/docs/about/style-guide/)。

## 样式化您的内容

该主题在 [`assets/scss` 文件夹](https://github.com/kubeflow/website/tree/master/themes/docsy/assets/scss)。

**不要修改这些文件**，实际上他们并不在仓库中，但却作为 [google/docsy](https://github.com/google/docsy) 仓库的一部分。

您可以覆盖默认样式并添加新样式：

* 通常，将您的文件放在 `website` 下的项目目录结构中，而不是放在主题目录中。
  使用与主题相同的文件名，并将文件放在相同的相对位置。
  Hugo 首先查找项目主目录中的文件（如果存在），然后查找主题目录下的文件。
  比如，Kubeflow 网站的 [`layouts/partials/navbar.html`](https://github.com/kubeflow/website/blob/master/layouts/partials/navbar.html)
  覆盖了主题的 [`layouts/partials/navbar.html`](https://github.com/kubeflow/website/blob/master/themes/docsy/layouts/partials/navbar.html)

* 您可以更新 Kubeflow 网站的项目变量 [`_variables_project.scss` 文件](https://github.com/kubeflow/website/blob/master/assets/scss/_variables_project.scss)。
  覆盖文件中的 [Docsy 变量](https://github.com/kubeflow/website/blob/master/themes/docsy/assets/scss/_variables.scss)值。 
  你也可以使用 `_variables_project.scss` 来指定 [Bootstrap 4 变量](https://getbootstrap.com/docs/4.0/getting-started/theming/)默认值为你自己的值。

* 自定义样式 [`_styles_project` 文件](https://github.com/kubeflow/website/blob/master/assets/scss/_styles_project.scss)

图片样式：

* 要查看样式图像的一些示例，请查看 Kubeflow 文档的 [OAuth 设置页](https://www.kubeflow.org/docs/gke/deploy/oauth-setup/)。 
  在 [page source](https://raw.githubusercontent.com/kubeflow/website/master/content/en/docs/gke/deploy/oauth-setup.md) 搜索 `.png`。

* 更多帮助，查看
  [Bootstrap 图片样式](https://getbootstrap.com/docs/4.0/content/images/)。

* 另请参阅 Bootstrap 实用程序，例如
  [borders](https://getbootstrap.com/docs/4.0/utilities/borders/)。

网站[前端页面](https://www.kubeflow.org/)：

* 查看 [page source](https://github.com/kubeflow/website/blob/master/content/en/_index.html)。

* CSS 样式在 [project 变量文件](https://github.com/kubeflow/website/blob/master/assets/scss/_variables_project.scss)。

* 页面使用 [cover block](https://www.docsy.dev/docs/adding-content/shortcodes/#blocks-cover) 定义主题。

* 页面页同样使用了 [linkdown block](https://www.docsy.dev/docs/adding-content/shortcodes/#blocks-link-down)。

## 使用 Hugo shortcodes

有时在一个地方定义一个信息片段并在我们需要的地方重用它是有用的。
例如，我们希望能够在整个文档中引用各种框架/库的最低版本，
这样不会造成维护噩梦。

为此，我们使用 Hugo 的“短代码”。
短代码类似于 Django 变量。 您在文件中定义短代码，然后使用特定标记
调用文档中的短代码。 构建页面时，该标记将替换为短代码文件的内容。

要创建短代码：

1. 在 `/website/layouts/shortcodes/` 文件夹创建 HTML 文件。
   文件名必须简短且有意义，因为它决定了您和其他人在文档中使用的短代码。

2. 对于文件内容，添加构建网页时应替换短代码标记的文本和 HTML 标记。

要在文档中使用短代码，请将短代码的名称用大括号和百分号括起来，如下所示：

  ```
  {{% shortcode-name %}}
  ```

短代码名称是文件名减去 `.html` 文件扩展名。

**例子：** 以下短代码定义了 Kubernetes 所需的最低版本：

* 短代码的文件名：

  ```
  kubernetes-min-version.html
  ```

* 短代码内容：

  ```
  1.8
  ```

* 在文档中的用法：

  ```
  You need Kubernetes version {{% kubernetes-min-version %}} or later.
  ```

有用的 Hugo 文档：

* [Shortcode 模板](https://gohugo.io/templates/shortcode-templates/)
* [Shortcodes](https://gohugo.io/content-management/shortcodes/)

## 文档版本

对每个文档的版本，我们为相关文档创建一个分支。
比如，v0.2 稳定分支文档保持在 [v0.2-branch](https://github.com/kubeflow/website/tree/v0.2-branch) 分支。
每个分支有一个相应的同步合并了每个提交的 PR 的 Netlify 站点。

The versioned sites follow this convention:

* `www.kubeflow.org` 总是执行当前 *master branch*
* `master.kubeflow.org` 总是指向 GitHub 头
* `vXXX-YYY.kubeflow.org` 执行 vXXX.YYY-branch 分支

我们还将每个版本连接到网站菜单栏上的下拉菜单。
有关如何将网站更新到新版本的信息，请参阅 [Kubeflow 发布指南](https://github.com/kubeflow/kubeflow/blob/master/docs_dev/releasing.md#releasing-a-new-version-of-the-website)。

每当任何文档引用任何源代码时，您都应该在链接中使用版本短代码，如下所示：

```
https://github.com/kubeflow/kubeflow/blob/{{< params "githubbranch" >}}/scripts/gke/deploy.sh
```

这可确保版本化网页中的所有链接都指向正确的分支。
