
原因在于，使用返回 None 来表示错误是导致未捕获的 'NoneType' object has no attribute 异常的常见来源：

```
>>> import random
>>> def sometimesReturnsNone():
...     if random.randint(1, 2) == 1:
...         return 'Hello!'
...     else:
...         return None
...
>>> returnVal = sometimesReturnsNone()
>>> returnVal.upper()
'HELLO!'
>>> returnVal = sometimesReturnsNone()
>>> returnVal.upper()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'NoneType' object has no attribute 'upper'
```

此错误信息相当模糊，且可能需要花费一些精力才能追溯其原因，追溯到一个通常返回预期结果但在发生错误时也可能返回 None 的函数。问题发生的原因是 sometimesReturnsNone() 返回了 None，而我们随后将其赋给了 returnVal 变量。但错误信息会让你以为问题出现在对 upper() 方法的调用中。

在 2009 年的一次会议演讲中，计算机科学家 Tony Hoare 为在 1965 年发明空引用（与 Python 的 None 值大致对应的通用值）道歉，他说：“I call it my billion dollar mistake. [...] I couldn’t resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last 40 years.” 你可以在 https://autor.com/billiondollarmistake 在线观看他的完整演讲。

## 引发异常 vs. 返回错误代码

在 Python 中，术语 exception 和 error 的含义大致相同：程序中通常表示问题的异常情况。

异常作为编程语言特性在 1980 年代和 1990 年代随着 C++ 和 Java 而流行起来。它们取代了错误代码的使用，错误代码是函数返回的值，用来指示问题。异常的好处在于，返回值仅与函数的功能相关，而不需同时表示错误的出现。

错误代码也可能在你的程序中引起问题。例如，Python 的 find() 字符串方法通常返回找到子字符串的索引，如果找不到，则返回 -1 作为错误代码。但由于我们也可以使用 -1 来指定字符串末尾的索引，不小心将 -1 用作错误代码可能会引入 bug。请在交互式 shell 中输入以下内容以查看其工作原理。

```
>>> print('Letters after b in "Albert":', 'Albert'['Albert'.find('b') + 1:])
Letters after b in "Albert": ert
>>> print('Letters after x in "Albert":', 'Albert'['Albert'.find('x') + 1:])
Letters after x in "Albert": Albert
```

代码中 'Albert'.find('x') 部分求值为错误代码 -1。这使得表达式 'Albert'['Albert'.find('x') + 1:] 求值为 'Albert'[-1 + 1:]，进而求值为 'Albert'[0:]，最后为 'Albert'。显然，这不是代码的预期行为。如果改用 index() 而不是 find()，例如 'Albert'['Albert'.index('x') + 1:]，就会引发异常，使问题变得明显且无法忽视。

另一方面，index() 字符串方法如果无法找到子字符串，则会引发 ValueError 异常。如果不处理此异常，程序将会崩溃——这种行为通常比未注意到错误更可取。

当异常表示实际错误时，异常类的名称通常以 “Error” 结尾，例如 ValueError、NameError 或 SyntaxError。表示不一定是错误的异常情况的异常类包括 StopIteration、KeyboardInterrupt 或 SystemExit。

## 摘要

函数是将程序代码分组的常见方式，它们要求你做出某些决定：如何命名它们、大小如何、应有多少参数，以及为这些参数传递多少参数。def 语句中的 * 和 ** 语法允许函数接收可变数量的参数，使其成为可变参数函数。

虽然不是函数式编程语言，但 Python 具有许多函数式编程语言使用的特性。函数是第一类对象，这意味着你可以将它们存储在变量中并作为参数传递给其他函数（在此上下文中称为高阶函数）。Lambda 函数提供了一种简短的语法，用于指定作为高阶函数参数的无名匿名函数。Python 中最常见的高阶函数是 map() 和 filter()，尽管你可以用列表推导式更快地执行它们提供的功能。

你的函数的返回值应始终为相同的数据类型。你不应将返回值用作错误代码：异常用于指示错误。特别是 None 值经常被错误地用作错误代码。

## 11 注释、文档字符串和类型提示

![](img/ab52fe24e511e29be73786864ebe0410_207_0.png)

你源代码中的注释和文档可能与代码本身一样重要。原因在于软件永远不会完成；你总需要进行更改，无论是添加新功能还是修复 bug。但除非你理解代码，否则无法更改代码，因此保持其可读状态很重要。正如计算机科学家 Harold Abelson、Gerald Jay Sussman 和 Julie Sussman 曾经写道：“Programs must be written for people to read, and only incidentally for machines to execute.”

> 程序必须为人们阅读而编写，而机器执行只是附带。

注释、文档字符串和类型提示有助于你保持代码的可读性。注释是你直接写在源代码中的简短、通俗易懂的解释，计算机会忽略它们。注释为未编写代码的其他人，或有时甚至为未来的代码编写者提供有用的说明、警告和提醒。几乎每个程序员都曾问自己：“Who wrote this unreadable mess?” 只有在找到答案是，“I did.” 时才作罢。

文档字符串是针对你的函数、方法和模块的 Python 特定文档形式。当你以文档字符串格式指定注释时，诸如文档生成器或 Python 内置 help() 模块之类的自动化工具可让开发者轻松查找有关你代码的信息。

类型提示是你可添加到 Python 源代码中的指令，用于指定变量、参数和返回值的 数据类型。这允许静态代码分析工具验证你的代码不会因类型错误的值而产生异常。类型提示首次出现在 Python 3.5 中，但由于它们基于注释，你可以在任何 Python 版本中使用它们。

本章重点介绍上述三种在代码中嵌入文档以使其更易读的技术。外部文档，如用户手册、在线教程和参考资料很重要，但本书不涵盖。如果你想了解更多关于外部文档的信息，请查看 https://www.sphinx-doc.org/ 上的 Sphinx 文档生成器。

### 注释

与大多数编程语言一样，Python 支持单行注释和多行注释。# 号与行尾之间的任何文本都是单行注释。虽然 Python 没有专门的多行注释语法，但三引号多行字符串可以充当其用途。毕竟，单独的字符串值不会导致 Python 解释器执行任何操作。请看此示例：

```
# This is a single-line comment.

"""This is a
multiline string that
also works as a multiline comment. """
```

如果你的注释跨越多行，使用单个多行注释比使用多个连续的单行注释更好，后者更难阅读，如下所示：

```
"""This is a good way
to write a comment
that spans multiple lines. """
# This is not a good way
# to write a comment
# that spans multiple lines.
```
