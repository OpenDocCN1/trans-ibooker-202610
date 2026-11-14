
感谢 [r/aws](https://www.reddit.com/r/aws) 知识渊博的社区，始终提供宝贵的见解和意见。

感谢我们的制作编辑 Christopher Faucher，让本书以最佳状态发布。也感谢我们的编辑 Virginia Wilson，在疫情期间抽出时间与初次写作的作者合作。您的耐心、建议和指导让我们得以完成本书，并保持了（某种程度上的）理智 :-)

# 第 1 章 安全

## 1.0 简介

根据 [IBM/Ponemon Institute Report](https://oreil.ly/YayAP) 报告，2021 年数据泄露的平均成本达到 424 万美元的新高。当您选择在云端运行应用程序时，您信任 AWS 提供运行云服务的安全基础设施，以便您专注于自己的创新和增值活动。

但云安全是您与 AWS 之间的 [共同责任](https://oreil.ly/4Sv24)。您负责配置 AWS Identity and Access Management (IAM) 策略、Amazon EC2 安全组和基于主机的防火墙等。换句话说，构成 AWS 云的硬件和软件平台的安全*属于* AWS 的责任。您在 AWS 账户中实施的软件和配置的安全*属于* 您的责任。

当您在 AWS 中部署云资源并应用配置时，了解维护安全环境所需的安全设置至关重要。本章的配方包含专注于安全的最佳实践和用例。由于安全无处不在，您将结合本书中的其他配方和章节使用这些配方。例如，您将在全书中看到使用 AWS Systems Manager Session Manager 连接到 EC2 实例。这些基础安全配方将为您提供在 AWS 上构建安全解决方案所需的工具。

除了本章内容外，还有许多优秀的资源可供您深入了解 AWS 上的安全主题。[“AWS 云安全基础”](https://oreil.ly/7rDIX) 在 2019 年 AWS 安全大会 re:Inforce 上演讲，提供了很好的概述。来自 AWS re:Invent 的更高级演讲 [“加密：它是最好的控制，也是最坏的控制”](https://oreil.ly/qnO16) 详细探讨了加密场景。

**提示**

AWS 发布了用于保护账户的 [最佳实践指南](https://oreil.ly/BZQRt)，所有 AWS 账户持有者都应熟悉这些最佳实践，因为它们在不断发展。

**警告**

本章涵盖重要的安全主题。由于 AWS 安全相关的服务和配置列表持续增长和演变，无法涵盖所有主题。AWS 保持其 [安全、身份和合规最佳实践网页](https://oreil.ly/Us5oz) 为最新。

## 工作站配置

您需要安装一些工具，以准备好本章的配方。

### 通用设置

按照 [“CLI 配方的通用工作站设置步骤”](#preface01.html#general_workstation_setup_steps_for_cli) 验证您的配置并设置所需的环境变量。然后，克隆章节代码仓库：

```
git clone https://github.com/AWSCookbook/Security
```

## 1.1 创建并假设用于开发人员访问的 IAM 角色

### 问题

为确保您不总是使用管理权限，您需要在 AWS 账户中创建一个用于开发的 IAM 角色。

### 解决方案

创建一个使用 IAM 策略的角色，该策略将允许稍后假设该角色。将 AWS 托管的 `PowerUserAccess` IAM 策略附加到该角色（请参阅 [图 1-1](#ch01.html#create_rolecomma_attach_policycomma_ass)）。

![](assets/awsc_0101.png)

#### 步骤

1.  创建一个名为 `assume-role-policy-template.json` 的文件，内容如下。这将允许 IAM 主体假设您接下来将创建的角色（仓库中提供了该文件）：

    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Principal": {
            "AWS": "PRINCIPAL_ARN"
          },
          "Action": "sts:AssumeRole"
        }
      ]
    }
    ```

    **提示**

    如果您使用 IAM 用户，并且删除了并重新创建了该 IAM 用户，由于 IAM 服务缓解权限提升风险的方式，此策略将不再继续生效。有关更多信息，请参阅 IAM 文档中的 [相关说明](https://oreil.ly/wR02t)。

2.  检索您用户的 [ARN](https://oreil.ly/BDayp) 并将其设置为变量：

    ```
    PRINCIPAL_ARN=$(aws sts get-caller-identity --query Arn --output text)
    ```

3.  使用 `sed` 命令替换 `assume-role-policy-template.json` 文件中的 `PRINCIPAL_ARN` 并生成 `assume-role-policy.json` 文件：

    ```
    sed -e "s|PRINCIPAL_ARN|${PRINCIPAL_ARN}|g" \
         assume-role-policy-template.json > assume-role-policy.json
    ```

4.  创建角色并指定假设角色策略文件：

    ```
    ROLE_ARN=$(aws iam create-role --role-name AWSCookbook101Role \
         --assume-role-policy-document file://assume-role-policy.json \
         --output text --query Role.Arn)
    ```

5.  将 AWS 托管的 `PowerUserAccess` 策略附加到角色：

    ```
    aws iam attach-role-policy --role-name AWSCookbook101Role \
         --policy-arn arn:aws:iam::aws:policy/PowerUserAccess
    ```

    **注意**

    AWS 提供了 [常见工作职能的访问策略](https://oreil.ly/iyKFx) 供您参考。这些策略可能是您为特定工作职能委派用户访问账户的良好起点；但是，最好为您的每个访问需求定义最小权限策略。

#### 验证检查

假设角色：

```
aws sts assume-role --role-arn $ROLE_ARN \
     --role-session-name AWSCookbook101
```

您应该会看到类似以下的输出：

```json
{
  "Credentials": {
    "AccessKeyId": "<snip>",
    "SecretAccessKey": "<snip>",
    "SessionToken": "<snip>",
    "Expiration": "2021-09-12T23:34:56+00:00"
  },
  "AssumedRoleUser": {
    "AssumedRoleId": "EXAMPLE:AWSCookbook101",
    "Arn": "arn:aws:sts::11111111111:assumed-role/AWSCookbook101Role/AWSCookbook101"
  }
}
```

**提示**

`AssumeRole` API 会从 `AWS Security Token Service` (STS) 向调用者返回一组用于角色会话的临时凭证，前提是该角色的 `AssumeRole` 策略中的权限允许。所有 IAM 角色都有关联的 `AssumeRole` 策略。您可以使用此输出 [为 AWS CLI 配置凭证](https://oreil.ly/F7ril)；将 `AccessKey`、`SecretAccessKey` 和 `SessionToken` 设置为环境变量；还可以在 AWS 控制台中使用 `Switch Role` 功能来假设角色。当您的应用程序需要进行 AWS API 调用时，您所选编程语言的 AWS SDK 会为它们处理此过程。

### 清理

按照 [章节代码仓库](https://github.com/AWSCookbook/Security) 中此配方文件夹中的步骤操作。

### 讨论
