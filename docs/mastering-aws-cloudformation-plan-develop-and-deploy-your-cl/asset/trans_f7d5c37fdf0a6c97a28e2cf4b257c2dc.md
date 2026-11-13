
至少有四个属性是强制性的：`image`、`network port`、`count`、`memory` 和 `CPU`；否则，我们无法为 ECS 任务声明任何内容。让我们在 Lambda 处理程序中添加检查：

`standard-app.py`

```python
def lambda_handler(event, context):
    # ...
    required_properties = ["ApplicationImage", "ApplicationPort", "TaskCount", "Memory", "CPU"]
    properties = event['fragment']['Resources']['Application']['Properties']
    for req in required_properties:
        if req not in properties.keys() or not properties[req]:
            response['status'] = "FAILED"
            return response
    # ...
```

如果至少有一个属性未设置或设置为 `null`，Lambda 将响应 `FAILED`。

除了必需属性外，我们还有可选属性。这些属性声明是否需要额外资源（如 RDS 实例或 ELB）及其属性。

`RDSEngine`、`RDSSize` 和 `RDSMultiAz` 属性提供我们将要配置的 RDS 实例的信息。如果未指定 `RDSEngine`，我们假设不需要数据库。对于 `RDSSize`，我们将使用默认值 `db.t2.micro`；对于 `RDSMultiAz`，默认值为 `false`。让我们将其添加到处理程序中：

`standard-app.py`

```python
def lambda_handler(event, context):
    # ...
    rds_props = {}
    rds_props['RDSEngine'] = ""
    rds_props['RDSSize'] = "db.t2.micro"
    rds_props['RDSMultiAz'] = "false"
    for key in rds_props.keys():
        if key in properties.keys() and properties[key]:
            rds_props[key] = properties[key]
    # ...
```

这里我们将默认值设置给 RDS 属性，并将 `RDSEngine` 设为 `null`。如果在 CloudFormation 请求中提供了这些值，我们将覆盖它们。

我们将对 ELB 属性执行相同操作：

`standard-app.py`

```python
def lambda_handler(event, context):
    # ...
    elb_props = {}
    elb_props['NeedBalancer'] = False
    elb_props['PubliclyAvailable'] = False
    for key in elb_props.keys():
        if key in properties.keys() and properties[key]:
            elb_props[key] = properties[key]
    # ...
```

现在我们已整理好所有属性，可以开始开发配置逻辑。

## 从宏声明资源

由于宏必须返回有效的 JSON 模板，这是我们第一次（希望也是最后一次）以 JSON 格式编写模板。

我们将改进 `render_ecs()` 函数，使其返回有效的声明。我们已经知道会接收应用程序镜像、端口、CPU 共享设置、任务数量和内存设置属性。

同时，我们知道需要配置 CloudWatch 日志、ECS 任务定义及其服务，两者均兼容 ECS Fargate。

让我们构建 ECS 任务定义的框架。我们将遵循与 CloudFormation JSON 模板相同的格式（完整版本可在 GitHub 上找到）：

`standard-app.py`

```python
def render_ecs(input):
    # ...
    ecs_task_definition = {
        # ...
        "Family": "",  # placeholder
        # ...
        "Name": "",  # placeholder
        "Image": "",  # placeholder
        # ...
        "ContainerPort": "",  # placeholder
        # ...
        "awslogs-stream-prefix": ""  # placeholder
        # ...
        "Cpu": "",  # placeholder
        "Memory": "",  # placeholder
        # ...
    }
```

我们的 ECS 任务定义现在是一个字典对象。我们需要用输入中的值填充这些占位符：

`standard-app.py`

```python
def render_ecs(input):
    # ...
    application_image = input['ApplicationImage']
    application_name = application_image.strip(":")
    ecs_task_definition_properties = ecs_task_definition['TaskDefinition']['Properties']
    ecs_task_definition_properties['Family'] = application_name
    ecs_task_definition_properties['ContainerDefinitions'][0]['Name'] = application_name
    ecs_task_definition_properties['ContainerDefinitions'][0]['Image'] = application_image
    # and so on...
```

任务定义填入所有值后，我们可以继续配置 ECS 和 CloudWatch 日志。

最终，我们的函数将如下所示：

`standard-app.py`

```python
def render_ecs(input):
    ecs_definition = {}
    ecs_task_definition = { ... }
    # generation of ecs_task_definition...
    ecs_service_definition = { ... }
    # generation of ecs_service_definition...
    ecs_definition['Logs'] = { ... }
    ecs_definition['Sg'] = { ... }
    # generation of the rest...
    ecs_definition.update(ecs_task_definition)
    ecs_definition.update(ecs_service_definition)
    return ecs_definition
```

最后，我们将获得应用程序所需的 CloudFormation 资源集合，例如 ECS 任务定义、服务、CloudWatch 日志、EC2 安全组等。

额外资源（ELB 和 RDS 实例）的生成方式类似，因此我们不再重点介绍，直接跳转到宏的部署。

## 部署标准应用

通过以下步骤，我们将部署宏并进行测试：

1.  由于我们的应用程序需要基础设施（ECS 集群、网络等），我们首先需要配置这些资源（可在代码库中找到）：

    ```bash
    $ aws cloudformation deploy --stack-name core \
        --template-file core.yaml \
        --capabilities CAPABILITY_IAM
    ```

2.  部署完成后，我们可以配置宏。我们的代码太大，无法放入 CloudFormation 模板，因此必须手动上传到 S3：

    ```bash
    $ aws s3 mb s3://masteringcfn  # your bucket name may differ
    $ zip lambda-macro.zip standard-app.py
    $ aws cloudformation deploy --stack-name standard-app-macro \
        --template-file macro.yaml \
        --capabilities CAPABILITY_IAM
    ```

3.  准备就绪。我们可以继续部署应用程序：

    ```bash
    $ aws cloudformation deploy --stack-name app \
        --template-file app.yaml
    ```

4.  让我们查看 CloudFormation 控制台。我们关注模板处理后的样子：

![Figure 8.3 – CloudFormation console – unprocessed template](image/B10533_08_003.jpg)

Figure 8.3 – CloudFormation console – unprocessed template

但我们关注的是处理后的模板：

![Figure 8.4 – CloudFormation console – processed template](image/B10533_08_004.jpg)

Figure 8.4 – CloudFormation console – processed template

大功告成！我们成功创建了第一个处理整个模板的宏！有了这些知识，我们可以根据需要调整和扩展 CloudFormation 模板。

## 概要

在本章中，我们了解了宏——一种允许我们转换模板的工具。我们学习了 AWS 自带的宏——`AWS::Serverless` 和 `AWS::Include`——以及宏的组成。

为了理解宏处理，我们还开发了自己的宏。现在我们知道如何以及何时使用它们。

宏让 AWS 工程师能够避免模板中的复杂性，将复杂性交由 Lambda 函数解决。宏如何使模板编写和阅读变得简单的最佳示例是 AWS `Serverless Application Model (SAM)`，我们将在 [*第 10 章*](#B10533_10_Final_NM_ePUB.xhtml#_idTextAnchor182) *使用 AWS SAM 部署无服务器应用程序* 中详细介绍。

在下一章中，我们将学习 AWS **Cloud Development Kit (CDK)**。CDK 引入了一种完全不同的创建和管理 CloudFormation 栈的方式，允许 AWS 工程师以编程方式声明基础设施。

下一章见！

## 问题

1.  当宏处理模板时，以何种格式返回？
