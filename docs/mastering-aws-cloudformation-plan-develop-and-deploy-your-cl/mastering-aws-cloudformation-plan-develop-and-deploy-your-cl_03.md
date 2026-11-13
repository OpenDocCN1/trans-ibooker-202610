## 第 1 章：CloudFormation 复习

云计算引入了一种全新的基础设施管理方式。




### 理解 AWS CloudFormation

### 简介

随着对 AWS **云** 需求的增长，常规的例行和操作任务变得麻烦起来。AWS 云允许任何类型的企业快速发展并解决所有关于计算能力的业务需求；然而，维护特定的资源堆栈却很困难。

DevOps 文化带来了一套方法论和工作方式，其中之一被称为 **基础设施即代码**。此过程是指将您的基础设施——网络、虚拟机、存储、数据库等——视为计算机程序来对待。

AWS **CloudFormation** 的开发旨在解决此类问题。

您可能已经具备一些 CloudFormation 的实操知识，但在我们深入学习高级模板开发、如何大规模预配、将 CloudFormation 与 CI/CD 流水线结合使用以及扩展其功能之前，让我们快速复习一下，再次了解什么是 CloudFormation 以及我们如何使用它。

在本章中，我们将学习以下内容：

*   AWS CloudFormation 的内部原理
*   创建和更新 CloudFormation 堆栈
*   管理 CloudFormation 的权限
*   检测堆栈中的非托管更改

### 技术要求

本章使用的代码可在本书的 GitHub 存储库中找到，地址为 `https://github.com/PacktPublishing/Mastering-AWS-CloudFormation/tree/master/Chapter1`。

查看以下视频以了解代码实战：

[`bit.ly/2WbU5Lh`](https://bit.ly/2WbU5Lh)

### 理解 AWS CloudFormation 的内部原理

AWS 服务由三部分组成：

*   API
*   后端
*   存储

我们通过调用其 API 服务与 AWS 交互。如果我们想创建一个 EC2 实例，则需要执行调用 `ec2:RunInstances`。

当我们开发模板并创建堆栈时，我们会调用 `cloudformation:CreateStack` API 方法。AWS CloudFormation 将接收命令及模板，对其进行验证，并开始创建资源，根据我们的声明向各种 AWS 服务发出 API 调用。

如果任何资源的创建失败，CloudFormation 将回滚更改并删除故障前创建的资源。但如果创建过程中没有错误，我们将看到我们的资源已在账户中完成预配。

如果我们想更改堆栈，只需更新模板文件并调用 `cloudformation:UpdateStack` API 方法。CloudFormation 将仅更新已更改的资源。如果更新过程失败，CloudFormation 将回滚更改并将堆栈恢复到之前的健康状态。

既然我们已了解这些，让我们开始创建堆栈。

### 创建您的第一个堆栈

我相信您以前做过这个。

我们首先从开发模板开始。这将是一个简单的 S3 存储桶。我将使用 **YAML** 模板格式，但您如果愿意也可以使用 **JSON** 格式：

`MyBucket.yaml`

```yaml
AWSTemplateFormatVersion: "2010-09-09"

Description: This is my first bucket

Resources:

  MyBucket:

    Type: AWS::S3::Bucket
```

现在我们只需使用 `awscli` 创建堆栈：

```bash
$ aws cloudformation create-stack \
    --stack-name mybucket \
    --template-body file://MyBucket.yaml
```

过一会儿，如果我们去 AWS 控制台或运行 `aws s3 ls`，将会看到我们的存储桶已创建。

现在让我们为存储桶添加一些公共访问权限：

`MyBucket.yaml`

```yaml
AWSTemplateFormatVersion: "2010-09-09"

Description: This is my first bucket

Resources:

  MyBucket:

    Type: AWS::S3::Bucket

    Properties:

      AccessControl: PublicRead
```

让我们运行 `update` 操作：

```bash
$ aws cloudformation update-stack \
    --stack-name mybucket \
    --template-body file://MyBucket.yaml
```

要清理工作区，只需使用以下命令删除堆栈：

```bash
$ aws cloudformation delete-stack --stack-name mybucket
```

现在让我们看看 CloudFormation IAM 权限。

### 理解 CloudFormation IAM 权限

我们已经知道，当我们创建或更新堆栈时，CloudFormation 会执行 API 调用。现在的问题是，CloudFormation 是否拥有与 `root` 用户相同的权限？

当您使用生产级 AWS 账户时，您需要控制人类（您自己和同事）和机器（构建系统、AWS 资源等）对环境的访问。这就是为什么控制 CloudFormation 的访问权限很重要。

默认情况下，当用户运行堆栈创建时，他们会调用 API 方法 `cloudformation:CreateStack`。CloudFormation 将在堆栈创建期间使用该用户的访问权限来调用其他 API 方法。

这意味着，如果我们的用户拥有允许操作 `ec2:*` 的 IAM 策略，但尝试使用 CloudFormation 创建 RDS 实例，堆栈将无法创建并报错，**User is unauthorized to perform this action**（用户未获授权执行此操作）。

让我们试一下。我们将创建一个具有 `ec2:*` 权限的 IAM 角色，假设该角色，并尝试创建相同的 `mybucket` 堆栈：

> **重要提示**
>
> 我们的 AWS 账户中已经有一个 IAM 用户 `Admin`，我们将把该用户添加为委托人。

`MyIamRole.yaml`

```yaml
AWSTemplateFormatVersion: "2010-09-09"

Description: "This is a dummy role"

Resources:

  IamRole:

    Type: AWS::IAM::Role

    Properties:

      AssumeRolePolicyDocument:

        Version: 2012-10-17

        Statement:

          - Sid: AllowAssumeRole

            Effect: Allow

            Principal:

              AWS:

                - !Join
                    - ""
                    - - "arn:aws:iam::"
                      - !Ref "AWS::AccountId"
                      - ":user/Admin"

            Action: "sts:AssumeRole"

      ManagedPolicyArns:

        - "arn:aws:iam::aws:policy/AmazonEC2FullAccess"
        - "arn:aws:iam::aws:policy/AWSCloudformationFullAccess"

Outputs:

  IamRole:

    Value: !GetAtt IamRole.Arn
```

如果我们创建此堆栈，假设该角色，并尝试创建之前的 `mybucket` 堆栈，它将无法创建并报错。让我们看一下：

```bash
$ aws cloudformation create-stack \
    --stack-name iamrole \
    --capabilities CAPABILITY_IAM \
    --template-body file://IamRole.yaml

$ IAM_ROLE_ARN=$(aws cloudformation describe-stacks \
    --stack-name iamrole \
    --query "Stacks[0].Outputs[?OutputKey=='IamRole'].OutputValue" \
    --output text)

$ aws sts assume-role --role-arn $IAM_ROLE_ARN \
    --role-session-name tmp

### Here goes the output of the command. I will store the access credentials in the env vars

$ export AWS_ACCESS_KEY_ID=…
$ export AWS_SECRET_ACCESS_KEY=…
$ export AWS_SESSION_TOKEN=…

$ aws cloudformation create-stack \
    --stack-name mybucket \
    --template-body file://MyBucket.yaml
```

我们将在 AWS 控制台上看到以下错误：

![图 1.1 – CloudFormation 控制台 – 堆栈事件](img/B10533_01_001.jpg)

图 1.1 – CloudFormation 控制台 – 堆栈事件

另一方面，我们不能为每个人提供 `AdminAccess` 策略，因此我们需要找到一种方法，在仅允许 CloudFormation 使用这些权限的前提下，结合必要权限使用 CloudFormation。

CloudFormation 支持 **服务角色**。服务角色是由不同 AWS 服务（如 EC2、ECS、Lambda 等）使用的 IAM 角色。CloudFormation 服务角色在堆栈和 **StackSets** 操作（创建、更新和删除）期间由 CloudFormation 使用：

1.  让我们为 CloudFormation 创建一个特定角色：

    `CfnIamRole.yaml`

    ```yaml
    AWSTemplateFormatVersion: "2010-09-09"

    Description: "This is a CFN role"

    Resources:

      IamRole:
    ```




## 第 1 章：CloudFormation 复习

### 为 CloudFormation 创建服务角色

```yaml
Type: AWS::IAM::Role

Properties:

  AssumeRolePolicyDocument:

    Version: 2012-10-17

    Statement:

      - Sid: AllowAssumeRole

        Effect: Allow

        Principal:

          Service: "cloudformation.amazonaws.com"

        Action: "sts:AssumeRole"

  ManagedPolicyArns:

    - "arn:aws:iam::aws:policy/AdministratorAccess"

Outputs:

  IamRole:

    Value: !GetAtt IamRole.Arn
```

2. 我们为服务角色创建此堆栈并获取 CloudFormation 角色 ARN：

```bash
$ aws cloudformation create-stack \
    --stack-name cfniamrole \
    --capabilities CAPABILITY_IAM \
    --template-body file://CfnIamRole.yaml

$ IAM_ROLE_ARN=$(aws cloudformation describe-stacks \
    --stack-name cfniamrole \
    --query "Stacks[0].Outputs[?OutputKey=='IamRole'].OutputValue" \
    --output text)
```

3. 现在我们运行堆栈的创建，它将使用我们的角色，指定 `Role ARN`：

```bash
$ aws cloudformation create-stack \
    --stack-name mybucket \
    --template-body file://MyBucket.yaml \
    --role-arn $IAM_ROLE_ARN
```

4. 过了一会儿，我们可以验证我们的堆栈已创建，并且看到我们的存储桶！

```bash
$ aws s3 ls
```

```text
### Output
2019-10-16 14:14:24 mybucket-mybucket-jqjpr6wmz19q
```

在继续之前，别忘了清理您的账户：

```bash
$ for i in mybucket iamrole cfniamrole; do aws cloudformation delete-stack --stack-name $i ; done
```

### 重要提示

请注意，在前面的示例中，我们为 CloudFormation 角色提供了一个 `AdminPolicy`（AWS 默认提供的那个）。

在生产级系统中，我们希望仅授予 CloudFormation 堆栈所需的特权。

有两种权限模式应用于 CloudFormation 角色：

* 我们有一份可使用的服务清单（例如 `EC2`、`RDS`、`VPC`、`DynamoDB`、`S3` 等）。
* 每个模板/堆栈组合将仅使用它所需的服务——例如，如果我们声明带有 `Simple Notification Service` (`SNS`) 的 Lambda 函数，则应创建仅包含 `Lambda` 和 `SNS` 策略的角色。

### 偏差检测

CloudFormation 作为一项服务，经常提到 `state`（状态）一词。状态基本上是包含一对值的清单信息：逻辑资源名称和物理资源 ID。

CloudFormation 使用其状态来了解要创建或更新哪些资源。如果我们创建一个包含逻辑名称为 `foo` 的资源的堆栈，在模板中更改此资源 (`foo`) 的属性，然后运行更新，CloudFormation 将会更改账户中相应的物理资源。

CloudFormation 有一组限制。例如，如果我们不引入更改，它不会更新堆栈。如果我们对资源执行手动更改，CloudFormation 仅在我们更改模板时才会更改它们。

开发人员一旦开始使用 CloudFormation，就不得不重新思考他们的基础设施管理方式，但我们将在后续章节讨论这一点。目前，我们想向您展示一个功能，它虽然不能解决手动干预的问题，但至少能在发生时通知我们。此功能称为偏差检测。

对于此示例，我们将使用与上一节相同的模板（虚拟 IAM 角色）：

```bash
$ aws cloudformation create-stack \
    --stack-name iamrole \
    --template-body file://IamRole.yaml \
    --capabilities CAPABILITY_IAM
```

过了一会儿，我们看到堆栈已创建：

![图 1.2 – CloudFormation 控制台](img/B10533_01_002.jpg)

图 1.2 – CloudFormation 控制台

注意右侧名为 `Drifts`（偏差）的链接。如果我们点击该链接，将看到 Drifts 菜单，下面显示 `Drift status: NOT_CHECKED`（偏差状态：未检查）。在撰写本文时，我们必须手动运行偏差检测，因此我们需要在堆栈上运行偏差检测。过了一会儿，我们将看到一切正常：

1. 我将运行 `Detect stack drifts`（检测堆栈偏差）并验证我的堆栈是否合规：

   ![图 1.3 – CloudFormation 控制台 – 偏差](img/B10533_01_003.jpg)

   图 1.3 – CloudFormation 控制台 – 偏差

2. 现在我们要做的是给角色添加一个额外的策略，然后重新运行偏差检测：

```bash
$ ROLENAME=$(aws cloudformation describe-stack-resources --stack-name iamrole --query "StackResources[0].PhysicalResourceId" --output text)
$ aws iam attach-role-policy --role-name $ROLENAME --policy-arn "arn:aws:iam::aws:policy/AdministratorAccess"
```

3. 我们现在可以再次检测 `drift`（偏差）：

   ![图 1.4 – CloudFormation 控制台 – 检测到偏差](img/B10533_01_004.jpg)

   图 1.4 – CloudFormation 控制台 – 检测到偏差

4. 如果我们检查 `IamRole` 资源并点击 `View drift details`（查看偏差详情），我们将看到具体更改了什么，以及它与 CloudFormation 状态有何不同：

   ![图 1.5 – CloudFormation 控制台 – 实际修改](img/B10533_01_005.jpg)

   图 1.5 – CloudFormation 控制台 – 实际修改

现在我们有两个选项：要么手动将资源的更改回滚，要么向模板添加任何虚拟属性并运行 `update-stack`。

我们已经了解了 CloudFormation 偏差、如何运行其偏差检测，以及之后必须采取的行动。但别担心——我们将在后续章节中再次探讨偏差。

### 总结

在本复习章节中，我们回顾了什么是 CloudFormation，如何创建和更新堆栈，为什么服务角色很重要，以及如何实现它们。我们还回顾了 CloudFormation 中的偏差是什么，何时发生，以及如何检测它们。

虽然这是一个介绍性章节，但我们涵盖了 CloudFormation 的基本构建块。在后续章节中，我们将再次使用服务角色和偏差检测，但首先，我们需要深入了解 CloudFormation 模板的内部结构，这将在下一章中进行。

### 问题

1. 创建 CloudFormation 堆栈时会调用哪个 API 方法？
2. 什么是 CloudFormation 服务角色？
3. 如果我们不指定 CloudFormation 服务角色，将使用哪些 IAM 策略？
4. 堆栈资源信息在 CloudFormation 中是如何存储的？
5. 如果我们删除 CloudFormation 创建的资源并尝试创建相同的堆栈，会发生什么？
6. 如果我们删除 CloudFormation 创建的资源并尝试 `update`（更新）相同的堆栈，会发生什么？
7. 为什么 CloudFormation 无法重新创建已删除的资源？

### 延伸阅读

* CloudFormation 服务角色：<https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-iam-servicerole.html>
* CloudFormation 中的偏差检测：<https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-stack-drift.html>
