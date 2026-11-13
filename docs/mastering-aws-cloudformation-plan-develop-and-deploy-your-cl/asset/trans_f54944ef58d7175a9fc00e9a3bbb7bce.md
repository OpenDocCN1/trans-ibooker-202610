
## 使用 cfn-init 进行引导

### 使用 cfn-signal 报告 EC2 资源就绪状态

为了报告 EC2 资源就绪状态，有一个名为 `cfn-signal` 的辅助脚本。正如其名称所示，该脚本向 CloudFormation 发送信号消息，报告资源是已创建还是创建失败。

让我们在 LNMP 栈中尝试一下。我们需要添加两个额外的代码块：

1.  我们首先向 EC2 实例添加 `CreationPolicy`：

    ```yaml
    # lnmp-signal.yaml
    Lnmp:
      Type: AWS::EC2::Instance
      CreationPolicy:
        ResourceSignal:
          Count: 1
          Timeout: PT5M
      Properties: #...
      Metadata: #...
    ```

    对于单个实例，`CreationPolicy` 没有太多属性——只有 `Count` 和 `Timeout`。

    `Timeout` 使用 ISO8601 ([https://www.iso.org/iso-8601-date-and-time-format.html](https://www.iso.org/iso-8601-date-and-time-format.html)) 持续时间格式声明，在前面的示例中，这是五分钟。如果 CloudFormation 没有从实例收到任何信号，它将因超时报告失败并回滚。

    但这只是 CloudFormation 中的声明。我们还需要指示实例报告就绪状态。

2.  `cfn-signal` 已预装在机器上，因此我们需要从 `UserData` 中调用它：

    ```yaml
    # lnmp-signal.yaml
    Lnmp:
      Type: AWS::EC2::Instance
      CreationPolicy: #...
      Properties:
        ImageId: !Ref ImageId
        InstanceType: t2.micro
        KeyName: !Ref KeyName
        UserData:
          Fn::Base64:
            Fn::Sub: |
              #!/bin/bash -xe
              /opt/aws/bin/cfn-init -v \
              --stack ${AWS::StackName} \
              --resource Lnmp \
              --configsets Configure \
              --region ${AWS::Region}
              # 发送创建信号
              /opt/aws/bin/cfn-signal -e $? \
              --stack ${AWS::StackName} \
              --resource Lnmp \
              --region ${AWS::Region}
    ```

    在这里，我们向 CloudFormation 提供一个退出代码（`-e` 参数），其值等于 `cfn-init` 的退出代码。如果 `cfn-init` 失败（退出代码不为 0），则 `cfn-signal` 将向 CloudFormation 发送不同的值，CloudFormation 将知道：出错了，我应该回滚。

`cfn-signal` 和 `CreationPolicy` 广泛用于 Auto Scaling 组，因为我们还可以指定 `MinSuccessfulInstancesPercent`，从而执行滚动更新。

请看以下示例：

```yaml
MyAsg:
  Type: AWS::AutoScaling::AutoScalingGroup
  CreationPolicy:
    AutoScalingCreationPolicy:
      MinSuccessfulInstancePercent: 50
```

此声明将指示 Auto Scaling 组仅在至少一半的组成员发送成功信号时确认其就绪状态；否则，Auto Scaling 组将报告创建失败。

### 总结

在本章中，我们学习了如何以更人性化的方式执行初始引导配置。我们了解了 `cfn-init`，这是 CloudFormation 原生的配置管理工具。我们用它部署了一个 Hello World 应用程序，甚至启动了我们自己的 LNMP 栈，并指示它向我们报告其创建状态。

使用 `cfn-init` 允许模板开发者将 CloudFormation 作为一个中心化工具，既用于预置资源和底层配置，也用于部署它们。`cfn-init` 还可以作为栈操作的一部分，用于安装和更新软件。

这是*第 2 部分、大规模预置与部署*的最后一章，我们在这里做了大量工作。我们现在能够使用 CloudFormation 构建生产就绪的基础设施，可以测试和验证我们的栈，可以应用持续交付，并可以使用 StackSets 在多个区域和账户中部署我们的栈。

我们主要利用核心 CloudFormation 功能完成了大量工作，但 CloudFormation 还能通过自定义资源和模板宏等强大工具进行扩展。在接下来的几章中，我们将学习如何使用这些工具。

此外，我们还将深入探讨 CloudFormation 的潜在继任者：无服务器应用程序模型 (SAM) 和 AWS CDK——云开发工具包。

在下一章中，我们将研究自定义资源，这是 CloudFormation 的一项扩展，允许我们声明和创建 AWS *之外* 的资源。我们将了解自定义资源以及如何以及何时使用它们。

敬请期待！

### 问题

1.  如果配置集包含多个配置项，它们按什么顺序执行？
2.  是否可以有多个配置集？
3.  `cfn-init` 是否需要 IAM 策略来访问资源元数据？
4.  是否可以使用 `cfn-init` 创建目录？
5.  什么是 `WaitCondition`？

### 延伸阅读

*   EC2 上的 LAMP 栈：[https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/deploying.applications.html#deployment-walkthrough-basic-server](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/deploying.applications.html#deployment-walkthrough-basic-server)
*   在 CloudFormation 栈上部署应用程序的最佳实践：[https://aws.amazon.com/blogs/devops/best-practices-for-deploying-applications-on-aws-cloudformation-stacks/](https://aws.amazon.com/blogs/devops/best-practices-for-deploying-applications-on-aws-cloudformation-stacks/)

# 第 3 部分：扩展 CloudFormation

CloudFormation 有其局限性。在最后这一部分，我们将教会它不仅仅在 AWS 中预置资源。我们将从自定义资源和模板宏开始，作为 CloudFormation 定制的第一步。完成这些后，我们将简要涉及其可能的继任者 AWS CDK，以及一种名为 SAM 的预置无服务器应用程序的新方式。

本部分包含以下章节：

*   [第 7 章](#B10533_07_Final_NM_ePUB.xhtml#_idTextAnchor132)，使用自定义资源在 AWS 之外创建资源
*   [第 8 章](#B10533_08_Final_NM_ePUB.xhtml#_idTextAnchor148)，使用模板宏动态渲染模板
*   [第 9 章](#B10533_09_Final_NM_ePUB.xhtml#_idTextAnchor165)，使用 AWS CDK 生成 CloudFormation 模板
*   [第 10 章](#B10533_10_Final_NM_ePUB.xhtml#_idTextAnchor182)，使用 AWS SAM 部署无服务器应用程序
*   [第 11 章](#B10533_11_Final_NM_ePUB.xhtml#_idTextAnchor197)，接下来是什么？

## 第 7 章：使用自定义资源在 AWS 之外创建资源

CloudFormation 通常是第一个开始支持新 AWS 服务的基础设施即代码工具；然而，有时我们需要与不支持 CloudFormation 的服务通信。有时我们甚至需要在 AWS 之外创建资源。

CloudFormation 最近开始支持外部资源提供商。类似于 Terraform 的提供商，您可以开发自己的提供商来管理不同云和管理服务提供商的基础设施。

由于这是一个全新功能（于 2019 年 11 月发布），它需要一段时间才能成熟。

在本章中，我们将使用自定义资源创建外部资源——这是在 AWS 之外创建您自己的资源的简便方法。

我们将了解它们是什么及其用例，并使用 AWS Lambda 编写我们的第一个自定义资源。

在本章中，我们将涵盖以下主题：

*   理解自定义资源
*   底层 Lambda 函数的内部原理
*   编写和管理您自己的自定义资源
*   处理自定义资源的更新、删除和失败

### 技术要求

本章使用的代码可以在本书的 GitHub 存储库中找到：[https://github.com/PacktPublishing/Mastering-AWS-CloudFormation/tree/master/Chapter7](https://github.com/PacktPublishing/Mastering-AWS-CloudFormation/tree/master/Chapter7)

查看以下视频以了解代码实战：

[https://bit.ly/3bTWq4q](https://bit.ly/3bTWq4q)

### 理解自定义资源
