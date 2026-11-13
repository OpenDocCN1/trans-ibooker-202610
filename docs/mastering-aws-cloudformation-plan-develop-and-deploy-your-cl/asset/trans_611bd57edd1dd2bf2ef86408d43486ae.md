
# 应用程序模板：hello-world

## 输出目录

`.

## 后续步骤

后续步骤可在 `./hello-world/README.md` 的 README 文件中找到。

## 步骤 7：检查新目录

让我们检查一下新目录：

```
$ tree .
.
├── README.md
├── __init__.py
├── events
│   └── event.json
├── hello_world
│   ├── __init__.py
│   ├── app.py
│   └── requirements.txt
├── template.yaml
└── tests
    ├── __init__.py
    └── unit
        ├── __init__.py
        └── test_handler.py
```

看起来一切都已就绪！我们可以立即部署此应用程序，但首先，让我们检查一下模板中有什么！有几个地方看起来很有趣：

### template.yaml：Globals（全局设置）

```yaml
Globals:
  Function:
    Timeout: 3
```

`Globals` 部分用于当我们需要为多个资源定义相同属性时。假设你正在构建一个由多个函数组成的 Lambda 应用程序。它们都将使用相同的运行时并具有相同的超时时间。为了避免多次重复相同的属性，我们只需将它们存储在 `Globals` 中。

### template.yaml：Resources（资源）

```yaml
Resources:
  HelloWorldFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: hello_world/
      Handler: app.lambda_handler
      Runtime: python3.8
      Events:
        HelloWorld:
          Type: Api
          Properties:
            Path: /hello
            Method: get
```

从这里我们可以了解到，`function` 定义有一个 `CodeUri` 属性，它指向存放源代码的本地路径。

其次，请看 `Events` 属性。此属性定义将触发函数的事件源。此外，当应用程序部署时，SAM 将 `create`（创建）此资源！在本例中，它将创建一个 API 网关。

### template.yaml：Outputs（输出）

```yaml
Outputs:
  HelloWorldApi:
    Description: "API Gateway endpoint URL for Prod stage for Hello World function"
    Value: !Sub "https://${ServerlessRestApi}.execute-api.${AWS::Region}.amazonaws.com/Prod/hello/"
  HelloWorldFunction:
    Description: "Hello World Lambda Function ARN"
    Value: !GetAtt HelloWorldFunction.Arn
  HelloWorldFunctionIamRole:
    Description: "Implicit IAM Role created for Hello World function"
    Value: !GetAtt HelloWorldFunctionRole.Arn
```

顶部的 API 是作为 `Events` 属性的一部分创建的。然而，IAM 角色在任何地方都未定义，那么我们如何获取其属性呢？

请记住，SAM 是一个宏。此角色（以及许多其他资源）将随在此模板中声明的函数一起创建。

## 步骤 8：构建并运行本地测试

接下来，让我们构建此应用程序并运行本地测试：

```
$ sam build --use-container
Starting Build inside a container
Building resource 'HelloWorldFunction'
Fetching lambci/lambda:build-python3.8 Docker container image....
Mounting /Users/karentovmasyan/hello-world/hello_world as /tmp/samcli/source:ro,delegated inside runtime container
Build Succeeded
Built Artifacts  : .aws-sam/build
Built Template   : .aws-sam/build/template.yaml
Commands you can use next
=========================
[*] Invoke Function: sam local invoke
[*] Deploy: sam deploy --guided
Running PythonPipBuilder:ResolveDependencies
Running PythonPipBuilder:CopySource
```

`build` 命令配合 `--use-container` 参数，将拉取 Docker 镜像并构建源函数，从而安装所有依赖项。构建产物将存储在本地目录中。

## 步骤 9：调用函数

让我们调用它：

```
$ sam local invoke
Invoking app.lambda_handler (python3.8)
Fetching lambci/lambda:python3.8 Docker container image...
Mounting /Users/karentovmasyan/hello-world/.aws-sam/build/HelloWorldFunction as /var/task:ro,delegated inside runtime container
START RequestId: c165be27-ff86-1ed2-f5c7-9158dce5d95b Version: $LATEST
END RequestId: c165be27-ff86-1ed2-f5c7-9158dce5d95b
REPORT RequestId: c165be27-ff86-1ed2-f5c7-9158dce5d95b  Init Duration: 199.95 ms  Duration: 3.57 ms  Billed Duration: 100 ms Memory Size: 128 MB  Max Memory Used: 23 MB
{"statusCode":200,"body":"{\"message\": \"hello world\"}"}
```

我们不需要为本地调用提供许多参数，因为只有一个 Lambda 函数和一个事件。但是，如果我们有多个事件和函数，我们就需要按名称指定它们，如下所示：

```
$ sam local invoke FunctionNameAsInTemplate \
  --event path/to/the/event.json
```

在实际响应之前，我们还会获得 Lambda 执行的持续时间和内存信息。当我们需要估算未来无服务器应用程序的成本时，这非常有用。

还有另一种测试函数执行的方法。让我们通过调用 API 网关来做同样的事情。

## 步骤 10：启动 API

我们将启动 API：

```
$ sam local start-api
```

在输出中，将会有一个我们可以调用的 URL。我们必须使用一个不同的终端窗口，因为当前窗口将显示 API 网关的日志：

```
$ curl localhost:3000/hello
{"message": "hello world"}
```

在 API 网关的日志中，我们将看到与运行本地调用时类似的输出。

我们玩得差不多了——现在让我们在云端运行此应用程序！

# 运行 SAM 应用程序

创建和运行 SAM 应用程序与创建 CloudFormation 堆栈略有不同。

遵循之前的步骤，我们将启动我们的 Hello, World 应用程序：

## 步骤 1：打包应用程序

```
$ aws s3 mb s3://mastering-cfn-sam-bucket
# your bucket name might differ!
$ sam package \
  --s3-bucket mastering-cfn-sam-bucket
```

最后一个命令将打包 Lambda 函数及其依赖项，将它们上传到 S3，然后用一个新值（指向 S3 对象）替换 `CodeUri`：

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
# ...
Resources:
  HelloWorldFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: s3://mastering-cfn-sam-bucket/849dd527a4c6568a34af04808b924158
      Handler: app.lambda_handler
      Runtime: python3.8
# the rest of the template...
```

## 步骤 2：部署应用程序

为了在本地存储此模板并将其用于部署，我们需要添加一个额外的参数：

```
sam package \
  --s3-bucket mastering-cfn-sam-bucket \
  --output-template-file template-out.yaml
```

这将把模板存储在工作目录中，并为我们提供创建应用程序堆栈所需的命令：

```
Successfully packaged artifacts and wrote output template to file template-out.yaml.
Execute the following command to deploy the packaged template
sam deploy --template-file /Users/karentovmasyan/MasteringCloudformation/Chapter10/hello-world/template-out.yaml --stack-name <YOUR STACK NAME>
```

让我们使用前面的命令进行部署：

```
$ sam deploy \
  --template-file template-out.yaml \
  --stack-name hello-world
  Deploying with following values
  ===============================
  Stack name           : hello-world
  Region               : None
  Confirm changeset    : False
  Deployment s3 bucket : None
```
