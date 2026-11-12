# 使用 Azure 的 C# 8 和 .NET Core 3 项目：构建满足现代软件要求的专业桌面、移动和 Web 应用程序指南 第 2 版








致我的女儿 Abigail，她慷慨、高尚、机智、幽默，并相信我；以及我的儿子 Dylan，他比我聪明得多，善良、温和，总是面带微笑。我视自己为世上最富有的人，能称你们俩为我的孩子，我为你们俩感到骄傲！

- Paul Michaels

# Packt

Packt.com

订阅我们的在线数字图书馆，即可完整访问超过 7,000 本书籍和视频，以及行业领先的工具，帮助您规划个人发展并推进职业生涯。如需更多信息，请访问我们的网站。

# 为什么要订阅？

- 利用超过 4,000 位行业专业人士提供的实用电子书和视频，减少学习时间，增加编码时间
- 使用专为您构建的技能计划，改善您的学习体验
- 每月获取一本免费电子书或视频
- 完全可搜索，轻松访问重要信息
- 复制粘贴、打印和加书签内容

你知道吗？Packt 为每本出版的书籍提供电子书版本，并提供 PDF 和 ePub 文件。您可以在 www.packt.com 升级到电子书版本，作为印刷书籍客户，您有权获得电子书副本的折扣。如需更多详情，请联系 customercare@packtpub.com。

在 www.packt.com 上，您还可以阅读一系列免费技术文章，订阅各种免费简报，并获取 Packt 图书和电子书的独家折扣和优惠。

# 贡献者

# 关于作者

**Paul Michaels** 是一名拥有 20 多年经验的首席开发人员。他喜欢编程、玩转新技术和解决问题。不工作时，您可以看到他在峰区骑自行车或散步、打乒乓球，或者尝试为妻子和两个孩子做饭。您可以在 Twitter 上关注他 @paul_michaels，或在 LinkedIn 上搜索 pcmichaels 找到他。他还写博客，链接可在他的 LinkedIn 和 Twitter 个人资料中找到。

> *特别感谢 Abigail Michaels 为本书提供了插画。*

**Dirk Strauss** 是 Embrace 的全栈开发人员。他喜欢学习并与他人分享所学内容。Dirk 为 Packt 出版了关于 C# 的书籍，也为 Syncfusion 出版了电子书。业余时间，他通过弹吉他和尝试学习 Jimi Hendrix 的乐句来放松。您可以在 Twitter 上找到他 @DirkStrauss。

> *我想感谢我的妻子、儿子和女儿的支持，她们始终陪伴在我身边。我全心全意地爱你们。*

**Jas Rademeyer** 在 IT 行业拥有超过 15 年的经验，职业生涯大部分时间专注于软件方面。他拥有信息科学学位，专攻多媒体，参与过开发的各个方面，从架构和解决方案设计到用户体验和培训。他目前担任技术解决方案经理，负责管理微软领域各种项目上的开发团队。他是一个重视家庭的人，内心是个音乐家，业余时间与妻子和两个孩子共度，并在教堂敬拜乐队中服事。

# 关于评审员

Alvin Ashcraft 是一名居住在费城附近的开发人员。他 23 年的职业生涯一直在使用 C#、Visual Studio、WPF、ASP.NET 等构建软件。他九次荣获 Microsoft MVP 称号。您可以在他的博客 Morning Dew 上阅读他每天为 .NET 开发人员分享的链接。他担任 Allscripts 的首席软件工程师，构建医疗保健软件。他之前曾受雇于包括 Oracle 在内的软件公司。他还为 Packt Publishing 评审过其他书名，例如 Mastering ASP.NET Core 2.0、Mastering Entity Framework Core 2.0 和 Learning ASP.NET Core 2.0。

> 我想感谢我可爱的妻子 Stelene 和我们三个了不起的女儿的支持。在我晚上和周末阅读并评审这些章节以帮助为 .NET 开发者交付一本有用、高质量的书籍时，她们非常体谅。

# Packt 正在寻找像您这样的作者

如果您有兴趣成为 Packt 的作者，请访问 authors.packtpub.com 并立即申请。我们与成千上万像您一样的开发人员和技术专业人士合作，帮助他们向全球技术社区分享见解。您可以提交通用申请，申请我们正在招募作者的特定热门话题，或提交自己的想法。






## 前言

.NET Core 是一种通用的、模块化的、跨平台的、开源的 .NET 实现。.NET Core 3 的最新版本提供了改进的性能，并支持桌面应用程序。.NET Core 3 不仅应该吸引新开发者开始学习该框架，还应该说服旧版开发者开始迁移他们的应用。

本书是《C# 7 和 .NET Core 2.0 Blueprints》的第二版，已更新为包含 C# 8 和 .NET Core 3.0 的最新功能和增强。本书是一本全面指南，提供 10 个真实世界的企业级应用程序。它将帮助你同时学习和实现概念，并通过在 ASP.NET Core 和 Azure 上构建满足现代软件要求的有效应用程序来取得进步。

我们将使用 Entity Framework Core 3 处理关系数据，并使用 ASP.NET Core 创建真实世界的 Web 应用程序。我们将了解读者如何将旧的 WinForms 应用升级到 .NET Core 的最新版本。我们还将使用 SignalR 创建一个实时聊天应用程序。最后，我们将学习使用 Azure Storage 进行无服务器计算，以及如何使用 Docker 和 Kubernetes 构建负载均衡的订单处理微服务。

总而言之，本书将通过各种项目和应用程序教你 Web 应用程序、无服务器计算和微服务的核心概念。按照本分步指南，你将能够创建 ASP.NET Core MVC 应用程序，并使用 Microsoft Azure 的前沿服务构建现代应用程序。

## 本书适用人群

本书适用于希望构建真实世界项目并学习 .NET Core 3 新功能的业余开发者/程序员以及专业人士。对于希望将旧版桌面软件迁移到 .NET Core 3 的开发人员，本书也会非常有用。假设读者具备 .NET Core 和 C# 的基础知识。

## 本书内容概述

- 第 1 章，电子书管理器和目录应用 – Windows 桌面上的 .NET Core 3，介绍 .NET Core 3 的关键功能——其主要功能是 .NET Core 支持桌面应用程序。你将基于本书前一版本创建一个 WinForms 应用程序，并将其升级为使用 .NET Core 3。然后，我们将介绍 XAML 岛，使用 UWP 创建新的桌面控件，并将其添加到现有 WinForms 应用程序中。
- 第 2 章，使用 Cosmos DB 的任务 Bug 记录 ASP.NET Core MVC 应用，专注于创建允许用户捕获任务并记录问题的 ASP.NET Core MVC 应用程序。该应用程序将允许你查看捕获的任务并采取行动。
- 第 3 章，ASP.NET Azure SignalR 聊天应用程序，使用 ASP.NET SignalR 创建一个实时聊天应用程序。实时 Web 功能是服务器端代码在事件发生时向连接的客户端推送内容的能力。创建完成后，我们将创建一个 Azure App Service 实例并将应用程序托管在那里。
- 第 4 章，使用 Entity Framework Core 的 Web 研究工具，向你介绍 Entity Framework Core，并展示如何创建可用于保存链接和社交媒体帖子以供研究用途的 ASP.NET Core MVC 应用程序。许多此类应用程序已存在，例如 Instapaper 和 Evernote。但是，本应用程序将向你展示如何自行构建并添加特定功能。
- 第 5 章，使用 Azure 逻辑应用和函数构建 Twitter 自动化活动管理器，探索 Azure 的逻辑应用。本章将指导你完成逻辑应用程序的创建，将应用程序集成到 Twitter，并允许用户将数据输入电子表格，然后自动在 Twitter 上发布。
- 第 6 章，使用 Identity Server 和 OAuth 2 的股票检查器，以 Identity Server OSS 作为模板说明身份验证的概念。本章将指导你创建自己的身份服务器，然后从 UWP 应用程序登录到它。
- 第 7 章，使用 Windows 服务和 Azure Storage 构建照片存储应用程序，说明无服务器计算的概念。你将创建一个将用户 PC 上的照片备份到 Azure Storage 的应用程序。当今有许多备份服务可供用户使用。Azure Blob 存储只是此类服务之一，它允许开发人员创建利用 Microsoft 服务器存储文件的应用程序。
- 第 8 章，使用 Docker 和 Azure Kubernetes 服务构建负载均衡的订单处理微服务，首先介绍微服务的概念，解释什么是微服务以及为什么要使用它们。在本章中，我们将介绍分布式系统的概念。我们将构建一个微服务，在 Azure Kubernetes 上配置 Kubernetes 集群，并使用存储队列与我们的微服务进行交互。
- 第 9 章，使用 Xamarin.Forms 和 Azure 认知服务构建情绪检测器移动应用程序，使用 Xamarin.Forms 创建移动应用程序。在本章中，我们将与 Azure 认知服务和设备上的摄像头集成，允许用户拍摄人脸照片，并由 Azure 返回对该人情绪的评分。然后我们将在屏幕上显示我们认为该人的感受。我们将跨平台编译此应用以适用于 Android。
- 第 10 章，21 世纪的 Eliza – UWP 和 MS 机器人框架，使用 .NET Core 3 设置新的 UWP 应用程序。这将是一个简单的聊天应用程序，但将与 LUIS 和旨在通过图灵测试的 MS 聊天机器人进行交互。
- 附录 A，WebAssembly，介绍 WebAssembly，它最近已集成到所有主要浏览器中，并允许代码编译为 WASM（一种浏览器的中间语言）。Microsoft 最近发布了称为 Blazor 的预览版，允许 Razor 语法在 JavaScript 位置运行。

### 充分利用本书

为了按照本书中的说明进行操作，你需要具备以下先决条件：

- Azure 订阅
- Visual Studio
- Excel/Office Online 副本
- OneDrive 账户
- Twitter
- Postman

### 下载示例代码文件

你可以在 www.packt.com 的账户中下载本书的示例代码文件。如果你在其他地方购买了此书，可以访问 www.packt.com/support 并注册，以便文件直接通过电子邮件发送给你。

你可以按照以下步骤下载代码文件：

- 1. 在 www.packt.com 上登录或注册。
- 2. 选择 SUPPORT 选项卡。
- 3. 单击 Code Downloads & Errata。
- 4. 在搜索框中输入书名，并按照屏幕上的说明操作。

下载文件后，请确保使用最新版本解压或提取文件夹：

- Windows 上的 WinRAR/7-Zip
- Mac 上的 Zip（例如 iZip/UnRarX）
- Linux 上的 7-Zip/PeaZip

本书的代码包也托管在 GitHub 上，网址为 https://github.com/PacktPublishing/C-8-and-.NET-Core-3-Projects-Using-Azure-Second-Edition。如果代码有更新，将在现有的 GitHub 仓库中进行更新。

我们还有来自丰富图书和视频目录的其他代码包，可在 https://github.com/PacktPublishing/ 上找到。去看看吧！

### 下载彩色图片

我们还提供一个包含本书中使用的屏幕截图/图表彩色图片的 PDF 文件。你可以在此处下载：https://static.packt-cdn.com/downloads/9781789612080_ColorImages.pdf。

### 本书使用的约定

全书使用了多种文本约定。

CodeInText：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 帐号。例如：“打开 Document.cs 文件，并将以下代码添加到类中。”

代码块按以下形式呈现：

```
public class Document
{
    public string Title { get; set; }
    public string FileName { get; set; }
    public string Extension { get; set; }
    public DateTime LastAccessed { get; set; }
    public DateTime Created { get; set; }
    public string FilePath { get; set; }
    public string FileSize { get; set; }
}
```

当我们希望引起你对代码块中特定部分的注意时，相关行或项目将设置为粗体：

```
public class Document
{
    public string Title { get; set; }
    public string FileName { get; set; }
    public string Extension { get; set; }
    public DateTime LastAccessed { get; set; }
    public DateTime Created { get; set; }
    public string FilePath { get; set; }
    public string FileSize { get; set; }
}
```

**粗体：**表示新术语、重要单词或屏幕上看到的单词。例如，菜单或对话框中的单词在文本中会这样出现。这里是一个示例：“还有其他选项可以为 **可为空** 选项配置。”

警告或重要说明会以这种方式显示。

技巧和窍门会以这种方式显示。




## 联系我们

读者的反馈始终受欢迎。

**一般反馈**：如果您对本书的任何方面有疑问，请在邮件主题中注明书名，并发送邮件至 customercare@packtpub.com。

**勘误**：虽然我们已竭力确保内容的准确性，但错误难免。如果您在本书中发现了错误，我们将不胜感激如果您能向我们报告。请访问 www.packt.com/submit-errata，选择您的书，单击“勘误提交表单”链接，并填写详细信息。

**盗版**：如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激如果您能向我们提供位置地址或网站名称。请通过 copyright@packt.com 与我们联系，并附上材料链接。

**如果您有兴趣成为作者**：如果您有擅长的主题，并且有兴趣撰写或参与编写一本书，请访问 authors.packtpub.com。

## 评论

请留下评论。阅读并使用本书后，为什么不在购买该书的网站上留下评论呢？这样，潜在读者就可以看到并使用您客观公正的意见来做出购买决定，我们在 Packt 可以了解您对我们产品的看法，我们的作者可以看到您对其书籍的反馈。谢谢！

有关 Packt 的更多信息，请访问 packt.com。
