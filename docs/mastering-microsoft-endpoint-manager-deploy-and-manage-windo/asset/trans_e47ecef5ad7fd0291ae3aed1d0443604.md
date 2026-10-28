
Teams 通过共享 OneDrive 和 SharePoint 文档、见解和状态更新，让您的团队保持同步，同时能够管理重要项目并轻松找到人员——无论身处何地，使用任何设备！

使用 Microsoft Teams，您可以执行以下操作：

- **通过聊天、会议和通话进行沟通：** 主持音频、视频和网络会议，并与组织内外的任何人聊天。
- **通过集成的 Microsoft 365 应用进行协作：** Teams 允许用户通过流行的 Microsoft 365 应用（从 Microsoft Word 到 Microsoft Power BI）共同创作和共享文件，从而让团队协作变得轻松。
- **自定义工作区并实现更多目标：** 使用 Teams，您可以集成 Microsoft 和第三方合作伙伴服务的应用，以满足您组织的独特需求。
- **跨设备连接：** Teams 和 Teams 设备可完美配合，提供智能会议和通话体验。找到适合您需求的设备，将您的最佳想法付诸实践。

## Microsoft Edge

Microsoft Edge 作为 Internet Explorer 的下一代现代迭代产品，已经存在了一段时间，于 2015 年首次发布。5 年后，基于开源软件项目 Chromium 构建的新版本 Edge 发布。它使用与 Google Chrome 浏览器相同的核心引擎。

Microsoft Edge 已被证明非常快速。它与 MEM 等其他 Microsoft 服务保持一致以设置策略，以及该应用跨平台同步个人历史记录和收藏夹设置等数据的能力，都受到了好评。这使得 Edge 至今仍是 Windows 10 的默认浏览器。

Microsoft Edge 可用于 Windows、macOS、iOS、Android 和 Linux。您可以选择要在不同平台上使用相同原生 Edge 体验的设备。

## Universal Print

您可能还记得以下工作流程——或者至今仍在这样做：启动 Windows Server 环境，添加打印服务器角色，然后开始将打印机和指定驱动程序添加到服务器。这并不现代或高效，不是吗？Universal Print 提供了相同甚至更多的功能，同时还消除了对本地基础架构的需求。它使您能够直接通过 Microsoft Azure 中的集中式门户管理打印机。告别在设备和/或黄金映像上安装（和维护）打印机驱动程序吧。额外的好处是，一切都可与 Azure AD 配合使用。这意味着用户可以使用他们用于其他 Microsoft 服务的同一组凭据，无论他们登录的是物理桌面还是云端运行的虚拟桌面。

## Microsoft Defender for Endpoint（前身为 MDATP）

Microsoft Defender for Endpoint 是 Microsoft Defender 的企业版，Windows 10 企业版和 Windows 11 企业版默认启用。它是一个云安全平台，旨在帮助企业网络预防、检测、调查和响应高级威胁。

该服务已端到端集成到 MEM 控制台中，因此可以轻松地作为安全基线的一部分，与其他合规性和安全设置及角色保持一致。

将 Defender 集成到 MEM 中的一大优势是，在您的组织使用配置包载入设备后，您将无需再次执行此操作。您的所有物理和/或云 PC 都将开箱即用地受到保护：

![](img/a3e9f26159d3739e434fe676b2987c1d_44_0.png)

图 1.7 – Defender for Endpoint – 管理控制台

Intune 中的 Defender for Endpoint 配置过程非常简单，我们将在本书后面更详细地说明。

## Exchange Online

Exchange Online 是一个用于电子邮件、日历、联系信息和任务的消息传递与协作平台。您可以通过 Microsoft Outlook、Outlook 网页版或 Outlook Mobile 访问所有这些内容。您可以在大多数设备上访问 Exchange Online，包括 Android、iOS 和 Windows 10 设备。

## SharePoint Online

SharePoint Online 是 Microsoft SharePoint Server 的云端演进。它帮助您创建以团队或沟通为中心的网站，以实现高效的协作和沟通。拥有相应 Microsoft 365 或 SharePoint Online 许可证的内部用户可以使用 SharePoint Online。他们可以与组织内外的其他人共享文件或文件夹。组织外部的共享可由网站管理员控制。

使用 SharePoint Online，用户可以执行以下操作：

- 构建网站、页面、文档库和列表。
- 添加 Web 部件以自定义其页面。
- 与团队共享重要视觉内容、新闻和更新。
- 在整个组织中搜索和发现网站、文件、人员和新闻。
- 使用流程、表单和列表管理其业务流程。
- 将其文件同步并存储在云端，以便任何人都可以安全地使用它们。
- 通过 SharePoint 移动应用随时了解最新动态。

## 总结

在本章中，您了解了在日常工作中或在使用 MEM 简化环境管理开销的过程中可能使用的所有不同 Microsoft 365 服务。

本章主要是为本书后续章节提供背景；现在我们将转换重点，带您踏上使用 MEM 进行统一端点管理的旅程。

## 问题

1. 使用 MEM 的主要好处是什么？
   A. MEM 确保组织不再需要凭据即可访问和共享公司数据。
   B. MEM 阻止远程设备和应用访问组织的资源。
   C. MEM 帮助保护组织的云和本地设备、应用及数据的安全。
2. Windows 365 Cloud PC 和 AVD 之间的主要区别是什么？
   A. Windows 365 Cloud PC 专为简单性而设计，AVD 则专为灵活性而设计。
   B. Windows 365 Cloud PC 采用固定价格成本模型，而 AVD 基于消耗量计费。

## 答案

1. (C)
2. (A)

## 拓展阅读

读完本章后，如果您想了解有关 MEM 的更多信息，请使用以下免费在线资源：

- MEM 基础知识：https://docs.microsoft.com/en-us/learn/paths/endpoint-manager-fundamentals/
- MEM 简介：https://docs.microsoft.com/en-us/learn/modules/intro-to-endpoint-manager/

## 2 什么是统一端点管理？

现代管理的基础有时难以理解。您将了解现代管理和零信任的概念，以及作为 Microsoft Endpoint Manager 一部分的 Intune，及其历史和架构概念，从而清楚地理解所有设备——物理设备、虚拟设备和移动设备——如何汇聚到一个管理控制台中。

在本章中，我们将讨论以下主题：

- 现代管理之路
- Microsoft Endpoint Manager 和 Intune
- 深入了解 Windows 10 企业版
- 自带设备
- 什么是零信任？

### Microsoft Endpoint Manager 和 Intune

什么是统一端点管理？通过 **Microsoft Endpoint Manager** 的概念，它又是怎样的？下图的高级架构图解释了 Microsoft Endpoint Manager 中的一切如何汇聚为统一的端点管理体验。

通过 **Windows 365** 端点和移动设备，一个控制台即可管理您的物理和云 PC。这是唯一可以统一管理它们的地方。此外，Intune 公司门户可以部署来自 Configuration Manager、Intune、Windows 365、Azure Virtual Desktop 和 Azure Active Directory 的应用——为所有应用提供统一的最终用户体验！

![](img/a3e9f26159d3739e434fe676b2987c1d_49_0.png)

图 2.2 – Microsoft Endpoint Manager 架构图

上图中的 Microsoft Endpoint Manager 架构展示了使用 Configuration Manager 和 Intune 在单一、统一端点管理解决方案中进行云管理之旅的三个阶段：

1. 租户附加
2. 共同管理工作负载
3. 云原生管理
