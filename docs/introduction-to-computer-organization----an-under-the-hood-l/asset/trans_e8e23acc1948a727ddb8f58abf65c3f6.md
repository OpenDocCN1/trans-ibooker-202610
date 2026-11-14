

您可能刚开始接触 Linux 命令行。在讲解程序的过程中，我会展示我用来创建它们的命令，但这只能为您提供基础知识。如果您花时间熟悉命令行的使用，工作效率会高得多。我发现 William Shotts 编著的《*The Linux Command Line*》（第二版，No Starch Press，2019 年出版）是一份极佳的资源。

您还应熟悉 Linux 中为我们将要使用的编程工具提供的文档。最简单的是内置在大多数程序中的帮助系统。您可以通过输入程序名称并仅加上 `--help` 选项来访问帮助。例如，`gcc --help` 会显示可用于 `gcc` 的命令行选项列表，并简要说明每个选项的作用。

大多数 Linux 程序都包含一份手册，通常称为 *man page*，它提供的文档比帮助工具更完整。可以使用 `man` 命令后跟程序名称来阅读。例如，`man man` 会显示 `man` 程序的手册页。

GNU 程序附带更完整的文档，可以使用 `info` 命令后跟程序名称来阅读。例如，`info info` 会显示 `info` 的使用手册，如下所示：

```
Next: Stand-alone Info,  Up: (dir)

Stand-alone GNU Info
********************

This documentation describes the stand-alone Info reader which you can
use to read Info documentation.

   If you are new to the Info reader, then you can get started by typing
'H' for a list of basic key bindings.  You can read through the rest of
this manual by typing <SPC> and <DEL> (or <Space> and <Backspace>) to
move forwards and backwards in it.

* Menu:

* Stand-alone Info::            What is Info?
* Invoking Info::               Options you can pass on the command line.
* Cursor Commands::             Commands which move the cursor within a node.
* Scrolling Commands::          Commands for reading the text within a node.
* Node Commands::               Commands for selecting a new node.
* Searching Commands::          Commands for searching an Info file.
* Index Commands::              Commands for looking up in indices.
* Xref Commands::               Commands for selecting cross-references.
* Window Commands::             Commands which manipulate multiple windows.
* Printing Nodes::              How to print out the contents of a node.
* Miscellaneous Commands::      A few commands that defy categorization.
* Variables::                   How to change the default behavior of Info.
* Colors and Styles::           Customize the colors used by Info.
* Custom Key Bindings::         How to define your own key-to-command bindings.
* Index::                       Global index.

-----Info: (info-stnd)Top, 31 lines --All-------------------------------------------
Welcome to Info version 6.7.  Type H for help, h for tutorial.
```

以 `*` 开头并以 `::` 结尾的项目是指向手册中其他页面的超链接。使用键盘上的方向键将光标移至此类项目内的任意位置，然后按回车键即可调出该页面。

为了获取 `info` 文档，我必须安装以下 Ubuntu 软件包：

1. `binutils-doc` 为 GNU 汇编器 `as`（有时称为 `gas`）添加有用的文档
2. `gcc-doc` 为 GNU `gcc` 编译器添加有用的文档

您需要安装的软件包可能因您使用的 Linux 发行版而异。多年来，我甚至不得不针对不同版本的 Ubuntu 更改此列表。

在大多数情况下，我使用无优化选项（`-O0` 选项）编译程序，因为目标是研究概念，而不是创建最高效的代码。只要安装了 `gcc`、`g++` 和 `as`，示例应该可以在任何 x86-64 GNU 开发环境中运行。然而，编译器生成的机器码可能因其特定配置和版本而异。您将在本书约一半处开始看到编译器生成的汇编语言。随着您继续阅读本书的其余部分，任何差异都应保持一致。

您还将使用文本编辑器进行所有编程。**请勿使用文字处理器。** 文字处理器会添加大量隐藏控制字符来格式化文本。这些隐藏字符会干扰编译器和汇编器，导致它们无法工作。

Linux 下有几款优秀的文本编辑器，每款都有其独特的风格。我最喜欢的编辑器时常会变。我建议您尝试几款可用的编辑器，然后决定您更喜欢哪一款。

以下是我用过的文本编辑器：

1. `nano` 一款简单的文本编辑器，包含在大多数 Linux 安装中。它使用命令行用户界面。文本可直接插入。Ctrl 键和“Meta”键用于指定操作文本的键盘序列。
2. `vi` 所谓的安装在所有 Linux（和 Unix）系统上。它提供面向模式的命令行用户界面。通过键盘命令操作文本。有几个命令可将 `vi` 置于文本插入模式。Esc 键用于返回命令模式。大多数 Linux 安装包含 `vim`（Vi IMproved），它具有编辑程序源代码时有用的额外功能。
3. `emacs` 使用命令行用户界面。文本可直接插入。Ctrl 键和 Meta 键用于指定操作文本的键盘序列。
4. `gedit` 如果您使用 GNOME 桌面，可能已安装。它使用图形用户界面，如果您习惯使用文字处理器，这可能会让您感到熟悉。
5. `kate` 如果您使用 KDE 桌面，可能已安装。它使用图形用户界面，如果您习惯使用文字处理器，这可能会让您感到熟悉。
6. Visual Studio Code 微软提供的一款免费编辑器，可在 Windows 7/8/10、Linux 和 macOS 上运行（[https://code.visualstudio.com/](https://code.visualstudio.com/)）。它使用图形用户界面，可用于编辑远程服务器和 Windows Subsystem for Linux 安装上的文本文件。它还允许您打开一个终端窗格以输入命令。

`vi` 和 `emacs` 也都有图形用户界面可用。

这些编辑器中的任何一个，以及许多其他编辑器，都是本书所涵盖编程工作的绝佳选择。不要花太多时间试图挑选“最好”的那一个。

## 您已学到的内容

1. **中央处理器 (CPU)** 控制计算机大部分活动的子系统。它还包含少量速度极快的内存。
2. **内存** 为程序和数据提供存储的子系统。
3. **输入/输出 (I/O)** 提供与外部世界及大容量存储设备通信手段的子系统。
4. **总线** CPU、内存和 I/O 之间的通信路径。
5. **程序执行** 程序运行时，三个子系统和总线如何被使用的概述。
6. **编程环境** 如何设置本书编程所需工具的示例。

在下一章中，您将开始学习数据如何在计算机中存储，初步接触 C 语言编程，并开始学习如何将调试器作为学习工具使用。

# 第 2 章：数据存储格式

![](image_fi/book_art/chapterart.png)

在本书中，我们将从一个不同的角度来审视计算机：我们不再把计算机看作程序、文件和图形的集合，而是将其视为数十亿个双态开关以及一个或多个*控制单元*——能够检测和改变开关状态的设备。在第 1 章中，我们讨论了通过输入和输出与计算机外部世界进行通信。在本章中，我们将开始探索计算机如何对数据进行编码以存储在内存中；然后我们将编写一些 C 语言程序来探究这些概念。

## 描述开关和开关组

