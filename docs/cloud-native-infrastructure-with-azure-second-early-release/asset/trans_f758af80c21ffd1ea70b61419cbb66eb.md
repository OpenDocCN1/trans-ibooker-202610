

new configuration it can be continuously integrated into your deployment pipeline.

## Terraform

Terraform 是 HashiCorp 的开源 IaC 工具，使用 Go 编程语言编写。单词 *terraform* 的意思是把一个星球的环境改造得与地球大气层非常相似，以至于人类物种能够在上面生存。当你把云服务提供商看作需要被改造为可管理的基础设施的环境时，这种类比就成立了，这样可以帮助你的应用程序充分利用云并发挥其真正潜力。

Terraform 帮助你以安全高效的方式在云中构建、更改和版本化基础设施。它使用声明式语法，这意味着你只需要告诉它*什么*你想要构建，而不是*如何*构建它。Terraform 的一个主要优势是它支持一种易于理解的领域特定语言（DSL），称为 HashiCorp 语言（HCL），相对容易被人类解读。或者，你也可以使用 JSON 来实现相同的目的。此外，Terraform 是云无关的，因此支持多个云提供商，这使得学习变得非常容易，因为你不必为不同的云提供商再次学习语言（HCL/JSON）。在本节中，我们将讨论如何利用 Terraform 按照 IaC 哲学在 Azure 上构建基础设施。

为了让你理解 Terraform 的工作原理，首先我们必须回顾一些基本术语：

### Providers

Terraform 中的 provider 通常（但不总是）是一个云提供商。provider 的职责是公开资源（例如云资源）并解释 API。Terraform 支持的一些 provider 有 Azure、Amazon Web Services（AWS）、Google Cloud Platform（GCP）和 PagerDuty。

### Plan

当你使用命令 `terraform plan` 创建基础设施时，Terraform 会为你创建一个执行计划，该计划基于你通过 Terraform 配置（.tf）文件编写的代码。执行计划包括 Terraform 将采取的构建所请求基础设施的步骤。

### Apply

当你对 Terraform 创建的计划感到满意时，你可以使用 `terraform apply` 命令来创建基础设施，最终达到期望状态（如果没有冲突或错误）。

### State

Terraform 需要一种方式来存储基础设施的状态，以便它可以管理它。状态存储在基于文本的 *state files* 中，这些文件保存了你的云资源与配置之间的映射。一般来说，Terraform 可以以两种方式存储基础设施的状态：本地保存在初始化它的系统上，或远程存储。推荐远程存储状态的方法，因为它可以备份你的基础设施状态，并且更多工程师可以一起构建基础设施，因为现在有一个单一事实来源，使得了解基础设施的当前状态成为可能。状态文件不应检入 Git 仓库，因为它可能包含机密信息；你可以使用 Azure blob storage 代替。Terraform 默认也支持状态文件的锁定。

### Module

terraform module 是一组一起使用的资源。模块通过抽象一次配置并多次使用的资源来帮助创建可重用的基础设施。使用模块的主要优势之一是它们允许你重用代码，该代码以后可以在团队之间跨各种项目共享。

### Backend

backend 描述了状态将如何加载。它是你状态文件最终将被存储的位置。你可以使用本地文件存储或 Azure blob storage 用于此目的。

Figure 2-4 显示了 provider 和 resource 块。provider 块提及 Azure 资源管理器的版本，而两个 resource 块创建 Azure 资源组和虚拟网络。

```
provider "azurerm" {
  version = "2.0.0"
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "dummy-resource-group"
  location = "West Europe"
}

resource "azurerm_virtual_network" "example" {
  name                 = "example-network"
  resource_group_name  = azurerm_resource_group.example.name
  location             = azurerm_resource_group.example.location
  address_space        = ["10.0.0.0/16"]
}
```

Figure 2-4. A simple Terraform file depicting resource creation and the Azure provider

现在我们已经介绍了 Terraform 的基础知识，让我们看看它们如何结合起来在 Azure 中配置云原生基础设施。Figure 2-5 描述了总体工作流程。

![](img/8d47d1b5dbca55e784ae602145a894c7_33_0.png)

Figure 2-5. Terraform workflow

要开始使用 Terraform，请遵循以下步骤：

- 使用 `terraform init` 命令初始化 Terraform。如果你没有初始化目录，就无法运行 `plan` 和 `apply`。
- 在你成功初始化 Terraform 后，使用 `terraform plan` 命令创建执行计划或进行试运行。
- 当你对计划的输出感到满意时，使用 `terraform apply` 命令创建你的基础设施。

根据你使用的 provider，基础设施将在相应的云提供商环境中进行配置（在我们的例子中，是 Microsoft Azure）。如果你想销毁你的基础设施，你可以简单地发出 `terraform destroy` 命令，这将删除与当前目录相对应的基础设施。使用 Terraform 删除基础设施时务必极其小心。

> **WARNING**
> 
> 使用 Terraform 删除基础设施时务必极其小心。一旦基础设施被删除，状态文件将使用更新后的值进行更新。在删除基础设施之前，请目视检查 `terraform destroy` 的输出，以免意外删除重要信息。

现在你对工作流程和操作细节有了大致的了解，让我们更深入地实践 Terraform。

### Installing Terraform

要将 Terraform 安装到你的机器上，⁴ 首先你必须为你的操作系统找到适当的二进制包（https://www.terraform.io/downloads.html）。如果你使用 Azure Cloud Shell，Terraform 的最新版本默认会存在。截至本文撰写时，Terraform 的当前版本是 0.12.28。要找到适当的二进制包：

```
# For Linux-based systems:
$ wget
https://releases.hashicorp.com/terraform/0.12.28/terraform_0.12.28_linux_amd64.zip

# For macOS-based systems:
$ wget
https://releases.hashicorp.com/terraform/0.12.28/terraform_0.12.28_darwin_amd64.zip
```

一旦你获取了 ZIP 文件，解压二进制文件并将单个二进制文件移动到 /usr/local/bin。你可以通过从 shell 使用 Terraform 的帮助来验证 Terraform 是否已安装：

```
$ ~ terraform --version
Terraform v0.12.28
$ ~ terraform --help
Usage: terraform [-version] [-help] <command> [args]
```

可用的执行命令列在下面。
最常用、最有用的命令首先显示，其次是较不常见或更高级的命令。如果你刚开始使用 Terraform，请坚持使用常用命令。对于其他命令，请在使用前阅读帮助和文档。

- Common commands:
  - apply                Builds or changes infrastructure
  - console              Interactive console for Terraform
  - interpolations
  - destroy              Destroy Terraform-managed infrastructure
  - env                  Workspace management
  - fmt                  Rewrites config files to canonical format
  - get                  Download and install modules for the configuration
  - graph                Create a visual graph of Terraform resources
  - import               Import existing infrastructure into Terraform
  - init                 Initialize a Terraform working directory
  - login                Obtain and save credentials for a remote host
  - logout               Remove locally-stored credentials for a remote host
  - output               Read an output from a state file
  - plan                 Generate and show an execution plan
  - providers            Prints a tree of the providers used in the configuration
  - refresh              Update local state file against real resources
  - show                 Inspect Terraform state or plan
  - taint                Manually mark a resource for recreation
  - untaint              Manually unmark a resource as tainted
  - validate             Validates the Terraform files
  - version              Prints the Terraform version
  - workspace            Workspace management

