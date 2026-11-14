

# 前言

## 代码下载

您可以通过以下步骤下载代码文件：

1.  在 [www.packt.com](http://www.packt.com) 登录或注册。
2.  选择 **SUPPORT** 选项卡。
3.  点击 **Code Downloads & Errata**（代码下载与勘误）。
4.  在搜索框中输入书名，然后按照屏幕上的说明操作。

文件下载完成后，请务必使用以下最新版本解压或提取文件夹：

-   Windows 系统使用 `WinRAR`/`7-Zip`
-   Mac 系统使用 `Zipeg`/`iZip`/`UnRarX`
-   Linux 系统使用 `7-Zip`/`PeaZip`

本书的代码包也托管在 GitHub 上，地址为 [https://github.com/PacktPublishing/Hands-On-System-Programming-with-Go](https://github.com/PacktPublishing/Hands-On-System-Programming-with-Go)。如果代码有更新，将会在现有的 GitHub 仓库中更新。

我们丰富的图书和视频目录中的其他代码包也可在 **[https://github.com/PacktPublishing/](https://github.com/PacktPublishing/)** 获取。快去看看吧！

## 彩色图片

我们还提供了一个 PDF 文件，其中包含本书使用的屏幕截图/图表的彩色图片。您可以在此处下载：[https://static.packt-cdn.com/downloads/9781789804072_ColorImages.pdf](https://static.packt-cdn.com/downloads/9781789804072_ColorImages.pdf)。

## 代码实战

访问以下链接查看代码运行的视频：[http://bit.ly/2ZWgJb5](http://bit.ly/2ZWgJb5)。

## Playground 示例

在本书的学习过程中，您会发现许多代码片段后面跟着一个指向 [https://play.golang.org](https://play.golang.org) 的链接，这是一项允许您运行 Go 应用程序（有一些限制）的服务。您可以在 [https://blog.golang.org/playground](https://blog.golang.org/playground) 阅读更多关于它的信息。

为了查看此类示例的完整源代码，您需要访问 Playground 链接。进入网站后，您可以按下 **Run** 按钮来执行应用程序。页面底部将显示输出。以下是在 Go Playground 中运行代码的示例：

![](assets/ea782ebb-71ed-4d93-a9bb-3211d5d063b3.png)

如果您愿意，可以通过向示例中添加和编辑更多代码来进行实验，然后运行它们。

## 约定用法

本书全文使用了许多文本约定。

`CodeInText`：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。例如："This type of service includes `load`, which adds a program to memory and prepares for its execution before passing control to the program itself, or `execute`, which runs an executable file in the context of a pre-existing process."（此类服务包括 `load`，它将程序加载到内存中，并在将控制权传递给程序本身之前准备其执行，或 `execute`，它在预先存在的进程上下文中运行可执行文件。）

代码块设置如下：

```
<meta name="go-import" content="package-name vcs repository-url">
```

任何命令行输入或输出均按如下方式编写：

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

**粗体**：表示新术语、重要单词或您在屏幕上看到的单词。例如，菜单或对话框中的单词会在文本中这样显示。例如："In the meantime, systems started to get distributed, and applications started to get shipped in containers, orchestrated by other system software, such as **Kubernetes**."（与此同时，系统开始变得分布式，应用程序开始以容器形式交付，由其他系统软件编排，例如 **Kubernetes**。）

警告或重要提示显示如下。提示和技巧显示如下。

## 联系我们

我们始终欢迎读者的反馈。

**常规反馈**：如果您对本书的任何方面有疑问，请在邮件主题中注明书名，发送邮件至 `customercare@packtpub.com`。

**勘误**：虽然我们已尽一切努力确保内容的准确性，但错误难免发生。如果您在本书中发现错误，我们将不胜感激如果您能向我们报告。请访问 [www.packt.com/submit-errata](http://www.packt.com/submit-errata)，选择您的书籍，点击勘误提交表单链接，并填写详细信息。

**盗版**：如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激如果您能向我们提供该位置地址或网站名称。请通过 `copyright@packt.com` 联系我们，并附上相关材料的链接。

**如果您有兴趣成为作者**：如果您在某个主题上有专长，并有兴趣撰写或为书籍做贡献，请访问 [authors.packtpub.com](http://authors.packtpub.com/)。

## 评论

请留下评论。当您阅读并使用过本书后，为什么不在购买它的网站上留下评论呢？潜在读者可以看到并利用您的公正意见做出购买决定，我们 Packt 可以了解您对我们产品的看法，我们的作者也能看到您对他们书籍的反馈。谢谢！

有关 Packt 的更多信息，请访问 [packt.com](http://www.packt.com/)。

# 第 1 部分：系统编程与 Go 简介

本部分介绍 Unix 和系统编程。它将帮助您了解 Unix 操作系统提供的组件，从内核 API 到文件系统，您将熟悉系统编程的基本概念。

本部分包含以下章节：

- 第 1 章，*系统编程简介*
- 第 2 章，*Unix 操作系统组件*
- 第 3 章，*Go 概览*

## 第 1 章：系统编程简介

本章介绍系统编程，探讨从其最初定义到随系统演变而发生转变的一系列主题。本章提供了一些基本概念，以及 Unix 及其资源（包括内核和**应用程序编程接口**（**API**））的概览。许多概念在此定义，并在本书其余部分中使用。

本章将涵盖以下主题：

- 什么是系统编程？
- 应用程序编程接口
- 理解保护环的工作原理
- 系统调用概览
- POSIX 标准

### 技术要求

如果您使用 Linux，本章不要求您安装任何特殊软件。

如果您是 Windows 用户，可以安装 **Windows Subsystem for Linux**（**WSL**）。请按照以下步骤安装 WSL：

1.  以管理员身份打开 PowerShell 并运行以下命令：

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

2.  提示时重启计算机。
3.  从 Microsoft Store 安装您喜欢的 Linux 发行版。

### 系统编程入门

多年来，IT 格局发生了巨大变化。挑战冯·诺依曼架构的多核 CPU、互联网和分布式系统只是过去 30 年中发生的一些变化。那么，系统编程在这个格局中处于什么位置呢？

### 面向软件的软件

让我们从标准教科书定义开始。

系统编程是编程计算机系统软件的活动。与应用程序编程相比，系统编程的主要区别特征在于：应用程序编程旨在生产直接为用户提供服务的软件（例如文字处理器），而系统编程旨在生产为其他软件提供服务的软件和软件平台，并设计为在性能受限的环境中工作，例如操作系统、计算科学应用、游戏引擎和 3A 视频游戏、工业自动化以及软件即服务应用。

该定义强调了系统应用程序的两个主要概念，如下所示：

