

```yaml
version: 0.2
phases:
  install:
    commands:
      - echo Entering the install phase...
      - gem install cfn-nag
  build:
    commands:
      - echo Entering the build phase...
      - cfn_nag_scan --input-path $CODEBUILD_SRC_DIR/cfn
```

现在你需要将 `buildspec.yml` 提交到 CodeCommit 仓库，以便 CodeBuild 仓库能够使用它。

执行此提交后，请访问 AWS 管理控制台上的 CodePipeline 服务。你应该会看到管道已开始执行（如 [图 3-4](#ch03.xhtml#fig_4_pipeline_running_after_a_new_file_is_committed_to) 所示）。

![](assets/securing_your_infrastructure_046357_04.png)

管道的第一阶段连接到 CodeCommit 仓库。管道将从 CodeCommit 仓库拉取变更并传递给下一个阶段，即 CodeBuild 项目。

构建过程完成后，管道将失败。这完全正常且在预料之中。当存在误配置资源（如你刚提交的安全组）时，该管道 *应该* 失败。但是，你如何知道管道失败正是 *因为这个原因* ，而不是其他原因呢？

这是一个深入管道日志查看发生了什么错误、何时何地发生的好时机。在 AWS 控制台中，进入 CodePipeline 并点击你刚创建的管道。“TestStage” 应显示为 “failed（失败）”。点击 TestStage 框内的 “Details（详情）” 链接（见 [图 3-5](#ch03.xhtml#fig_5_opening_the_error_in_codepipeline)）。

![](assets/securing_your_infrastructure_046357_05.png)

此链接将带你进入最后一次运行的执行日志，以便查看管道各阶段运行时执行了什么。它应该类似于下面的片段：

```
[Container] 2022/03/23 23:36:50 Running command cfn_nag_scan --input-path $CODEBUILD_SRC_DIR
------------------------------------------------------------
/codebuild/output/src699107553/src/secgroup.yml
------------------------------------------------------------------------------------------------------------------------
| WARN W28
|
| Resource: ["MisconfiguredSecGroup"]
| Line Numbers: [9]
|
| Resource found with an explicit name, this disallows updates that require replacement of this resource
------------------------------------------------------------
| WARN W9
|
| Resource: ["MisconfiguredSecGroup"]
| Line Numbers: [9]
|
| Security Groups found with ingress cidr that is not /32
------------------------------------------------------------
| WARN W2
|
| Resource: ["MisconfiguredSecGroup"]
| Line Numbers: [9]
|
| Security Groups found with cidr open to world on ingress.  This should never be true on instance.  Permissible on ELB
------------------------------------------------------------
| FAIL F1000
|
| Resource: ["MisconfiguredSecGroup"]
| Line Numbers: [9]
|
| Missing egress rule means all traffic is allowed outbound.  Make this explicit if it is desired configuration
------------------------------------------------------------
| WARN W36
|
| Resource: ["MisconfiguredSecGroup"]
| Line Numbers: [9]
|
| Security group rules without a description obscure their purpose and may lead to bad practices in ensuring they only allow traffic from the ports and sources/destinations required.
Failures count: 1
Warnings count: 4
[Container] 2022/03/23 23:36:50 Command did not exit successfully cfn_nag_scan --input-path $CODEBUILD_SRC_DIR exit status 2
```

正如日志所示，以 `WARN` 和 `FAIL` 开头的部分是你的 CloudFormation 文件中的误配置。失败是因为 `secgroup.yml` 文件存在多项安全违规。该安全组有以下两项警告和一项错误：

*   `WARN W2`：IP 范围对全球开放
*   `FAIL F1000`：缺少出站规则
*   `WARN W36`：安全组规则缺少描述

## 总结

在本章中，你创建了一个 CodeCommit 仓库、一个 CodePipeline 和一个 CodeBuild 项目。当你向 CodeCommit 仓库提交代码时代码提交时，CodePipeline 就会被触发。CodePipeline 将包含 `cfn-nag`（一种 CloudFormation 检查器）的 CodeBuild 项目作为一个阶段。

你了解了三种协同保护环境的安全控制类型，并且了解到在软件开发生命周期中尽早添加预防性检查有助于防止安全事件。你还了解到，如果你控制着代码部署的入口点，你就可以控制是否将特定的 IaC 文件部署到你的环境中。

理解日志和错误有助于减少 Bug 并实现成功的构建。像 `cfn-nag` 这样的扫描器能让你将误配置隔离为代码中的特定行，从而在部署前修复它们。

Automatoonz 团队开局良好。他们（以及你）已经创建了一个用于进行 IaC 检查并防止安全误配置在生命周期早期出现的初始管道。在下一章中，我们将向你展示如何通过添加日志记录和监控功能来进一步优化该管道，以检测和防止篡改。

本章要点快速回顾：

*   开发人员应具备三种安全控制来保障 IaC 安全：预防性、检测性和纠正性。
*   设置 CodePipeline 时，代码提交会触发预防性检查。
*   开源检查工具 `cfn-nag` 会查找代码中常见的安全误配置。

# 第 4 章 通过自动化控制访问

在本章中，我们将重点关注身份。具体而言，我们将讨论如何通过自动化实现身份验证和授权。你将了解可用于防止和检�环境中与身份和访问管理 (IAM) 相关误配置工具。

作为提醒，*身份验证* 是你证明自己是谁、验证身份的方式；*授权* 是授予用户执行某项操作的权限。

让我们先思考一下你目前所处的工作环境（如果适用）。你知道有多少账户过度授权吗？你实际使用了多少权限？

有两种类型的身份：人和机器。*人工身份* 是人类用于日常、非关键或一次性操作的账户。具有 *机器身份* 的账户则由机器用于自动化或执行某些特权操作。在理想世界中，人工身份仅用于读取由机器身份维护的数据。所有操作都应尽可能通过机器身份实现自动化。

在我们的案例研究中，Automatoonz DevSecOps 团队成员需要为其基础设施的用户（人类）以及基础设施本身（机器）提供身份。机器身份将允许诸如 EC2 或 S3 等服务相互通信。如果 AWS 内的基础设施没有正确的权限，你将始终收到 “Permission Denied（拒绝访问）” 错误。

我们将通过考察最小权限原则如何对你的信息安全计划至关重要来开始本章。然后我们将转向微调权限，介绍如何利用资源标签和职责分离来扩展 IAM 权限。我们还将讨论为什么在 IAM 安全控制方面，预防与检测同样重要。最后，我们将创建一个管道，用于检测不符合安全最佳实践的 IAM 策略。

### 注意

本书假设你具备一定的 AWS �础知识，因此我们不会深入讲解 AWS IAM 的基础知识。如果你需要复习 IAM，请访问 [AWS 文档](https://docs.aws.amazon.com/IAM/latest/UserGuide/intro-structure.xhtml)。

## 最小权限原则

