

# CloudFormation 内部函数与参数

## 使用 `Fn::Ref` 引用资源

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

## 向 CloudFormation 提供参数

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

## 使用列表参数与 `Fn::Select`

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

## 使用 `Fn::Cidr` 进行子网计算

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

# 使用条件元素

我们希望拥有可复用的模板，但有时我们需要在一种情况下创建资源，在另一种情况下不创建，或者可能使用不同的属性创建资源。

条件可解决此类问题，主要有两种用法：在资源下指定 `Condition`，或在资源属性中使用条件内部函数。

我们已经知道 `Condition` 是一个严格的布尔变量，由参数和条件函数求值得出。我们已经学习了条件在资源声明中的用法（如需复习，请跳转至*深入模板内部*一节），现在让我们看另一个有用的示例。

假设我们有一个由启动模板构建的 `AutoScaling` 组。我们预期测试环境和生产环境的负载不同，因此希望相应地调整 EC2 实例的规格。目前，我们在测试环境使用 `t3.micro`，在生产环境使用 `m5.large`。

