

例如，假设你想要打印一个 40 × 20 的星号（*）块。迄今为止，最慢的方法是逐个字符地打印。在 IDLE 中，这段代码运行得非常缓慢。

```
for i in range(20):
    for j in range(40):
        print('*', end='')
    print()
```

你可以一次打印一整行星号，从而获得更好的性能。

```
row_of_asterisks = '*' * 40
for i in range(20):
    print(row_of_asterisks)
```

但最佳性能是通过修改代码来实现的，即仅在组装好一个大的多行输出字符串后，只调用一次 print 函数。

单击此处查看代码图片

```
row_of_asterisks = '*' * 40
s = ''
for i in range(20):
    s += row_of_asterisks + '\n'
print(s)
```

这个示例还可以进一步改进，利用字符串类的 join 方法。这段代码之所以更好，是因为它使用了列表的原地追加，而不是追加到字符串（字符串每次都必须创建一个新字符串）。

单击此处查看代码图片

```
row_of_asterisks = '*' * 40
list_of_str = []
for i in range(20):
    list_of_str.append(row_of_asterisks)
print('\n'.join(list_of_str))
```

更棒的是，下面是一行代码版本！

单击此处查看代码图片

```
print('\n'.join(['*' * 40] * 20))
```

## 4.2.22 在大数字内部使用下划线

在编程中，你有时需要处理大型数字字面量。下面是一个示例：

```
CEO_salary = 15000000
```

这样的数字在编程代码中难以阅读。你可能希望使用逗号作为分隔符，但逗号被保留用于其他目的，例如创建列表。幸运的是，Python 提供了另一种技术：你可以在数字字面量内部使用下划线（_）。

```
CEO_salary = 1_500_000
```

根据以下规则，下划线可以放在数字内部的任何位置。其效果是 Python 读取数字时，就好像没有下划线一样。这种技术涉及几条规则。

- 不能连续使用两个下划线。
- 不能使用前导或后置下划线。如果使用前导下划线（如 \_1），则该数字被视为变量名。
- 可以在小数点的两侧使用下划线。

这种技术仅影响数字在代码本身中的显示方式，而不影响任何内容的打印方式。要打印带有千位分隔符的数字，请使用第 5 章“精确格式化文本”中描述的 `format` 函数或方法。

## 4.3 从命令行运行 Python

如果你一直在 IDLE 中运行 Python 程序——无论是一次输入一条命令，还是作为脚本——一种提高执行速度的方法是改为从命令行运行程序；特别是，这样做大大缩短了执行 `print` 函数调用所需的时间。

命令行操作的一些特性取决于你使用的操作系统。本节介绍两种最常见的操作系统：Windows 和 Macintosh。

### 4.3.1 在基于 Windows 的系统上运行

与 Macintosh 不同，Windows 系统通常不预装 Python 2.0 版本，只要你自己安装 Python 3，这种做法实际上可以为你省去不少麻烦。

要从命令行使用 Python，首先启动 DOS Box 应用程序，它是所有 Windows 系统上的主要应用程序。Python 应该很容易获得，因为它应该位于 PATH 设置包含的目录中。在运行 Windows DOS Box 时，检查此设置很容易。

在 Windows 中，你还可以通过打开控制面板，选择系统，然后选择高级选项卡来检查 PATH 设置。然后单击环境变量。

然后，只要程序在你的 PATH 中，你就应该能够直接运行 Python 程序。要从命令行运行程序，请输入 `python` 和源文件名（主模块），包括 `.py` 扩展名。

```
python test.py
```

### 4.3.2 在 Macintosh 系统上运行

Macintosh 系统通常预装了 Python 版本；不幸的是，在较新的系统中，版本是 Python 2.0 而不是 Python 3.0。

要确定已安装哪个版本用于命令行使用，首先在 Macintosh 系统上启动终端应用程序。你可能需要先单击启动台图标。你应该会发现自己处于默认目录中，无论它是什么。你可以使用以下命令来确定你拥有哪个命令行版本的 Python：

```
python -v
```

如果 Python 版本是 2.0+，你会收到类似下面的消息：

```
python 2.7.10
```

但如果你下载了某个 Python 3.0 版本，你也应该加载了该版本的 Python。但是，要运行它，你必须使用命令 **python3** 而不是 **python**。如果你加载了 **python3**，你可以从命令行按如下方式验证确切版本：

```
python3 -v
python 3.7.0
```

例如，如果文件 `test.py` 在当前目录中，并且你想将其作为 Python 3.0 程序编译，那么请使用以下命令：

```
python3 test.py
```

Python 命令（无论是 **python** 还是 **python3**）有一些有用的变体。如果你输入 `-h`，“帮助”标志，你会得到所有可以与该命令一起使用的可能标志的打印输出，以及相关的环境变量。

```
python3 -h
```

### 4.3.3 使用 pip 或 pip3 下载包

本书中的某些包要求你在使用这些包之前先从 Internet 下载并安装它们。第一个要求这样做的章节是第 12 章，它介绍了 **numpy** 包。

本书中提到的所有包都完全免费（就像大多数 Python 包一样）。更棒的是，**pip** 实用程序（包含在 Python 3 下载中）会去查找你命名的包；因此你只需要一个 Internet 连接！

在基于 Windows 的系统上，使用以下命令下载并安装所需的包。

```
pip install package_name
```

顺便说一句，包名不使用文件扩展名：

```
pip install numpy
```

在 Macintosh 系统上，你可能需要使用 **pip3** 实用程序，它是在你安装 Python 3 时随之下载的。（你可能也继承了一个 pip 版本，但它可能已过时且无法使用。）

```
pip3 install package_name
```

## 4.4 编写和使用文档字符串

Python _文档字符串_ 使你能够利用编写注释所做的工作来获得免费的在线帮助。当你使用 IDLE 运行时，以及从命令行使用 **pydoc** 实用程序时，该帮助都可供你使用。

你可以为函数和类编写文档字符串。虽然本书尚未介绍如何编写类，但原理是相同的。下面是一个带有文档字符串的函数示例。

单击此处查看代码图片

```
def quad(a, b, c):
    '''Quadratic Formula function.

    This function applies the Quadratic Formula
    to determine the roots of x in a quadratic
    equation of the form ax^2 + bx + c = 0.
    '''
    determin = (b * b - 4 * a * c) ** .5
    x1 = (-b + determin) / (2 * a)
    x2 = (-b - determin) / (2 * a)
    return x1, x2
```

当此文档字符串在函数定义中输入时，你可以在 IDLE 中获得帮助：

单击此处查看代码图片

```
>>> help(quad)
Help on function quad in module __main__:

quad(a, b, c)
    Quadratic Formula function.

    This function applies the Quadratic Formula
    to determine the roots of x in a quadratic
    equation of the form ax^2 + bx + c = 0.
```

编写文档字符串的机制遵循若干规则。

- 文档字符串本身必须紧跟在函数标题之后。
- 它必须是一个使用三引号特性的字面量字符串。（你实际上可以使用任何样式的引号，但如果你想跨多行，则需要字面量引号。）
- 文档字符串还必须与函数标题下的“级别 1”缩进对齐：例如，如果函数标题下紧随其后的语句缩进四个空格，那么文档字符串的开头也必须缩进四个空格。
- 文档字符串的后续行可以按你选择的方式缩进，因为字符串是一个字面量字符串。你可以将后续行左对齐，或者继续你开始使用文档字符串时的缩进。无论哪种情况，Python 在线帮助都会以有用的方式对齐文本。

