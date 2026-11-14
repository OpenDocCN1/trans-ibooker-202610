
你有一个正在访问关系型数据库的无服务器函数，需要实现连接池以尽量减少数据库连接数量并提高性能。

## 解决方案

创建一个 RDS Proxy，将其与你的 RDS MySQL 数据库关联，并配置 Lambda 连接到该代理，而不是直接访问数据库（请参阅[图 4-3](#ch04.html#lambda_connection_path_to_database_via)）。

![](assets/awsc_0403.png)

### 先决条件

* 在两个可用区中创建了隔离子网并关联了路由表的 VPC
* 一个 RDS MySQL 实例
* 一个你希望连接到 RDS 数据库的 Lambda 函数

### 准备工作

请遵循 [章节代码仓库](https://github.com/AWSCookbook/Databases) 中该配方文件夹内的步骤。

### 步骤

1. 创建一个名为 `assume-role-policy.json` 的文件，内容如下（仓库中已提供该文件）：

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "rds.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

2. 使用 `assume-role-policy.json` 文件为 RDS Proxy 创建一个 IAM 角色：

```
aws iam create-role --assume-role-policy-document \
    file://assume-role-policy.json --role-name AWSCookbook403RDSProxy
```

你应该会看到类似如下的输出：

```
{
  "Role": {
  "Path": "/",
  "RoleName": "AWSCookbook403RDSProxy",
  "RoleId": "<<ID>>",
  "Arn": "arn:aws:iam::111111111111:role/AWSCookbook403RDSProxy",
  "CreateDate": "2021-09-21T22:33:57+00:00",
  "AssumeRolePolicyDocument": {
    "Version": "2012-10-17",
    "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "rds.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
    ]
  }
  }
}
```

3. 创建一个供 RDS Proxy 使用的安全组：

```
RDS_PROXY_SG_ID=$(aws ec2 create-security-group \
    --group-name AWSCookbook403RDSProxySG \
    --description "Lambda Security Group" --vpc-id $VPC_ID \
    --output text --query GroupId)
```

4. 创建 RDS Proxy。这将花费片刻时间：

```
RDS_PROXY_ENDPOINT_ARN=$(aws rds create-db-proxy \
    --db-proxy-name $DB_NAME \
    --engine-family MYSQL \
    --auth '{
            "AuthScheme": "SECRETS",
            "SecretArn": "'"$RDS_SECRET_ARN"'",
            "IAMAuth": "REQUIRED"
          }' \
    --role-arn arn:aws:iam::$AWS_ACCOUNT_ID:role/AWSCookbook403RDSProxy \
    --vpc-subnet-ids $ISOLATED_SUBNETS \
    --vpc-security-group-ids $RDS_PROXY_SG_ID \
    --require-tls --output text \
    --query DBProxy.DBProxyArn)
```

等待 RDS Proxy 变为可用状态：

```
aws rds describe-db-proxies \
    --db-proxy-name $DB_NAME \
    --query DBProxies[0].Status \
    --output text
```

5. 获取 `RDS_PROXY_ENDPOINT`：

```
RDS_PROXY_ENDPOINT=$(aws rds describe-db-proxies \
    --db-proxy-name $DB_NAME \
    --query DBProxies[0].Endpoint \
    --output text)
```

6. 接下来，你需要一个允许 Lambda 函数生成 IAM 认证令牌的 IAM 策略。创建一个名为 `template-policy.json` 的文件，内容如下（仓库中已提供该文件）：

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "rds-db:connect"
      ],
      "Resource": [
        "arn:aws:rds-db:AWS_REGION:AWS_ACCOUNT_ID:dbuser:RDSProxyID/admin"
      ]
    }
  ]
}
```

7. 从 RDS Proxy 端点 ARN 中分离出 Proxy ID。后续步骤配置 IAM 策略时需要用到 Proxy ID：

```
RDS_PROXY_ID=$(echo $RDS_PROXY_ENDPOINT_ARN | awk -F: '{ print $7} ')
```

8. 使用 `sed` 命令结合你已设置的环境变量，替换模板文件中的值：

```
sed -e "s/AWS_ACCOUNT_ID/${AWS_ACCOUNT_ID}/g" \
    -e "s|AWS_REGION|${AWS_REGION}|g" \
    -e "s|RDSProxyID|${RDS_PROXY_ID}|g" \
policy-template.json > policy.json
```

9. 使用刚创建的文件创建 IAM 策略：

```
aws iam create-policy --policy-name AWSCookbook403RdsIamPolicy \
    --policy-document file://policy.json
```

你应该会看到类似如下的输出：

```
{
  "Policy": {
  "PolicyName": "AWSCookbook403RdsIamPolicy",
  "PolicyId": "<<Id>>",
  "Arn": "arn:aws:iam::111111111111:policy/AWSCookbook403RdsIamPolicy",
  "Path": "/",
  "DefaultVersionId": "v1",
  "AttachmentCount": 0,
  "PermissionsBoundaryUsageCount": 0,
  "IsAttachable": true,
  "CreateDate": "2021-09-21T22:50:24+00:00",
  "UpdateDate": "2021-09-21T22:50:24+00:00"
  }
}
```

10. 将该策略附加到 DBAppFunction Lambda 函数的角色上：

```
aws iam attach-role-policy --role-name $DB_APP_FUNCTION_ROLE_NAME \
    --policy-arn arn:aws:iam::$AWS_ACCOUNT_ID:policy/AWSCookbook403RdsIamPolicy
```

使用此命令检查代理何时进入可用状态，然后继续：

```
aws rds describe-db-proxies --db-proxy-name $DB_NAME \
    --query DBProxies[0].Status \
    --output text
```

11. 将 `SecretsManagerReadWrite` 策略附加到 RDS Proxy 的角色：

```
aws iam attach-role-policy --role-name AWSCookbook403RDSProxy \
    --policy-arn arn:aws:iam::aws:policy/SecretsManagerReadWrite
```

#### 提示

在生产场景中，你应该将此权限范围缩小到应用程序所需访问的最小机密资源，而不是授予 `SecretsManagerReadWrite`，后者允许对所有机密进行读/写操作。

12. 向 RDS 实例的安全组添加一条入站规则，允许来自 RDS Proxy 安全组的 TCP 端口 3306（MySQL 引擎的默认 TCP 端口）访问：

```
aws ec2 authorize-security-group-ingress \
    --protocol tcp --port 3306 \
    --source-group $RDS_PROXY_SG_ID \
    --group-id $RDS_SECURITY_GROUP
```

你应该会看到类似如下的输出：

```
{
  "Return": true,
  "SecurityGroupRules": [
  {
    "SecurityGroupRuleId": "sgr-<<ID>>",
    "GroupId": "sg-<<ID>>",
    "GroupOwnerId": "111111111111",
    "IsEgress": false,
    "IpProtocol": "tcp",
    "FromPort": 3306,
    "ToPort": 3306,
    "ReferencedGroupInfo": {
    "GroupId": "sg-<<ID>>"
    }
  }
  ]
}
```

#### 注意

安全组可以引用其他安全组。由于 VPC 内的动态 IP 地址，这被认为是在不过度开放安全组的情况下授予访问权限的最佳方式。更多信息请参阅[配方 2.5](#ch02.html#granting_dynamic_access_by_referencing)。

13. 向 RDS Proxy 注册目标：

```
aws rds register-db-proxy-targets \
    --db-proxy-name $DB_NAME \
    --db-instance-identifiers $RDS_DATABASE_ID
```

你应该会看到类似如下的输出：

```
{
  "DBProxyTargets": [
  {
    "Endpoint": "awscookbook403db.<<ID>>.us-east-1.rds.amazonaws.com",
    "RdsResourceId": "awscookbook403db",
    "Port": 3306,
    "Type": "RDS_INSTANCE",
    "TargetHealth": {
    "State": "REGISTERING"
    }
  }
  ]
}
```

使用此命令检查目标注册的状态。等待状态变为 AVAILABLE：
