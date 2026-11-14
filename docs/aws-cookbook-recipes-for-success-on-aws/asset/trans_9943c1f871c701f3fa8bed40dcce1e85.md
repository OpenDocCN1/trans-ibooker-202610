
```
aws s3api put-bucket-notification-configuration \
    --bucket awscookbook507-$RANDOM_STRING \
    --notification-configuration file://notification.json
```

15. 上传文件到 S3 以触发导入：

```
aws s3 cp ./sample_data.csv s3://awscookbook507-$RANDOM_STRING
```

### 验证检查

在 DynamoDB 控制台查看结果，或使用此 CLI 命令扫描表：

```
aws dynamodb scan --table-name AWSCookbook507
```

### 注意

DynamoDB 的一大特性是它提供了 AWS API 端点，以便您的应用程序通过 AWS SDK 轻松进行 CRUD 操作。

### 清理

按照 [章节代码仓库](https://github.com/AWSCookbook/Serverless) 中此配方文件夹内的步骤操作。

### 讨论

您可以将 AWS Lambda 和 Amazon DynamoDB 结合使用，以构建具有大规模可扩展数据库持久性的应用程序，同时将所需的运营开销降至最低。希望构建应用程序而无需担心服务器基础设施的软件架构师和开发人员可能会发现将这两项服务结合使用很有用。

### 警告

在撰写本文时，Lambda 函数[会在 900 秒后超时](https://oreil.ly/MnJNV)。这可能会导致大型 CSV 文件出现问题，或者 DynamoDB 表没有足够的写入容量。

事件驱动应用程序也是在 AWS 上构建现代云原生应用程序的一个重要概念。当您创建 `notification.json` 文件时，您指定了 Lambda 函数和 S3 存储桶，以及一个用于监视上传的键模式，以便在对象被放入存储桶时触发 Lambda 函数。使用事件驱动架构有助于最大限度地降低运行应用程序相关的成本和复杂性，因为函数逻辑仅在需要时运行。

### 挑战 1

向 `sample_data.csv` 文件添加一些新数据，从存储桶中删除该文件，然后重新上传该文件以触发新的导入。请注意，现有数据将保留，新数据将被添加。

### 挑战 2

更改 S3 通知和 Lambda 函数，以允许在解决方案中使用其他文件名。

### 挑战 3

为您的 Lambda 函数创建一个细粒度的 IAM 策略，将函数授予的 DynamoDB 表访问权限缩小到最小范围。

## 5.8 使用预置并发减少 Lambda 启动时间

### 问题

您需要确保对无服务器函数的预定数量（五次）调用尽可能快。您需要消除与冷启动相关的任何延迟。

### 解决方案

创建一个 Lambda 函数并将函数并发设置为 5（请参阅[图 5-8](#ch05.html#provisioned_concurrency_for_a_lambda_fu)）。

![](assets/awsc_0508.png)

#### 先决条件

*   允许 Lambda 函数执行的 IAM 角色（在章节先决条件中提供）

#### 步骤

1.  创建一个名为 `lambda_function.py` 的文件，内容如下（仓库中提供了该文件）：

```
from datetime import datetime
import time

def lambda_handler(event, context):
    time.sleep(5)
    print('AWS Cookbook Function run at {}'.format(str(datetime.now())))
```

2.  压缩函数代码：

```
zip lambda_function.zip lambda_function.py
```

3.  创建 Lambda 函数：

```
aws lambda create-function \
    --function-name AWSCookbook508Lambda \
    --runtime python3.8 \
    --package-type "Zip" \
    --zip-file fileb://lambda_function.zip \
    --handler lambda_function.lambda_handler --publish \
    --timeout 20 \
    --role \
    arn:aws:iam::$AWS_ACCOUNT_ID:role/AWSCookbookLambdaRole
```

4.  使用此命令确定 Lambda 函数何时进入活动状态：

```
aws lambda get-function --function-name AWSCookbook509Lambda \
    --output text --query Configuration.State
```

#### 注意

[Lambda 函数别名](https://oreil.ly/8vXHA)允许您引用函数的特定版本。每个 Lambda 函数可以有一个或多个别名。初始别名名为 `LATEST`。

5.  为 Lambda 函数配置预置并发：

```
aws lambda put-provisioned-concurrency-config \
    --function-name AWSCookbook508Lambda \
    --qualifier LATEST \
    --provisioned-concurrent-executions 5
```

#### 验证检查

连续调用该函数六次以查看限制被触发：

```
aws lambda invoke --function-name AWSCookbook508Lambda response.json &
aws lambda invoke --function-name AWSCookbook508Lambda response.json
```

#### 注意

您部署的 Lambda 函数需要并行运行，以演示预置并发功能的能力。您可能需要编写一个简单的脚本，并行多次运行此命令，以证明已启用预置并发。

#### 清理

按照 [章节代码仓库](https://github.com/AWSCookbook/Serverless) 中此配方文件夹内的步骤操作。

### 讨论

您的代码执行环境预配由 Lambda 服务为您处理。这是它作为 AWS 上的全托管服务的一个优势。由于执行环境是按需预配的，因此需要少量时间为您预配此环境。这被称为 `cold start`（冷启动）。Lambda 会在一段时间内保持您的执行环境已预配（或“热”状态），以便在您的函数再次被调用时能够快速启动。当您需要函数快速响应并实现更高并发时，可以避免冷启动，并使用预置并发来保持多个执行环境副本处于“热”状态。

一些开发人员和软件架构师需要构建具有以毫秒为单位的时间敏感型需求的解决方案，用于基于微服务的应用程序。当您使用 Lambda 预置并发时，可以将函数被调用时所需的启动时间降至最低。

#### 挑战 1

在 Lambda 函数前配置 API Gateway，并使用 [bees with machine guns](https://oreil.ly/EmxSn) 或 [ApacheBench](https://oreil.ly/3i7WV) 等工具模拟用户负载。

#### 挑战 2

配置应用程序自动扩展，根据时间和/或性能指标（例如响应时间）修改 Lambda 函数的预置并发。

## 5.9 使用 Lambda 访问 VPC 资源

### 问题

您需要一个无服务器函数来访问在 VPC 中拥有端点的 ElastiCache 集群。

### 解决方案

创建一个具有 Redis 客户端连接和打包的 Lambda 函数，并指定 VPC 子网和安全组。然后创建 ElastiCache 子网组和 ElastiCache 集群。调用 Lambda 并将集群端点传递给函数进行测试（请参阅[图 5-9](#ch05.html#lambda_accessing_an_elasticache_cluster)）。

![](assets/awsc_0509.png)

#### 先决条件

*   允许 Lambda 函数执行的 IAM 角色（在章节先决条件中提供）

#### 准备

按照 [章节代码仓库](https://github.com/AWSCookbook/Serverless) 中此配方文件夹内的步骤操作。

#### 步骤

1.  您需要赋予 Lambda 函数在 VPC 中执行的能力，因此请将 `AWSLambdaVPCAccess` 的 IAM 托管策略附加到 IAM 角色（在章节先决条件中创建）：

```
aws iam attach-role-policy --role-name AWSCookbookLambdaRole \
    --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaVPCAccessExecutionRole
```

2.  从 Python 包索引 (PyPI) 将 Redis Python 包安装到当前目录：

```
pip install redis -t .
```

3.  压缩函数代码：

```
zip -r lambda_function.zip lambda_function.py redis*
```

4.  为 Lambda 创建安全组：
