

## Visual Studio

对于 C# 来说，排名第一的开发环境是 Visual Studio。截至撰写本文时，最新正式版为 2019 版，并且提供一个名为 Community 的免费版 [见图 1-1]。一分钟后，你将学习如何安装它。在整本书中，我将使用 2021 年 1 月可用的 Visual Studio 版本，其中包含计划在下一个正式版中推出的功能预览、.NET 5 平台和 C# 9.0 语言等。

![](img/32b055c352c70d22a656320349173d95_31_0.png)

**Figure 1-1.** *Visual Studio Community*

### Windows 版本

Visual Studio 需要 Windows 操作系统。如果你对 *Visual Studio system requirements* 进行网络搜索，你会找到受支持的 Windows 版本 [见图 1-2]。

### Visual Studio 2019 系统要求

以下产品支持以下最低系统要求：

| Supported Operating Systems | Visual Studio 2019 will install and run on the following operating systems (64 bit recommended; ARM is not supported): |
|---|---|
|  | • Windows 10 version 1703 or higher: Home, Professional, Education, and Enterprise (LTSC and S are not supported)<br>• Windows Server 2019: Standard and Datacenter<br>• Windows Server 2016: Standard and Datacenter<br>• Windows 8.1 (with Update 2919355): Core, Professional, and Enterprise<br>• Windows Server 2012 R2 (with Update 2919355): Essentials, Standard, Datacenter<br>• Windows 7 SP1 (with latest Windows Updates): Home Premium, Professional, Enterprise, Ultimate |

**Figure 1-2.** *Windows versions supported*

正如你所见，你不需要拥有最新最好的 Windows 版本。截至 2021 年 1 月，你甚至可以安装 Windows 7 Service Pack 1。此外，为了配合本书学习，你甚至不需要最新的 Visual Studio 版本。书中几乎所有的示例在旧版本上也能完美运行。

### 非 Windows 操作系统

如果你电脑上没有 Windows 操作系统，你会很高兴听到 Visual Studio Code 开发环境的消息。这是一款免费的多平台 IDE，也可在 Linux 或 Mac 上运行，让你可以在这些系统上用 C# 编程。

在本书记载的示例中，我将使用安装在 Windows 上的 Visual Studio Community。我建议你也这样做。如果你做不到，请使用 Visual Studio Code，并注意有些地方可能与书中看到的有一些不同。

![](img/32b055c352c70d22a656320349173d95_34_0.png)

**Figure 1-4.** *Registering your copy of Visual Studio*

如果你在 Visual Studio 首次启动时跳过此步骤，你可以稍后通过选择 Help ➤ Register Product in Visual Studio 来注册。

### 更新和反馈

信息技术变化迅速。你阅读本书时某些内容可能有所不同。如果出现重要的 Visual Studio 变更，你可以查看我的网站 [http://moderniProgramovani.cz/en/](http://moderniProgramovani.cz/en/) 获取更新信息。

此外，我欢迎任何关于本书的反馈——你对改进的建议、学习时的感受、使用本书教学的经验等。我的电子邮件地址是 vystavel@moderniProgramovani.cz。谢谢！

### 总结

在本书记载中，你将学习编程，特别是 C# 编程语言。你将通过许多实践练习学习 C# 编程，这些练习将引导你进入越来越复杂的主题。为了能够跟随练习，你需要按以下方式准备电脑：

- 你需要一台装有 Windows 操作系统的电脑 [至少 Windows 7 Service Pack 1]。
- 你需要安装合适的开发环境。在本书中，我将使用免费的 Visual Studio Community。

## PART I: Data

## CHAPTER 2: Your First Program

现在你的电脑已经准备就绪，让我们开始编程吧！在本章中，你将在 C# 语言中创建你的第一个程序，并学习为此需要执行的所有步骤。

### Seeing It in Action

在本章中，你将创建一个向用户显示消息 “I am starting to program in C#.” 的程序 [见图 2-1]。

![](img/32b055c352c70d22a656320349173d95_37_0.png)

*Figure 2-1.* Your first program

### Creating the Project

你通过创建新项目来开始每个新程序，现在就让我们这样做。

### Launching Visual Studio

启动 Visual Studio。应出现类似图 2-2 的启动屏幕。

![](img/32b055c352c70d22a656320349173d95_38_0.png)

*Figure 2-2.* Visual Studio Start screen

向下拖动滚动条并选择 “Create a new project” 操作 [见图 2-3]。

![](img/32b055c352c70d22a656320349173d95_39_0.png)

*Figure 2-3.* Choosing for a new project

![](img/32b055c352c70d22a656320349173d95_40_0.png)

*Figure 2-4.* Selecting a project template

### Creating New Project

出现的对话框 [见图 2-4] 要求你选择新的项目模板。选择 Console App C# 模板并按下 Next 按钮。

在下一个对话框 [见图 2-5] 中，输入 “My first program” 作为新项目名称并按下 Create 按钮。

![](img/32b055c352c70d22a656320349173d95_41_0.png)

Figure 2-5. Entering a name for your new project

### Writing the Program Code

最重要的一步是编写程序代码，继续阅读。

### The Look of the Development Environment

创建项目后，Visual Studio 的外观如图所示 2-6。

![](img/32b055c352c70d22a656320349173d95_42_0.png)

**Figure 2-6.** *The look of Visual Studio*

开发环境窗口的主要部分被 *源代码编辑器* 占据。在其中，Program.cs 文件已打开，正如标签标题所建议的那样。Program.cs 是你的新控制台项目的主要文件。如你所见，它已经包含了一些源代码。

你可能会想知道这段代码从何而来。你还没有写过一行代码！答案是，当你选择 Console App 模板时，Visual Studio 生成了代码。正如你在创建项目时所看到的，Visual Studio 包含许多不同的模板；这些模板是不同类型程序的现成项目骨架。

你可以看到代码中包含了一些奇怪的词，如 `using`、`namespace`、`class` 等等。我现在不会解释这些，因为你目前不需要详细理解它们。但 Visual Studio 需要这些行，所以就让它们保持原样。你需要知道的是在哪里编写自己的语句，这就是我接下来要解释的。

```csharp
// Output of text to the user
Console.WriteLine("I am starting to program in C#.");

// Waiting for Enter
Console.ReadLine();
```

Visual Studio 现在看起来像图 2-8。

![](img/32b055c352c70d22a656320349173d95_44_0.png)

**Figure 2-8.** *Entering your first code statements*

请仔细检查你是否在与我相同的位置输入了这些语句。再次强调，它们必须在大括号之间。并且要小心大括号。不要意外删除任何一个。

Program.cs 的源代码现在看起来像这样：

```
using System;

namespace My_first_program
{
    class Program
    {
        static void Main(string[] args)
        {
            // Output of text to the user
            Console.WriteLine("I am starting to program in C#.");

            // Waiting for Enter
            Console.ReadLine();
        }
    }
}
```

### Understanding Your First Statements

这些语句的作用是什么？

- Console.WriteLine 输出 [写入] 一行内容给用户。
- Console.ReadLine 通常读取用户通过键盘输入的一行文本。在本例中，其目的是让你的程序在完成后等待用户按下 Enter。换句话说，你不希望程序窗口立即消失。
- 从两个斜杠 [//] 开始直到对应行末的所有内容都会被忽略。此文本包含你的备注/注释。Visual Studio 会将其显示为绿色。

