
你应该从安全角度使用 CloudTrail，但也可以从应用程序调试的角度使用它。假设你有一个事件驱动的应用程序，当你向 `S3` 存储桶上传文件时会触发 `Lambda` 函数（请参阅 [Recipe 5.7](#ch05.html#automating_csv_import_into_dynamodb_fro)）。如果 `Lambda` 函数调用的 `IAM` 策略不正确，你将能在 `CloudTrail` 日志中看到 `Deny`（拒绝）。这对构建和设计由 Amazon `EventBridge` 驱动的事件驱动应用程序的应用程序开发人员和架构师很有帮助。

#### 挑战

如果你拥有包含多个账户的 AWS Organization，请配置一个[组织跟踪](https://oreil.ly/4qAbK)。（请参阅 [Recipe 9.6](#ch09.html#setting_up_aws_organizations_and_aws_si) 以设置 AWS Organization。）

# 9.4 设置根用户登录的电子邮件告警

## 问题

当根用户登录 AWS 账户时，你希望收到电子邮件通知。

## 解决方案

创建一个 `SNS` 主题并订阅它。然后创建一个 Amazon `EventBridge` 规则，该规则包含一个用于搜索根用户登录并触发 `SNS` 主题的模式，如 [图 9-13](#ch09.html#logging_and_alerting_for_root_logins) 所示。

![](assets/awsc_0913.png)

### 先决条件

*   启用了 `CloudTrail` 的 AWS 账户（请参阅 [Recipe 9.3](#ch09.html#enabling_cloudtrail_logging_for_your_aw)）

### 步骤

1.  创建一个 `SNS` 主题：

    ```
    TOPIC_ARN=$(aws sns create-topic \
         --name root-login-notify-topic \
         --output text --query TopicArn)
    ```

2.  订阅 `SNS` 主题。这会向你指定的地址发送一封确认邮件：

    ```
    aws sns subscribe \
         --topic-arn $TOPIC_ARN \
         --protocol email \
         --notification-endpoint your-email@example.com
    ```

3.  在收件箱中找到 AWS 发送的确认邮件，然后点击“确认订阅”。

在本章仓库的根目录下，`cd` 到 `904-Setting-Up-Email-Alerts-for-Root-Login` 目录并执行后续步骤。

4.  现在创建一个名为 `assume-role-policy.json` 的假设角色策略 JSON 语句，以便在下一步中使用（该文件已在仓库中提供）：

    ```
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Principal": {
            "Service": "events.amazonaws.com"
          },
          "Action": "sts:AssumeRole"
        }
      ]
    }
    ```

5.  创建角色并指定 `assume-role-policy.json` 文件：

    ```
    aws iam create-role --role-name AWSCookbook904RuleRole \
         --assume-role-policy-document \
         file://assume-role-policy.json
    ```

6.  现在将托管的 `AmazonSNSFullAccess` `IAM` 策略附加到 `IAM` 角色：

    ```
    aws iam attach-role-policy \
         --policy-arn arn:aws:iam::aws:policy/AmazonSNSFullAccess \
         --role-name AWSCookbook904RuleRole
    ```

7.  为 AWS 控制台登录事件创建一个名为 `event-pattern.json` 的文件（该文件已在仓库中提供）：

    ```
    {
      "detail-type": [
      "AWS API Call via CloudTrail",
      "AWS Console Sign In via CloudTrail"
      ],
      "detail": {
      "userIdentity": {
        "type": [
          "Root"
        ]
      }
      }
    }
    ```

8.  创建一个 `EventBridge` 规则，用于监控跟踪中的根用户登录活动：

    ```
    aws events put-rule --name "AWSCookbook904Rule" \
         --role-arn "arn:aws:iam::$AWS_ACCOUNT_ID:role/AWSCookbook904RuleRole" --event-pattern file://event-pattern.json
    ```

9.  将 `EventBridge` 规则的目标设置为你的 `SNS` 主题：

    ```
    aws events put-targets --rule AWSCookbook904Rule \
         --targets "Id"="1","Arn"="$TOPIC_ARN"
    ```

#### 验证检查

使用根账户登录你的 AWS 账户，等待几分钟，然后检查邮箱中来自 `SNS` 的消息。

### 清理

按照 [章节代码仓库](https://github.com/AWSCookbook/Security) 中此 Recipe 文件夹内的步骤操作。

## 讨论

设置根用户登录告警是一种检测机制，你可以将其分层纳入安全策略，以便了解账户内的异常活动。这是一种具有成本效益的解决方案，用于监控 AWS 账户是否存在通过根用户的非预期访问。由于根用户是最强大、权限最高的身份（且应仅用于特定的非频繁任务），因此了解何时访问了它非常重要。

> **注意**
>
> 在某些情况下，需要根用户在 AWS 账户中执行特定的特权操作和任务。有关需要根用户的操作列表，请参阅 AWS [支持文档](https://oreil.ly/NvidX)。

### 挑战

添加另一个 `EventBridge` 规则，当根用户密码更改时通知同一个 `SNS` 主题（这是一个[提示](https://oreil.ly/vzvwz)）。

# 9.5 为根用户设置多因素认证

## 问题

你需要为 AWS 账户的根用户启用多因素认证。

## 解决方案

使用根用户凭证登录你的 AWS 账户。导航到 `IAM` 控制台，使用兼容 `U2F` 的硬件设备或符合[基于时间的一次性密码 (TOTP)](https://oreil.ly/TtQtd) 标准的虚拟设备启用多因素认证（请参阅 [图 9-14](#ch09.html#enabling_mfa_for_the_root_user_in_your)）。

![](assets/awsc_0914.png)

### 步骤

1.  使用与根用户关联的电子邮件地址登录 [AWS 控制台](https://console.aws.amazon.com)。你可以通过输入根用户电子邮件地址后显示的“忘记密码”链接来重置根用户的密码；单击“下一步”。你应该看到的登录对话框如 [图 9-15](#ch09.html#selecting_the_root_user_login_option) 所示。

    ![](assets/awsc_0915.png)

    > **提示**
    >
    > 由于你将不频繁使用根用户登录，一旦启用 `MFA`，你就不需要存储该用户的密码。每次需要访问账户时，你都可以[重置密码](https://oreil.ly/ar0H9)；请注意保护与根用户关联的邮箱的访问安全。

2.  登录 AWS 控制台后，从用户界面右上角选择[我的安全凭证](https://oreil.ly/z5xsh)，如 [图 9-16](#ch09.html#navigating_to_the_my_security_credentia) 所示。

    ![](assets/awsc_0916.png)

3.  在 `IAM` 控制台的“您的安全凭证”页面上展开“多因素认证”面板，然后单击“激活 MFA”按钮。选择你将使用的[设备类型](https://oreil.ly/VVL9g)，然后单击“继续”（请参阅 [图 9-17](#ch09.html#selecting_a_virtual_mfa_device)）。

    > **警告**
    >
    > 如果你使用基于软件的密码管理器实用程序来存储虚拟 `MFA` 设备信息，请勿将根用户密码存储在同一个密码管理器中。如果你的密码管理器实用程序或保管库遭到破解，你的第二因子和密码组合在一起就能访问你的 AWS 账户。同样，你的电子邮件账户密码信息也不应与虚拟 `MFA` 设备存储在同一位置，因为只要有权访问与根用户关联的邮箱，就能成功完成根用户密码重置过程。

    ![](assets/awsc_0917.png)

4.  按照提示显示一个 `QR` 码以便用虚拟设备扫描，或者插入硬件令牌并按照提示操作。输入代码后，你将看到一个指示 `MFA` 设置已完成的窗口（请参阅 [图 9-18](#ch09.html#confirmation_of_mfa_device_setup)）。

    ![](assets/awsc_0918.png)

    > **提示**
    >
    > 你可以打印显示的 `QR` 码副本，并将其保存在物理安全的位置作为备份，以防你失去对虚拟 `MFA` 设备的访问权限。

#### 验证检查
