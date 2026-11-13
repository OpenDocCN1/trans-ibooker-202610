

# CDK：测试、资产与集成

缺乏测试支持也源于对测试内容的模糊认识。我们需要测试什么？资源是否会被创建？资源是否会*完全*按照我们的声明被创建？为什么不应该这样做？

然而，始终应该测试的是集成。在大型项目和基础设施中，我们不仅仅关心一个包含一堆机器的小型网络。那里有成百上千个计算、存储、传输、分析及其他资源，所有这些都由不同的团队开发和预配。

CDK 提供了一种创建基础设施的不同方式。我们不再编写关于创建什么的声明式指令，而是使用**面向对象编程**（**OOP**）范式编写计算机程序。每个资源都是一个类，并与其他类存在关系。`VPC` 是一个类。`Subnet` 是一个与 `VPC` 相关的类。`RDS 数据库子网组` 是一个与子网相关的类。

这些关系是以编程方式声明的，此外，CDK 是一个编程语言框架。这意味着你不是在编写模板——你实际上是在编写一段软件！

这使你能够针对它运行复杂的测试，利用软件工程领域的最佳实践。而这一切甚至不需要花费一分钱去创建数十或数百个资源来进行验证！

而在 CDK 的帮助下，测试并不是我们能做的唯一事情。将应用程序与基础设施结合起来又如何？

## 添加工件

当我们使用纯 CloudFormation 时，无法向其中添加工件，仅仅因为 CloudFormation 是一个用于预配基础设施和资源的系统，我们的应用程序将运行在其上。通过 `cfn-init` 部分添加了对将应用程序添加到堆栈的支持。借助无服务器应用程序模型，可以拥有包含基础设施模板的一体化源代码，但这仅适用于基于 AWS Lambda 的应用程序。

CDK 不仅仅是一个生成 CloudFormation 模板的框架——它在其之上附带了许多抽象。

其中一种抽象是 `assets`（资产），即 CDK 构造，用于打包应用程序工件，无论它是二进制文件、ZIP 文件、目录还是 Docker 镜像。

想象一下，你有一个项目结构，源代码位于 `foo` 目录中。该目录还包含一个 Dockerfile，它会被构建并推送到 AWS ECR 仓库。

在这种情况下，我们可以使用资产来打包镜像并将其包含在我们的基础设施中，如下例所示：

```python
import aws_cdk.aws_ecs as ecs

from aws_cdk.aws_ecr_assets import DockerImageAsset

# 使用应用路径声明 Asset 对象
asset = DockerImageAsset(self,
                         "image",
                         directory="/path/to/foo")

# 声明任务定义
td = ecs.FargateTaskDefinition(self,
                               "td",
                               memory_limit_mib=512,
                               cpu=256)

# 将基于 Asset 镜像构建的容器添加到任务定义
td.add_container("container", image=ecs.ContainerImage.from_ecr_repository(asset.repository,
                                                                           asset.image_uri))
```

这段代码的作用是：它构建并推送包含原始应用程序源代码的 Docker 镜像，并将其发送到 ECR 仓库。然后，它创建一个包含容器定义的 ECS 任务定义，指向 ECR 镜像 URI 作为源。

这种抽象使我们能够真正地将应用程序与基础设施混合在一起，将它们作为一个整体进行管理！而且由于 CDK 是一个多语言项目，越来越多的产品和项目可以利用这一点进行构建，而无需强迫开发者学习额外的语言（尽管这仍然是一项很不错的技能）。

CDK 才刚刚开始它的旅程，但其未来充满希望且将改变游戏规则。它可能不会取代 CloudFormation（这样一个系统根本无法被取代），但迟早，它将充当开发者编写的代码与实际运行、服务的基础设施之间的翻译机制。

## 总结

在本章中，我们了解了未来可以期待什么。首先，我们回顾了基础设施的过去与未来——什么在改变，这些变化对我们这些 AWS 专业人士意味着什么。然后，我们定义了 CloudFormation 与其最大竞争对手 Terraform 之间的主要区别，以及为什么这两个系统遵循如此不同的路径。在本章末尾，我们探讨了 CDK 为我们带来的实际价值。

牢记这些，你应该能够理解在你的旅程中下一步该去向何方。你是否想继续精通 CloudFormation？读完这本书后，你所需要的唯一信息都在 CloudFormation 的文档中。CloudFormation 开发团队正在努力开发和演进他们的产品，所以请花些时间查看他们的覆盖路线图和公共仓库，那里有大量有用的信息。

你是否想开始实践软件开发？如果是，CDK 是一个很好的起点。

祝你在这段旅程中一切顺利。让我们看看它将带我们去往何方！

## 延伸阅读

*   AWS 关于 CloudFormation Registry 的博客：[https://aws.amazon.com/blogs/aws/cloudformation-update-cli-third-party-resource-support-registry/](https://aws.amazon.com/blogs/aws/cloudformation-update-cli-third-party-resource-support-registry/)
*   AWS CloudFormation GitHub 项目：[https://github.com/aws-cloudformation](https://github.com/aws-cloudformation)
*   AWS CloudFormation 覆盖路线图：[https://github.com/aws-cloudformation/aws-cloudformation-coverage-roadmap](https://github.com/aws-cloudformation/aws-cloudformation-coverage-roadmap)
*   AWS CloudFormation 精选列表：[https://github.com/aws-cloudformation/awesome-cloudformation](https://github.com/aws-cloudformation/awesome-cloudformation)

## 评估

### 第 1 章：CloudFormation 复习

1.  `CreateStack`。

2.  CloudFormation 服务角色是一个 IAM 角色，在执行堆栈操作前由 CloudFormation 代入。附加到该角色的策略将用于执行堆栈操作。

3.  附加到运行 CloudFormation 堆栈操作的 IAM 实体（用户或角色）上的策略。

4.  此信息（物理资源 ID 及其元数据）存储在 CloudFormation 的状态中。

5.  如果我们尝试创建相同的堆栈（即调用 `CreateStack` API 调用），该调用将失败并返回 `400 AlreadyExists` 错误。

6.  如果我们在模板或参数没有任何更改的情况下运行堆栈更新，则不会发生任何事情，因为没有更改。如果资源被手动删除，CloudFormation 不会察觉。但如果我们更新已删除的资源，操作将会失败，因为 CloudFormation 资源仍存在于状态中，且 CloudFormation *认为* 它仍然在那里。

7.  因为 CloudFormation 的状态是不可变的，且无法由最终用户管理。为了重新创建已删除的资源，我们必须通过从模板中删除它来将其从状态中删除，然后再将其添加回去。

### 第 2 章：高级模板开发

1.  不能。资源的 `Condition` 属性只能采用 `Condition` 名称字符串格式。

2.  可以，可以将 `Fn::Ref`、`Fn::GetAttr` 或其他相关内部函数与条件函数（如 `Fn::If`）一起使用。

3.  `UpdatePolicy` 是一个资源属性，用于处理 Auto Scaling 组等资源的更新。`UpdateReplacePolicy` 用于管理在堆栈操作期间被 CloudFormation 替换的资源的行为。

4.  `Retain`，因为我们无法承受在模板或堆栈操作中因错误而删除任务关键型数据库。

5.  `Fn::ImportValue` 获取其他堆栈导出输出的值。

