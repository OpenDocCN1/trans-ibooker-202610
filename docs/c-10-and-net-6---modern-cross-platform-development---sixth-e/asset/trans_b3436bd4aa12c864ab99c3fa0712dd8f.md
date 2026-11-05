
## 使用 Google 搜索答案

你可以使用高级搜索选项搜索 Google，以提高找到所需内容的机会：

- 1. 导航到 Google。
- 2. 使用简单的 Google 查询搜索有关垃圾回收的信息，并注意你很可能会在看到计算机科学中垃圾回收的维基百科定义之前，先看到你所在地区垃圾回收服务的许多广告。
- 3. 通过将其限制在有用的站点（如 Stack Overflow），并删除我们可能不关心的语言，如 C++、Rust 和 Python，或显式添加 C# 和 .NET 来改进搜索，如以下搜索查询所示：

```
garbage collection site:stackoverflow.com +C# -Java
```

## 订阅官方 .NET 博客

为了及时了解 .NET，一个值得订阅的优秀博客是官方 .NET 博客，由 .NET 工程团队撰写，你可以在以下链接找到它：https://devblogs.microsoft.com/dotnet/。

## 观看 Scott Hanselman 的视频

来自 Microsoft 的 Scott Hanselman 有一个关于他们没教你计算机知识的优秀 YouTube 频道：http://computerstufftheydidntteachyou.com/。

我推荐给所有从事计算机工作的人。

## 练习和探索

让我们现在通过尝试回答一些问题、进行一些实践操作，并更详细地深入本章涵盖的主题来测试你的知识和理解。

## 练习 1.1 – 测试你的知识

请尝试回答以下问题，记住虽然大多数答案可以在本章中找到，但你应该进行一些在线研究或编写代码来回答其他问题：

- 1. Visual Studio 2022 比 Visual Studio Code 更好吗？
- 2. .NET 6 比 .NET Framework 更好吗？
- 3. 什么是 .NET Standard，为什么它仍然重要？
- 4. 为什么程序员可以使用不同的语言，例如 C# 和 F#，来编写在 .NET 上运行的应用程序？
- 5. .NET 控制台应用程序的入口点方法名称是什么，应该如何声明？
- 6. 什么是顶层程序，如何访问任何命令行参数？
- 7. 在提示符下输入什么来构建和执行 C# 源代码？
- 8. 使用 .NET Interactive Notebooks 编写 C# 代码有哪些好处？
- 9. 你会在哪里查找 C# 关键字的帮助？
- 10. 你会在哪里查找常见编程问题的解决方案？

附录《测试你的知识问题的答案》可从 GitHub 仓库 README 中的链接下载：https://github.com/markjprice/cs10dotnet6。

## 练习 1.2 – 在任何地方练习 C#

你不需要 Visual Studio Code，甚至也不需要 Windows 或 Mac 上的 Visual Studio 2022 就可以编写 C#。你可以前往 .NET Fiddle – https://dotnetfiddle.net/ – 在线开始编码。

## 练习 1.3 – 探索主题

一本书是一种精心策划的体验。我尝试在印刷书籍中包含主题的正确平衡。其他我撰写的内容可以在本书的 GitHub 仓库中找到。

我相信这本书涵盖了 C# 和 .NET 开发人员应该具备或了解的所有基础知识和技能。一些较长的示例最好作为指向 Microsoft 文档或第三方文章作者的链接包含在内。

使用以下页面上的链接了解本章涵盖主题的更多详细信息：

https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-1--hello-c-welcome-net

## 摘要

在本章中，我们：

- 设置你的开发环境。
- 讨论了现代 .NET、.NET Core、.NET Framework、Xamarin 和 .NET Standard 之间的相似性和差异。
- 使用 .NET SDK 的 Visual Studio Code 和 Windows 版 Visual Studio 2022 创建了一些简单的控制台应用程序。
- 使用 .NET Interactive Notebooks 执行代码片段以进行学习。
- 学会了如何从 GitHub 仓库下载本书的解决方案代码。
- 最重要的是，学会了如何查找帮助。

在下一章，你将学习如何“说”C#。

## 02

## 说 C#

本章全部关于 C# 编程语言的基础知识。在本章中，你将学习如何使用 C# 的语法编写语句，以及介绍你每天会使用的一些常用词汇。此外，到本章结束时，你将有信心知道如何临时存储和处理计算机内存中的信息。

本章涵盖以下主题：

- 介绍 C# 语言
- 理解 C# 语法和词汇
- 使用变量
- 进一步探索控制台应用程序

## 介绍 C# 语言

本书的这一部分是关于 C# 语言——你每天用来编写应用程序源代码的语法和词汇。

编程语言与人类语言有很多相似之处，只是编程语言中你可以自己创造单词，就像 Dr. Seuss 一样！

在 Dr. Seuss 于 1950 年撰写的《If I Ran the Zoo》一书中，他写道：

> "And then, just to show them, I'll sail to Ka-Troo And Bring Back an It-Kutch, a Preep, and a Proo, A Nerkle, a Nerd, and a Seersucker, too!"

## 理解语言版本和功能

本书的这一部分涵盖 C# 编程语言，主要面向初学者，因此涵盖了所有开发人员需要了解的基础主题，从声明变量到存储数据再到如何定义自己的自定义数据类型。

本书涵盖了 C# 语言从 1.0 版本到最新 10.0 版本的功能。

如果你已经熟悉 C# 的旧版本，并且渴望了解最新版本 C# 中的新功能，我通过在下面列出语言版本及其重要的新功能，以及可以了解它们的章节编号和主题标题，使你更容易跳转。

## C# 1.0

C# 1.0 于 2002 年发布，包含了静态类型面向对象现代语言的所有重要功能，如你在 *第 2 章到第 6 章* 中所见。

## C# 2.0

C# 2.0 于 2005 年发布，重点是通过使用泛型实现强数据类型，以提高代码性能并减少类型错误，包括以下表中列出的主题：

| Feature | Chapter | Topic |
|---|---|---|
| Nullable value types | 6 | Making a value type nullable |
| Generics | 6 | Making types more reusable with generics |

## C# 3.0

C# 3.0 于 2007 年发布，重点是通过 **语言集成查询 (LINQ)** 以及匿名类型和 lambda 表达式等相关功能实现声明式编码，包括以下表中列出的主题：

| Feature | Chapter | Topic |
|---|---|---|
| Implicitly typed local variables | 2 | Inferring the type of a local variable |
| LINQ | 11 | All topics in Chapter 11, Querying and Manipulating Data Using LINQ |

## C# 4.0

C# 4.0 于 2010 年发布，重点是提高与动态语言（如 F# 和 Python）的互操作性，包括以下表中列出的主题：

| Feature | Chapter | Topic |
|---|---|---|
| Dynamic types | 2 | Storing dynamic types |
| Named/optional arguments | 5 | Optional parameters and named arguments |

## C# 5.0

C# 5.0 于 2012 年发布，重点是通过在编写看起来像同步语句的同时自动实现复杂的有限状态机来简化异步操作支持，包括以下表中列出的主题：

| Feature | Chapter | Topic |
|---|---|---|
| Simplified asynchronous tasks | 12 | Understanding async and await |

## C# 6.0

C# 6.0 于 2015 年发布，重点是对语言进行小幅改进，包括以下表中列出的主题：

| Feature | Chapter | Topic |
|---|---|---|
| static imports | 2 | Simplifying the usage of the console |
| Interpolated strings | 2 | Displaying output to the user |
| Expression bodied members | 5 | Defining read-only properties |

## C# 7.0

C# 7.0 于 2017 年 3 月发布，重点是添加诸如元组和模式匹配等函数式语言功能，以及对语言的小幅改进，包括以下表中列出的主题：

| Feature | Chapter | Topic |
|---|---|---|
| Binary literals and digit separators | 2 | Storing whole numbers |
| Pattern matching | 3 | Pattern matching with the if statement |
| out variables | 5 | Controlling how parameters are passed |
| Tuples | 5 | Combining multiple values with tuples |
| Local functions | 6 | Defining local functions |

## C# 7.1

C# 7.1 于 2017 年 8 月发布，重点是对语言进行小幅改进，包括以下表中列出的主题：

| Feature | Chapter | Topic |
|---|---|---|
| Default literal expressions | 5 | Setting fields with default literal |
| Inferred tuple element names | 5 | Inferring tuple names |
| async Main | 12 | Improving responsiveness for console apps |
