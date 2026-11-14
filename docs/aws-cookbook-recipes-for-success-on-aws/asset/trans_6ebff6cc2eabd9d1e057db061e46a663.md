
本章中的示例将帮助您探索多项归属于“Serverless（无服务器）”范畴的 AWS 服务。您将有机会通过挑战来扩展解决方案，并获得一些引领无服务器行业趋势的新服务的实践经验。

## 工作站配置

请遵循[“CLI 配方的通用工作站设置步骤”](#preface01.html#general_workstation_setup_steps_for_cli)来验证您的配置并设置所需的环境变量。然后，克隆本章代码仓库：

```
git clone https://github.com/AWSCookbook/Serverless
```

## 章节先决条件

### Lambda 函数执行的 IAM 角色

创建一个名为 `assume-role-policy.json` 的文件，内容如下（该文件已在章节仓库根目录下提供）：

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

> **注意**
>
> 当您在 AWS Management Console 中创建 Lambda 函数并为执行角色选择“Create a new role from AWS policy templates”时，会自动创建一个类似的角色。

使用 `assume-role-policy.json` 文件中的语句创建 IAM 角色：

```
aws iam create-role --role-name AWSCookbookLambdaRole \
    --assume-role-policy-document file://assume-role-policy.json
```

将 `AWSLambdaBasicExecutionRole` IAM 托管策略附加到该 IAM 角色：

```
aws iam attach-role-policy --role-name AWSCookbookLambdaRole \
    --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```

## 5.1 配置 ALB 以调用 Lambda 函数

### 问题

您有一项需求：整个 Web 应用程序必须通过负载均衡器暴露在互联网上。您的应用程序架构包含无服务器函数。您需要某个函数能够响应特定 URL 路径的 HTTP 请求。

### 解决方案

授予 Elastic Load Balancing 服务调用 Lambda 函数的权限；然后创建一个 Lambda 函数。创建一个 ALB 目标组；然后将 Lambda 函数注册到该目标组。将目标组与 ALB 上的监听器关联。最后，添加一条监听器规则，将 `/function` 路径的流量导向 Lambda 函数（参见[图 5-1](#ch05.html#lambda_function_invoked_by_alb)）。

![](assets/awsc_0501.png)

### 先决条件

*   具有两个可用区中公有子网及关联路由表的 VPC
*   一个 Application Load Balancer，包含以下内容：
    *   一个允许来自全球的 80 端口流量的关联安全组
    *   一个 80 端口的监听器
*   允许 Lambda 函数执行的 IAM 角色（在章节先决条件中提供）

### 准备工作

请遵循本示例文件夹中 [章节代码仓库](https://github.com/AWSCookbook/Serverless) 的步骤。

### 步骤

1.  压缩仓库中本示例目录下提供的函数代码。此代码将用于 Lambda 函数。您可以根据需要修改此代码，但我们已为您提供了可用的代码。例如，使用以下命令压缩代码：

    ```
    zip lambda_function.zip lambda_function.py
    ```

2.  创建一个响应 HTTP 请求的 Lambda 函数：

    ```
    LAMBDA_ARN=$(aws lambda create-function \
        --function-name AWSCookbook501Lambda \
        --runtime python3.8 \
        --package-type "Zip" \
        --zip-file fileb://lambda_function.zip \
        --handler lambda_function.lambda_handler --publish \
        --role \
        arn:aws:iam::$AWS_ACCOUNT_ID:role/AWSCookbookLambdaRole \
        --output text --query FunctionArn)
    ```

3.  创建一个目标类型设置为 `lambda` 的 ALB 目标组：

    ```
    TARGET_GROUP_ARN=$(aws elbv2 create-target-group \
        --name awscookbook501tg \
        --target-type lambda --output text \
        --query TargetGroups[0].TargetGroupArn)
    ```

4.  使用 `add-permission` 命令授予 Elastic Load Balancing 服务调用您的 Lambda 函数的权限：

    ```
    aws lambda add-permission \
        --function-name $LAMBDA_ARN \
        --statement-id load-balancer \
        --principal elasticloadbalancing.amazonaws.com \
        --action lambda:InvokeFunction \
        --source-arn $TARGET_GROUP_ARN
    ```

5.  使用 `register-targets` 命令将 Lambda 函数注册为目标：

    ```
    aws elbv2 register-targets \
        --target-group-arn $TARGET_GROUP_ARN \
        --targets Id=$LAMBDA_ARN
    ```

6.  修改 ALB 上 80 端口的监听器；然后创建一条规则，将目标为 `/function` 路径的流量转发到您的目标组：

    ```
    RULE_ARN=$(aws elbv2 create-rule \
        --listener-arn $LISTENER_ARN --priority 10 \
        --conditions Field=path-pattern,Values='/function' \
        --actions Type=forward,TargetGroupArn=$TARGET_GROUP_ARN \
        --output text --query Rules[0].RuleArn)
    ```

#### 验证检查

测试调用以验证请求 `/function` 时是否会调用 Lambda 函数：

```
curl -v $LOAD_BALANCER_DNS/function
```

### 清理

请遵循本示例文件夹中 [章节代码仓库](https://github.com/AWSCookbook/Serverless) 的步骤。

## 讨论

开发人员和软件架构师可以利用 Lambda 函数为大型系统中的某类事件提供编程式响应。这种计算类型最常用于函数负责一小单元工作时。AWS 于 2018 年增加了 [Application Load Balancers 调用 Lambda 函数](https://oreil.ly/R15DP) 的功能。

当最终用户请求 ALB 上的特定 URL 路径（由开发人员配置）时，ALB 可将请求传递给 Lambda 函数以处理响应。ALB 随后接收函数的输出，并将结果作为 HTTP 响应返回给最终用户。ALB 可为单个负载均衡器配置多个路径和目标，将部分流量发送给特定目标（Lambda 函数、容器、EC2 实例等）。ALB 还支持[使用标头值路由到 Lambda 函数](https://oreil.ly/ljYBe)。这种简单的架构具有极高的成本效益和可扩展性。

这种灵活性允许单个 ALB 处理应用程序的所有流量，并在构建需要通过 HTTP/HTTPS 暴露的系统时提供良好的构建模块。

### 挑战 1

添加一个响应 ALB 上另一路径的 Fargate 任务。

### 挑战 2

尝试使用 [Amazon API Gateway](https://aws.amazon.com/api-gateway) 在 Lambda 函数前进行代理。

## 5.2 使用 Lambda 层打包库

### 问题

您有使用外部库的 Python 代码，需要将其包含在无服务器函数部署中。

### 解决方案

创建一个文件夹并使用 `pip` 将 Python 包安装到该文件夹。然后，将该文件夹压缩为 `.zip` 文件，并使用该文件创建一个 Lambda 层供您的函数使用（参见[图 5-2](#ch05.html#lambda_function_layer_creation_and_repr)）。

![](assets/awsc_0502.png)

### 先决条件

*   允许 Lambda 函数执行的 IAM 角色（在章节先决条件中提供）

### 步骤

1.  在本章仓库的根目录下，`cd` 到 `502-Packaging-Libraries-with-Lambda-Layers` 目录并执行后续步骤：

    ```
    cd 502-Packaging-Libraries-with-Lambda-Layers/
    ```

2.  压缩仓库中提供的 `lambda_function.py`：

    ```
    zip lambda_function.zip lambda_function.py
    ```

    您应该会看到类似以下的输出：

    ```
    updating: lambda_function.py (deflated 49%)
    ```

3.  创建一个我们将向其添加层的 Lambda 函数：
