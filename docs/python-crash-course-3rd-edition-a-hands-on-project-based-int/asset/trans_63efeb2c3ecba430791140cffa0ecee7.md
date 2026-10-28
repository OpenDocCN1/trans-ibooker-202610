

## 旧版 macOS 上的 Python 2

在 Monterey（macOS 12）之前的旧版 macOS 上，默认安装了过时的 Python 2 版本。在这些系统上，`python` 命令指向的是过时的系统解释器。如果你使用的 macOS 版本装有 Python 2，请务必使用 **python3** 命令，这样你使用的始终是你自己安装的 Python 版本。

## Linux 上的 Python

几乎所有 Linux 系统都默认自带 Python。但是，如果系统上的默认版本低于 Python 3.9，你应该安装最新版本。如果你想要最新的功能（比如 Python 改进后的错误消息），也可以安装最新版本。以下说明适用于大多数基于 apt 的系统。

## 使用默认的 Python 安装

如果你想使用 `python3` 所指向的 Python 版本，请确保已安装以下三个附加包：

```bash
$ sudo apt install python3-dev python3-pip python3-venv
```

这些包包含对开发人员有用的工具，以及允许你安装第三方包的工具，例如本书“项目”部分中使用的那些包。

## 安装最新版本的 Python

我们将使用一个名为 `deadsnakes` 的包，它可以轻松安装多个版本的 Python。输入以下命令：

```bash
$ sudo add-apt-repository ppa:deadsnakes/ppa
$ sudo apt update
$ sudo apt install python3.11
```

这些命令会将 Python 3.11 安装到你的系统上。

输入以下命令以启动运行 Python 3.11 的终端会话：

```bash
$ python3.11
>>>
```

你在本书中看到 `python` 命令的地方，请改用 `python3.11`。从终端运行程序时，也需要使用这个命令。

你还需要安装两个额外的包，才能充分利用你的 Python 安装：

```bash
$ sudo apt install python3.11-dev python3.11-venv
```

这些包包含你在安装和运行第三方包时所需的模块，例如本书后半部分项目中使用的那些包。

> **注意**
>
> *deadsnakes 包已经积极维护了很长时间。当更新版本的 Python 发布时，你可以使用相同的命令，只需将 `python3.11` 替换为当前可用的最新版本即可。*

## 检查你正在使用的 Python 版本

如果你在运行 Python 或安装附加包时遇到任何问题，那么确切地知道你正在使用的 Python 版本会很有帮助。你可能安装了多个 Python 版本，却不清楚当前正在使用哪个版本。

在终端中执行以下命令：

```bash
$ python --version
Python 3.11.0
```

这会确切地告诉你 `python` 命令当前指向哪个版本。更简短的命令 `python -V` 会给出相同的输出。

## Python 关键字和内置函数

Python 自带一组关键字和内置函数。在 Python 中命名事物时，了解这些非常重要：你的名称不能与这些关键字相同，也不应与函数名相同，否则你会覆盖这些函数。

在本节中，我们将列出 Python 的关键字和内置函数名，以便你知道要避免使用哪些名称。

### Python 关键字

以下每个关键字都有特定的含义，如果你试图将其中任何一个用作变量名，将会看到错误。

| False | await | else | import | pass |
|---|---|---|---|---|
| None | break | except | in | raise |
| True | class | finally | is | return |
| and | continue | for | lambda | try |
| as | def | from | nonlocal | while |
| assert | del | global | not | with |
| async | elif | if | or | yield |

## Python 内置函数

如果你将下面某个可用的内置函数用作变量名，不会报错，但你会覆盖该函数的行为：

| abs() | hash() | slice() |
|---|---|---|
| aiter() | help() | sorted() |
| all() | hex() | staticmethod() |
| any() | id() | str() |
| anext() | input() | sum() |
| ascii() | int() | super() |
| bin() | isinstance() | tuple() |
| bool() | issubclass() | type() |
| breakpoint() | iter() | vars() |
| bytearray() | len() | zip() |
| bytes() | list() | __import__() |
| callable() | locals() | |
| chr() | map() | |
| classmethod() | max() | |
| compile() | memoryview() | |
| complex() | min() | |
| delattr() | next() | |
| dict() | object() | |
| dir() | oct() | |
| divmod() | open() | |
| enumerate() | ord() | |
| eval() | pow() | |
| exec() | print() | |
| filter() | property() | |
| float() | range() | |
| format() | repr() | |
| frozenset() | reversed() | |
| getattr() | round() | |
| globals() | set() | |
| hasattr() | setattr() | |

## B

## 文本编辑器和 IDE

![](img/8f15563c16d2a98c4d3115150679b092_748_0.png)

程序员会花费大量时间编写、阅读和编辑代码，因此使用文本编辑器或 IDE（集成开发环境）来尽可能高效地完成这项工作至关重要。一个好的编辑器会处理一些简单任务，比如高亮代码结构，让你在工作中发现常见错误。但它不会做太多事情，以免分散你的思考。编辑器还具有一些有用的功能，如自动缩进、显示合适行长的标记，以及常见操作的键盘快捷键。

*IDE* 是一种文本编辑器，但包含了大量其他工具，如交互式调试器和代码自省功能。IDE 会在你输入代码时检查代码，并尝试了解你正在构建的项目。例如，当你开始键入函数名时，IDE 可能会显示该函数接受的所有参数。当一切正常并且你理解所看到的内容时，这种行为会非常有用。但作为初学者，它也可能令人不知所措，并且在你不确定为什么代码在 IDE 中无法运行时，会难以排查问题。

如今，文本编辑器和 IDE 之间的界限已经模糊。大多数流行的编辑器都拥有一些曾经专属于 IDE 的功能。同样，大多数 IDE 也可以配置为以更轻量的模式运行，从而减少工作时的干扰，但在需要时仍可使用更高级的功能。

如果你已经安装了自己喜欢的编辑器或 IDE，并且它已经配置好，可以配合系统中安装的最新 Python 版本使用，那么我建议你继续使用你熟悉的工具。探索不同的编辑器可能很有趣，但这也是一种逃避学习新语言工作的方式。

如果你还没有安装编辑器或 IDE，我推荐 VS Code，原因如下：

- 它是免费的，并根据开源许可证发布。
- 它可以安装在所有主要操作系统上。
- 它对初学者友好，但也足够强大，许多专业程序员将其作为主要编辑器。
- 它能找到你已安装的 Python 版本，并且通常不需要任何配置即可运行你的第一个程序。

它有一个集成终端，因此你的输出会显示在与代码相同的窗口中。

有一个 Python 扩展可用，它能让编辑器在编写和维护 Python 代码时非常高效。

它的可定制性很强，因此你可以调整它以适应自己的代码工作方式。

在本附录中，你将学习如何开始配置 VS Code，使其适合你。你还将学到一些能让你更高效工作的快捷键。打字速度快并不像许多人想象的那么重要，但了解你的编辑器并懂得如何高效地使用它则非常有帮助。

话虽如此，VS Code 并不适合所有人。如果它由于某种原因在你的系统上运行不佳，或者在工作时让你分心，那么还有其他许多编辑器可能会更吸引你。本附录包含一些你应该考虑的其他编辑器和 IDE 的简要介绍。

## 高效使用 VS Code

在第 1 章中，你安装了 VS Code 并添加了 Python 扩展。本节将向你展示一些可以进行的进一步配置，以及高效处理代码的快捷键。

## 配置 VS Code

更改 VS Code 默认配置设置的方法有几种。有些更改可以通过界面完成，有些则需要修改配置文件。这些更改有时会影响到你在 VS Code 中执行的所有操作，而其他更改只会影响包含配置文件的文件夹内的文件。

