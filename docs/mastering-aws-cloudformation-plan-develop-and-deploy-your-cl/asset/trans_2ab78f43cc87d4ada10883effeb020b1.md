

# CloudFormation 输出和伪参数

## 栈输出和跨栈引用

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

# AWS 伪参数

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

## AWS::AccountId

我们需要使用 `AccountId` 的用例之一是当我们使用 IAM 主体时。

如果你熟悉 IAM，那么你知道我们可以将 AWS 账户 ID 设置为主体元素。如果你需要复习一下关于 IAM 的知识，可以阅读 [https://medium.com/@thomas.storm/aws-iam-deep-dive-chapter-1-essentials-a9cfb1931a01](https://medium.com/@thomas.storm/aws-iam-deep-dive-chapter-1-essentials-a9cfb1931a01) 上的一系列优秀博客。

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

与任何其他参数一样，伪参数可以与内部函数结合使用。以下是一个示例（你可能还记得它来自 [*第 1 章*](#B10533_01_Final_NM_ePUB.xhtml#_idTextAnchor020)*，CloudFormation 复习*）我们在 `Principal` 中指定账户内的所有用户：

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

## AWS::NoValue

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

## AWS::Region

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

