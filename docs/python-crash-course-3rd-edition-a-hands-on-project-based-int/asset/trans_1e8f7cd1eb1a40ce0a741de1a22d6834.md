

你可以通过按下 CTRL-D 或输入命令 `exit()` 来关闭 Python 解释器。

> **注意**
> 
> *在较新的 macOS 系统上，你会看到百分号 (%) 作为终端提示符，而不是美元符号 ($)。*

## 安装 VS Code

要安装 VS Code 编辑器，你需要从 [https://code.visualstudio.com](https://code.visualstudio.com) 下载安装程序。点击 **Download** 按钮，然后打开一个 **Finder** 窗口并前往 **Downloads** 文件夹。将 **Visual Studio Code** 安装程序拖到你的应用程序文件夹，然后双击安装程序运行它。

跳过下面关于 Linux 上 Python 的部分，并按照第 9 页“运行 Hello World 程序”中的步骤操作。

## Linux 上的 Python

Linux 系统专为编程而设计，因此大多数 Linux 电脑上已预装了 Python。编写和维护 Linux 的人员希望你在某个时候进行自己的编程，并鼓励你这样做。因此，几乎没有什么需要安装，也只有少数设置需要更改即可开始编程。

## 检查你的 Python 版本

通过在系统上运行终端应用程序打开一个终端窗口（在 Ubuntu 中，你可以按 CTRL-ALT-T）。要了解安装了哪个版本的 Python，请输入小写的 *p* 的 `python3`。当 Python 已安装时，此命令会启动 Python 解释器。你应该会看到指示已安装 Python 版本的输出。你还应该看到一个 Python 提示符 (>>>)，在那里你可以开始输入 Python 命令：

```text
$ python3
Python 3.10.4 (main, Apr   , 09:04:19) [GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

此输出表明 Python 3.10.4 目前是此计算机上安装的 Python 默认版本。当你看到此输出后，按 CTRL-D 或输入 `exit()` 以离开 Python 提示符并返回终端提示符。每当你在本书中看到 python 命令时，请改用 **python3**。

你需要 Python 3.9 或更高版本才能运行本书中的代码。如果你的系统上安装的 Python 版本早于 Python 3.9，或者你想更新到当前可用的最新版本，请参阅附录 A 中的说明。

## 在终端会话中运行 Python

你可以通过打开终端并输入 **python3** 来尝试运行 Python 代码片段，就像你在检查版本时所做的那样。再次执行此操作，并在 Python 运行后，在终端会话中输入以下行：

```python
>>> print("Hello Python interpreter!")
Hello Python interpreter!
>>>
```

消息应直接打印在当前终端窗口中。记住，你可以通过按下 CTRL-D 或输入命令 **exit()** 来关闭 Python 解释器。

## 安装 VS Code

在 Ubuntu Linux 上，你可以从 Ubuntu 软件中心安装 VS Code。点击菜单中的 Ubuntu 软件图标并搜索 *vscode*。点击名为 **Visual Studio Code** 的应用（有时称为 *code*），然后点击 **Install**。安装完成后，在你的系统上搜索 *VS Code* 并启动该应用。

## 运行 Hello World 程序

在安装了最新版本的 Python 和 VS Code 后，你几乎就可以在文本编辑器中运行第一个 Python 程序了。但在这样做之前，你需要为 VS Code 安装 Python 扩展。

### *为 VS Code 安装 Python 扩展*

VS Code 支持多种不同的编程语言；为了充分发挥其作为 Python 程序员的潜力，你需要安装 Python 扩展。此扩展增加了编写、编辑和运行 Python 程序的支持。

要安装 Python 扩展，请点击管理图标，它看起来像 VS Code 应用左下角的齿轮。在出现的菜单中，点击 **Extensions**。在搜索框中输入 **python** 并点击 **Python** 扩展。（如果看到多个名为 *Python* 的扩展，请选择由 Microsoft 提供的那个。）点击 **Install** 并安装系统完成安装所需的任何其他工具。如果看到需要安装 Python 的消息，而你已经安装了，可以忽略此消息。

> **注意**
> 
> *如果你使用的是 macOS 且弹出窗口要求你安装命令行开发工具，请点击 **Install**。你可能会看到一条消息说安装时间过长，但在一台合理的互联网连接上，它应该只需要 10 或 20 分钟。*

## 运行 hello_world.py

在编写第一个程序之前，在你的桌面上创建一个名为 *python_work* 的文件夹用于你的项目。最好在文件和文件夹名称中使用小写字母和下划线代替空格，因为 Python 使用这些命名约定。你可以在桌面以外的其他位置创建此文件夹，但如果你将 *python_work* 文件夹直接保存在桌面上，后续步骤会更容易跟随。

打开 VS Code，如果 **Get Started** 选项卡仍打开则关闭它。通过点击 **File▸New File** 或按 CTRL-N（macOS 上为 ⌘-N）新建一个文件。将文件保存为 *hello_world.py*，保存在你的 *python_work* 文件夹中。扩展名 *.py* 告诉 VS Code 你的文件是用 Python 编写的，并告诉它如何运行程序以及以有帮助的方式高亮文本。

保存文件后，在编辑器中输入以下行：

```python
print("Hello Python world!")
```

要运行你的程序，请选择 **Run▸Run Without Debugging** 或按 CTRL-F5。VS Code 窗口底部应出现一个终端屏幕，显示你的程序输出：

```
Hello Python world!
```

你可能会看到一些额外的输出，显示用于运行程序的 Python 解释器。如果你想简化显示的信息，只显示你的程序输出，请参阅附录 B。你也可以在附录 B 中找到有关如何更高效地使用 VS Code 的有用建议。

如果看不到此输出，程序中可能出了问题。检查你输入的行上的每个字符。你是否不小心将 `print` 大写了？你是否忘记了一个或两个引号或括号？编程语言期望非常具体的语法，如果你没有提供，就会得到错误。如果无法运行程序，请参阅下一节中的建议。

## 故障排除

如果无法运行 *hello_world.py*，以下是你可以尝试的几种补救措施，它们也是任何编程问题的良好通用解决方案：

当程序包含重大错误时，Python 会显示 *traceback*，即错误报告。Python 会浏览文件并尝试识别问题。检查 traceback；它可能会给你一个线索，说明是什么问题阻止了程序运行。

离开你的电脑，休息一下，然后再试一次。记住语法在编程中非常重要，因此像不匹配的引号或不匹配的括号这样简单的事情也会阻止程序正常运行。重新阅读本章的相关部分，查看你的代码，并尝试找到错误。

理清问题。一旦 *hello_world.py* 运行起来，你就可以开始学习 Python，你的编程工作将变得更有趣、更令人满意。

## 从终端运行 Python 程序

你将直接在文本编辑器中运行大部分程序。然而，有时直接从终端运行程序也很有用。例如，你可能想在不打开进行编辑的情况下运行现有程序。

只要你知道如何访问程序文件所在的目录，就可以在任何安装了 Python 的系统上执行此操作。要尝试此操作，请确保你已将 *hello_world.py* 文件保存在桌面上的 *python_work* 文件夹中。

### *在 Windows 上*

你可以使用终端命令 `cd`，即 *change directory*，在命令窗口中浏览文件系统。命令 `dir`，即 *directory*，会显示当前目录中存在的所有文件。

打开一个新终端窗口并输入以下命令以运行 *hello_world.py*：

```
C:\> cd Desktop\python_work
C:\Desktop\python_work> dir
hello_world.py
C:\Desktop\python_work> python hello_world.py
Hello Python world!
```

首先，使用 `cd` 命令导航到 *python_work* 文件夹，该文件夹位于 *Desktop* 文件夹中。接下来，使用 `dir`

