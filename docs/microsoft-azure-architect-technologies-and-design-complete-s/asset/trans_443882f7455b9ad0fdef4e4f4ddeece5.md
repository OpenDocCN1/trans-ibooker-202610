

- 使用 Visual Studio 部署
- 使用 ARM 模板部署
- 使用 DevOps 和 Azure DevOps 工作

但在开始这些主题之前，先提供一些关于开发和部署主题的历史会是个好主意，因为这方面的内容很多，可以追溯到 1970 年代。成功的开发流程取决于许多因素，已有大量书籍对此进行了详细阐述。本内容并非旨在取代或与之竞争。尽管如此，仍有一些通用活动可应用于所有开发团队流程。其中一些你已经接触过，也适用于通用的项目管理术语。通用或核心活动如下：

- 需求收集
- 设计
- 构建
- 测试
- 调试
- 部署
- 维护

由于你了解混合模型，你可以将概念上的要点应用于上述这些活动，这些活动可以根据不断变化的软件技术、方法论、框架和实践进行修改和增强。对前面那组要点进行少量更改就会产生新的混合技术、方法论或编码模型。例如，结构化编程、面向对象编程（OOP）、SCRUM、快速应用开发（RAD）、敏捷，以及现在使用持续集成/持续交付（CI/CD）实践的 DevOps 模型，都是在先前活动的基础上通过学习和优化构建的混合概念。这些技术仍然存在，当你在使用它们时，要认识到你正在遵循来自那个特定时代的代码模式、技术和方法。在许多方面，你可以根据其运行的模型来判断一个 IT 应用的年代。以下是可能有争议且简化的时间线。

- 1970s/80s: Waterfall
- 1990s: OOP, RAD, and Scrum
- 2000s: Agile
- 2010s: DevOps

现在你已经了解了一些历史，请认识到这方面的内容很多，而且这是一个团队和大量高技能人员共同合作实现伟大目标的领域。如果你对此感兴趣，就进入这个领域并大胆尝试。你将在本节后面更深入地关注 DevOps，现在让我们从使用 Visual Studio 的简单部署方法开始。

## 使用 Visual Studio 部署

如果你是一个独自从事项目的个人开发者，那么这无疑是将相对复杂的应用程序部署到 Azure App Service 的最简单方法。你可以直接在 Azure 门户中在 Azure App Service 上创建应用服务。使用 Visual Studio 可能有点具有挑战性，因此掌握它的人也将掌握复杂代码的创建。练习 8.5 将指导你部署上一章中介绍的代码。

## EXERCISE 8.5

## 使用 Visual Studio 部署 Azure App Service

1. 打开 Visual Studio 并单击 Create A New Project。选择 ASP.NET Core Web Application 并输入项目名称。选择 .NET Core，选择 ASP.NET Core 3.1，选择 Web Application，你应该会看到类似图 8.20 的内容。

FIGURE 8.20 Creating an ASP.NET Core application in Visual Studio

![](img/95043b3df926bcdadadb082b49262037_619_0.png)

2. 用以下位置找到的代码替换以下文件：

github.com/benperk/ASA/tree/master/Chapter08/Ch08Ex04

（你必须更新代码以使用你的 Azure Key Vault、命名空间和 Azure SQL 详细信息。）

Program.cs

Startup.cs

3. 安装以下 NuGet 包。从菜单中选择 Tools: NuGet Package Manager, Package Manager Console，并依次输入以下命令：

```
install-package Microsoft.Azure.KeyVault
install-package Microsoft.Azure.Services.AppAuthentication
install-package System.IdentityModel.Tokens.Jwt
install-package Microsoft.Extensions.Configuration.AzureKeyVault
install-package System.Data.SqlClient
```

4. 按 Ctrl+Shift+B 编译 Visual Studio 解决方案。现在，右键单击你的 Visual Studio 项目并从弹出菜单中选择 Publish。选择 App Service，选择 Existing，然后单击 Create Profile 按钮。 （参见图 8.21。）

FIGURE 8.21 Publishing an ASP.NET Core application in Visual Studio to an app service

![](img/95043b3df926bcdadadb082b49262037_620_0.png)

5. 选择要部署代码的订阅、资源组和 Web 应用，然后单击 OK 按钮。现在，单击 Publish 按钮。

6. 你还需要额外执行练习 7.1（配置托管标识）和练习 7.2（配置 EasyAuth）以使应用程序按预期呈现。向前看一下图 8.31，它说明了托管标识、密钥保管库访问策略和 EasyAuth 配置要求。

就这样。再次查看图 8.21；除了部署到应用服务之外，还有部署应用服务到 Linux 的选项，用于将 Azure 虚拟机部署到 IIS，以及部署到文件夹的选项。如果你选择了 Azure 虚拟机，你将被提示选择要部署代码的目标 VM，类似于图 8.22。

FIGURE 8.22 Publishing an ASP.NET Core application in Visual Studio to an Azure virtual machine

![](img/95043b3df926bcdadadb082b49262037_621_0.png)

从 Visual Studio 部署到虚拟机有两个先决条件。无论部署到 Azure 平台上的机器还是其他机器，需求都相同。首先确保 Azure 虚拟机具有可部署到的终结点。理想情况下，这将是公共域名或 IP 地址。其次，Azure VM 必须配置为支持 ASP.NET Web 部署。这意味着你需要在 Azure VM 上安装并配置 Web Deploy 模块。你可以在此处找到更多详细信息：

```
www.iis.net/downloads/microsoft/web-deploy
```

此功能已在 Azure App Service 上默认配置并准备好接收你的部署。这不是 PaaS 提供的很棒吗？

## 使用 ARM 模板部署

你已经学习了一些用于使用相同映像、快照、备份和磁盘进行部署的方法。Azure Resource Manager（ARM）模板是实现此目的的另一种方式。ARM 模板是描述你的 Azure 资源为脚本形式的 JSON 文档。查看位于此处的名为 ARM-template-VM.json 的文件：

github.com/benperka/ASA/tree/master/Chapter08

你可以通过单击 Review + Create 刀片上的 Download a Template For Automation 链接来提取它，类似于图 8.23。

FIGURE 8.23 Downloading a template for automation

![](img/95043b3df926bcdadadb082b49262037_622_0.png)

该下载 ARM 模板的链接存在于大多数 Azure 产品中，但它代表给定资源的默认预配状态模板。如果你需要在资源创建后获取 ARM 模板，导航菜单中有一个名为 Export Template 的链接。使用 Export 模板的好处是，如果你进行了一些配置，例如要求 HTTPS 或仅允许 TLS 1.2，那么这些配置将被包含在 ARM 模板中。这很重要，因为如果你由于练习 6.2 中创建的 Azure Policy 而要求 HTTPS 和 TLS 1.2，尝试在未设置这些属性的情况下创建这些资源将会失败。查看位于 GitHub 上与前面提供的 VM 相同位置的 ARM-template-APP-SERVICE.json 文件。浏览它，你会看到 "httpsOnly": true 和 "minTlsVersion": "1.2" 已正确设置在模板中。你可以以多种方式组织你的 ARM 模板以进行部署。到目前为止提供的示例，以及 GitHub 上的示例模板，代表了一个 VM 模板、一个 App Service 模板和一个 Azure SQL 模板（参见 ARM-template-AZURE-SQL.json）。该场景代表将多层 Azure 产品部署到同一资源组。每个资源都按照预定的顺序手动预配。

