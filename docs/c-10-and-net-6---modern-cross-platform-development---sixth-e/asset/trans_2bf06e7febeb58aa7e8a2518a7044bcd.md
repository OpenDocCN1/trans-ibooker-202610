
- Windows、Mac 或 Linux 的 Visual Studio Code
- GitHub Codespaces

第三方创建了自己的 C# 代码编辑器，例如 JetBrains Rider。

## 选择合适的工具和应用类型进行学习

学习 C# 和 .NET 最佳的工具和应用类型是什么？

学习时，最好的工具是能够帮助你编写代码和配置，但不会隐藏实际发生的事情的工具。IDE 提供了易于使用的图形用户界面，但在底层它们为你做了什么呢？在学习时，更基础的代码编辑器更接近实际操作，同时提供编写代码的帮助会更好。

尽管如此，你也可以认为最好的工具是你已经熟悉的，或者你或你的团队用作日常开发工具的工具。出于这个原因，我希望你能自由选择任何 C# 代码编辑器或 IDE 来完成本书中的编码任务，包括 Visual Studio Code、Windows 版 Visual Studio、Mac 版 Visual Studio，甚至 JetBrains Rider。

在本书第三版中，我为 Visual Studio for Windows 和 Visual Studio Code 的所有编码任务都提供了详细的逐步说明。不幸的是，这很快变得混乱且令人困惑。在本第六版中，我仅在第 1 章中提供了如何在 Windows 版 Visual Studio 2022 和 Visual Studio Code 中创建多个项目的详细逐步说明。此后，我提供适用于所有工具的项目名称和通用说明，以便你可以使用自己偏好的工具。

学习 C# 语言结构和许多 .NET 库的最佳应用类型是不会因不必要的应用程序代码而分散注意力的类型。例如，只为了学习如何编写 switch 语句，就没有必要创建一个完整的 Windows 桌面应用程序或网站。

因此，我认为学习第 1 章至第 12 章的 C# 和 .NET 主题的最佳方法是构建控制台应用程序。然后，从第 13 章到第 19 章及以后，你将构建网站、服务以及图形化桌面和移动应用。

## .NET Interactive Notebooks 扩展的优缺点

Visual Studio Code 的另一个好处是 .NET Interactive Notebooks 扩展。该扩展提供了一个简单且安全的场所来编写简单的代码片段。它使你能够创建一个单一的笔记本文件，该文件混合了 Markdown（富格式文本）和使用 C# 以及其他相关语言（如 PowerShell、F# 和 SQL（用于数据库））的代码“单元格”。

然而，.NET Interactive Notebooks 也有一些限制：

- 它们无法从用户读取输入，例如，你不能使用 ReadLine 或 ReadKey。
- 它们不能接收参数传递。
- 它们不允许你定义自己的命名空间。
- 它们没有任何调试工具（但这些功能未来会推出）。

## 使用 Visual Studio Code 进行跨平台开发

最现代且轻量的代码编辑器选择，并且是微软唯一跨平台的编辑器，是 Microsoft Visual Studio Code。它可以在所有常见操作系统上运行，包括 Windows、macOS 以及多种 Linux 发行版，包括 Red Hat Enterprise Linux（RHEL）和 Ubuntu。

Visual Studio Code 拥有大量且不断增长的扩展集来支持 C# 之外的许多语言，因此是现代跨平台开发的良好选择。

由于其跨平台且轻量，它可以安装在你的应用将被部署到的所有平台上，以便快速修复错误等。选择 Visual Studio Code 意味着开发人员可以使用跨平台代码编辑器来开发跨平台应用。

Visual Studio Code 对 Web 开发有强大的支持，尽管目前对移动和桌面开发的支持较弱。

Visual Studio Code 支持 ARM 处理器，因此你可以在 Apple Silicon 计算机和 Raspberry Pi 上进行开发。

Visual Studio Code 迄今为止是最受欢迎的集成开发环境，在 Stack Overflow 2021 调查中，超过 70% 的专业开发人员选择了它。

## 使用 GitHub Codespaces 进行云端开发

GitHub Codespaces 是一个基于 Visual Studio Code 的完全配置好的开发环境，可以在云托管的环境中启动并通过任何 Web 浏览器访问。它支持 Git 仓库、扩展和内置命令行界面，因此你可以从任何设备进行编辑、运行和测试。

## 使用 Visual Studio for Mac 进行通用开发

Microsoft Visual Studio 2022 for Mac 可以创建大多数类型的应用程序，包括控制台应用、网站、Web 服务、桌面和移动应用。

要编译面向 iOS 等 Apple 操作系统的应用以在 iPhone 和 iPad 等设备上运行，你必须拥有 Xcode，而 Xcode 仅在 macOS 上运行。

## 使用 Visual Studio for Windows 进行通用开发

Microsoft Visual Studio 2022 for Windows 可以创建大多数类型的应用程序，包括控制台应用、网站、Web 服务、桌面和移动应用。尽管你可以使用其 Xamarin 扩展通过 Visual Studio 2022 for Windows 编写跨平台移动应用，但你仍需要 macOS 和 Xcode 来编译它。

它仅在 Windows 版本 7 SP1 或更高版本上运行。你必须在 Windows 10 或 Windows 11 上运行它，才能创建从 Microsoft Store 安装并在沙箱中运行以保护计算机的通用 Windows 平台（UWP）应用。

## 我使用过的工具

为了编写和测试本书的代码，我使用了以下硬件：

- HP Spectre（Intel）笔记本电脑
- Apple Silicon Mac mini（M1）台式机
- Raspberry Pi 400（ARM v8）台式机

我使用了以下软件：

- Visual Studio Code 在：
  - Apple Silicon Mac mini（M1）台式机上的 macOS
  - HP Spectre（Intel）笔记本电脑上的 Windows 10
  - Raspberry Pi 400 上的 Ubuntu 64
- Windows 版 Visual Studio 2022 在：
  - HP Spectre（Intel）笔记本电脑上的 Windows 10
- Mac 版 Visual Studio 2022 在：
  - Apple Silicon Mac mini（M1）台式机上的 macOS

我希望你也能使用多种硬件和软件，因为看到平台之间的差异会加深你对开发挑战的理解，尽管上述任何一种组合都足以学习 C# 和 .NET 的基础知识以及如何构建实用的应用和网站。

![](img/c63b00dca7192cc58cfb9e1f1e64f483_35_0.png)

> **更多信息：** 你可以通过阅读我在以下链接撰写的额外文章来学习如何使用配备 Ubuntu Desktop 64 位的 Raspberry Pi 400 用 C# 和 .NET 编写代码：https://github.com/markjprice/cs9dotnet5-extras/blob/main/raspberry-pi-ubuntu64/README.md。

## 跨平台部署

你选择的代码编辑器和开发操作系统不会限制你的代码部署位置。

.NET 6 支持以下部署平台：

- **Windows**：Windows 7 SP1 或更高版本。Windows 10 版本 1607 或更高版本，包括 Windows 11。Windows Server 2012 R2 SP1 或更高版本。Nano Server 版本 1809 或更高版本。
- **Mac**：macOS Mojave（版本 10.14）或更高版本。
- **Linux**：Alpine Linux 3.13 或更高版本。CentOS 7 或更高版本。Debian 10 或更高版本。Fedora 32 或更高版本。openSUSE 15 或更高版本。Red Hat Enterprise Linux（RHEL）7 或更高版本。SUSE Enterprise Linux 12 SP2 或更高版本。Ubuntu 16.04、18.04、20.04 或更高版本。
- **Android**：API 21 或更高版本。
- **iOS**：10 或更高版本。

.NET 5 及更高版本中的 Windows ARM64 支持意味着你可以在 Windows ARM 设备（如 Microsoft Surface Pro X）上进行开发和部署。但据说在 Apple M1 Mac 上使用 Parallels 和 Windows 10 ARM 虚拟机进行开发速度大约是两倍快！

## 下载和安装 Windows 版 Visual Studio 2022

许多专业 Microsoft 开发人员在日常开发工作中使用 Windows 版 Visual Studio 2022。即使你选择使用 Visual Studio Code 来完成本书中的编码任务，你也可能想熟悉一下 Windows 版 Visual Studio 2022。

如果你没有 Windows 电脑，那么可以跳过本节，继续下一节，在那里你将下载并在 macOS 或 Linux 上安装 Visual Studio Code。

自 2014 年 10 月以来，Microsoft 已向学生、开源贡献者和个人免费提供专业质量的 Windows 版 Visual Studio 版本。它被称为 Community Edition。本书的任何版本都适用。如果你尚未安装它，现在让我们安装：

1. 从以下链接下载适用于 Windows 的 Microsoft Visual Studio 2022 版本 17.0 或更高版本：https://visualstudio.microsoft.com/downloads/。
2. 启动安装程序。
3. 在 **工作负载** 选项卡上，选择以下内容：
   - **ASP.NET 和 Web 开发**
   - **Azure 开发**
   - **.NET 桌面开发**
   - **使用 C++ 的桌面开发**
