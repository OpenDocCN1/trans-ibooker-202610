## A 安装和故障排除

![](img/8f15563c16d2a98c4d3115150679b092_740_0.png)

有许多版本的 Python 可用，并且在每个操作系统上设置它的方法也各不相同。如果第 1 章中的方法不起作用，或者如果你想安装与当前安装的版本不同的 Python 版本，本附录中的说明可以帮助你。

### Windows 上的 Python

第 1 章中的说明向你展示了如何使用 https://python.org 上的官方安装程序安装 Python。如果使用安装程序后无法运行 Python，本节中的故障排除说明应该可以帮助你让 Python 运行起来。

#### 使用 py 而不是 python

如果你运行了最近的 Python 安装程序，然后在终端中发出 **python** 命令，你应该会看到终端会话的 Python 提示符（>>>）。当 Windows 不识别 python 命令时，它要么会打开 Microsoft Store，因为它认为 Python 未安装，要么你会收到诸如“未找到 Python”之类的消息。如果 Microsoft Store 打开，请关闭它；使用 https://python.org 上的官方 Python 安装程序比微软维护的安装程序更好。

最简单的解决方案，无需对系统进行任何更改，是尝试 **py** 命令。这是一个 Windows 实用程序，可找到系统上安装的最新版本的 Python 并运行该解释器。如果此命令有效并且你想使用它，只需在本书中看到 **python** 或 **python3** 命令的地方使用 **py** 即可。

#### 重新运行安装程序

python 不起作用的最常见原因是人们在运行安装程序时忘记选择将 Python 添加到 PATH 选项；这是一个很容易犯的错误。PATH 变量是一个系统设置，告诉 Python 在哪里查找常用程序。在这种情况下，Windows 不知道如何找到 Python 解释器。

在这种情况下最简单的修复方法是再次运行安装程序。如果 https://python.org 上有更新的安装程序可用，请下载新的安装程序并运行它，确保选中 **Add Python to PATH** 复选框。

如果你已经拥有最新的安装程序，请再次运行它并选择 **Modify** 选项。你会看到可选功能列表；在此屏幕上保留默认选项。然后单击 **Next** 并选中 **Add Python to Environment Variables** 复选框。最后，单击 **Install**。该





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
