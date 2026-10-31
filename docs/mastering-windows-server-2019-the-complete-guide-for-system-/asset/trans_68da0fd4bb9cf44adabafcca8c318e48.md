
运行容器基础设施也会影响你在 Windows Server 许可方面的决策。虽然你的主机容器服务器无论是 Windows Server Standard 还是 Datacenter 都可以运行无限数量的传统容器，如果你想进入全新的增强型 Hyper-V 隔离容器世界，规则也同样适用。运行 Windows Server Standard 的容器主机服务器最多只能运行两个 Hyper-V 容器，但将主机升级到 Windows Server Datacenter 即可恢复到无限类别。一种容易记住的方法是，每个 Hyper-V 容器本质上就是自己的 VM，因此在 Standard 操作系统中限制为两个，无论是讨论常规 VM 还是 Hyper-V 容器 VM。

## Three different interfaces

现在让我们讨论可以在 Windows Server 2019 机器上运行的不同占用空间和用户界面。可以使用三种 Windows Server 变体，适合你的取决于你所寻找的功能和安全性。

### Desktop Experience

这是全球 Windows Server 中最常见的选择。无论你是在构建 Windows Server 2019 Standard 还是 Datacenter，你都可以选择是否带图形用户界面运行 Windows Server。传统的外观和感觉以及点选式界面称为 Desktop Experience。这允许诸如通过 RDP 连接到服务器、拥有传统桌面、能够直接从已登录的服务器使用图形化 Server Manager 等操作，总的来说，如果你是服务器管理新手，这是最佳选择。

如果你熟悉在 Windows 10 内部的导航，那么你至少应该能够在运行 Desktop Experience 的 Windows Server 2019 中找到方向。这就是本书大部分内容将聚焦的 Windows Server 2019 版本，几乎所有的截图都将来自 Desktop Experience 环境。

### Server Core

正如我们在安装 Windows Server 2019 时会看到的，安装的默认选项不是 Desktop Experience。这意味着选择默认安装路径会将一个无头版本的 Windows Server 安装到你的机器上，最常见称为 Server Core。无头特性使 Server Core 比桌面版本更快更高效，这是有道理的，因为它不必运行所有额外代码，也不必消耗所有额外资源来启动和显示庞大的图形界面。

Windows Server 中你想做的几乎任何事情都可以在 Server Core 或 Desktop Experience 上完成，主要区别在于界面和安全性。要使用 Server Core，你必须非常熟悉命令行界面（即 PowerShell），并且还必须将远程服务器管理视为与服务器交互的可靠方式。我们将在 *Chapter 10, Server Core* 中更多地讨论 Server Core。

除了性能之外，Server Core 带来的最大好处是安全性。绝大多数试图攻击 Windows Server 的恶意软件都依赖于存在于 Desktop Experience GUI 中的项目。由于这些东西在 Server Core 中甚至根本没有运行——唉，即使你想也无法到达 *desktop*——对 Server Core 机器的攻击要少得多、成功率也低得多。

### Nano Server – now only for containers

确实存在第三种 Windows Server 2019 平台，称为 **Nano Server**。这是一个微小的 Windows Server 版本，像 Server Core 一样无头，但占用空间更小。上次我启动 Nano Server 时，完整操作系统消耗的数据不到 500 MB，这令人难以置信。

在 Server 2016 发布前后，Nano Server 似乎被讨论得更多，因为当时 Microsoft 正积极推进计划在 Nano Server 中包含大量角色，以便我们开始用 Nano 替换一些臃肿、过大日常服务器。过去你可以利用 Windows Server 安装介质（我想你仍可以用 Server 2016 安装介质做到）生成一个 VHDX 文件，让你启动进入 Nano Server 并试用，但那种 *Nano Server as an actual server* 的思维方式自此已被抛弃。

自 Windows Server 版本 1803 起（我们将在本章下一节讨论 *Server 1803* 是什么意思），Nano Server 已与容器使用绑定。事实上，启动 Nano Server 的唯一方式是将其作为 **container base OS image** 下载，然后在现有的容器主机服务器上启动该镜像。我们将在 *Chapter 13, Containers and Nano Server* 中更详细地讨论两者。如果你知道容器和现代应用程序是什么，并且有兴趣使用它们，那么学习有关 Nano Server 的所有知识将受益匪浅。如果你没有条件使用容器，你可能永远不会在环境中遇到 Nano Server。

## Licensing models – SAC and LTSC

关于如何设置 Windows Server 的另一个决策是你想要遵循哪种许可/支持模型和发布节奏。你可以走两条不同的路径。如果两者都需要，可以在一个环境中混合使用。

### Semi-Annual Channel (SAC)

如果你选择运行 Windows Server 的 SAC 版本，操作系统的命名约定会发生变化。与其称之为 Server 2019，你实际上运行的是 Windows Server 1803、1809、1903、1909 等等。它遵循与 Windows 10 相同的理念和发布节奏。这意味着这些新的 Windows Server SAC 版本的发布间隔比以往服务器都要短得多。SAC 通道计划每年接收两次主要版本——通常在春季和秋季。由于发布节奏快，SAC 版本的 Windows Server 支持时间仅为 18 个月。如果你使用 SAC，最好习惯在版本发布后不久就升级到最新版本。

如果一年更换两次服务器操作系统听起来令人望而生畏，你并不孤单。值得庆幸的是，Microsoft 认识到这一点，并意识到一般的服务器管理员群体不会将其用于日常服务器。相反，SAC 版本的 Windows Server 仅用于运行容器和容器化应用程序。在这个灵活的应用程序托管新世界中，应用程序以基础设施资源可按需启动或关闭的方式编写，容器是 DevOps 拼图中的非常重要的部分。如果你托管或构建这类应用程序，你几乎肯定会使用容器——现在或将来。当你发现自己处于研究和弄清容器的位置时，你可能会发现实现高性能容器环境的最佳方式是在 SAC 服务器版本上托管它。

> 需要注意的是，SAC 版本的 Windows Server 仅提供 Server Core 版本——这里找不到图形桌面界面！

### Long-Term Servicing Channel (LTSC)

你们中有些人可能认为 LTSC 是笔误，因为在过去几年中该模型称为 **Long-Term Servicing Branch (LTSB)**。虽然你可以使用任一名称，人们通常会知道你在说什么，但 LTSC 现在是正确的术语。

Windows Server 2019 是一个 LTSC 版本。本质上，LTSC 版本就是我们一直认为的传统 Windows Server 操作系统版本。Server 2008、Server 2008 R2、Server 2012、Server 2012 R2、Server 2016，现在的 Server 2019 都是 LTSC 版本。变化在于 LTSC 版本现在将包含更少 *wow, that's so awesome and brand new* 的东西，因为我们将通过 SAC 版本以更短期的方式看到并获得关于那些全新事物的提示并推出它们。因此，你的 SAC 版本大约每六个月发布一次，然后每两年到三年我们会体验到一个新的 LTSC 版本，它将所有这些更改汇总到一个新完整版本中，同时也授予你访问桌面体验图形界面的权限。

虽然 SAC 通常完全围绕 DevOps 和容器，LTSC 服务器则用于运行几乎所有其他内容。你不会想安装域控制器、证书服务器或文件服务器然后每六个月更换一次服务器。因此，对于这些场景，你总是会选择 LTSC。

同时请记住，大多数 Windows Server 管理员仍然使用 Desktop Experience 图形界面部署服务器，这意味着对于这些服务器用途，你只对 LTSC 感兴趣。SAC 版本的 Windows Server 不包含 Desktop Experience——SAC 仅专注于 Server Core，而 Server Core 没有 GUI。

使用 LTSC 版本的 Windows Server，你继续获得我们习惯的支持：五年主流支持，随后是五年可用的扩展支持。

在整本书中，我们将使用并积累 Windows Server 2019 – LTSC 版本的经验。

## Overview of new and updated features

Windows Server 操作系统的最新版本始终是其前身的演进。内部确实包含一些全新的技术，但现有技术被更新以包含新功能和功能的地方甚至更多。让我们花几分钟时间概述 Windows Server 2019 中存在的一些新功能。
