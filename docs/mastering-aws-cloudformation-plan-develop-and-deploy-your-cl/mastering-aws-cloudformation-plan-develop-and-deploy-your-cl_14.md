## 第 11 章：未来展望

欢迎来到《精通 AWS CloudFormation》的最后一章！

在本章中，我们将坐下来放松一下，因为我们已经花费了大量精力学习 CloudFormation、它的功能、特性和扩展。本章将让我们洞悉 CloudFormation 和基础设施即代码的未来发展。

你可能会好奇，为什么我们要通过预测来结束这本书，而你可能更有兴趣继续练习使用 CloudFormation。

你可能是对的。这本书旨在帮助你精通 CloudFormation，你已经为此忙碌了 10 个漫长的章节。但有一件事你需要考虑。

世界在不断变化，IT、开发、云提供商和 AWS 也是如此。我们无法完全准备好迎接未来，但我们可以尝试预测它。归根结底，不仅是技术技能，还有博学和心态造就了一名优秀的工程师。

我们将尝试了解以下主题：

* 基础设施即代码的未来
* 理解 Terraform 与 CloudFormation 的区别
* 理解云开发工具包 (CDK) 的价值

### 基础设施即代码的未来

在预见未来之前，我们需要了解过去。

*基础设施即代码* 这个术语并不新鲜。它的根源可以追溯到 90 年代，当时系统管理员使用 Perl 和 Shell 脚本来自动化他们的日常工作。系统管理员与开发人员相似，但关注点不同（主要关注自动化和基础设施的稳定性）。




### 基础设施即代码的演进

这种关注点的转变始于 2006 年 Amazon Web Services 的推出，只需几分钟，您就能拥有一队随时可用的虚拟机。供应速度的提升引入了扩展性能的问题。

此外，使用高级编程语言开发自己的基础设施管理工具也很困难。最终，配置管理工具开始出现：`Chef`、`Puppet`、`Ansible` 等。

后来，幂等性和可声明式基础设施成为必要需求。配置管理工具能提供基础设施的状态，但将其从几十台扩展到几百台、再到数千台受管机器很困难，这些系统无法承受如此负载。

工程师们还希望将资源供应和配置管理结合起来。`CloudFormation` 于 2011 年发布，是第一个能同时做到这两点的工具。它是一个复杂的系统，但改变了我们实施运维和基础设施部署的方式。您不再需要创建服务请求或在 AWS 控制台中点击操作，只需编写一个 `JSON` 模板即可供应整个基础设施。

随后在 2014 年，HashiCorp 推出了 CloudFormation 的竞争对手——`Terraform`。Terraform 提供了不同的方法，并支持多云提供商，例如 `GCP` 和 `Azure`。另一个所谓的 `killer feature`（杀手级特性）是其创建 Provider 的能力，Provider 是充当 Terraform 与服务提供商之间接口的软件程序。

Terraform 使用其自己的 `domain-specific language`（`DSL`，领域特定语言），称为 `HashiCorp Configuration Language`（`HCL`）。其语法类似于 `JSON`，但更易于阅读和审查。

CloudFormation 和 Terraform 走上了不同的发展道路，我们将在下一节中回顾这一点，但目前我们需要坚持讲述这个故事。

随着云服务提供商的成长和发展，术语 `infrastructure`（基础设施）越来越多地失去了其原始含义。基础设施不再是一堆虚拟机或数据库：它是作为软件业务和产品的基础基石而存在的事物。

在越来越多的服务中应用 `infrastructure as code` 并将其与 `software development life cycle`（`SDLC`，软件开发生命周期）集成，引入了另一组问题。

什么是 `build artifact`（构建产物）？它是基础设施即代码的最终产品吗？它是指示服务提供商启动特定资源的声明文件吗？它是一个框架？还是提供了必要资源和端点的基础设施状态？

如果是这样，它的 `code`（代码）部分是什么？我们如何将通用的软件工程实践应用于它，例如单元测试和集成测试？我们如何确保基础设施的变更不会破坏应用程序？

此外，我们能否将应用软件与基础设施集成？我们能否真正将它们作为单个软件组件结合起来？

各种平台，例如 `Kubernetes`，提供了将应用程序的基础设施（或其引用）与源代码结合在一起的机会，而底层基础设施是自我管理的，不需要太多关注。

基础设施已从互联网时代的 `just virtual machines`（仅仅是虚拟机）演进为基础设施即服务、平台即服务和软件即服务。现在您可以简单地订购计算、存储或任何其他单元，并获得一个用于工作的端点。例如，您无需创建 NoSQL 数据库，而是可以创建一个 `DynamoDB` 表并立即使用它，而无需考虑底层基础设施、冗余和性能。

在这种抽象层级上，我们开始注意到应用程序（例如，一段软件）与底层基础设施之间没有太大区别。这是否意味着我们可以开始将它们结合起来？

让我们看看这段伪代码：

```
from infrastructure import network, database, compute
from application import Application
import source_code

### Declare infrastructure to run application on
net = network.new(cidr="10.0.0.0/16",
                  subnets=3,
                  mask=24)

db = database.new(engine="mysql",
                  cpu=2,
                  mem=2048,
                  network=net)

node_cluster = compute.vm_cluster.new(count=3,
                                      cpu=2,
                                      mem=2048,
                                      network=net)

### Declare application and attach to an infrastructure
app = Application.new()
app.use_db(db)
app.run_on(node_cluster)
app.build.from_source(source_code)
```

如果这段代码存在，它将连接到云环境，创建我们声明使用的所有内容，然后在其上构建和部署应用程序。

即使我们可以在同一代码中结合实际应用程序和基础设施，我们仍然面临工程角色的问题。那么运维人员是谁？软件工程师？系统工程师？DevOps 工程师？站点可靠性工程师？

可以肯定的是，经典的孤岛模型——开发人员和运维人员在不同的责任区工作，拥有不同的优先级——正在逐渐消失。如今，我们有了解操作系统和计算机网络的开发人员，也有会写代码的系统工程师。

行业正朝着融合方向转变，基础设施即代码极有可能最终仅仅成为 `code`（代码）。

但我们尚未到达那里。在您继续云自动化之旅之前，我希望您比较这两个竞争对手——CloudFormation 和 Terraform——并了解它们的区别。学习这一点将帮助您为您特定的当前或未来情况做出正确决策。

### 理解 Terraform 与 CloudFormation 的区别

首先，我们评估 CloudFormation 和 Terraform 并非为了找出市场上**最好**的基础设施即代码工具。我们的目标是找到一个适合目的和我们用例（无论它将是什么）的工具。

我们将查看以下特性：

*   Provider 支持
*   声明语法
*   开发和部署方法论

让我们从 Provider 支持开始。

### Provider 支持

作为 Amazon Web Services 的发明，CloudFormation 最初仅支持其自身作为唯一的服务提供商。

Terraform 被开发为一种 `cloud-agnostic`（云无关/云中立）工具，意味着它能与各种云和服务提供商通信，从而使其成为统管一切的单一工具。

这一概念通过使用 Provider 实现——Provider 是处理服务提供商与 Terraform 之间通信的特定抽象。Provider 使用与 Terraform 相同的语言编写（例如 `Golang`），可在本地使用（如果您需要为特定目的创建自己的 Provider），或通过 `OpenSource` 分发并通过特殊表单提交给 HashiCorp。

Terraform 拥有大量 Provider，既有由供应商开发的（`https://www.terraform.io/docs/providers/index.html`），也有社区开发的（`https://www.terraform.io/docs/providers/type/community-index.html`）。HashiCorp 在构建框架和包装器上投入巨资，以使 Provider 开发对每个人都变得可行。





一方面，Terraform 作为一种通用的基础设施即代码工具取得的成功，让主要云提供商的竞争对手——AWS CloudFormation、Azure Resource Manager 和 Google Cloud Deployment Manager——毫无机会。另一方面，Terraform 严重依赖这些提供商的开发速度——显而易见，云提供商的工具将率先支持最新的服务和 API 变更。除此之外，工程师还必须负责向后兼容性，因为提供商的新版本可能无法与模板中资源声明的旧版本配合工作。

然而，创建外部资源（例如 AWS 之外的资源）的能力是令人向往的。虽然自定义资源（CRs）允许我们这样做，但很难大规模地妥善组织它们。想象一下为 GCP 创建 CRs，以便你可以使用 CloudFormation 在 GCP 中创建资源。

CloudFormation 开发团队针对这一问题提出了一个集中式解决方案。一项名为 CloudFormation Registry 的新功能允许我们创建自己的资源提供商——以类似于 Terraform 的方式——不仅使用 Go，还使用 Java（正式发布）和 Python（撰写时处于开发者预览阶段）！

CloudFormation Registry 不在本书讨论范围内，因为 AWS 工程师极不可能需要创建自己的提供商；然而，Atlassian、Datadog、New Relic、SpotInst 等服务提供商已经在接入过程中。CloudFormation 很快很可能会在支持的供应商种类上与 Terraform 竞争。

让我们继续探索，看看语法差异。

### 声明语法

我们知道，CloudFormation 模板根据开发者的偏好使用 JSON 或 YAML 编写。CloudFormation 的声明是一个键值块，其中键是资源的逻辑名称，值是其属性，如类型和属性。

以下代码展示了 CloudFormation 资源的资源声明：

```yaml
MyResourceLogicalName:
  Type: AWS::Service::ResourceType
  Properties:
    PropertyA: some value
    PropertyB: some value
    # and so on
```

此声明使得在声明中使用 `Fn::Sub`、`Fn::Ref` 和 `Fn::GetAtt` 等函数引用资源标识符变得容易。

Terraform 拥有自己的领域特定语言，称为 HashiCorp Configuration Language (HCL)。HCL 是一种类 JSON 语言，在其之上添加了少量的语法糖。

假设我们想使用 Terraform 在 AWS 中创建一个 VPC 和一个子网。我们的模板文件将如下所示：

```hcl
resource "aws_vpc" "vpc" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "subnet" {
  vpc_id     = "${aws_vpc.vpc.id}"
  cidr_block = "10.0.1.0/24"
}
```

正如你所见，存在巨大差异。

资源的实际类型是关键字 `resource` 后的第一个字符串，随后是逻辑名称（`resource "RESOURCE_TYPE" "LOGICAL_NAME" {}`），而在 CloudFormation 中，我们拥有资源的逻辑名称，后跟在单独字段中的类型。

第二个区别是，我们需要提供资源类型、逻辑名称和属性（`${aws_vpc.vpc.id}`），以便我们可以在另一个资源中引用它，因为 Terraform 不支持任何默认返回值，如 CloudFormation 和 `Fn::Ref`。

还有许多其他差异，例如 HCL 中的迭代和条件，它们用于赋予提供 CloudFormation 已支持的相同功能的能力，但这不是我们目前的主要关注点。

现在让我们看看最后但同样重要的一点——部署和开发。让我们深入了解。

### 开发和部署方法论

CloudFormation 在开发方面很灵活。AWS 工程师可以使用嵌套堆栈，或使用 `exports` 和 `Fn::ImportValue` 进行跨堆栈资源引用，以独立管理每一块基础设施。内部函数的巨大多样性和敏捷性使 AWS 工程师能够创建可复用的模板。

说到部署和堆栈操作，CloudFormation 应有尽有。ChangeSets 允许对堆栈更新进行细粒度控制，而 `aws cloudformation deploy` 命令使为新旧堆栈创建持续交付管道变得简单。

如下列表所示，CloudFormation 拥有针对各种情况的大量能力：

*   对于大规模基础设施或产品，有 StackSets。
*   对于定制化，有模板宏和自定义资源。
*   对于配置管理，有 `cfn-init`。
*   对于预置安全，有 CloudFormation 服务角色。
*   在发生故障时，有 CloudFormation 最重要且最有用的功能之一，即自动无缝回滚。

Amazon Web Services 通过使 CloudFormation 适应任何类型的公司，在 AWS 上运行其应用程序和系统，做了一项出色的工作。

Terraform 遵循不同的路径。它不专注于单一云——相反，它试图支持尽可能多的云提供商、第三方服务，甚至外部系统。

你可以将已知的配置管理系统（如 Ansible、Puppet、Chef 和 SaltStack）与 Terraform 结合使用，通过预配器（provisioners）来实现，如果你曾将它们用于本地基础设施并正在迁移到云端，这将节省大量时间。Terraform 能够通过其 API 使用 `data` 从提供商检索信息，例如，它可以调用 AWS 的 API 以获取现有 VPC 的 ID 并用其创建子网。

Terraform 不支持回滚，所以如果事情变糟，你必须依赖其状态文件备份——存储在本地或远程。

话虽如此，决定权在你——工程师——手中，由你决定是想使用 Terraform 还是 CloudFormation。如果你习惯使用 YAML 和 JSON 并计划仅与 AWS 合作，则选择 CloudFormation。如果你正在寻找一个供应商无关的工具，拥有数十种服务和开源社区的支持，那么你可能想看看 Terraform。

本章不仅关乎使用声明式语言定义基础设施。在下一节中，我们将看看它们潜在的继任者以及它们为 IT 领域的每个人带来的价值。

### 理解 Cloud Development Kit 的价值

基于 DSL 的工具，如 CloudFormation 和 Terraform，仍然是基础设施即代码的标准。它们广为人知，拥有庞大的全球社区，并持续发展。

然而，它们也存在一些问题，而其继任者 CDK 能够解决这些问题。

在本节中，我们将抢先了解 CDK 为行业带来的变化和价值。

我们将从测试能力开始，因为这是基础设施即代码的主要痛点之一。

### 测试基础设施

为了测试计算机程序，我们所要做的就是构建它，然后运行一些编写好的测试场景。软件的预期行为是已知的，所以你所需要的只是获得正确的结果。

在*第 4 章* *持续集成和部署* 中，我们通过部署基础设施并在其上运行特定的基础设施测试，用 CloudFormation 做到了这一点。对于声明式工具，这是我们测试它们的唯一可行方法，但为测试启动单独的基础设施是一个漫长且在某些情况下非常昂贵的过程。在某些情况下，这甚至是不可能的。

这主要是因为模板中声明资源的方式并不意味着服务提供商能够处理该信息。例如，CloudFormation 无法判断我们为 EC2 实例提供的 AMI ID 是否正确。此外，当我们编写 Pod 声明时，Kubernetes 无法判断 Docker 镜像是否存在。





### CDK：测试、资产与集成

缺乏测试支持也源于对测试内容的模糊认识。我们需要测试什么？资源是否会被创建？资源是否会*完全*按照我们的声明被创建？为什么不应该这样做？

然而，始终应该测试的是集成。在大型项目和基础设施中，我们不仅仅关心一个包含一堆机器的小型网络。那里有成百上千个计算、存储、传输、分析及其他资源，所有这些都由不同的团队开发和预配。

CDK 提供了一种创建基础设施的不同方式。我们不再编写关于创建什么的声明式指令，而是使用**面向对象编程**（**OOP**）范式编写计算机程序。每个资源都是一个类，并与其他类存在关系。`VPC` 是一个类。`Subnet` 是一个与 `VPC` 相关的类。`RDS 数据库子网组` 是一个与子网相关的类。

这些关系是以编程方式声明的，此外，CDK 是一个编程语言框架。这意味着你不是在编写模板——你实际上是在编写一段软件！

这使你能够针对它运行复杂的测试，利用软件工程领域的最佳实践。而这一切甚至不需要花费一分钱去创建数十或数百个资源来进行验证！

而在 CDK 的帮助下，测试并不是我们能做的唯一事情。将应用程序与基础设施结合起来又如何？

### 添加工件

当我们使用纯 CloudFormation 时，无法向其中添加工件，仅仅因为 CloudFormation 是一个用于预配基础设施和资源的系统，我们的应用程序将运行在其上。通过 `cfn-init` 部分添加了对将应用程序添加到堆栈的支持。借助无服务器应用程序模型，可以拥有包含基础设施模板的一体化源代码，但这仅适用于基于 AWS Lambda 的应用程序。

CDK 不仅仅是一个生成 CloudFormation 模板的框架——它在其之上附带了许多抽象。

其中一种抽象是 `assets`（资产），即 CDK 构造，用于打包应用程序工件，无论它是二进制文件、ZIP 文件、目录还是 Docker 镜像。

想象一下，你有一个项目结构，源代码位于 `foo` 目录中。该目录还包含一个 Dockerfile，它会被构建并推送到 AWS ECR 仓库。

在这种情况下，我们可以使用资产来打包镜像并将其包含在我们的基础设施中，如下例所示：

```python
import aws_cdk.aws_ecs as ecs

from aws_cdk.aws_ecr_assets import DockerImageAsset

### 使用应用路径声明 Asset 对象
asset = DockerImageAsset(self,
                         "image",
                         directory="/path/to/foo")

### 声明任务定义
td = ecs.FargateTaskDefinition(self,
                               "td",
                               memory_limit_mib=512,
                               cpu=256)

### 将基于 Asset 镜像构建的容器添加到任务定义
td.add_container("container", image=ecs.ContainerImage.from_ecr_repository(asset.repository,
                                                                           asset.image_uri))
```

这段代码的作用是：它构建并推送包含原始应用程序源代码的 Docker 镜像，并将其发送到 ECR 仓库。然后，它创建一个包含容器定义的 ECS 任务定义，指向 ECR 镜像 URI 作为源。

这种抽象使我们能够真正地将应用程序与基础设施混合在一起，将它们作为一个整体进行管理！而且由于 CDK 是一个多语言项目，越来越多的产品和项目可以利用这一点进行构建，而无需强迫开发者学习额外的语言（尽管这仍然是一项很不错的技能）。

CDK 才刚刚开始它的旅程，但其未来充满希望且将改变游戏规则。它可能不会取代 CloudFormation（这样一个系统根本无法被取代），但迟早，它将充当开发者编写的代码与实际运行、服务的基础设施之间的翻译机制。

### 总结

在本章中，我们了解了未来可以期待什么。首先，我们回顾了基础设施的过去与未来——什么在改变，这些变化对我们这些 AWS 专业人士意味着什么。然后，我们定义了 CloudFormation 与其最大竞争对手 Terraform 之间的主要区别，以及为什么这两个系统遵循如此不同的路径。在本章末尾，我们探讨了 CDK 为我们带来的实际价值。

牢记这些，你应该能够理解在你的旅程中下一步该去向何方。你是否想继续精通 CloudFormation？读完这本书后，你所需要的唯一信息都在 CloudFormation 的文档中。CloudFormation 开发团队正在努力开发和演进他们的产品，所以请花些时间查看他们的覆盖路线图和公共仓库，那里有大量有用的信息。

你是否想开始实践软件开发？如果是，CDK 是一个很好的起点。

祝你在这段旅程中一切顺利。让我们看看它将带我们去往何方！

### 延伸阅读

*   AWS 关于 CloudFormation Registry 的博客：[`aws.amazon.com/blogs/aws/cloudformation-update-cli-third-party-resource-support-registry/`](https://aws.amazon.com/blogs/aws/cloudformation-update-cli-third-party-resource-support-registry/)
*   AWS CloudFormation GitHub 项目：[`github.com/aws-cloudformation`](https://github.com/aws-cloudformation)
*   AWS CloudFormation 覆盖路线图：[`github.com/aws-cloudformation/aws-cloudformation-coverage-roadmap`](https://github.com/aws-cloudformation/aws-cloudformation-coverage-roadmap)
*   AWS CloudFormation 精选列表：[`github.com/aws-cloudformation/awesome-cloudformation`](https://github.com/aws-cloudformation/awesome-cloudformation)

### 评估

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





### 第 3 章：堆栈的验证、Lint 检查与部署

1.  从 9,000 开始及以上。

2.  **Lint 检查**是根据 Linter 规则评估模板的过程，由 `cfn-lint` 执行。**验证**由 CloudFormation 执行，检查模板是否有效且可被 CloudFormation 处理。

3.  有以下几个步骤：

    (a) CloudFormation 将检查堆栈是否存在。

    (b) CloudFormation 将为新堆栈或现有堆栈创建一个 `ChangeSet`（更改集）。

    (c) CloudFormation 将执行 `ChangeSet` 并监听堆栈事件。

    (d) 如果发生故障，CloudFormation 将回滚更改并告知用户相关问题。

4.  不行，`ChangeSets` 必须具有唯一的名称，否则你会收到 `400 AlreadyExists` 错误。

5.  规则文件名和规则类名必须相同。

6.  只需对模板或参数进行必要的更改，然后运行堆栈更新即可。

### 第 4 章：持续集成与部署

1.  `buildspec.yml` 是一个构建规范文件，包含 AWS CodeBuild 处理构建的指令。

2.  CloudFormation 制品是指模板，某些情况下还包括参数文件。它们存储在 S3 上。

3.  你可以使用 CloudFormation 信号、等待条件和更新策略。

4.  `ListStackResources`。

5.  源、构建和部署。可以使用各种服务，例如 S3、CodeBuild、CodeDeploy 和 CloudFormation。

### 第 5 章：使用 StackSets 向多个区域和账户部署

1.  可以，但不推荐。届时你将必须显式指定这两个角色的名称。

2.  可以，这由一个名为 `Maximum Concurrent Accounts`（最大并发账户数）的属性管理。

3.  按区域顺序（按定义顺序从第一个到最后一个），然后按账户顺序（按定义顺序从第一个到最后一个）。

4.  不行，你必须先删除所有堆栈实例。

5.  管理员角色的唯一操作是代入执行角色。

### 第 6 章：使用 cfn-init 进行 EC2 实例的配置管理

1.  `cfn-init` 辅助脚本按以下顺序处理这些配置节：`packages`、`groups`、`users`、`sources`、`files`、`commands`，然后是 `services`。是的，建议针对复杂配置或按不同顺序运行配置项时这样做。

2.  不行。

3.  不能在配置集中使用 `files` 指令。你必须使用 `commands` 指令，但可以在文件的完整路径中添加缺失的目录，`cfn-init` 会自动创建它（类似于 `mkdir -p` 命令）。

4.  `WaitCondition` 是一种 CloudFormation 资源，用于让 CloudFormation 等待资源完成。例如，你可以将 `WaitCondition` 与 `cfn-signal` 结合使用，使 CloudFormation 仅在 `cfn-init` 运行完成后才通知用户资源创建情况。

### 第 7 章：使用自定义资源创建 AWS 之外的资源

1.  `Create`、`Update` 和 `Delete`。

2.  仅通过 Lambda Layers。或者直接将模块内容复制到 Lambda 函数的代码中。

3.  CloudFormation 作为请求方，AWS Lambda 用于预置，CloudWatch 用于日志和指标，以及其他 AWS 服务（如果它们在自定义资源的逻辑中被使用）。

4.  `responseReason`。

5.  事件是传递给 Lambda 函数的实际负载，必须进行处理。上下文是 Lambda 请求的元数据，包含各种信息，例如请求的时间戳。

### 第 8 章：使用模板宏动态渲染模板

1.  JSON。它既可以是模板的一部分，也可以是完整模板，具体取决于声明了 `Transform` 部分还是使用了 `Fn::Transform` 函数。

2.  不行，因为宏及其函数必须位于同一区域。

3.  堆栈操作将被视为失败，CloudFormation 将开始回滚。

4.  可以，但你需要注意宏的求值顺序 (https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-macros.html#template-macros-use)。

### 第 9 章：使用 AWS CDK 生成 CloudFormation 模板

1.  构造是 CDK 的构建块。任何 CDK 代码块，例如 `App`、`Stack`、`Asset` 或 `Resource` 都被视为一个构造。

2.  你必须通过将堆栈类作为值分配给它，来单独定义每个堆栈。

3.  通过向 VPC 构造添加子网配置。最初，CDK 是由 TypeScript 开发的，但也有适用于 Python、Java、C# 和 JavaScript 的库。CDK 开发团队正在致力于支持更多语言。

4.  不行，CDK 将使用默认凭证和配置连接到 AWS。但在生产环境中使用 CDK 时，建议提供账户 ID 和区域。

### 第 10 章：使用 AWS SAM 部署无服务器应用程序

1.  可以，通过使用 CloudFormation 特定的资源类型。

2.  `sam local start-api`。

3.  不行，SAM CLI 没有这个命令。你必须使用 AWS CLI 来删除堆栈（`aws cloudformation delete-stack`）。

4.  可以，但你必须对 DynamoDB 使用 CloudFormation 特定类型，因为 `SimpleTable` 没有此属性。

5.  应用程序的源代码及其依赖项作为 ZIP 文件上传到 S3。模板会被修改以指向 S3 上的源代码，并保存到磁盘或打印到终端输出。

# 其他您可能感兴趣的书籍

如果您喜欢这本书，您可能会对 Packt 出版的以下书籍感兴趣：

**AWS 安全性食谱**  
Heartin Kanikathottu  
ISBN: 978-1-83882-625-3

*   跨账户创建和管理用户、组、角色和策略
*   使用 AWS 托管服务进行日志记录、监控和审计
*   使用采用机器学习的 AWS 托管服务检查合规性
*   为 EC2 实例和应用程序提供安全性和可用性
*   使用对称和非对称加密保护数据
*   使用联合登录管理用户池和身份池

**AWS SysOps 实战指南 - 第 2 版**  
Eric Z. Beard, Rowan Udell, 等  
ISBN: 978-1-83855-018-9

*   通过创建 IAM 用户并避免使用根用户登录来保护您的账户
*   使用 AWS Control Tower 简化多账户着陆区的创建
*   掌握 Amazon S3，实现无限、具成本效益的数据存储
*   探索 AWS 云上的各种计算资源，如 EC2 和 AWS Lambda
*   使用 Amazon VPC、访问控制列表和安全组配置安全网络
*   使用成本估算工具估算每月账单
*   学习使用 Amazon Route 53、Amazon CloudFront 和 S3 托管网站

# 撰写评论 - 让其他读者知道您的想法




请通过在您购买本书的网站上留下评论，与其他读者分享您对本书的看法。如果您是在亚马逊购买的本书，请在本书的亚马逊页面给我们留下真实的评价。这至关重要，因为其他潜在读者可以看到并利用您的公正意见做出购买决定，我们可以了解客户对我们产品的看法，而我们的作者也能看到您对他们与 Packt 合作创作的这本书的反馈。这只会占用您几分钟时间，但对其他潜在客户、我们的作者和 Packt 来说却非常宝贵。谢谢！
