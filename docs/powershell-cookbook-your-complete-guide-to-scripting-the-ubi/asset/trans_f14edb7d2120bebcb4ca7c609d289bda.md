

# 前言

我认为，对于初学者而言，建立在列夫·维果茨基活动理论基础上的方法会更好。维果茨基区分了*胜任力*与*表现*。他指出，当我们在*专家*的指导下时，我们的*表现*可以超越我们的*胜任力*。设想这样一种情况：我想查找某个日期之前修改的所有文件，却不懂任何 PowerShell——我（目前）不具备执行此任务的*胜任力*。但因为我有远见买了这本书，我所要做的就是翻开[食谱 20.3](#ch20.xhtml#find_all_files_modified_before_a_certain)。在那里我看到了解决方案。我输入命令，并*执行*了一项非常复杂的任务。李（专家）帮我解决了一个复杂的问题（表现），尽管我不懂 PowerShell（胜任力）。

尽管这很棒，但神奇的时刻发生在下一步。既然我现在能够成功执行一项复杂任务，我就可以开始实验，而在实验中，我建立并提升了自己的胜任力。

我运行这些命令。然后我故意犯个错，看看错误消息是什么。下次再见到该错误消息，我就知道自己犯了什么错。从这里，我使用 `Get-Help` 查找命令，探索还能使用哪些其他参数。李的“解决方案”提供了一个成功的滩头阵地。然后我们利用好奇心去探索——这又如何？我想知道……为什么不？也许……？这就是正道。而且简直太有趣了。

我们的好奇心驱动着我们的学习、理解和胜任力。在各个阶段，我们的好奇心都会遇到需要深入探索的问题。此时，我们可以回到 [PowerShell 指南之旅](#preface02.xhtml#a-guided-tour-of-windows-powershell) 和 [基础知识](#part02.xhtml#part2) 章节。因真实问题而阅读这些章节，会让它们更加相关、难忘且极具影响力。

Jeffrey Snover  
Lee Holmes 的同事，  
及 Microsoft 技术研究员

# 序言

2002 年底，Slashdot 发布了一则关于微软正在开发“下一代 Shell”的传闻故事。作为 Shell 及其脚本语言所释放威力的长期爱好者，这则帖子立刻引起了我的兴趣。这个 Shell 能否提供我长久以来在 Unix 系统上喜爱的命令行威力和生产力？

既然我六个月前刚加入微软，我抓住机会终于弄清了一个源自 Slashdot 的“微软谜团”。帖子提到与 .NET Framework 的强集成，所以我向一个内部 C# 邮件列表发布了询问。我得到回复，该项目代号为“Monad”，我随即用它找到了一个内部原型构建版。

“原型”算是个客气的说法。在早期阶段，该构建版主要是一个概念验证。想清屏？没问题！只要靠着 Enter 键不放，直到之前的命令和输出滚出视野即可！但即使在这些早期阶段，也立刻清楚地表明 Monad 标志着命令行 Shell 的一场革命。与许多这种规模的事物一样，它的美妙不言自明。Monad 在其命令之间传递全保真 .NET 对象。对于甚至最复杂的命令，Monad 废除了（直到那时为止的标准）对脆弱的基于文本解析的需求。简单而强大的数据操作工具支持这一新模型，创造了一个既强大又易用的 Shell。

不久后，我加入了 Monad 开发团队，尽自己的一份力将这项技术杰作带给世界其他地方。从那时起，Monad 发展成为一个真实、有形的产品——现在称为 PowerShell。

那么为什么要写一本关于它的书？为什么要写*这本*书？

许多用户学习 PowerShell 是为了学习 PowerShell 本身。任何切实的好处都是副作用。但其他人可能更倾向于在新技术解决其需求时，机会主义地学习它。如何使用 PowerShell 导航文件系统？如何管理文件和文件夹？如何检索网页？

这本书专注于通过针对您最迫切问题的基于任务的解决方案，帮助您学习 PowerShell。阅读一个食谱、一章或整本书——无论如何，您都能学到东西。

## 适读人群

这本书帮助您使用 PowerShell *完成工作*。它包含数百个针对特定真实问题的解决方案。对于系统管理，您将找到大量示例，展示如何管理文件系统、Windows 注册表、事件日志、进程等。对于企业管理，您将找到两章完整内容，专门介绍 Windows Management Instrumentation (WMI)、Active Directory 和其他面向企业的任务。

在此过程中，您还将学到大量关于 PowerShell 的知识：其特性、命令及其脚本语言——但最重要的是，您将解决问题。

## 本书结构

本书由五个主要部分组成：PowerShell 指南之旅、PowerShell 基础知识、常见任务、管理员任务和详细参考。

### 第 I 部分

[PowerShell 指南之旅](#preface02.xhtml#a-guided-tour-of-windows-powershell) 从高层快速浏览 PowerShell。它介绍 PowerShell 的核心特性：

* 交互式 Shell
* 新的命令模型
* 基于对象的管道
* 专注于管理员的敏锐焦点
* 统一的学习与发现模型
* 无处不在的脚本编写
* 与关键管理技术的集成
* 统一的数据存储交互模型

该指南帮助您整体熟悉 PowerShell。这种熟悉感将为您理解本书其余部分的解决方案创建一个心智框架。

### 第 II 部分

第 [1](#ch01.xhtml#the_windows_powershell_interactive_shell) 章至第 [8](#ch08.xhtml#utility_scripting) 章涵盖支撑本书解决方案的基础知识。本部分向您介绍 PowerShell 交互式 Shell、基本管道和对象概念，以及 PowerShell 脚本语言的许多特性。

### 第 III 部分

第 [9](#ch09.xhtml#simple_files) 章至第 [19](#ch19.xhtml#visual_studio_code) 章涵盖您在开始用 PowerShell 解决更复杂问题时最常遇到的任务。这包括处理简单和结构化文件、联网脚本、代码重用、用户交互等。

### 第 IV 部分

第 [20](#ch20.xhtml#files_and_directories) 章至第 [31](#ch31.xhtml#event_handling) 章专注于系统和企业管理中最常见的任务。第 [20](#ch20.xhtml#files_and_directories) 章至第 [25](#ch25.xhtml#system_services) 章专注于单个系统：文件系统、注册表、事件日志、进程、服务等。第 [26](#ch26.xhtml#active_directory) 章和第 [27](#ch27.xhtml#enterprise_computer_management) 章专注于 Active Directory，以及管理联网或加入域的系统中最典型的任务。第 [28](#ch28.xhtml#windows_management_instrumentation) 章和第 [29](#ch29.xhtml#chapter_remoting) 章专注于稳健多机管理的两个关键方面：WMI 和 PowerShell 远程处理。

### 第 V 部分

许多书籍在附录中堆砌无用信息仅为增加页数。然而在本书中，详细参考支撑起了一个学习和使用 PowerShell 不可或缺的重要资源。附录涵盖：

* PowerShell 语言和环境
* 正则表达式语法和 PowerShell 侧重示例
* XPath 快速参考
* .NET `string` 格式化语法和 PowerShell 侧重示例
* .NET `DateTime` 格式化语法和 PowerShell 侧重示例
* 管理员友好的 .NET 类及其用法
* 管理员友好的 WMI 类及其用法
* 管理员友好的 COM 对象及其用法
* 精选事件及其用法
* PowerShell 标准动词

## 使用本书所需条件

