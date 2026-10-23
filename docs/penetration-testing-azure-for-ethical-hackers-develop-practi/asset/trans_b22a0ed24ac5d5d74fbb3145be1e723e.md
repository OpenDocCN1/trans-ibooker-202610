

- **任务 3：** 安装并验证 Azure PowerShell 模块
- **任务 4：** 安装并验证 Azure AD PowerShell 模块

让我们按如下方式安装 Azure PowerShell 和 Azure AD PowerShell：

- 1. 在你的渗透测试 VM 上以管理员身份打开 PowerShell。
- 2. 使用以下命令验证是否已安装 PowerShellGet：

```
Get-Module -ListAvailable PowerShellGet
```

它看起来会像这样：

![](img/4fa338e84e0a590967a142dd798ba4e2_85_0.png)

图 2.35 – 将 PowerShell gallery 配置为受信任的存储库

- 3. 我们将要安装的两个模块都将从 PowerShell gallery 存储库下载。为了避免收到有关存储库信任级别的消息，我们可以使用以下命令将其配置为受信任的存储库。系统提示安装 NuGet 提供程序时，输入 Y 表示 yes，然后按 Enter：

```
Set-PSRepository -Name PSGallery -InstallationPolicy Trusted
```

它看起来会像这样：

![](img/4fa338e84e0a590967a142dd798ba4e2_85_1.png)

图 2.36 – 将 PowerShell gallery 配置为受信任的存储库

- 4. 使用以下命令安装 Azure PowerShell 模块：

```
Install-Module -Name Az -AllowClobber
```

- 5. 使用以下命令验证安装：

```
Get-Module -ListAvailable Az.*
```

它看起来会像这样：

![](img/4fa338e84e0a590967a142dd798ba4e2_86_0.png)

图 2.37 – 验证 Azure PowerShell 模块安装

- 6. 使用以下命令安装 Azure AD PowerShell 模块：

```
Install-Module AzureAD -AllowClobber
```

- 7. 使用以下命令验证安装：

```
Get-Module -ListAvailable AzureAD
```

它看起来会像这样：

![](img/4fa338e84e0a590967a142dd798ba4e2_86_1.png)

图 2.38 – 验证 Azure AD PowerShell 模块安装

- 8. 将 PowerShell 控制台保持打开以进行下一个练习。

我们现在已在 VM 上设置了大部分基础工具。为了为我们在 Linux 中使用的工具提供一些额外支持，我们接下来将在 Linux 子系统上安装 Azure CLI。

## 实践练习：在你的渗透测试 VM（WSL）上安装 Azure CLI

Azure CLI 是管理员和开发人员用来连接 Azure 并在 Azure 资源上执行管理命令的另一个命令行工具。诸如 Lava 之类的 Azure 特定利用框架需要安装 Azure CLI。在本练习中，我们将在 WSL 中安装 Azure CLI。按如下方式进行：

- 1. 在你的渗透测试 VM 上之前打开的 PowerShell 控制台中，输入 bash 然后按 Enter 切换到 Windows Subsystem for Linux。
- 2. 在 bash shell 中，使用以下命令切换到 root 用户。系统提示时输入管理员密码：
- 3. 使用以下命令安装 Azure CLI：
- 4. 使用以下命令验证安装：
- 5. 在继续进行下一个练习之前关闭打开的 PowerShell 控制台。

```
sudo su -
```

它看起来会像这样：

![](img/4fa338e84e0a590967a142dd798ba4e2_87_0.png)

图 2.39 – 切换到 root 用户

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

```
az version
```

它看起来会像这样：

![](img/4fa338e84e0a590967a142dd798ba4e2_87_1.png)

图 2.40 – 验证 Azure CLI 安装

我们的 VM 现在已按我们需要的方式配置完毕，我们现在准备开始讨论工具。这些将是管理工具和安全工具的混合，将在 Azure 渗透测试的多个不同层级中使用。

注意
强烈建议在不使用渗透测试 VM 时将其关闭以节省一些成本。你可以通过 Azure 门户停止 VM 来实现。

## Azure 渗透测试工具

在本書的整个过程中，我们将使用不同的工具来模拟对手的战术和技术。我们可以根据其典型用途将这些工具分为不同的类别。

首先，我们有 **Windows 或 Linux 管理工具**，概述如下：

- 管理员通常用于一般系统管理的工具
- **示例：** `JQ`、`httpie`、`wget`、`curl`、`unzip` 和 `PowerShell`

接下来是 **通用渗透测试工具**，概述如下：

- 通用服务和漏洞识别工具
- **示例：** `gobuster`、`nmap`、`dns scan` 和 `hydra`

最后，我们有 **Azure 特定渗透测试工具**，概述如下：

- 针对 Azure 平台相关漏洞进行优化的渗透测试工具
- **示例：** MicroBurst、Lava、Koboko、PowerZure、Stormspotter 和 BloodHound

目前这类 Azure 特定工具非常少，而且大多数都是开源的。这通常意味着创建者和贡献者会在自己的时间里发布和更新这些工具，因此经常有机会让其他人为这些工具做出贡献。本书的作者（以及我们的技术审阅者）都为开源工具做出了贡献，包括上述一些工具，我们的工作证明了如果需要的工具或功能不存在，你完全可以自己编写。

需要注意的一个重要区别是通用渗透测试工具和 Azure 特定工具之间的区别。虽然许多通用渗透测试工具可以用于攻击 Azure，但它们通常需要配置为专注于 Azure 域和服务。本书中提到的 Azure 特定工具要么是专门构建为针对 Azure 域进行范围限定，要么是围绕现有的 Microsoft **应用程序编程接口（APIs）**、PowerShell 模块和库构建，以直接集成到 Azure 中。

当我们进入其他章节中的动手练习和场景时，我们将确保 *描述我们将使用的工具*（以便你了解该工具及其用例）、*如何在你的渗透测试系统上安装该工具*，以及——当然——*如何使用该工具来实现所述目标*。

在我们阅读本书时请记住一点：如果你在渗透测试中反复做某件事，可能值得为其创建一个工具。本书中介绍的许多工具都是为取代在 Azure 渗透测试中反复手动执行的过程而开发的。

如果没有工具可以实现你想要自动化的功能，请查看现有项目并找到一个项目进行贡献，或创建新项目。请记住，本书中概述的大多数工具和技术都是由慷慨付出时间的人为安全社区的改善而创建的。如果你有机会，请花点时间感谢为这些工具集做出贡献的任何人，尤其是本书中使用的那些。

## 总结

你可能会问自己：*我们真的需要花费 30 多页来学习如何在 Azure 中部署资源吗？* 这是一个公平的问题，对于那些已经知道本章所涵盖所有内容的人来说，恭喜你带着现有的扎实知识基础来阅读本书。

对于那些需要更多指导才能完成本章的人，不要感到气馁，如果这有点让人不知所措。学习如何在 Azure 中部署和管理资源可能有点棘手，但一旦进入实际测试，这将给你带来优势。

本章涵盖了我们访问和测试 Azure 环境所需的许多工具和技术基础。这些工具在这些练习之外进行自己的测试时将至关重要，因此请将本章加入书签，以便在未来需要构建另一个 Azure 测试环境时使用。

现在我们已经了解了 Azure 的基础知识，并且有一个准备好进行测试的环境，在下一章中，我们可以开始测试 Azure 中的漏洞。
