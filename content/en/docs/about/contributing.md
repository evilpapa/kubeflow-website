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

### Sign the CLA

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

- 从标记的 **good first issue** 开始。
  For example, see the good first issues in the [kubeflow/website repository](https://github.com/kubeflow/website/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
  for doc updates, and in the [kubeflow/kubeflow repository](https://github.com/kubeflow/kubeflow/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
  for updates to the core Kubeflow code.
- For issues that require deeper knowledge of one or more technical aspects, look at issues labeled **help wanted**.
  For example, see these issues in the [kubeflow/kubeflow repository](https://github.com/kubeflow/kubeflow/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22)
- Examine the issues in any of the [Kubeflow repositories](https://github.com/kubeflow).

## Owners files and PR workflow

Our PR workflow is nearly identical to Kubernetes'.
Most of these instructions are a modified version of Kubernetes' [contributors](https://github.com/kubernetes/community/blob/master/contributors/guide/README.md)
and [owners](https://github.com/kubernetes/community/blob/master/contributors/guide/owners.md#code-review-using-owners-files) guides.

### Overview of OWNERS files

OWNERS files are used to designate responsibility over different parts of the Kubeflow codebase.
Today, we use them to assign the **reviewer** and **approver** roles used in our two-phase code review process.
Our OWNERS files were inspired by [Chromium OWNERS files](https://chromium.googlesource.com/chromium/src/+/master/docs/code_reviews.md),
which in turn inspired [GitHub's CODEOWNERS files](https://help.github.com/articles/about-codeowners/).

The velocity of a project that uses code review is limited by the number of people capable of reviewing code.
The quality of a person's code review is limited by their familiarity with the code under review.
Our goal is to address both of these concerns through the prudent use and maintenance of OWNERS files

<a name="owners-1"></a>
### OWNERS

Each directory that contains a unit of independent code or content may also contain an OWNERS file.
This file applies to everything within the directory, including the OWNERS file itself, sibling files, and child directories.

OWNERS files are in YAML format and support the following keys:

- `approvers`: a list of GitHub usernames or aliases that can `/approve` a PR
- `labels`: a list of GitHub labels to automatically apply to a PR
- `options`: a map of options for how to interpret this OWNERS file, currently only one:
  - `no_parent_owners`: defaults to `false` if not present; if `true`, exclude parent OWNERS files.
    Allows the use case where `a/deep/nested/OWNERS` file prevents `a/OWNERS` file from having any
    effect on `a/deep/nested/bit/of/code`
- `reviewers`: a list of GitHub usernames or aliases that are good candidates to `/lgtm` a PR

All users are expected to be assignable.
In GitHub terms, this means they are either collaborators of the repo, or members of the organization to which the repo belongs.

A typical OWNERS file looks like:

```
approvers:
  - alice
  - bob     # this is a comment
reviewers:
  - alice
  - carol   # this is another comment
  - sig-foo # this is an alias
```

#### OWNERS_ALIASES

Each repo may contain at its root an OWNERS_ALIAS file.

OWNERS_ALIAS files are in YAML format and support the following keys:

- `aliases`: a mapping of alias name to a list of GitHub usernames

We use aliases for groups instead of GitHub Teams, because changes to GitHub Teams are not publicly auditable.

A sample OWNERS_ALIASES file looks like:

```
aliases:
  sig-foo:
    - david
    - erin
  sig-bar:
    - bob
    - frank
```

GitHub usernames and aliases listed in OWNERS files are case-insensitive.

### The code review process

- The **author** submits a PR
- Phase 0: Automation suggests **reviewers** and **approvers** for the PR
  - Determine the set of OWNERS files nearest to the code being changed
  - Choose at least two suggested **reviewers**, trying to find a unique reviewer for every leaf
    OWNERS file, and request their reviews on the PR
  - Choose suggested **approvers**, one from each OWNERS file, and list them in a comment on the PR
- Phase 1: Humans review the PR
  - **Reviewers** look for general code quality, correctness, sane software engineering, style, etc.
  - Anyone in the organization can act as a **reviewer** with the exception of the individual who
    opened the PR
  - If the code changes look good to them, a **reviewer** types `/lgtm` in a PR comment or review;
    if they change their mind, they `/lgtm cancel`
  - Once a **reviewer** has `/lgtm`'ed, [prow](https://prow.k8s.io)
    ([@k8s-ci-robot](https://github.com/k8s-ci-robot/)) applies an `lgtm` label to the PR
- Phase 2: Humans approve the PR
  - The PR **author** `/assign`'s all suggested **approvers** to the PR, and optionally notifies
    them (eg: "pinging @foo for approval")
  - Only people listed in the relevant OWNERS files, either directly or through an alias, can act
    as **approvers**, including the individual who opened the PR
  - **Approvers** look for holistic acceptance criteria, including dependencies with other features,
    forwards/backwards compatibility, API and flag definitions, etc
  - If the code changes look good to them, an **approver** types `/approve` in a PR comment or
    review; if they change their mind, they `/approve cancel`
  - [prow](https://prow.k8s.io) ([@k8s-ci-robot](https://github.com/k8s-ci-robot/)) updates its
    comment in the PR to indicate which **approvers** still need to approve
  - Once all **approvers** (one from each of the previously identified OWNERS files) have approved,
    [prow](https://prow.k8s.io) ([@k8s-ci-robot](https://github.com/k8s-ci-robot/)) applies an
    `approved` label
- Phase 3: Automation merges the PR:

  - If all of the following are true:

    - All required labels are present (eg: `lgtm`, `approved`)
    - Any blocking labels are missing (eg: there is no `do-not-merge/hold`, `needs-rebase`)

  - And if any of the following are true:

    - there are no presubmit prow jobs configured for this repo
    - there are presubmit prow jobs configured for this repo, and they all pass after automatically
      being re-run one last time

  - Then the PR will automatically be merged

### Quirks of the process

There are a number of behaviors we've observed that while _possible_ are discouraged, as they go
against the intent of this review process. Some of these could be prevented in the future, but this
is the state of today.

- An **approver**'s `/lgtm` is simultaneously interpreted as an `/approve`
  - While a convenient shortcut for some, it can be surprising that the same command is interpreted
    in one of two ways depending on who the commenter is
  - Instead, explicitly write out `/lgtm` and `/approve` to help observers, or save the `/lgtm` for
    a **reviewer**
  - This goes against the idea of having at least two sets of eyes on a PR, and may be a sign that
    there are too few **reviewers** (who aren't also **approver**)
- Technically, anyone who is a member of the Kubeflow GitHub organization can drive-by `/lgtm` a
  PR
  - Drive-by reviews from non-members are encouraged as a way of demonstrating experience and
    intent to become a collaborator or reviewer
  - Drive-by `/lgtm`'s from members may be a sign that our OWNERS files are too small, or that the
    existing **reviewers** are too unresponsive
  - This goes against the idea of specifying **reviewers** in the first place, to ensure that
    **author** is getting actionable feedback from people knowledgeable with the code
- **Reviewers**, and **approvers** are unresponsive
  - This causes a lot of frustration for **authors** who often have little visibility into why their
    PR is being ignored
  - Many **reviewers** and **approvers** are so overloaded by GitHub notifications that @mention'ing
    is unlikely to get a quick response
  - If an **author** `/assign`'s a PR, **reviewers** and **approvers** will be made aware of it on
    their [PR dashboard](https://k8s-gubernator.appspot.com/pr)
  - An **author** can work around this by manually reading the relevant OWNERS files,
    `/unassign`'ing unresponsive individuals, and `/assign`'ing others
  - This is a sign that our OWNERS files are stale; pruning the **reviewers** and **approvers** lists
    would help with this
  - It is the PR **authors** responsibility to drive a PR to resolution. This means if the PR **reviewers** are unresponsive they should escalate as noted below
    - e.g ping **reviewers** in a timely manner to get it reviewed
    - If the **reviewers** don't respond look at the OWNERs file in root and ping **approvers** listed there
- **Authors** are unresponsive
  - This costs a tremendous amount of attention as context for an individual PR is lost over time
  - This hurts the project in general as its general noise level increases over time
  - Instead, close PR's that are untouched after too long (we currently have a bot do this after 90 days)

## Automation using OWNERS files

### [`prow`](https://git.k8s.io/test-infra/prow)

Prow receives events from GitHub, and reacts to them. It is effectively stateless. The following
pieces of prow are used to implement the code review process above.

- [cmd: tide](https://git.k8s.io/test-infra/prow/cmd/tide)
  - per-repo configuration:
    - `labels`: list of labels required to be present for merge (eg: `lgtm`)
    - `missingLabels`: list of labels required to be missing for merge (eg: `do-not-merge/hold`)
    - `reviewApprovedRequired`: defaults to `false`; when true, require that there must be at least
      one [approved pull request review](https://help.github.com/articles/about-pull-request-reviews/)
      present for merge
    - `merge_method`: defaults to `merge`; when `squash` or `rebase`, use that merge method instead
      when clicking a PR's merge button
  - merges PR's once they meet the appropriate criteria as configured above
  - if there are any presubmit prow jobs for the repo the PR is against, they will be re-run one
    final time just prior to merge
- [plugin: assign](https://git.k8s.io/test-infra/prow/plugins/assign)
  - assigns GitHub users in response to `/assign` comments on a PR
  - unassigns GitHub users in response to `/unassign` comments on a PR
- [plugin: approve](https://git.k8s.io/test-infra/prow/plugins/approve)
  - per-repo configuration:
    - `issue_required`: defaults to `false`; when `true`, require that the PR description link to
      an issue, or that at least one **approver** issues a `/approve no-issue`
    - `implicit_self_approve`: defaults to `false`; when `true`, if the PR author is in relevant
      OWNERS files, act as if they have implicitly `/approve`'d
  - adds the `approved` label once an **approver** for each of the required
    OWNERS files has `/approve`'d
  - comments as required OWNERS files are satisfied
  - removes outdated approval status comments
- [plugin: blunderbuss](https://git.k8s.io/test-infra/prow/plugins/blunderbuss)
  - determines **reviewers** and requests their reviews on PR's
- [plugin: lgtm](https://git.k8s.io/test-infra/prow/plugins/lgtm)
  - adds the `lgtm` label when a **reviewer** comments `/lgtm` on a PR
  - the **PR author** may not `/lgtm` their own PR
- [pkg: k8s.io/test-infra/prow/repoowners](https://git.k8s.io/test-infra/prow/repoowners/repoowners.go)
  - parses OWNERS and OWNERS_ALIAS files
  - if the `no_parent_owners` option is encountered, parent owners are excluded from having
    any influence over files adjacent to or underneath of the current OWNERS file

## Maintaining OWNERS files

OWNERS files should be regularly maintained.

We encourage people to self-nominate or self-remove from OWNERS files via PR's. Ideally in the future
we could use metrics-driven automation to assist in this process.

We should strive to:

- grow the number of OWNERS files
- add new people to OWNERS files
- ensure OWNERS files only contain org members and repo collaborators
- ensure OWNERS files only contain people are actively contributing to or reviewing the code they own
- remove inactive people from OWNERS files

OWNERS 使用的坏用例：

- directories that lack OWNERS files, resulting in too many hitting root OWNERS
- OWNERS files that have a single person as both approver and reviewer
- OWNERS files that haven't been touched in over 6 months
- OWNERS files that have non-collaborators present

OWNERS 良好用例：

- there are more `reviewers` than `approvers`
- the `approvers` are not in the `reviewers` section
- OWNERS files that are regularly updated (at least once per release)
