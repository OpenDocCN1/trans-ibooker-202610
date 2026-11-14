
首先，为您的数据库启用 Data API 并为您的 EC2 实例配置 IAM 权限。然后，从 CLI 和 RDS 控制台进行测试。这允许您的应用程序连接到 Aurora Serverless 数据库，如 [图 4-8](#ch04.html#an_application_using_the_rds_data_api) 所示。

![](assets/awsc_0408.png)

## 先决条件

*   在两个 AZ 中创建了具有隔离子网的 VPC 以及关联的路由表。
*   已部署 PostgreSQL RDS 实例和 EC2 实例。您需要具备连接到这些实例以进行测试的能力。

## 准备工作

按照 [章节代码仓库](https://github.com/AWSCookbook/Databases) 中此配方文件夹内的步骤操作。

## 步骤

1.  在 Aurora Serverless 集群上启用 Data API：

    ```
    aws rds modify-db-cluster \
        --db-cluster-identifier $CLUSTER_IDENTIFIER \
        --enable-http-endpoint \
        --apply-immediately
    ```

2.  确保 `HttpEndpointEnabled` 设置为 `true`：

    ```
    aws rds describe-db-clusters \
        --db-cluster-identifier $CLUSTER_IDENTIFIER \
        --query DBClusters[0].HttpEndpointEnabled
    ```

3.  从 CLI 测试一条命令：

    ```
    aws rds-data execute-statement \
        --secret-arn "$SECRET_ARN" \
        --resource-arn "$CLUSTER_ARN" \
        --database "$DATABASE_NAME" \
        --sql "select * from pg_user" \
        --output json
    ```

    （可选）您也可以使用 Amazon RDS 查询编辑器通过 AWS 控制台测试访问。首先从终端运行这两条命令，以便复制并粘贴这些值：

    ```
    echo $SECRET_ARN
    echo $DATABASE_NAME
    ```

4.  使用管理员权限登录 AWS 控制台并转到 RDS 控制台。在左侧边栏菜单中，单击“查询编辑器”。填写值并选择“连接到数据库”，如 [图 4-9](#ch04.html#connect_to_database_settings) 所示。

    ![](assets/awsc_0409.png)

5.  运行相同的查询并在查询编辑器下方查看结果（请参阅 [图 4-10](#ch04.html#rds_query_editor)）：

    ```
    SELECT * from pg_user;
    ```

    ![](assets/awsc_0410.png)

6.  配置 EC2 实例以将 Data API 与数据库集群配合使用。创建一个名为 `policy-template.json` 的文件，内容如下（仓库中提供了该文件）：

    ```
    {
      "Version": "2012-10-17",
      "Statement": [
      {
        "Action": [
        "rds-data:BatchExecuteStatement",
        "rds-data:BeginTransaction",
        "rds-data:CommitTransaction",
        "rds-data:ExecuteStatement",
        "rds-data:RollbackTransaction"
        ],
        "Resource": "*",
        "Effect": "Allow"
      },
      {
        "Action": [
        "secretsmanager:GetSecretValue",
        "secretsmanager:DescribeSecret"
        ],
        "Resource": "SecretArn",
        "Effect": "Allow"
      }
      ]
    }
    ```

7.  使用您设置的环境变量，通过 `sed` 命令替换模板文件中的值：

    ```
    sed -e "s/SecretArn/${SECRET_ARN}/g" \
        policy-template.json > policy.json
    ```

8.  使用刚创建的文件创建 IAM 策略：

    ```
    aws iam create-policy --policy-name AWSCookbook408RDSDataPolicy \
        --policy-document file://policy.json
    ```

9.  将 `AWSCookbook408RDSDataPolicy` 的 IAM 策略附加到 EC2 实例的 IAM 角色：

    ```
    aws iam attach-role-policy --role-name $INSTANCE_ROLE_NAME \
        --policy-arn arn:aws:iam::$AWS_ACCOUNT_ID:policy/AWSCookbook408RDSDataPolicy
    ```

### 验证检查

创建并填充一些 SSM 参数以存储值，以便从 EC2 实例检索它们：

```
aws ssm put-parameter \
    --name "Cookbook408DatabaseName" \
    --type "String" \
    --value $DATABASE_NAME 

aws ssm put-parameter \
    --name "Cookbook408ClusterArn" \
    --type "String" \
    --value $CLUSTER_ARN

aws ssm put-parameter \
    --name "Cookbook408SecretArn" \
    --type "String" \
    --value $SECRET_ARN
```

使用 SSM Session Manager 连接到 EC2 实例（请参阅 [配方 1.6](#ch01.html#connecting_to_ectwo_instances_using_aws)）：

```
aws ssm start-session --target $INSTANCE_ID
```

设置区域：

```
export AWS_DEFAULT_REGION=us-east-1
```

检索 SSM 参数值并将它们设置为环境变量：

```
DatabaseName=$(aws ssm get-parameters \
    --names "Cookbook408DatabaseName" \
       --query "Parameters[*].Value" --output text)

SecretArn=$(aws ssm get-parameters \
    --names "Cookbook408SecretArn" \
       --query "Parameters[*].Value" --output text)

ClusterArn=$(aws ssm get-parameters \
    --names "Cookbook408ClusterArn" \
       --query "Parameters[*].Value" --output text)
```

对数据库运行查询：

```
aws rds-data execute-statement \
    --secret-arn "$SecretArn" \
    --resource-arn "$ClusterArn" \
    --database "$DatabaseName" \
    --sql "select * from pg_user" \
    --output json
```

退出 Session Manager 会话：

```
exit
```

## 清理

按照 [章节代码仓库](https://github.com/AWSCookbook/Databases) 中此配方文件夹内的步骤操作。

## 讨论

Data API 为 Aurora 提供了一个 HTTPS 端点，并使用 IAM 身份验证，允许您的应用程序通过 HTTPS 在数据库上执行 SQL 语句，而不是使用传统的 TCP 数据库连接。

#### 提示

根据 [Aurora 用户指南](https://oreil.ly/wvADq)，对 Data API 的所有调用都是同步的，查询的默认超时时间为 45 秒。如果您的查询时间超过 45 秒，可以使用 `continueAfterTimeout` 参数来处理长时间运行的查询。

与其他使用 IAM 身份验证的 AWS 服务 API 一样，使用 Data API 执行的所有活动都会被 CloudTrail 记录，以确保存在审计跟踪，这有助于满足您的安全和审计要求。您可以通过将 IAM 策略与应用程序的角色关联来控制和委派对 Data API 端点的访问。例如，如果您想授予应用程序仅使用 Data API 从数据库读取的能力，可以编写一个省略 `rds-data:CommitTransaction` 和 `rds-data:RollbackTransaction` 权限的策略。

RDS 控制台中的查询编辑器提供了一种基于 Web 的方式，用于对数据库执行 SQL 查询。这是一种方便的机制，供开发人员和 DBA 快速完成特定任务。您在本配方中分配给 EC2 实例的相同权限，需要通过 IAM 角色授予您的开发人员和 DBA。

## 挑战

创建并部署一个 Lambda 函数，使其拥有访问您预配置的 RDS Data API 的权限。

# 第 5 章 无服务器

## 5.0 简介

技术行业术语*无服务器*有时会导致困惑，因为服务器确实涉及与此执行模型相关的云服务。优势在于最终用户无需担心管理底层基础设施和平台。云提供商（在本例中为 AWS）负责所有管理、操作系统更新、可用性、容量等。

就 [AWS 上可用的“无服务器”服务](https://oreil.ly/5CIjp) 而言，有许多选项可供利用这些优势。以下是一些示例：

*   AWS Lambda 和 Amazon Fargate 用于计算
*   Amazon EventBridge、Amazon SNS、Amazon SQS 和 Amazon API Gateway 用于应用程序集成
*   Amazon S3、Amazon DynamoDB 和 Amazon Aurora Serverless 用于数据存储

AWS 上无服务器的主要优势如下：

**成本节约**：您只需为所用内容付费。

**可扩展性**：根据需要扩展；缩减以节省成本。

**减少管理**：无需部署服务器或管理系统。

**灵活性**：AWS Lambda 支持多种编程语言。
