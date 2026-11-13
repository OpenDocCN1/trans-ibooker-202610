
## 复习问题

2. 在 CloudFormation 的转换请求中，什么是 `fragment`？

3. 如果宏函数部署在 `us-east-1` 区域，我们能否在 `us-east-2` 中创建一个使用它的堆栈？

4. 如果宏函数返回的状态不是 `SUCCESS` 会发生什么？

5. 能否在单个模板中使用多个宏？

## 进一步阅读

*   Alex DeBrie 关于 CloudFormation 宏的博客：<https://www.alexdebrie.com/posts/cloudformation-macros/>
*   包含宏示例的 GitHub 仓库：<https://github.com/aws-cloudformation/aws-cloudformation-macros>

# 第 9 章：使用 AWS CDK 生成 CloudFormation 模板

现在我们将了解 AWS 的最新发展之一——`Cloud Development Kit`，即 `CDK`。
CDK 并非旨在取代 CloudFormation。
相反，它的开发是为了给 CloudFormation 增加额外的功能。

CDK 是一个支持多种编程语言的框架，允许 AWS 工程师真正地将基础设施声明为代码，从而提高 AWS 开发速度。
本章结束时，你将拥有一个由 CDK 生成的运行中基础设施。

本章将涵盖以下主题：

*   介绍 AWS CDK
*   使用 AWS CDK 促进模板开发
*   使用 AWS CDK 和 Python 编写你的第一个模板

## 技术要求

本章使用的代码可以在本书的 GitHub 仓库中找到：<https://github.com/PacktPublishing/Mastering-AWS-CloudFormation> 和 <https://github.com/PacktPublishing/Mastering-AWS-CloudFormation/tree/master/Chapter9/app>。

查看以下视频以了解代码实战：

<https://bit.ly/3aPNXxJ>

## 介绍 AWS CDK

CDK 并非首次尝试使用高级编程语言生成 CloudFormation 模板。
由于编写复杂的 CloudFormation 模板可能是一项包含许多重复操作的长期任务，开发者们有兴趣寻找一种不同的模板生成方式。

其中一种方式是一个名为 `Troposphere` 的 Python 框架，由 Cloudtools 团队开发并于 2013 年发布 (<https://github.com/cloudtools/troposphere>)。
AWS Labs 在 2017 年做出了另一次尝试，当时他们发布了一个名为 `GoFormation` 的类似库，用 Golang 编写 (<https://github.com/awslabs/goformation>)。

最终，AWS 推出了一个框架，它不仅提供了完整的基础设施即代码体验，还支持多种编程语言。
该框架被称为 AWS Cloud Development Kit，即 CDK。

CDK 支持以下编程语言：

*   TypeScript
*   JavaScript
*   Python
*   C#
*   Java

CDK 团队承诺会向此列表添加更多语言，但在此期间，任何愿意将 CDK 集成到其开发工作流中的公司都应使用其最熟悉的语言。

作为一个框架，CDK 有一个术语叫 `constructs`（构造），它代表需要预配置的基础设施构建块。
这些构造可以是以下内容：

*   `CloudFormation 资源`：示例包括 S3 存储桶、DynamoDB 表和 EC2 实例。
*   `Apps`：作为单一目的的资源逻辑分组，并生成一个或多个堆栈。
*   `Stacks`：部署单元，类似于 CloudFormation 堆栈。
*   `Environments`：部署目标；可以是不同的区域和/或账户。
*   `Assets`：与应用程序捆绑的本地文件或 Docker 镜像。可以是 Lambda 函数的处理程序代码或 ECS 任务的 Docker 镜像。

当模板开发者开始使用 CDK 时，他们需要将每个资源定义为变量——例如，S3 存储桶的定义如下所示。

如果你还不理解下面的代码，别担心！我们稍后会介绍：

```python
from aws_cdk import core
import aws_cdk.aws_s3 as s3

class BucketStack(core.Stack):
    def __init__(self, scope: core.Construct, id: str, **kwargs) -> None:
        super().__init__(scope, id, **kwargs)
        bucket = s3.Bucket(self, "myBucket")

app = core.App()
BucketStack(app, "bucket", env=core.Environment(account="X"))
app.synth()
```

在此代码中，堆栈实际上是一个 Python `class`（`BucketStack`），带有必要的构造属性。
我们声明一个 `bucket` 变量，它是 `s3.Bucket` 类型的对象，其中 `s3` 是一个导入的库，即 `aws_cdk.aws_s3`。
堆栈被分配为应用程序 `App` 的成员。
我们还指定了要部署的账户，但我们将在下一节中介绍这一点。

一旦我们构建自己的模板，将获得更深入的 CDK 知识，但首先，让我们看看 CDK 如何让我们的模板开发生活更轻松。

## 使用 AWS CDK 促进模板开发

如果我们已经拥有丰富的 CloudFormation 经验，为什么还要选择 CDK？
虽然 CloudFormation 拥有许多强大的功能和与 AWS 的深度集成，但其声明式的本质可能会给刚开始实践基础设施即代码和 AWS 的人带来负担。

当你需要创建一组示例资源并希望将大多数属性设置为默认值时，CloudFormation 很简单。
但让我们看看 S3 存储桶的完整声明：

```yaml
Type: AWS::S3::Bucket
Properties:
  AccelerateConfiguration: ...
  AccessControl: ...
  AnalyticsConfigurations: [...]
  BucketEncryption: ...
  BucketName: ...
  CorsConfiguration: ...
  InventoryConfigurations: [...]
  LifecycleConfiguration: ...
  LoggingConfiguration: ...
  MetricsConfigurations: [...]
  NotificationConfiguration: [...]
  # And counting!..
```

大多数属性甚至不是字符串或布尔值；它们是需要填充的单独键值部分。
这不是最难的任务，但它需要时间来找到适当的声明部分，并避免弄乱 YAML 或 JSON 格式。
添加属性块甚至额外资源可能更简单。

假设我们想要在一个 VPC 中拥有一个与路由表关联的子网，该路由表有一条通往互联网的路由。

使用 CloudFormation 将包含以下资源：

```yaml
Vpc:
  Type: AWS::EC2::VPC
  Properties: ...
Igw:
  Type: AWS::EC2::InternetGateway
IgwAttach:
  Type: AWS::EC2::VPCGatewayAttachment
  Properties: ...
Subnet:
  Type: AWS::EC2::Subnet
  Properties: ...
RouteTable:
  Type: AWS::EC2::RouteTable
  Properties: ...
PublicRoute:
  Type: AWS::EC2::Route
  Properties: ...
PublicRouteTableAssoc:
  Type: AWS::EC2::SubnetRouteTableAssociation
  Properties: ...
```

同样，这是我们必须进行的最少资源声明数量。

在 CDK 中执行此操作将更简单，因为其编程性质：

```python
CIDR = "10.0.0.0/16"
vpc = ec2.Vpc(self,
              "Vpc",
              cidr=CIDR,
              subnet_configuration=[
                  ec2.SubnetConfiguration(name="Public", subnet_type=ec2.SubnetType.PUBLIC, cidr_mask=24),
                  ec2.SubnetConfiguration(name="Private", subnet_type=ec2.SubnetType.PRIVATE, cidr_mask=24)
              ])
```

因为 CDK 内部有大量抽象，我们不需要显式地逐一定义资源——CDK 会为我们完成这项工作。

另一个有益的特性是我们可以使用编程语言的所有能力，例如迭代和条件判断。

例如，假设我们想要在多个区域部署我们的堆栈。
我们可以通过初始化 `Stack` 类来指示 CDK 创建一个堆栈：

```python
MyStack(app, "StackName", env=core.Environment(account="X",
                                               region="foo"))
```
