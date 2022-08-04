+++
title = "贡献"
description = "Kubeflow 贡献指引"
weight = 20
aliases = ["/docs/contributing/"]
+++

本文档是有关如何为代码库做出贡献的唯一真实来源。
我们很乐意接受您对该项目的补丁和贡献。
您只需要遵循一些小准则。

## 开始

### 签署 CLA

对该项目的贡献必须附有贡献者许可协议 (CLA)。 
您（或您的雇主）保留您贡献的版权。
这使我们有权在项目中使用和重新分配您的贡献。
前往 <https://cla.developers.google.com/> 查看您当前存档的协议或签署新协议。

您通常只需要提交一次 CLA，因此如果您已经提交了一份（即使是针对不同的项目），
您可能不需要再次提交。

### 遵守行为准则

请务必阅读并遵守我们的[行为准则](https://github.com/kubeflow/community/blob/master/CODE_OF_CONDUCT.md)和
[包容性文件](https://github.com/kubeflow/community/blob/master/INCLUSIVITY.md)。

## 加入社区

如果您想执行以下操作，请遵循以下说明：

- 成为 Kubeflow GitHub 组织的成员（见下文）
- 成为 Kubeflow 构建警察或发布团队的一员
- 被认可为对 Kubeflow 做出贡献的个人或组织

### 加入 Kubeflow GitHub 组织

在要求加入社区之前，我们要求您首先做出少量贡献，
以表明您打算继续为 Kubeflow 做出贡献。

- **注意**: 任何人都可以为 Kubeflow 做出贡献，将自己
  添加为 [org.yaml](https://github.com/kubeflow/internal-acls/blob/master/github-orgs/kubeflow/org.yaml) 中的成员不是强制性的步骤。 

有多种方式可以为 Kubeflow 做出贡献：

- 提交 PRs
- 报告错误或提供反馈的文件问题
- 回答有关 Slack 或 GitHub 讨论的问题

您可以使用[此表](http://devstats.kubeflow.org/d/9/developers-summary)查看您做出了多少贡献。

- **注意**: 这仅计算 GitHub 相关的贡献方式

当你已经准备加入

- 发送 PR 将自己添加为 [org.yaml](https://github.com/kubeflow/internal-acls/blob/master/github-orgs/kubeflow/org.yaml#L19) 的成员。
  按照[加入 Kubeflow GitHub 组织中的说明](https://github.com/kubeflow/internal-acls#joining-kubeflow-github-organization)，
  了解有关要包含在 PR 中的工件以及如何测试 PR 的更多信息。
- PR 合并后，管理员会向您发送邀请
  - 这是一个手动过程，通常每周运行几次
  - 如果一周后没有收到邀请，请在 [kubeflow#community](https://kubeflow.slack.com/messages/C8Q0QJYNB/convo/CABQ2BWHW-1544147308.002500/) 联系

### 公司/组织

如果您希望您的公司或组织因对 Kubeflow 做出贡献
或参与社区（作为用户数）而获得认可，
请发送 PR，将相关信息添加到 [member_organizations.yaml](https://github.com/kubeflow/community/blob/master/member_organizations.yaml)。

如果您希望员工的 GitHub 贡献归因于您的公司，
请让他们在其 GitHub 个人资料中设置公司字段。

## 您的第一个贡献

### 找到可以做的事情

随时欢迎帮助！
例如，文档（如您现在正在阅读的文本）总是可以使用改进。
总有可以阐明的代码和可以重命名或注释的变量或函数。
总是需要更多的测试覆盖率。
你明白了——如果你看到一些你认为应该修复的东西，你应该主导它。
这是您如何开始的。

### 入门问题

找到具有良好切入点的 Kubeflow 问题：

- 从 **good first issue** 标记开始。
  比如，在 [kubeflow/website 仓库](https://github.com/kubeflow/website/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)查看第一个 good first issues
  以获取文档更新，在 [kubeflow/kubeflow 仓库](https://github.com/kubeflow/kubeflow/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
  查看核心代码更新。
- 对于需要更深入讨论有一到多个技术方面的问题，请查看标记为 **help wanted** 的问题。
  比如，在 [kubeflow/kubeflow 参考](https://github.com/kubeflow/kubeflow/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22)查看此问题。
- Examine the issues in any of the [Kubeflow repositories](https://github.com/kubeflow).

## Owners 文件及 PR 工作流

我们的 PR 工作流程与 Kubernetes 几乎相同。
这些说明大部分是 Kubernetes 的 [贡献者](https://github.com/kubernetes/community/blob/master/contributors/guide/README.md)
及 [owners](https://github.com/kubernetes/community/blob/master/contributors/guide/owners.md#code-review-using-owners-files) 指南修改的版本。

### OWNERS 文件概述

OWNERS 文件用于指定对 Kubeflow 代码库不同部分的责任。
今天，我们使用它们来分配在我们的两阶段代码审查过程中使用的 **审查者** 和 **批准者** 角色。
OWNERS 文件受 [Chromium OWNERS 文件](https://chromium.googlesource.com/chromium/src/+/master/docs/code_reviews.md)启发，
而后者又启发自 [GitHub's CODEOWNERS 文件](https://help.github.com/articles/about-codeowners/)。

使用代码审查的项目的速度受到能够审查代码的人数的限制。
一个人的代码审查质量受到他们对被审查代码的熟悉程度的限制。
我们的目标是通过谨慎使用和维护 OWNERS 文件来解决这两个问题

<a name="owners-1"></a>
### OWNERS

每个包含独立代码或内容单元的目录也可能包含一个 OWNERS 文件。
此文件适用于目录中的所有内容，包括 OWNERS 文件本身、同级文件和子目录。

OWNERS 文件为 YAML 格式并支持以下键：

- `approvers`: 可以 `/approve` PR 的 Github 用户名或者别名列表
- `labels`: 自动应用到 PR 的 Github 标签列表
- `options`: 解释 OWNERS 文件的 map 选项，现阶段只有一个：
  - `no_parent_owners`: 不存在时默认为 `false`；如果为 `true`，排除父级 OWNERS 文件。
    允许 `a/deep/nested/OWNERS` 组织 `a/OWNERS` 文件对
    `a/deep/nested/bit/of/code` 产生影响
- `reviewers`: 适合 `/lgtm` PR 的Github 用户名或者别名列表

所有用户都应该是可分配的。
在 GitHub 中，这意味着他们要么是 repo 的合作者，要么是 repo 所属组织的成员。

典型的 OWNERS 文件如下所示：

```
approvers:
  - alice
  - bob     # 这里是注释
reviewers:
  - alice
  - carol   # 这里也是注释
  - sig-foo # 这里是个别名
```

#### OWNERS_ALIASES

每个 repo 的根目录可能包含一个 OWNERS_ALIAS 文件。

OWNERS_ALIAS 文件为 YAML 格式并支持以下键：

- `aliases`: 别名到 GitHub 用户名列表的映射

我们为组使用别名而不是 GitHub Teams，因为对 GitHub Teams 的更改不可公开审核。

示例 OWNERS_ALIASES 文件如下所示：

```
aliases:
  sig-foo:
    - david
    - erin
  sig-bar:
    - bob
    - frank
```

OWNERS 文件中列出的 GitHub 用户名和别名不区分大小写。

### 代码审查过程

- **author** 提交 PR
- 阶段 0: 自动化建议 PR 的 **reviewers** 和 **approvers**
  - 确定最接近被更改代码的 OWNERS 文件集
  - 选择至少两个 **reviewers**，尝试为每个叶子 OWNERS 文件找到一个唯一的审稿人，
    为 PR 请求他们进行审稿
  - 择建议的 **approvers**，从每个 OWNERS 文件中选择一个，并在 PR 的评论中列出它们
- 阶段 1: 人工审查 PR
  - **Reviewers** 查看代码质量、正确性、健全的软件工程、风格等。
  - 织中的任何人都可以担任 **reviewer** 但开启 PR 的个人除外
  - 如果代码更改看起来何时，**reviewer** 在 PR 评论输入 `/lgtm` 或审查；
    如果他们改变主意 `/lgtm cancel`
  - 一旦 **reviewer** 被 `/lgtm`，[prow](https://prow.k8s.io)
    ([@k8s-ci-robot](https://github.com/k8s-ci-robot/)) 接受 `lgtm` 标签到 PR
- 阶段 2: 人工批准 PR
  - PR **author** `/assign` 所有建议的 **approvers** 到 PR，并可选的通知到
    他们 (比如："pinging @foo 来批准")
  - 只有相关 OWNERS 文件中列出的人员，直接或者间接的别名，才能充当
    **approvers**，包括打开 PR 的个人
  - **Approvers** 找整体验收标准，包括与其他功能的依赖关系、前向/后向兼容性、API 和标志定义等
  - 如果代码更改在他们看来不错， **approver** 在 PR 评论中输入 `/approve` 或 审查；
    如果他们改变主意 `/approve cancel`
  - [prow](https://prow.k8s.io) ([@k8s-ci-robot](https://github.com/k8s-ci-robot/)) 更新
    PR 评论，指出仍需要 **approvers** 进行批准
  - 一旦所有 **approvers** (来自先前确定的每个 OWNERS 文件中的一个) 批准，
    [prow](https://prow.k8s.io) ([@k8s-ci-robot](https://github.com/k8s-ci-robot/)) 将应用 `approved` 标签
- 阶段 3: 自动合并 PR：

  - 如果以下所有条件都为真：

    - 存在所有必需的标签（例如：`lgtm`，`approved`）
    - 缺少任何阻止标签（例如：没有 `do-not-merge/hold`, `needs-rebase`）

  - 如果以下任何一项为真：

    - 没有为此 repo 配置 presubmit prow 作业
    - 有为此 repo 配置的 presubmit prow 作业，并且它们在最后一次自动重新运行后都通过

  - 然后PR会自动合并

### 过程的怪癖

我们观察到的许多行为 _可能_ 会受到阻止，
因为它们违背了审核的流程。
其中一些可能在未来被阻止，但这是今天的状态。

- **approver** 的 `/lgtm` 同时被解释为 `/approve`
  - 虽然对某些人来说是一种方便的快捷方式，但令人惊讶的是，
    根据评论者的身份，以两种方式之一解释相同的命令
  - 相反，明确帮写明 `/lgtm` 和 `/approve` 来帮助观察者， 
    或为 **reviewer** 保存 `/lgtm`
  - 这违背了对 PR 至少有两对眼睛盯着它的的原则，并表明
    又太少的 **reviewers** (它不能同时是 **approver**)
- 从技术上讲，任何 Kubeflow GitHub 组织的成员都可以 `/lgtm` 一个 PR
  - 鼓励来自非成员的评论作为实验证明，以及意图成为一名协作或评论者
  - 来自成员中的 `/lgtm` 可能表明我们的 OWNERS 文件过小，
    或已存在的 **reviewers** 反应太过迟钝
  - 这违背了在最初指定 **reviewers** 的原则，以确保
    **author** 从熟悉代码的人那里获得可操作的反馈
- **Reviewers** 和 **approvers** 无反应
  - 这让那些通常不知道他们的 PR 为何被忽略的 **authors** 感到沮丧
  - 许多 **reviewers** 及 **approvers** 都被 GitHub 通知打扰，而不对其他人 @提及 的问题进行快速相应
  - 如果一个 **author** `/assign` 一个 PR，**reviewers** 和 **approvers** 将能在
    他们的 [PR dashboard](https://k8s-gubernator.appspot.com/pr) 知道。
  - **author** 可以手动读取相关 OWNERS 文件，
    `/unassign` 无响应的人，并 `/assign` 给其他人
  - 这是我们的所有者文件过时的迹象；修剪 **审阅人** 和 **批准人** 列表将对此有所帮助
  - PR **authors** 有责任推动 PR 问题的解决。这意味着如果 PR **reviewers** 未响应，则应该进行升级
    - 例如，即使 ping **reviewers** 去审阅
    - 如果 **reviewers** 未响应，查看 root 下的 OWNER 文件，来 ping 列表中的 **approvers**
- **Authors** 未响应
  - 这会花费大量的注意力，因为随着时间的推移，个人 PR 的上下文会丢失
  - 这通常会损害项目，因为其总体噪音水平会随着时间的推移而增加
  - 相反，关闭太长时间后未触及的 PR（我们目前有一个机器人在 90 天后执行此操作）

## 使用 OWNERS 文件的自动化

### [`prow`](https://git.k8s.io/test-infra/prow)

Prow 从 GitHub 接收事件，并对它们做出反应。它实际上是无状态的。
以下部分用于实现上面的代码审查过程。

- [cmd: tide](https://git.k8s.io/test-infra/prow/cmd/tide)
  - per-repo 配置:
    - `labels`: 合并所需的标签列表（例如：`lgtm`）
    - `missingLabels`: 合并所需的标签列表（例如：`do-not-merge/hold`）
    - `reviewApprovedRequired`: 默认 `false`；为 true 时，需要至少一个
      [approved pull request review](https://help.github.com/articles/about-pull-request-reviews/)
      以进行合并
    - `merge_method`: 默认 `merge`；当为 `squash` 或 `rebase`，, 在单击 PR 的合并按钮时使用该合并方法
  - merges PR's once they meet the appropriate criteria as configured above
  - if there are any presubmit prow jobs for the repo the PR is against, they will be re-run one
    final time just prior to merge
- [plugin: assign](https://git.k8s.io/test-infra/prow/plugins/assign)
  - 在 PR 上为 GitHub users 响应 `/assign` 回复
  - 在 PR 上为 GitHub users 响应 `/unassign` 回复
- [plugin: approve](https://git.k8s.io/test-infra/prow/plugins/approve)
  - per-repo 配置：
    - `issue_required`: 默认为 `false`；如果为 `true`，需要 PR 描述链接到
      一个 issue，或者至少一个 **approver** 发布 `/approve no-issue`
    - `implicit_self_approve`: 默认为 `false`; ；如果为 `true`，如果 PR 作者在相关的
      OWNERS 文件，隐式地进行 `/approve`
  - adds the `approved` label once an **approver** for each of the required
    OWNERS files has `/approve`'d
  - comments as required OWNERS files are satisfied
  - 删除过时的审批状态注释
- [plugin: blunderbuss](https://git.k8s.io/test-infra/prow/plugins/blunderbuss)
  - 确定 **reviewers** 并要求他们对 PR 进行审阅
- [plugin: lgtm](https://git.k8s.io/test-infra/prow/plugins/lgtm)
  - 当 **reviewer** 为 PR 添加 `/lgtm` 时增加 `lgtm` 标签
  - **PR author** 可能不会为他们的 PR 添加 `/lgtm`
- [pkg: k8s.io/test-infra/prow/repoowners](https://git.k8s.io/test-infra/prow/repoowners/repoowners.go)
  - 解析 OWNERS 和 OWNERS_ALIAS 文件
  - 如果遇到 `no_parent_owners` 选项，则父所有者将不会对
    当前 OWNERS 文件附近或下方的文件产生任何影响

## 维护 OWNERS 文件

OWNERS 文件应定期维护。

我们鼓励人们通过 PR 自行提名或自行从 OWNERS 文件中删除。理想情况下，
我们可以在未来使用指标驱动的自动化来协助这个过程。

我们应该努力：

- 增加 OWNERS 文件的数量
- 将新人添加到 OWNERS 文件
- 确保 OWNERS 文件仅包含组织成员和 repo 协作者
- 确保 OWNERS 文件只包含积极贡献或审查他们拥有的代码的人
- 从 OWNERS 文件中删除不活跃的人

OWNERS 使用的坏用例：

- 缺少 OWNERS 文件的目录，导致访问 root OWNERS 过多
- 拥有一个人同时作为审批和审阅作者的 OWNERS 文件
- 超过 6 个月未触及的 OWNERS 文件
- 存在非协作者的 OWNERS 文件

OWNERS 良好用例：

- `reviewers` 超过 `approvers`
- `approvers` 不在该 `reviewers` 部分
- 定期更新的 OWNERS 文件（每个版本至少一次）
