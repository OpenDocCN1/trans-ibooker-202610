
进程彼此保持独立，即使运行相同的程序。例如，如果你同时运行多个 Python 程序的实例，每个进程可能具有各自不同的变量值。每个进程，即使是运行相同程序的进程，都有自己的 cwd 和环境变量设置。一般来说，命令行一次只运行一个进程（尽管你可以同时打开多个命令行）。

每个操作系统都有查看正在运行进程列表的方法。在 Windows 上，你可以按 CTRL-SHIFT-ESC 调出任务管理器应用程序。在 macOS 上，你可以运行 应用程序 ▸ 实用工具 ▸ 活动监视器。在 Ubuntu Linux 上，你可以按 CTRL-ALT-DEL 打开一个也称为任务管理器的应用程序。这些任务管理器可以在进程无响应时强制终止正在运行的进程。

## 命令行

命令行是一个基于文本的程序，让你输入命令来与操作系统交互并运行程序。你也可能听到它被称为命令行界面（CLI，读音与“fly”押韵）、命令提示符、终端、shell 或控制台。它提供了图形用户界面（GUI，读作“gooey”）的替代方案，后者允许用户通过不仅仅是基于文本的界面与计算机交互。GUI 通过视觉信息向用户展示，使其比命令行更容易地完成任务。大多数计算机用户将命令行视为高级功能，从不使用它。令人望而生畏的部分原因在于完全缺乏使用提示；尽管 GUI 可能会显示一个按钮告诉你在哪里点击，但空白的终端窗口并不会提醒你输入什么。

但熟练使用命令行是有充分理由的。其一，设置环境通常需要使用命令行而不是图形窗口。其二，输入命令比用鼠标点击图形窗口要快得多。基于文本的命令也比拖动一个图标到另一个图标更不含糊。这使它们更适合自动化，因为你可以将多个具体命令组合成脚本来执行复杂的操作。

命令行程序存在于你计算机上的可执行文件中。在此上下文中，我们通常称其为 shell 或 shell 程序。运行 shell 程序会使终端窗口出现：

- 在 Windows 上，shell 程序位于 C:\Windows\System32\cmd.exe。
- 在 macOS 上，shell 程序位于 /bin/bash。
- 在 Ubuntu Linux 上，shell 程序位于 /bin/bash。

多年来，程序员为 Unix 操作系统创建了许多 shell 程序，例如 Bourne Shell（在名为 sh 的可执行文件中）和后来的 Bourne-Again Shell（在名为 Bash 的可执行文件中）。Linux 默认使用 Bash，而 macOS 在 Catalina 及更高版本中使用类似的 Zsh 或 Z shell。由于其不同的发展历史，Windows 使用名为命令提示符的 shell。所有这些程序做的事情都一样：它们呈现一个终端窗口，其中包含基于文本的 CLI，用户可以在其中输入命令并运行程序。

在本节中，你将学习命令行的一些通用概念和常用命令。你可以掌握大量晦涩的命令并成为真正的巫师，但只需知道十几个左右就能解决大多数问题。确切的命令名称在不同操作系统上可能略有不同，但底层概念是相同的。

## 打开终端窗口

要打开终端窗口，请执行以下操作：

- 在 Windows 上，单击开始按钮，键入命令提示符，然后按 ENTER。
- 在 macOS 上，单击右上角的 Spotlight 图标，键入终端，然后按 ENTER。
- 在 Ubuntu Linux 上，按 WIN 键调出 Dash，键入终端，然后按 ENTER。或者使用键盘快捷键 CTRL-ALT-T。

像显示 >>> 提示的交互式 shell 一样，终端显示一个 shell 提示符，你可以在其中输入命令。在 Windows 上，提示符将是你当前所在文件夹的完整路径：

```
C:\Users\Al>your commands go here
```

在 macOS 上，提示符显示你的计算机名称、冒号以及 cwd，其中主文件夹表示为波浪号（~）。之后是你的用户名后跟美元符号（$）：

```
Al’s-MacBook-Pro:~ al$ your commands go here
```

在 Ubuntu Linux 上，提示符与 macOS 提示符类似，只是以用户名和 at（@）符号开头：

```
al@al-VirtualBox:~$ your commands go here
```

许多书籍和教程将命令行提示符表示为简单的 $ 以简化示例。可以自定义这些提示符，但这超出了本书的范围。

## 从命令行运行程序

要运行程序或命令，将其名称输入命令行。让我们运行随操作系统附带的默认计算器程序。输入以下内容到命令行：

- 在 Windows 上，输入 calc.exe。
- 在 macOS 上，输入 `open -a Calculator`。（技术上，这运行了 `open` 程序，然后运行计算器程序。）
- 在 Linux 上，输入 `gnome-calculator`。

程序名称和命令在 Linux 上区分大小写，但在 Windows 和 macOS 上不区分大小写。这意味着尽管你在 Linux 上必须键入 `gnome-calculator`，你可以在 Windows 上键入 `calc.exe`，在 macOS 上键入 `OPEN -a Calculator`。

将这些计算器程序名称输入命令行等同于从开始菜单、Finder 或 Dash 运行计算器程序。这些计算器程序名称之所以能作为命令工作，是因为 `calc.exe`、`open` 和 `gnome-calculator` 程序存在于包含在 PATH 环境变量中的文件夹中。“环境变量和 PATH”第 35 页对此进行了进一步解释。但简而言之，当你在命令行输入程序名称时，shell 会检查是否在 PATH 列出的文件夹之一中存在该名称的程序。在 Windows 上，shell 会先在 cwd（你可以在提示符中看到）中查找程序，然后再检查 PATH 中的文件夹。要告诉 macOS 和 Linux 的命令行首先检查 cwd，你必须在文件名前输入 `./`。

如果程序不在 PATH 列出的文件夹中，你有两个选择：

- 使用 `cd` 命令将 cwd 更改为包含程序的文件夹，然后输入程序名称。例如，你可以输入以下两个命令：
- 输入可执行程序文件的完整文件路径。例如，与其输入 `calc.exe`，你可以输入 `C:\Windows\System32\calc.exe`。

```
cd C:\Windows\System32
calc.exe
```

在 Windows 上，如果程序以文件扩展名 `.exe` 或 `.bat` 结尾，包含扩展名是可选的：输入 `calc` 与输入 `calc.exe` 效果相同。macOS 和 Linux 中的可执行程序通常没有标记为可执行的扩展名；相反，它们设置了可执行权限。“无需命令行运行 Python 程序”第 39 页有更多信息。

## 使用命令行参数

*命令行参数* 是你在命令名称后输入的文本片段。与传递给 Python 函数调用的参数类似，它们为命令提供特定的选项或附加指示。例如，当你运行命令 `cd C:\Users` 时，`C:\Users` 部分是传递给 `cd` 命令的参数，告诉 `cd` 将 cwd 更改为哪个文件夹。或者，当你使用 `python yourScript.py` 命令从终端窗口运行 Python 脚本时，`yourScript.py` 部分是告诉 `python` 程序要查找指令的文件的参数。

*命令行选项*（也称为标志、开关或简单选项）是单个字母或短词的命令行参数。在 Windows 上，命令行选项通常以正斜杠（/）开头；在 macOS 和 Linux 上，它们以单个破折号（-）或双破折号（--）开头。你在运行 macOS 命令 open -a Calculator 时已经使用了 -a 选项。命令行选项在 macOS 和 Linux 上通常区分大小写，但在 Windows 上不区分大小写，我们用空格分隔多个命令行选项。

文件夹和文件名是常见的命令行参数。如果文件夹或文件名包含空格作为其名称的一部分，请用双引号将名称括起来以避免混淆命令行。例如，如果你想更改到名为 *Vacation Photos* 的文件夹，输入 cd Vacation Photos 会使命令行认为你传递了两个参数，Vacation 和 Photos。相反，你输入 cd "Vacation Photos"：

```
C:\Users\Al>cd "Vacation Photos"

C:\Users\Al\Vacation Photos>
```

许多命令的另一个常见参数是 macOS 和 Linux 上的 --help 以及 Windows 上的 /?。它们会调出与命令相关联的信息。例如，如果你在 Windows 上运行 cd /?，shell 会告诉你 cd 命令的作用并列出其他命令行参数：

```
C:\Users\Al>cd /?
Displays the name of or changes the current directory.

CHDIR [/D] [drive:][path]
CHDIR [..]
CD [/D] [drive:][path]
CD [..]

..  Specifies that you want to change to the parent directory.

Type CD drive: to display the current directory in the specified drive.
Type CD without parameters to display the current drive and directory.

Use the /D switch to change current drive in addition to changing current directory for a drive.
--snip--
```

此帮助信息告诉我们 Windows cd 命令也被称为 chdir。（大多数人不会在较短的 cd 命令就能完成相同操作时键入 chdir。）方括号包含可选参数。例如，CD [/D] [drive:][path] 告诉你可以使用 /D 选项指定驱动器或路径。

不幸的是，虽然 /? 和 --help 信息为有经验的用户提供了命令的提醒，但解释通常可能很晦涩。它们不是初学者的好资源。你最好使用一个
