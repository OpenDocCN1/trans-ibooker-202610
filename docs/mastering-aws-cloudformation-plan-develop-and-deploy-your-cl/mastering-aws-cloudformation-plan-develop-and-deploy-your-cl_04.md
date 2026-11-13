## 第 2 章：高级模板开发

牢记 CloudFormation 的基础知识，让我们现在转向高级开发技术。

在本章中，我们将深入了解 CloudFormation 模板的核心部分，并通过构建一个三层应用程序来实现，该应用程序由 Web 层（面向客户的前端）、中间件层（处理来自前端的命令）和存储层（存储记录的数据库）组成。

别担心！我们还不打算编写任何软件（仅编写 CloudFormation 模板）。目前，我们将专注于预置基础设施。

本章将涵盖以下主题：



*   深入了解模板内部结构
*   创建可复用模板
*   使用条件元素
*   删除策略
*   引用现有堆栈
*   AWS 伪参数
*   结合 Parameter Store 和 Secrets Manager 的动态引用

### 技术要求

本章使用的代码可在本书的 GitHub 仓库中找到：[`github.com/PacktPublishing/Mastering-AWS-CloudFormation/tree/master/Chapter2`](https://github.com/PacktPublishing/Mastering-AWS-CloudFormation/tree/master/Chapter2)。

### 深入了解模板内部结构

模板是 CloudFormation 的主要构建块。我们可以将模板视为给 CloudFormation 的声明式指令——例如，创建什么资源以及创建多少个。

无论模板文件是 JSON 还是 YAML 格式，都由若干元素组成，我们将在本节其余部分进行描述。

### AWSTemplateFormatVersion

`AWSTemplateFormatVersion` 部分负责指示 CloudFormation 我们将提供哪个版本的模板。

不要将其与 CloudFormation 的 API 版本混淆，`AWSTemplateFormatVersion` 关乎模板的结构。

当 AWS 添加此模块以解决 CloudFormation 潜在的破坏性变更时，它只能有一个值：`2010-09-09`。此版本自 CloudFormation 诞生之初就存在，但 AWS 始终有可能为模板格式添加更新的版本。在此之前，此部分是可选的，如果未提供，CloudFormation 将假定您的模板具有上述默认格式版本。

### Description

Description 部分是可选的，仅服务于一个目的——解释您的模板是什么，以及如果将其推送到 CloudFormation 将创建什么内容。它不允许引用参数，并支持单行或多行值。

您的描述可以如下所示：

```
Description: Core network template consisting of VPC, IGW, RouteTables, and Subnets
```

也可以如下所示：

```
Description: >

Core network template. Consists of:

- VPC
- IGW
- Subnets
- RouteTables
```

没有具体规则，但请记住，与模板中的注释不同，如果您通过 API、AWS CLI 或 AWS Console 访问，描述也会在 CloudFormation 服务中显示。

总而言之，Description 的最佳实践是为 CloudFormation 用户存储有用信息。

### Metadata

虽然 Metadata 部分看似没什么用，但它增加了额外的配置能力。

您可以将元数据与 CloudFormation 特定资源结合使用，例如：

*   `AWS::CloudFormation::Init`
*   `AWS::CloudFormation::Interface`
*   `AWS::CloudFormation::Designer`

我们稍后将更详细地介绍这些内容。

### Parameters

`Parameters` 是最重要但却是可选的部分之一，它允许我们使模板可复用。将参数视为堆栈的变量：VPC 和子网的 CIDR 范围、EC2 和 RDS 实例的实例类型，等等。

参数的用法因场景而异。使用参数最流行的方式之一是允许在不更改模板资源的情况下创建不同的堆栈。

例如，请看以下代码：

```
Parameters:
  InstanceType:
    Type: String

Resources:
  Ec2Instance:
    Type: AWS::EC2::Instance
    Properties:
      # …
      InstanceType: !Ref InstanceType
      # …
```

在这种情况下，您将 EC2 的 `InstanceType` 与实际模板分开定义，这意味着您可以为不同的环境（开发、测试、生产）或其他情况选择不同的类型。

重要的是要知道参数具有以下属性：`Default`、`AllowedValues` 和 `AllowedPattern`。

这些属性不是强制性的，但使用它们是最佳实践。

例如，假设您想将 `Environment` 标签指定为参数，并且您有 `dev`、`test` 和 `prod` 环境。您可能希望防止模板用户指定不应存在的标签（如 `foo` 或 `bar`）。为此，您需要使用 `AllowedValues`：

```
Parameters:
  Environment:
    Type: String
    AllowedValues: [dev, test, prod]
```

当您希望在特定正则表达式中匹配参数时，应使用 `AllowedPattern`。例如，当您需要提供 IP 地址或 CIDR 范围时，您需要确保完全传递以下内容：

```
Parameters:
  SubnetCidr:
    Type: String
    AllowedPattern: '((\\d{1,3})\\.){3}\\d{1,3}/\\d{1,2}'
```

> **重要提示**
> 您必须确保遵循 Java 正则表达式语法！

`Default` 是一个有助于确保参数始终具有值的属性。根据设计，当您尝试创建堆栈但未指定参数值时，模板验证（在创建前步骤）将会失败。

在本例中，我们将 Docker 镜像标签作为参数传递给 ECS 任务定义——容器定义。为了确保在任何情况下都有镜像，我们将默认值设为 `latest`：

```
Parameters:
  DockerImageVersion:
    Type: String
    Default: latest
```

另一方面，有时您需要复查模板用户是否始终清楚自己在做什么，因此作为模板开发者，由您决定是否要设置默认值。

不用说，参数具有类型。类型列表可在 CloudFormation 文档 [页面](https://docs.aws.amazon.com/en_pv/AWSCloudFormation/latest/UserGuide/parameters-section-structure) 上找到。

最常用的类型之一是 `String`，因为大多数 `Resources` 的属性值都需要此数据类型，但我们也可以指定数组和数字。

### AWS 特定参数类型

使用 AWS 特定参数类型非常方便，例如 `AWS::EC2::AvailabilityZone::Name`、`AWS::EC2::Image::Id`、`AWS::EC2::Instance::Id`、`AWS::EC2::KeyPair::KeyName` 和 `AWS::EC2::VPC::Id`。其好处在于，CloudFormation 会在验证期间检查提供给此特定参数类型的值对于此类资源是否有效。

例如，如果您想将子网 ID 指定为参数，您将不得不为其编写正确的正则表达式，而 `AWS::EC2::Subnet::Id` 已内置了此检查。

### Mappings

`Mappings` 与参数类似，但以字典格式提供。

### Mappings 内部函数

与使用 `Fn::Ref` 获取值的参数不同，Mappings 的值可以使用 `Fn::FindInMap` 函数引用。

Mappings 最常见的用法是为不同区域指定 AMI ID：

```
Mappings:
  RegionMap:
    us-east-1:
      HVM64: ami-0ff8a91507f77f867
      HVMG2: ami-0a584ac55a7631c0c
    us-west-1:
      HVM64: ami-0bdb828fd58c52235
      HVMG2: ami-066ee5fd4a9ef77f1
    eu-west-1:
      HVM64: ami-047bb4163c506cd98
      HVMG2: ami-0a7c483d527806435
    ap-northeast-1:
      HVM64: ami-06cd52961ce9f0d85
      HVMG2: ami-053cdd503598e4a9d
    ap-southeast-1:
      HVM64: ami-08569b978cc4dfa10
      HVMG2: ami-0be9df32ae9f92309

Resources:
  Ec2Instance:
    Type: "AWS::EC2::Instance"
    Properties:
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", HVM64]
```

请注意，维护 Mappings 可能比较麻烦，因此使用它们的最佳方式是存储**很少更改的常量**值。

### Conditions

`Conditions` 与 `parameters` 类似，但其值为 `true` 或 `false`。Conditions 在模板的单独块中声明，并在资源声明中引用。

> **重要提示**




### 条件

请注意，声明中的 `Conditions` 不能手动设置为 `true` 或 `false`。布尔值仅从内部函数的结果中获取，例如 `Fn::Equals`、`Fn::If`、`Fn::Not`、`Fn::And` 和 `Fn::Or`！

例如，请看以下代码：

```yaml
AWSTemplateFormatVersion: '2010-09-09'

Parameters:
  Env:
    Default: dev
    Description: 定义环境（dev、test 或 prod）
    Type: String
    AllowedValues: [dev, test, prod]

Conditions:
  IsProd: !Equals [!Ref Env, 'prod']

Resources:
  Bucket:
    Type: "AWS::S3::Bucket"
    Condition: IsProd
```

在这种情况下，我们必须手动提供 `Env` 参数，其值必须等于 `prod`；否则，资源将*不会*被创建。

`Conditions` 与特定的布尔内部函数配合工作，但这些函数也可以在条件块之外工作。当我们开始开发模板时，我们将更详细地介绍这些内容。

### Transform

当我们想要在模板中运行 CloudFormation 特定的宏时，会声明 transform 块。这是一个深度话题，我们将在本书的最后一部分 *Extending CloudFormation*（扩展 CloudFormation）中介绍。

### Resources

这是 `template` 文件中主要且唯一必需的块。顾名思义，资源部分提供我们想要预配置的资源。

我们创建的资源类型可能各不相同。有人认为任何 AWS 资源都可以由 CloudFormation 创建，但有多个服务（如 AWS Organizations、Control Tower 等）不支持 CloudFormation。

在开始创建堆栈之前，您必须首先查阅 CloudFormation 文档中的资源和属性参考，以确保您想要使用的服务受 CloudFormation 支持。更多信息，请访问 [`docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html`](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)。

资源具有多个属性，列表如下：

*   `Type`：资源类型（实例、子网、DynamoDB 表、Lambda 函数等）。
*   `Properties`：资源的配置。
*   `DependsOn`：用于添加间接依赖。
*   `CreationPolicy`：用于通知 CloudFormation 等待完成信号（我们将在创建 AutoScaling 组时介绍此内容）。
*   `DeletionPolicy`：用于指示当我们从堆栈中删除资源（或连同堆栈一起删除）时，CloudFormation 应如何处理该资源——例如，从*状态*中删除。用于重要资源，如存储后端。
*   `UpdatePolicy`：用于指示 CloudFormation 如何处理 AutoScaling 组、ElastiCache 复制组和 Lambda 函数别名的间接更新。
*   `UpdateReplacePolicy`：与 `DeletionPolicy` 类似，`UpdateReplacePolicy` 用于在资源必须被新资源替换时管理剩余资源。它在同一逻辑资源和默认策略 `Delete` 内工作。

### Outputs

Outputs 是我们在堆栈创建后从堆栈中*导出*的值。

Outputs 本身并不带来太多好处，但在上一章中，我们使用 outputs 来自动获取我们创建的 IAM 角色的 ARN。

Outputs 可以检索资源的物理名称或 ID 及其属性。

例如，当我们创建 IAM 用户时，可以检索其 API 密钥和密钥，并提供给最终用户：

```yaml
Resources:
  Bob:
    Type: AWS::IAM::User
    Properties:
      UserName: Bob
  BobApiKey:
    Type: AWS::IAM::AccessKey
    Properties:
      UserName: !Ref Bob

Outputs:
  BobKey:
    Value: !Ref BobApiKey
  BobSecret:
    Value: !GetAtt BobApiKey.SecretAccessKey
```

正如您所见，我们检索了 Bob 的 API 密钥和密钥，并将其暴露在 outputs 中。

### 重要提示

请注意，任何有权阅读堆栈描述的人都可以访问 outputs，因此暴露安全凭证是一项有风险的操作。如果您需要为此使用 outputs，请确保只有授权用户才能访问您的堆栈，即使他们只有只读权限。

Outputs 还有一个强大的功能称为 exports（导出值）。Exports 允许模板开发人员引用在另一个堆栈中创建的现有资源。对于大型工作负载，使用 exports 是最佳实践，因为在这些场景中，您通常有一个共享堆栈（被许多应用程序使用的资源）和特定于应用程序的堆栈。

现在，让我们开始开发我们的模板。在继续之前，请确保您理解前面的所有部分，因为我们将在后续章节中大量依赖它们。

### 创建可复用模板

我们已经知道我们将部署一个三层应用程序，但我们将拥有不止三个堆栈。

我们将创建的第一个堆栈是核心堆栈。

该堆栈将包含以下内容：

*   网络（VPC、子网等）
*   IAM（角色和用户）

我们将为应用程序准备两个环境：测试和生产。这些环境在规模、资源数量和安全设置方面会有所不同。

我们的模板代码将非常庞大，因此您将只看到与特定主题相关的代码块；完整的源代码可以在 Packt 的代码仓库中找到。

在开始之前，让我们思考一下如何组织模板及其参数。由于我们将为两个不同的堆栈（生产和测试）复用同一个模板，我们需要分离网络范围并使用不同的命名约定。

在网络方面，我们的堆栈将包含以下内容：

*   1 个 VPC
*   3 个公有子网
*   3 个 WebTier 子网
*   3 个中间件子网
*   3 个数据库子网
*   1 个互联网网关
*   1 个 NAT 网关
*   1 个公有路由表
*   1 个私有路由表
*   2 个 IAM 角色（面向管理员和开发人员）

这些资源将拥有诸如 `Name` 和 `Env` 之类的标签。

在这种情况下，我们希望为资源参数化属性，例如 CIDR 范围和标签，因此我们将拥有以下参数：

*   VPC CIDR 范围
*   公有子网 1 CIDR 范围
*   公有子网 2 CIDR 范围
*   公有子网 3 CIDR 范围
*   WebTier 子网 1 CIDR 范围
*   WebTier 子网 2 CIDR 范围
*   WebTier 子网 3 CIDR 范围
*   中间件子网 1 CIDR 范围
*   中间件子网 2 CIDR 范围
*   中间件子网 3 CIDR 范围
*   数据库子网 1 CIDR 范围
*   数据库子网 2 CIDR 范围
*   数据库子网 3 CIDR 范围
*   环境

所有这些参数的类型均为 `String`。让我们编写 `Parameters` 部分：

```yaml
Parameters:
  VpcCidr:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  PublicSubnetCidr1:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  PublicSubnetCidr2:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  PublicSubnetCidr3:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  # And so on...
  DatabaseSubnetCidr1:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  DatabaseSubnetCidr2:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  DatabaseSubnetCidr3:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  Environment:
    Type: String
    AllowedValues: ['prod', 'test']
```

这就是我们核心堆栈的参数部分。现在，在资源中，我们将需要使用内部函数 `Fn::Ref` 来在资源属性中使用参数的值。我们还将使用另一个内部函数 `Fn::Join` 来拼接一个漂亮的字符串名称用于我们的 VPC：

```yaml
Resources:
  Vpc:
    Type: "AWS::EC2::VPC"
    Properties:
      CidrBlock: !Ref 'VpcCidr'
      EnableDnsHostnames: True
      EnableDnsSupport: True
      InstanceTenancy: Default
      Tags:
        - Key: 'Name'
```





### CloudFormation 内部函数与参数

### 使用 `Fn::Ref` 引用资源

```yaml
Value: !Join ['-', [!Ref 'Environment', 'vpc']]

- Key: 'Env'
  Value: !Ref 'Environment'
```

其余资源将使用类似的模式。

### 重要提示

`Fn::Ref` 函数也可用于引用堆栈中某一资源的特定属性以供另一资源使用。在这种情况下，我们向 `Fn::Ref` 提供资源的逻辑名称，而不是参数。

在下面的示例中，我们使用 `Fn::Ref` 引用 `VpcId` 以关联我们即将创建的子网：

```yaml
PublicSubnet1:
  Type: "AWS::EC2::Subnet"
  Properties:
    CidrBlock: !Ref 'PublicSubnetCidr1'
    VpcId: !Ref Vpc
```

请注意，我在模板文件中未指定参数的默认值。在这种情况下，我必须在 `create-stack` 命令中提供它们；否则，验证将无法通过。

### 向 CloudFormation 提供参数

向 CloudFormation 提供参数主要有两种方式：作为 JSON 文件或作为位置参数。

### 使用 JSON 参数文件

对于包含大量参数的大型模板，建议使用 JSON 文件：

```json
[
  {
    "ParameterKey": "...",
    "ParameterValue": "..."
  }
]
```

在我们的案例中，它将如下所示：

**testing.json**

```json
[
  {
    "ParameterKey": "Environment",
    "ParameterValue": "Testing"
  },
  {
    "ParameterKey": "VpcCidr",
    "ParameterValue": "10.0.0.0/16"
  },
  {
    "ParameterKey": "PublicSubnetCidr1",
    "ParameterValue": "10.0.1.0/24"
  },
  {
    "ParameterKey": "PublicSubnetCidr2",
    "ParameterValue": "10.0.2.0/24"
  }
  // 以此类推...
]
```

创建模板时，我们在参数中指定参数文件：

```bash
$ aws cloudformation create-stack \
  --stack-name core \
  --template-body file://core.yaml \
  --parameters file://testing.json
```

### 使用位置参数

如果我们有默认值且只想进行少量更改，可以逐个将参数指定为位置参数：

```bash
$ aws cloudformation create-stack \
  --stack-name core \
  --template-body file://core.yaml \
  --parameters \
  ParameterKey="Environment",ParameterValue="Testing" \
  ParameterKey="VpcCidr",ParameterValue="10.0.0.0/16"
```

如果参数较少，这种方式很合适，但对于大量参数来说过于复杂。

### 重要提示

参数作为列表提供，元素由空格分隔。请注意 `List` 类型参数，您需要使用双反斜杠：

`ParameterKey=List,ParameterValue=Element1\\\\,Element2`

### 使用列表参数与 `Fn::Select`

让我们对参数做一个小调整。由于每层每个子网都有多个 CIDR 范围，让我们改用 `List` 参数而非 `String`：

```yaml
Parameters:
  VpcCidr:
    Type: String
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  PublicSubnetCidrs:
    Type: List<String>
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  # 以此类推...
  MiddlewareSubnetCidrs:
    Type: List<String>
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  DatabaseSubnetCidrs:
    Type: List<String>
    AllowedPattern: '((\d{1,3})\.){3}\d{1,3}/\d{1,2}'
  Environment:
    Type: String
    AllowedValues: ['prod', 'test']
```

由于使用了列表，我们无法再使用 `Fn::Ref`（严格来说可以，但会传递整个列表）。取而代之，我们将使用 `Fn::Select`：

```yaml
PublicSubnet1:
  Type: "AWS::EC2::Subnet"
  Properties:
    CidrBlock: !Select [0, 'PublicSubnetCidrs']
    VpcId: !Ref Vpc

PublicSubnet2:
  Type: "AWS::EC2::Subnet"
  Properties:
    CidrBlock: !Select [1, 'PublicSubnetCidrs']
    VpcId: !Ref Vpc

PublicSubnet3:
  Type: "AWS::EC2::Subnet"
  Properties:
    CidrBlock: !Select [2, 'PublicSubnetCidrs']
    VpcId: !Ref Vpc
```

我们仍可使用 `AllowedPattern`，从而确保使用正确的变量。现在，我们需要稍微修改参数文件：

```json
{
  "ParameterKey": "PublicSubnetCidrs",
  "ParameterValue": [
    "10.0.1.0/24",
    "10.0.2.0/24",
    "10.0.3.0/24"
  ]
}
```

现在看起来好多了，对吧？

### 使用 `Fn::Cidr` 进行子网计算

另一方面，逐个指定 CIDR 范围可能既棘手又费力，尤其是当您拥有大量子网时。专门针对子网范围，我们可以使用一个名为 `Fn::Cidr` 的内部函数。

`Fn::Cidr` 函数用于将较大的子网范围拆分为较小的范围。这听起来起初可能有点难，但如果您理解如何计算子网掩码，您将受益于减小参数文件体积，同时在模板中保留所需的逻辑。

让我们回到 VPC。我们在参数中指定 VPC CIDR 范围。我们的参数文件现在应如下所示：

```json
[
  {
    "ParameterKey": "VpcCidr",
    "ParameterValue": "10.0.0.0/16"
  }
]
```

为了将我们的大子网拆分为较小的子网（`/24`），我们需要向 `Fn::Cidr` 函数提供以下参数：

- **ipBlock**：我们的 CIDR 范围。
- **count**：我们想要生成多少个 CIDR 范围。
- **cidrBits**：这将告诉 CFN 生成哪些范围。对于 `/24` CIDR 范围，我们需要指定 `8`。

我们知道将有九个子网（公共、中间件和数据库子网各三个），因此我们的内部函数将如下所示：

```yaml
Fn::Cidr:
  - !Ref VpcCidr
  - 9
  - 8
```

或者，使用短语法，它将如下所示：

`!Cidr [!Ref VpcCidr, 9, 8]`

此函数返回一个 CIDR 范围列表，因此当我们声明子网时，需要将其与 `Fn::Select` 结合使用。我们的模板将如下所示：

```yaml
PublicSubnet1:
  Type: "AWS::EC2::Subnet"
  Properties:
    CidrBlock: !Select [0, !Cidr [!GetAtt Vpc.CidrBlock, 9, 8]]
    VpcId: !Ref Vpc

PublicSubnet2:
  Type: "AWS::EC2::Subnet"
  Properties:
    CidrBlock: !Select [1, !Cidr [!GetAtt Vpc.CidrBlock, 9, 8]]
    VpcId: !Ref Vpc

PublicSubnet3:
  Type: "AWS::EC2::Subnet"
  Properties:
    CidrBlock: !Select [2, !Cidr [!GetAtt Vpc.CidrBlock, 9, 8]]
    VpcId: !Ref Vpc
```

使用 `Fn::Cidr` 将允许您避免在参数中逐个指定 CIDR 范围。

### 重要提示

使用此函数时应格外小心，因为它要求您非常好地规划和设计网络。

您每次创建子网都会使用 `Fn::Cidr`，并且每次都需要确保 CIDR 范围块的数量相同；否则，您将遇到子网划分问题，导致堆栈部署失败。

如果您单独创建网络堆栈，架构设计良好，且清楚自己在做什么，那么请坚持使用 `Fn::Cidr`。

我们将大量使用参数和内部函数，但现在让我们了解 CloudFormation 的另一个重要功能，称为 `Conditions`（条件）。

### 使用条件元素

我们希望拥有可复用的模板，但有时我们需要在一种情况下创建资源，在另一种情况下不创建，或者可能使用不同的属性创建资源。

条件可解决此类问题，主要有两种用法：在资源下指定 `Condition`，或在资源属性中使用条件内部函数。

我们已经知道 `Condition` 是一个严格的布尔变量，由参数和条件函数求值得出。我们已经学习了条件在资源声明中的用法（如需复习，请跳转至*深入模板内部*一节），现在让我们看另一个有用的示例。

假设我们有一个由启动模板构建的 `AutoScaling` 组。我们预期测试环境和生产环境的负载不同，因此希望相应地调整 EC2 实例的规格。目前，我们在测试环境使用 `t3.micro`，在生产环境使用 `m5.large`。




### CloudFormation 中的条件

因为我们只有两个环境，所以可以只使用一个条件——`ProdEnv`。为什么？因为在资源属性中，我们使用了一个条件函数 `Fn::If`。我们使用 `Fn::If`，因为它很明显有三个输入：条件、`True` 时的值和 `False` 时的值。因此，如果不是生产环境，我们肯定知道它是测试环境。

###### 让我们开发模板

### 参数

```yaml
Parameters:
  Env:
    Type: String
    Default: test
    AllowedValues: ["test", "prod"]
  ImageId:
    Type: AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>
    Default: '/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2'
```

### 条件

```yaml
Conditions:
  IsProd: !Equals [!Ref Env, "prod"]
```

### 资源

```yaml
Resources:
  WebLt:
    Type: AWS::EC2::LaunchTemplate
    Properties:
      LaunchTemplateName: web
      LaunchTemplateData:
        ImageId: !Ref ImageId
        InstanceType: !If [IsProd, m5.large, t3.micro]
```

### 长语法格式

最后一行的长语法格式如下：

```yaml
InstanceType:
  Fn::If:
    - IsProd
    - m5.large
    - t3.micro
```

### 多环境条件

`Fn::If` 函数也支持嵌套。如果我们有三个环境（测试、预发布和生产），那么我们需要在这些环境中使用不同的实例类型，并且需要额外的条件和稍长的条件函数：

### 条件

```yaml
Conditions:
  IsProd: !Equals [!Ref Env, "prod"]
  IsStaging: !Equals [!Ref Env, "stage"]
```

### 资源

```yaml
Resources:
  WebLt:
    Type: AWS::EC2::LaunchTemplate
    Properties:
      LaunchTemplateName: web
      LaunchTemplateData:
        ImageId: !Ref ImageId
        InstanceType: !If [IsProd, m5.large, !If [IsStaging, t3.large, t3.micro]]
```

条件有各种用例，其中大多数会在你开始在 AWS 上构建大型基础设施时出现。虽然你会经常发现它们很方便，但请记住本书中的限制和使用示例。

现在让我们转到另一个重要主题——删除策略。

### 删除策略

当我们创建堆栈时，需要确保关键任务资源免受意外删除。

在某些情况下，这通过 `EnableTerminationProtection` 为 EC2 和 RDS 等服务启用。装有对象的 S3 存储桶将无法删除，因为必须先清空它们。

删除策略允许你在 CloudFormation 中降低此风险。此外，删除策略除了基本的终止保护外，还提供了更多功能。

例如，假设你创建了一个测试堆栈，测试阶段结束后不再需要它，但你需要数据库的转储（实际上是 RDS 实例的快照）。有时，你不想重新创建相同的数据结构，或者数据库中已经有你想要移至生产环境的重要数据。

让我们看看删除策略是否能帮到我们：

### 快照删除策略

```yaml
Resources:
  VeryImportantDb:
    Type: AWS::RDS::Instance
    DeletionPolicy: Snapshot
    Properties:
      # Here you set properties for your RDS instance.
```

当我们从模板中移除此资源或删除堆栈时，CloudFormation 会通知 RDS 对实例创建最终快照，然后将其删除。

### 重要提示

删除策略快照并非适用于每一种资源。撰写本文时，支持的资源包括 `AWS::EC2::Volume`、`AWS::ElastiCache::CacheCluster`、`AWS::ElastiCache::ReplicationGroup`、`AWS::Neptune::DBCluster`、`AWS::RDS::DBCluster`、`AWS::RDS::DBInstance`、`AWS::Redshift::Cluster`。

设计基础设施时请记住这一点。

### 保留删除策略

另一种情况是我们希望在删除堆栈时保留某些资源。这里我们使用名为 `Retain` 的删除策略：

```yaml
Resources:
  EvenMoreImportantDb:
    Type: AWS::RDS::Instance
    DeletionPolicy: Retain
    Properties:
      # Here you set properties for your RDS instance.
```

删除时，该数据库实例将从 CloudFormation 的状态中移除，但资源本身将保留在我们的账户中。

### 删除删除策略

另一个删除策略显然是 `Delete`，这是模板中所有资源的默认策略。如果我们不为资源指定任何删除策略，它将随堆栈一起被删除或从堆栈中删除。

### 将删除策略与条件结合

不幸的是，删除策略无法与 `Conditions` 结合使用，因为它们只允许存储 `String` 值。如果我们想为不同的条件设置不同的删除策略，该怎么办？

这将要求我们多次声明同一资源，但使用不同的逻辑名称、条件和删除策略。

如果我们正确设置条件，就不必担心资源重复：

### 示例：每个环境不同的删除策略

### 参数

```yaml
Parameters:
  Environment:
    Type: String
    AllowedValues: ["dev", "test", "prod"]
    Default: "dev"
```

### 条件

```yaml
Conditions:
  ProdEnv: !Equals [!Ref Environment, "prod"]
  TestEnv: !Equals [!Ref Environment, "test"]
  DevEnv: !Equals [!Ref Environment, "dev"]
```

### 资源

```yaml
Resources:
  ProdDatabase:
    Condition: ProdEnv
    Type: AWS::RDS::DBInstance
    DeletionPolicy: Retain
    Properties:
      # Properties for production database
  TestDatabase:
    Condition: TestEnv
    Type: AWS::RDS::DBInstance
    DeletionPolicy: Snapshot
    Properties:
      # Properties for test database
  DevDatabase:
    Condition: DevEnv
    Type: AWS::RDS::DBInstance
    DeletionPolicy: Delete
    Properties:
      # Properties for dev database
```

这给我们的模板增加了额外的复杂性，但使其更加通用。

保护数据很重要，所以请始终确保为存储服务（如数据库）和存储后端（如 S3 存储桶或 EBS 卷）使用删除策略。

现在我们知道了如何降低数据丢失的风险，让我们继续下一个主题。

### 引用现有堆栈

在具有共享责任模型的大型环境中，不同团队管理自己的堆栈资源，我们有时会遇到资源或资源属性（例如 RDS 实例端点）必须在不同堆栈之间共享的情况。

请记住，我们的应用程序中有一个网络堆栈。该堆栈中的资源（如子网）用于应用程序模板中。

每当你部署虚拟机、AutoScaling 组、负载均衡器或数据库集群时，都需要指定子网 ID。我们可以将该子网 ID 指定为参数，然后使用 `Fn::Ref` 将其映射到资源属性中，但这将需要许多不必要的操作，通过使用导出和 `Fn::ImportValue` 可以避免这些操作。

在第 1 章 CloudFormation 复习中，我们使用输出来获取 IAM 角色的 ARN，并在 AWS 命令行中使用它。

输出可用于快速访问已创建资源的属性，但也可以转换为导出。

导出是物理资源 ID 的别名。每个 `Export` 名称在一个区域内必须唯一，但不必在单个账户内唯一。

这意味着当我们想要引用在另一个堆栈中创建的资源属性时，我们需要创建一个带有 `Export` 名称的输出记录。完成此操作并创建堆栈后，我们可以使用名为 `Fn::ImportValue` 的内部函数来引用它。

`Fn::ImportValue` 函数将在区域内整个账户的 CloudFormation 导出值中查找必要的属性，并将必要的条目写入资源属性。

如果这听起来有点复杂，让我们通过一个示例来看看它是如何工作的。在这里，我们在核心堆栈中创建子网，并需要在 WebTier（和其他）堆栈中使用它们的 ID。这是我们的核心模板的样子：

### 示例：包含子网的核心堆栈

`core.yaml`

```yaml
Parameters:
  # ...

Resources:
  Vpc:
```




### CloudFormation 输出和伪参数

### 栈输出和跨栈引用

```yaml
Type: AWS::EC2::VPC

Properties:
  # ...
```

```yaml
PublicSubnet1:
  Type: AWS::EC2::Subnet
  Properties:
    # ...
  # 我们资源的其余部分...
```

Outputs 部分如下：

```yaml
Outputs:
  VpcId:
    Value: !Ref Vpc
    Export:
      Name: Vpc
  PublicSubnet1Id:
    Value: !Ref PublicSubnet1
    Export:
      Name: PublicSubnet1Id
  PublicSubnet2Id:
    Value: !Ref PublicSubnet2
    Export:
      Name: PublicSubnet2Id
  PublicSubnet3Id:
    Value: !Ref PublicSubnet3
    Export:
      Name: PublicSubnet3Id
  # 以此类推
```

以下是我们如何从核心栈引用导出的值：

**webtier.yaml**

```yaml
Resources:
  WebTierAsg:
    Type: AWS::AutoScaling::AutoScalingGroup
    Properties:
      # 一些属性...
      VpcZoneIdentifier:
        - !ImportValue PublicSubnet1Id
        - !ImportValue PublicSubnet2Id
        - !ImportValue PublicSubnet3Id
      # 以此类推...
```

你可能已经注意到，我在这里做了额外的工作，通过创建额外的导出并逐个导入这些值。

有时，你需要导入单个属性，例如共享密钥或用于 ECS 任务执行的 IAM 角色，但对于列表项，例如子网或可用区，将它们组合成列表并作为列表引用是明智的。

让我们将前面的示例重构为更简单的结构：

**core.yaml**

```yaml
Outputs:
  PublicSubnetIds:
    Value: !Split [",", !Join [",", [!Ref PublicSubnet1, !Ref PublicSubnet2, !Ref PublicSubnet3]]]
    Export:
      Name: PublicSubnetIds
```

在这里，我们用子网 ID 创建一个逗号分隔的字符串，然后使用 `Fn::Split` 将这个长字符串转换为列表。由于没有用于生成元素列表的内部函数，我们必须使用这个变通方法。

现在，我们可以用更简短的形式导入此值：

**webtier.yaml**

```yaml
Resources:
  WebTierAsg:
    Type: AWS::AutoScaling::AutoScalingGroup
    Properties:
      # 一些属性
      VpcZoneIdentifier: !ImportValue PublicSubnetIds
```

由于我们已经传递了一个值列表，CloudFormation 将正确解析它，并按我们需要的方式创建实例的 `AutoScaling` 组。

输出/导出也支持条件函数。在上一节（*删除策略*）中，我们不得不多次声明相同的数据库实例。如果我们为每个实例创建导出，栈创建将会失败，因为并非每一个都被创建。在那里使用条件函数也是明智的。

### 重要提示

请注意，输出不像资源那样拥有 `condition` 属性。我们必须使用条件函数，以便为导出使用正确的值。

**database.yaml**

```yaml
Outputs:
  DbEndpoint:
    Value: !If [ProdEnv, !GetAtt ProdDatabase.Endpoint.Address, !If [TestEnv, !GetAtt TestDatabase.Endpoint.Address, !GetAtt DevDatabase.Endpoint.Address]]
    Export:
      Name: DbEndpoint
```

这是短语法形式使我们的模板几乎无法阅读的情况之一，所以让我们用标准语法形式编写它：

```yaml
Outputs:
  DbEndpoint:
    Value:
      Fn::If:
        - ProdEnv
        - !GetAtt ProdDatabase.Endpoint.Address
        - Fn::If:
            - TestEnv
            - !GetAtt TestDatabase.Endpoint.Address
            - !GetAtt DevDatabase.Endpoint.Address
    Export:
      Name: DbEndpoint
```

看起来好多了，对吧？不过，由你决定在声明复杂条件检查时选择哪种语法形式（长或短）。只别忘了你写代码不仅是给机器（在我们的例子中是 CloudFormation）看的，也是给你的同事看的。

现在，让我们转到下一个重要主题，即关于 AWS 伪参数。

### AWS 伪参数

是什么让这些参数如此特别？嗯，事实上它们是从 AWS 本身获取的。

这些参数的当前列表如下：

- `AWS::AccountId`
- `AWS::NotificationARNs`
- `AWS::NoValue`
- `AWS::Region`
- `AWS::StackId`
- `AWS::StackName`
- `AWS::URLSuffix`
- `AWS::Partition`

我们不会涵盖所有参数，只介绍最常用的那些。

### AWS::AccountId

我们需要使用 `AccountId` 的用例之一是当我们使用 IAM 主体时。

如果你熟悉 IAM，那么你知道我们可以将 AWS 账户 ID 设置为主体元素。如果你需要复习一下关于 IAM 的知识，可以阅读 [`medium.com/@thomas.storm/aws-iam-deep-dive-chapter-1-essentials-a9cfb1931a01`](https://medium.com/@thomas.storm/aws-iam-deep-dive-chapter-1-essentials-a9cfb1931a01) 上的一系列优秀博客。

同时，暴露你的 AWS 账户 ID 是危险的；当我们指定此类敏感信息时，应始终坚持使用 AWS 伪参数。

`AWS::AccountId` 的示例用法如下：

```yaml
Resources:
  MyIamRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Action:
              - sts:AssumeRole
            Principal:
              AWS: !Ref "AWS::AccountId"
            Sid: "AllowRoleAssume"
      # IAM 角色属性的其余部分...
```

与任何其他参数一样，伪参数可以与内部函数结合使用。以下是一个示例（你可能还记得它来自 *第 1 章**，CloudFormation 复习*）我们在 `Principal` 中指定账户内的所有用户：

```yaml
MyIamRole:
  Type: AWS::IAM::Role
  Properties:
    AssumeRolePolicyDocument:
      Version: 2012-10-17
      Statement:
        - Sid: AllowAssumeRole
          Effect: Allow
          Principal:
            AWS:
              - !Join
                - ""
                - - "arn:aws:iam::"
                  - !Ref "AWS::AccountId"
                  - ":user/${aws.username}"
          Action: "sts:AssumeRole"
```

请注意 `${aws.username}` 变量。此变量不是 CloudFormation 的一部分，而是 IAM 策略变量的一部分。

当我们启动栈部署时，CloudFormation 将用实际值替换伪参数。对于 `AWS::AccountId`，该值将是创建栈的账户。

### AWS::NoValue

`AWS::NoValue` 短语相当于 *null* 值。除非你想将其与条件函数结合使用，否则没有太多实际用途。重要的是要知道，如果你为资源属性提供 `AWS::NoValue`，它的作用就好像你根本没有指定此属性一样。

一个用例是用它来从快照或从头创建数据库：

```yaml
Resources:
  MyDB:
    Type: AWS::RDS::DBInstance
    Properties:
      AllocatedStorage: "5"
      DBInstanceClass: db.t3.micro
      Engine: MySQL
      EngineVersion: "5.7"
      DBSnapshotIdentifier:
        Fn::If:
          - UseDBSnapshot
          - Ref: DBSnapshotName
          - Ref: AWS::NoValue
```

在最后几行中，我们可以看到如果条件不满足，则不会使用 `DBSnapshotIdentifier` 属性。

### AWS::Region

某些资源要求你指定 AWS 区域。硬编码区域可能不是安全问题，但会在我们在不同区域部署相同栈时给我们带来麻烦。即使你不打算部署多区域应用程序，使用此伪参数也是最佳实践。

例如，当我们创建 ECS 任务定义并想要使用 CloudWatch Logs 作为日志驱动程序时，我们需要指定日志组和 AWS 区域：

```yaml
Resources:
  EcsTaskDefinition:
    Type: AWS::ECS::TaskDefinition
    Properties:
      # 一些属性...
      ContainerDefinition:
        - Name: mycontainer
          # 一些容器属性...
          LogConfiguration:
            LogDriver: awslogs
            Options:
              awslogs-group: myloggroup
              awslogs-region: !Ref "AWS::Region"
```




awslogs-stream-prefix: ""

创建此堆栈时，CloudFormation 将在预置堆栈资源的同一区域中创建一个 CloudWatch 日志组。

### AWS::StackId 和 AWS::StackName

`AWS::StackId` 和 `AWS::StackName` 伪参数在您需要为资源指定标签时非常方便。一个示例用法是将应用程序名称赋予堆栈名称，并在 `Tags` 中重复使用它：

```
Resources:
  SomeResource:
    Type: # any, that supports tags
    Properties:
      # Some properties..
      Tags:
        - Key: Application
          Value: !Ref "AWS::StackName"
```

在 `Tags` 值中引用 `AWS::StackName` 将指明该资源所属的 CloudFormation 堆栈。

### AWS::URLSuffix

`AWS::URLSuffix` 伪参数用于指定 AWS URL。在 99% 的情况下，它将是 [amazonaws.com](http://amazonaws.com)，但如果您计划将应用程序部署到特殊区域（如中国或 AWS GovCloud），则必须使用它们各自的 URL 后缀。

如果 IAM 角色等服务需要由 AWS 承担（例如 ECS 的任务执行角色），则需要 URL 后缀：

```
Resources:
  EcsTaskExecutionRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Sid: AllowAssumeRole
            Effect: Allow
            Principal:
              Service: !Join ["." ["ecs-tasks", !Ref "AWS::URLSuffix"]]
            Action: "sts:AssumeRole"
      # Some role properties...
```

使用伪参数将使您的模板可重复使用且灵活。如果您必须在另一个区域甚至另一个账户中部署应用程序，重构的工作量将微乎其微。

### AWS::Partition

当我们需要手动定义资源的 ARN 时，通常会按以下方式编写：`arn:aws:service:region:account_id:...`

这里的 `aws` 一词指的是分区 —— 我们可以将其称为 **命名空间**。在大多数情况下，您将看到 "aws"，但您应该知道还有其他分区。例如，中国区域拥有分区 `aws-cn`，而 GovCloud 区域拥有分区 `aws-us-gov`。

话虽如此，让我们继续讨论另一个重要主题 —— 模板中的动态引用。

让我们进入本章的最后一部分，内容是关于将动态引用与 AWS SSM Parameter Store 和 AWS Secrets Manager 结合用于您的堆栈。

### 将 Parameter Store 和 Secrets Manager 用于动态引用

在本章开头，我们探讨了通过 JSON 文件或命令行参数传递参数的方法。

虽然在基础设施即代码中将参数保存在 **版本控制系统** (**VCS**) 中是一种已知做法，但这会引入额外的复杂性。

如果我们将参数存储在 VCS 中，我们需要负责加密敏感数据，例如密码或其他凭证。如果我们通过命令行参数传递它们，我们必须确保不会出错或打错字（这在 IT 工程师的生活中是常见现象）。

为了解决这些问题，我们可以将模板参数存储在 SSM Parameter Store 或 Secrets Manager 中（用于密码）。

它们都支持版本控制，因此如果出现错误，我们随时可以将参数恢复到之前的版本，然后重新部署我们的堆栈。

使用 Parameter Store 和 Secrets Manager 的另一个好处是，我们可以让不负责 CloudFormation 开发的开发人员对堆栈进行细微更改，例如更改实例类型或增加 `AutoScaling` 组的大小。

现在我们要做的是在核心模板中添加一些参数，供应用程序模板使用：

**core.yaml**

```
Resources:
  # Some resources...
  WebAsgMinSize:
    Type: AWS::SSM::Parameter
    Properties:
      Type: String
      Value: "1"
  WebAsgMaxSize:
    Type: AWS::SSM::Parameter
    Properties:
      Type: String
      Value: "1"
  WebAsgDesiredSize:
    Type: AWS::SSM::Parameter
    Properties:
      Type: String
      Value: "1"
  # The rest...
```

然后，我们将 SSM 参数添加到输出中：

```
Outputs:
  # Original outputs...
  WebAsgMinSize:
    Value: !Ref WebAsgMinSize
    Export:
      Name: WebAsgMinSize
  WebAsgMaxSize:
    Value: !Ref WebAsgMaxSize
    Export:
      Name: WebAsgMaxSize
  WebAsgDesiredSize:
    Value: !Ref WebAsgDesiredSize
    Export:
      Name: WebAsgDesiredSize
```

我们为 Web 层堆栈创建了三个参数供使用。现在，我们需要在 Web 层模板中引用这些参数。有两种方法可以做到这一点：

*   从参数中引用它们
*   从资源声明中引用它们

您可能记得，在创建 WebTier 模板时，我们在参数部分使用了特定于 AWS 的参数（如果需要复习，请参阅 *使用条件元素* 部分)。

虽然这种用法相对明显且简单，但这将要求我们首先创建一个核心堆栈，然后将 SSM Parameter Store 参数的名称复制粘贴到模板的参数部分。

为了减少手动操作，明智的做法是直接在资源声明中 *解析* 参数。

其工作原理如下：参数创建完成且您拥有其名称后，可以使用 Jinja 格式将其传递给资源属性：`"{{resolve:service-name:reference-key}}"`。

我们可以使用 SSM 参数名称或 ARN。

在我们的案例中，我们导出参数的 ARN。现在让我们在 Web 层 `AutoScaling` 组资源中解析它：

**webtier.yaml**

```
Resources:
  # WebTier resources...
  WebTierAsg:
    Type: AWS::AutoScaling::AutoScalingGroup
    Properties:
      # Properties of ASG...
      DesiredCapacity:
        Fn::Join:
          - ":"
          - - "{{resolve:ssm"
            - !ImportValue WebAsgDesiredSize
            - "1}}"
      MaxSize:
        Fn::Join:
          - ":"
          - - "{{resolve:ssm"
            - !ImportValue WebAsgMaxSize
            - "1}}"
      MinSize:
        Fn::Join:
          - ":"
          - - "{{resolve:ssm"
            - !ImportValue WebAsgMinSize
            - "1}}"
```

在前面的代码中，我们的属性引用的不是模板参数，而是 SSM 参数的值。

### SSM Parameter Store 限制

编写本文时，无法在 SSM Parameter Store 中引用参数的最新版本。如果您更改了参数，仍然必须在 resolve 块中更改其版本。

我们可以使用相同的方法随机生成数据库密码，这样就不必对其进行硬编码和/或暴露它：

**database.yaml**

```
Resources:
  DbPwSecret:
    Type: AWS::SecretsManager::Secret
    Properties:
      GenerateSecretString:
      GenerateStringKey: "DbPassword"
  RdsInstance:
    Type: AWS::RDS::DBInstance
    Properties:
      # Some RDS Instance properties...
      MasterUserPassword:
        Fn::Join:
          - ":"
          - - "{resolve:secretsmanager"
            - !Ref DbPwSecret
            - "SecretString:DbPassword}}"
```

请注意，与 SSM 参数不同，我们不指定密钥的版本，而是指定我们要引用的实际键。

### 总结

虽然我们在本章中没有部署任何堆栈，但我们涵盖了 CloudFormation 主要构建块的方方面面。

我们学习了如何使模板可重用和可回收，如何使用条件控制它们，以及如何保护堆栈资源免受意外或故意删除。我们还介绍了动态参数引用，在本书后续章节中我们将越来越多地使用它。

在继续下一章之前，请确保您已浏览存储库中的模板并理解每一行。

在下一章中，我们将深入探讨预检查和预置堆栈资源的各种方法。



### 问题

1.  我们可以在 `Resource` 的 `Conditions` 属性中使用 `Intrinsic` 函数吗？

2.  我们可以在 `Resource` 属性中对条件使用 `Intrinsic` 函数吗？

3.  `UpdatePolicy` 和 `UpdateReplacePolicy` 有什么区别？

4.  对于任务关键型数据库，应使用哪种 `DeletionPolicy`？

5.  `Fn::ImportValue` 用于从何处导入值？

6.  是否可以创建多个同名的导出？

7.  是否有专门用于 AMI 的参数数据类型？

8.  什么是 `AWS::NoValue`？它的使用场景有哪些？

### 延伸阅读

*   模板结构：<https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html>
*   动态引用：<https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/dynamic-references.html>
*   删除策略：<https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-deletionpolicy.html>
*   示例模板：<https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-sample-templates.html>
