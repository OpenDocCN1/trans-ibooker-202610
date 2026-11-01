
字符串驻留是一种优化技术，解释器和编译器在许多不同的语言中使用。你可以在 https://en.wikipedia.org/wiki/String_interning 上找到更多细节。

## Python 的伪自增和自减运算符

在 Python 中，你可以使用增强赋值运算符将变量的值增加 1 或减少 1。代码 spam += 1 和 spam -= 1 分别将 spam 中的数值递增和递减 1。

其他语言，例如 C++ 和 JavaScript，有 ++ 和 -- 运算符用于自增和自减。（“C++” 名称本身就反映了这一点；它是一个带点调侃的笑话，表示它是 C 语言的增强形式。）C++ 和 JavaScript 中的代码可能有 ++spam 或 spam++ 之类的操作。Python 明智地没有包含这些运算符，因为它们 notoriously 容易出现微妙的错误（详见 https://softwareengineering.stackexchange.com/q/59880）。

但以下 Python 代码完全合法：

```
>>> spam = --spam
>>> spam
42
```

你首先应该注意的是，在 Python 中 ++ 和 -- “运算符”实际上并不会递增或递减 spam 中的值。相反，首部的 - 是 Python 的一元取反运算符。它允许你写出这样的代码：

```
>>> spam = 42
>>> -spam
-42
```

在值前面使用多个一元负运算符是合法的。使用两个运算符会得到值的负数的负数，对于整数值来说最终只是原始值：

```
>>> spam = 42
>>> -(-spam)
42
```

这是一个非常愚蠢的操作，你几乎永远不会在实际代码中看到一元取反运算符被使用两次。（但如果你看到了，很可能是因为程序员学习的是另一种语言，然后写出了有问题的 Python 代码！）

还有一个 + 一元运算符。它将整数值求值为与原始值相同的符号，也就是说，它实际上什么都不做：

```
>>> spam = 42
>>> +spam
42
>>> spam = -42
```

```
>>> +spam
-42
```

写 +42（或 ++42）看起来和 --42 一样愚蠢，那么 Python 为什么还要有这个一元运算符呢？它仅存在于为了补充 - 运算符，以便在为自己的类重载这些运算符时使用。（这是一堆你可能不熟悉的术语！你将在第 17 章学习更多关于运算符重载的内容。）

+ 和 - 一元运算符只有在 Python 值前面才有效，而不是在其后面。虽然 spam++ 和 spam-- 在 C++ 或 JavaScript 中可能是合法代码，但在 Python 中它们会产生语法错误：

```
>>> spam++
  File "<stdin>", line 1
    spam++
         ^
SyntaxError: invalid syntax
```

Python 没有自增和自减运算符。语言语法的一个怪癖只是让它看起来像是有。

## 全或无

all() 内置函数接受一个序列值，例如列表，如果该序列中的所有值都是“真值”，则返回 True。如果一个或多个值是“假值”，则返回 False。你可以将函数调用 all([False, True, True]) 视为等价于表达式 False and True and True。

你可以使用 all() 结合列表推导式，首先基于另一个列表创建一个布尔值列表，然后评估其整体值。例如，在交互式 shell 中输入以下内容：

```
>>> spam = [67, 39, 20, 55, 13, 45, 44]
>>> [i > 42 for i in spam]
[True, False, False, True, False, True, True]
>>> all([i > 42 for i in spam])
False
>>> eggs = [43, 44, 45, 46]
>>> all([i > 42 for i in eggs])
True
```

all() 工具如果 spam 或 eggs 中的所有数字都大于 42，则返回 True。

但如果你将一个空序列传递给 all()，它总是返回 True。在交互式 shell 中输入以下内容：

```
>>> all([])
True
```

最好将 all([]) 视为评估断言“此列表中没有项目是假值”，而不是“此列表中的所有项目都是真值”。否则，你可能会得到一些奇怪的结果。例如，在交互式 shell 中输入以下内容：

```
>>> spam = []
>>> all([i > 42 for i in spam])
True
>>> all([i < 42 for i in spam])
True
>>> all([i == 42 for i in spam])
True
```

这段代码似乎表明不仅 spam（一个空列表）中的所有值都大于 42，而且它们也小于 42 且恰好等于 42！这在逻辑上似乎不可能。但请记住，这三个列表推导式都求值为空列表，这就是为什么它们中没有项目是假值，而 all() 函数返回 True 的原因。

## 布尔值是整数值

正如 Python 认为浮点值 42.0 等于整数值 42，它也认为布尔值 True 和 False 分别等价于 1 和 0。在 Python 中，bool 数据类型是 int 数据类型的子类。（我们将在第 16 章介绍类和子类。）你可以使用 int() 将布尔值转换为整数：

```
>>> int(False)
0
>>> int(True)
1
>>> True == 1
True
>>> False == 0
True
```

你也可以使用 isinstance() 确认布尔值被视为一种整数类型：

```
>>> isinstance(True, bool)
True
>>> isinstance(True, int)
True
```

值 True 的数据类型是 bool。但由于 bool 是 int 的子类，True 也是一个 int。这意味着你可以在几乎可以使用整数的任何地方使用 True 和 False。这可能导致一些奇怪的代码：

```
>>> True + False + True + True  # Same as 1 + 0 + 1 + 1
3
>>> -True          # Same as -1.
-1
```

```
>>> 42 * True      # Same as 42 * 1 mathematical multiplication.
42
>>> 'hello' * False # Same as 'hello' * 0 string replication.
''
>>> 'hello'[False]  # Same as 'hello'[0]
'h'
>>> 'hello'[True]   # Same as 'hello'[1]
'e'
>>> 'hello'[-True]  # Same as 'hello'[-1]
'o'
```

当然，仅因为你可以用数字使用布尔值并不意味着你应该这样做。之前的示例都难以阅读，不应在实际代码中使用。最初，Python 没有 bool 数据类型。它直到 Python 2.3 才添加布尔值，此时它将 bool 设为 int 的子类以便于实现。你可以在 PEP 285 中阅读 bool 数据类型的历史，网址为 https://www.python.org/dev/peps/pep-0285/。

顺便说一下，True 和 False 仅在 Python 3 中才成为关键字。这意味着在 Python 2 中，可以使用 True 和 False 作为变量名，导致像这样的看似悖论的代码：

```
Python 2.7.14 (v2.7.14:84471935ed, Sep 16 2017, 20:25:58) [MSC v.1500 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> True is False
False
>>> True = False
>>> True is False
True
```

幸运的是，这种令人困惑的代码在 Python 3 中是不可能的，如果你尝试使用关键字 True 或 False 作为变量名，它会引发语法错误。

## 链接多种运算符

在同一表达式中链接不同种类的运算符可能会产生意外的错误。例如，这个（ admittedly 不现实的）示例在单个表达式中使用了 == 和 in 运算符：

```
>>> False == False in [False]
True
```

这个 True 结果令人惊讶，因为你期望它求值为以下之一：

- (False == False) in [False]，结果为 False。
- False == (False in [False])，结果也为 False。

但 False == False in [False] 与这两个表达式都不等价。相反，它等价于 (False == False) and (False in [False])，正如

42 < spam < 99 等价于 (42 < spam) and (spam < 99)。该表达式根据以下图表求值：

```
(False == False) and (False in [False])
↓
(True) and (False in [False])
↓
(True) and (True)
↓
True
```

False == False in [False] 表达式是一个有趣的 Python 谜题，但在任何实际代码中出现的可能性都不大。

## Python 的反重力功能

要启用 Python 的反重力功能，在交互式 shell 中输入以下内容：

```
>>> import antigravity
```

这一行是一个有趣的彩蛋，会打开网络浏览器到一个关于 Python 的经典 XKCD 漫画条，网址为 https://xkcd.com/353/。你可能会惊讶 Python 可以打开你的网络浏览器，但这是 webbrowser 模块提供的内置功能。Python 的 webbrowser 模块有一个 open() 函数，它会找到你操作系统的默认网络浏览器并打开一个浏览器窗口到特定 URL。在交互式 shell 中输入以下内容：

```
>>> import webbrowser
>>> webbrowser.open('https://xkcd.com/353/')
```

webbrowser 模块功能有限，但它可以用于引导用户到互联网上的更多信息。

## 总结

很容易忘记计算机和编程语言是由人类设计的，并且有其自身的局限性。大量软件建立在语言设计者和硬件工程师的创造之上并依赖于它们。他们非常努力地确保如果你程序中有 bug，是因为你的程序有缺陷，而不是运行它的解释器软件或 CPU 硬件。我们最终会把这些工具视为理所当然。

但这就是学习计算机和软件的奇特角落有价值的原因。当你的代码引发错误或崩溃（甚至只是行为奇怪，让你想“这很奇怪”）时，你需要了解常见的陷阱来调试这些问题。

你几乎肯定不会遇到本章提到的任何问题，但了解这些小细节会让你成为一名经验丰富的 Python 程序员。

