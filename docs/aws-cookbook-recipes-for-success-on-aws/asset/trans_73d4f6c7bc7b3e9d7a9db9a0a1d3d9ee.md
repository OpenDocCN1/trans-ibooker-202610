
```
LAMBDA_ARN=$(aws lambda create-function \
    --function-name AWSCookbook502Lambda \
    --runtime python3.8 \
    --package-type "Zip" \
    --zip-file fileb://lambda_function.zip \
    --handler lambda_function.lambda_handler --publish \
    --role \
    arn:aws:iam::$AWS_ACCOUNT_ID:role/AWSCookbookLambdaRole \
    --output text --query FunctionArn)
```

4. 创建一个用于存放层内容的目录：

```
mkdir python
```

5. 使用 `pip` 将最新的 `requests` 模块安装到该目录：

```
pip install requests --target="./python"
```

您应该会看到类似以下的输出：

```
Collecting requests
  Using cached requests-2.26.0-py2.py3-none-any.whl (62 kB)
Collecting certifi>=2017.4.17
  Using cached certifi-2021.5.30-py2.py3-none-any.whl (145 kB)
Collecting idna<4,>=2.5
  Using cached idna-3.2-py3-none-any.whl (59 kB)
Collecting urllib3<1.27,>=1.21.1
  Using cached urllib3-1.26.7-py2.py3-none-any.whl (138 kB)
Collecting charset-normalizer~=2.0.0
  Using cached charset_normalizer-2.0.6-py3-none-any.whl (37 kB)
Installing collected packages: urllib3, idna, charset-normalizer, certifi, requests
Successfully installed certifi-2021.5.30 charset-normalizer-2.0.6 idna-3.2 requests-2.26.0 urllib3-1.26.7
```

6. 压缩该目录的内容：

```
zip -r requests-layer.zip ./python
```

您应该会看到类似以下的输出：

```
  adding: python/ (stored 0%)
  adding: python/bin/ (stored 0%)
  adding: python/bin/normalizer (deflated 28%)
  adding: python/requests-2.26.0.dist-info/ (stored 0%)
  adding: python/requests-2.26.0.dist-info/RECORD (deflated 55%)
  adding: python/requests-2.26.0.dist-info/LICENSE (deflated 65%)
  adding: python/requests-2.26.0.dist-info/WHEEL (deflated 14%)
  adding: python/requests-2.26.0.dist-info/top_level.txt (stored 0%)
  adding: python/requests-2.26.0.dist-info/REQUESTED (stored 0%)
  adding: python/requests-2.26.0.dist-info/INSTALLER (stored 0%)
  adding: python/requests-2.26.0.dist-info/METADATA (deflated 58%)
```

#### 注意

Lambda 层需要特定的文件夹结构。当您使用 `pip` 将 `requests` 模块安装到目录中的 Python 文件夹时，会创建此文件夹结构：

```
$ tree -L 1 python/
python/
├── bin
├── certifi
├── certifi-2020.12.5.dist-info
├── chardet
├── chardet-4.0.0.dist-info
├── idna
├── idna-2.10.dist-info
├── requests
├── requests-2.25.1.dist-info
├── urllib3
└── urllib3-1.26.4.dist-info

11 directories, 0 files
```

7. 发布层并设置一个环境变量以在后续步骤中使用：

```
LAYER_VERSION_ARN=$(aws lambda publish-layer-version \
    --layer-name AWSCookbook502RequestsLayer \
    --description "Requests layer" \
    --license-info "MIT" \
    --zip-file fileb://requests-layer.zip \
    --compatible-runtimes python3.8 \
    --output text --query LayerVersionArn)
```

8. 更新 Lambda 以使用您创建的层：

```
aws lambda update-function-configuration \
    --function-name AWSCookbook502Lambda \
    --layers $LAYER_VERSION_ARN
```

您应该会看到类似以下的输出：

```
{
  "FunctionName": "AWSCookbook502Lambda",
  "FunctionArn": "arn:aws:lambda:us-east-1:111111111111:function:AWSCookbook502Lambda",
  "Runtime": "python3.8",
  "Role": "arn:aws:iam::111111111111:role/AWSCookbookLambdaRole",
  "Handler": "lambda_function.lambda_handler",
  "CodeSize": 691,
  "Description": "",
  "Timeout": 3,
  "MemorySize": 128,
...
```

### 验证检查

测试 Lambda：

```
aws lambda invoke \
    --function-name AWSCookbook502Lambda \
    response.json && cat response.json
```

### 清理

按照 [章节代码仓库](https://github.com/AWSCookbook/Serverless) 中此配方文件夹内的步骤操作。

## 讨论

Lambda 层可用于扩展默认 Lambda 运行时中可用的包，也可用于为您的函数提供自定义运行时。默认运行时与 Amazon Linux 相关联。自定义运行时可以在 Amazon Linux 之上开发，以支持您自己的编程语言需求。请参阅此 [教程](https://oreil.ly/XTWHX) 以发布自定义运行时。

在本配方中，您将 Python `requests` 模块打包为一个层，并部署了一个使用该模块的 Python 函数。层可被多个函数使用、与其他 AWS 账户共享，还可以进行版本控制，以便您部署和测试层的新版本，而不会影响正在为您的函数使用的现有版本。

### 挑战

创建另一个使用相同层的 Lambda 函数。

# 5.3 按计划调用 Lambda 函数

## 问题

您需要每分钟运行一次无服务器函数。

## 解决方案

向您的 Lambda 函数添加权限，以允许 EventBridge 服务调用该函数。然后配置一个 EventBridge 规则，使用针对一分钟的计划表达式，将您的函数作为目标（请参见[图 5-3](#ch05.html#eventbridge_triggering_a_time_based_inv)）。

![](assets/awsc_0503.png)

### 先决条件

*   您想要触发的 Lambda 函数
*   允许 Lambda 函数执行的 IAM 角色（在章节先决条件中提供）

### 准备

按照 [章节代码仓库](https://github.com/AWSCookbook/Serverless) 中此配方文件夹内的步骤操作。

### 步骤

1. 创建一个具有每分钟速率的计划表达式的事件规则：

```
RULE_ARN=$(aws events put-rule --name "EveryMinuteEvent" \
    --schedule-expression "rate(1 minute)")
```

#### 注意

定义基于时间的事件规则时，您可以对计划表达式使用速率表达式和 [Cron 格式](https://crontab.guru)。您创建的规则的 Cron 表达式语法如下所示：

```
RULE_ARN=$(aws events put-rule --name "EveryMinuteEvent" \
--schedule-expression "cron(* * * * ? *)")
```

有关计划表达式的更多信息，请参阅此 [支持文档](https://oreil.ly/K7e8Z)。

2. 向 Lambda 函数添加权限，以便 EventBridge 服务可以调用它：

```
aws lambda add-permission --function-name $LAMBDA_ARN \
    --action lambda:InvokeFunction --statement-id events \
    --principal events.amazonaws.com
```

您应该会看到类似以下的输出：

```
{
  "Statement": "{\"Sid\":\"events\",\"Effect\":\"Allow\",\"Principal\":{\"Service\":\"events.amazonaws.com\"},\"Action\":\"lambda:InvokeFunction\",\"Resource\":\"arn:aws:lambda:us-east-1:111111111111:function:AWSCookbook503Lambda\"}"
}
```

3. 将您的 Lambda 函数作为目标添加到您创建的规则中：

```
aws events put-targets --rule EveryMinuteEvent \
    --targets "Id"="1","Arn"="$LAMBDA_ARN"
```

您应该会看到类似以下的输出：

```
{
  "FailedEntryCount": 0,
  "FailedEntries": []
}
```

#### 注意

有许多可用的目标选项。有关最新列表，请查看 [文档](https://oreil.ly/Rggow)。

#### 验证检查

跟踪 CloudWatch Logs 日志组，以观察函数每 60 秒被调用一次：

```
aws logs tail "/aws/lambda/AWSCookbook503Lambda" --follow --since 10s
```

#### 注意

您可能需要等待片刻以创建日志组。如果日志组不存在，您将收到以下错误：

```
An error occurred (ResourceNotFoundException) when calling the FilterLogEvents operation: The specified log group does not exist.
```

您应该会看到类似以下的输出：
