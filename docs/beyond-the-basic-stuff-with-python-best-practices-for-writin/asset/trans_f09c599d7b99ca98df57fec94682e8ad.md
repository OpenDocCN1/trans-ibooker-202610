
## 处理变量值

你经常需要检查和修改变量存储的值。
Python 有几种方法可以做到这一点。让我们看几个例子。

### 链式赋值和比较运算符

当你需要检查一个数字是否在某个范围内时，你可能会像这样使用布尔 and 运算符：

```
# Unpythonic Example
if 42 < spam and spam < 99:
```

但是 Python 允许你链式使用比较运算符，这样就不需要使用 and 运算符了。以下代码与前一个示例等价：

```
# Pythonic Example
if 42 < spam < 99:
```

同样适用于链式使用 = 赋值运算符。你可以在一行代码中将多个变量设置为相同的值：

```
>>> # Pythonic Example
>>> spam = eggs = bacon = 'string'
>>> print(spam, eggs, bacon)
string string string
```

要检查这三个变量是否相同，你可以使用 and 运算符，或者更简单地，链式使用 == 比较运算符来判断相等。

```
>>> # Pythonic Example
>>> spam = eggs = bacon = 'string'
>>> spam == eggs == bacon == 'string'
True
```

链式运算符是 Python 中一个小而有用的快捷方式。然而，如果使用不当，它们可能会导致问题。第 8 章演示了一些使用它们可能在你的代码中引入意外 bug 的情况。

### 检查变量是否属于多个值中的一个

你有时可能会遇到前一节描述情况的反面：检查单个变量是否属于多个可能值中的一个。你可以使用 or 运算符来做到这一点，例如在表达式 spam == 'cat' or spam == 'dog' or spam == 'moose' 中。所有那些冗余的 “spam ==” 部分使这个表达式有点笨拙。

相反，你可以将多个值放入一个元组中，并使用 in 运算符检查变量的值是否存在于该元组中，如以下示例所示：

```
>>> # Pythonic Example
>>> spam = 'cat'
>>> spam in ('cat', 'dog', 'moose')
True
```

不仅这种惯用法更容易理解，而且根据 timeit 的说法，它也稍快一些。

## 摘要

所有编程语言都有自己的惯用法和最佳实践。本章重点介绍 Python 程序员编写“pythonic”代码的特定方式，以充分利用 Python 的语法。

pythonic 代码的核心是来自 Zen of Python 的 20 条格言，它们是编写 Python 的粗略指南。这些格言是观点，并非编写 Python 代码的严格必要条件，但值得牢记。

Python 的显著缩进（不要与显著空白混淆）引发了新 Python 程序员最多的抗议。虽然几乎所有编程语言通常都使用缩进来使代码更易读，但 Python 要求使用它来代替其他语言常用的大括号。

尽管许多 Python 程序员在 for 循环中使用 range(len()) 惯例，但 enumerate() 函数提供了一种更清晰的方法来在遍历序列时获取索引和值。同样，与手动调用 open() 和 close() 相比，with 语句是处理文件更简洁且不易出错的方式。with 语句确保只要执行移出 with 语句的块，close() 就会被调用。

Python 有几种字符串插值方式。原始方式是使用 %s 转换说明符来标记在原始字符串中应包含字符串的位置。从 Python 3.6 开始的现代方式是使用 f-strings。F-strings 用字母 f 为字符串字面量做前缀，并使用大括号标记在字符串内部可以放置字符串（或整个表达式）的位置。

The [:] syntax for making shallow copies of lists is a bit odd-looking and not necessarily pythonic, but it’s become a common way to quickly create a shallow list.

Dictionaries have a get() and setdefault() method for dealing with nonexistent keys. Alternatively, a collections.defaultdict dictionary will use a default value for nonexistent keys. Also, although there is no switch statement in Python, using a dictionary is a terse way to implement its equivalent without using several if-elif-else statements, and you can use ternary operators when evaluating between two values.

A chain of == operators can check whether multiple variables are equal to each other, whereas the in operator can check whether a variable is one of many possible values.

本章介绍了几种 Python 语言惯用法，为你提供了如何编写更 pythonic 代码的提示。在下一章中，你将了解一些初学者容易陷入的 Python gotchas 和 pitfalls。

## 7

## 编程术语

![](img/ab52fe24e511e29be73786864ebe0410_133_0.png)

在 XKCD 漫画 “Up Goer Five” (https://xkcd.com/1133/) 中，网络漫画艺术家 Randall Munroe 使用仅 1,000 个最常见的英语单词为 Saturn V 火箭创建了技术示意图。漫画将所有技术术语分解成幼儿也能理解的句子。但它也强调了为什么我们不能用简单术语解释一切：解释 “Thing to help people escape really fast if there’s a problem and everything is on fire so they decide not to go to space” 可能比 “Launch Escape System” 更容易让普通受众理解。但它对 NASA 工程师来说在日常工作中说起来太冗长了。即便如此，他们可能更愿意使用缩写 LES。

尽管计算机术语对新程序员来说可能令人困惑和难以接受，但它是必要的简写。Python 和软件开发中的几个术语在含义上有细微差别，甚至经验丰富的开发人员有时也会随意互换使用。这些术语的技术定义在不同编程语言之间可能有所不同，但本章介绍的术语与 Python 相关。你将对它们背后的编程语言概念获得广泛但不深入的理解。

本章假设你尚未熟悉 classes 和 object-oriented programming (OOP)。我在此限制了对 classes 和其他 OOP 术语的解释，但这些术语将在第 15 到 17 章中更详细地解释。

## 定义

随着房间里程序员数量接近两人，关于语义争论的可能性接近 100%。语言是流动的，人类是词语的主人而不是相反。一些开发人员可能会略微不同地使用术语，但熟悉这些术语仍然很有用。本章探讨这些术语以及它们之间的比较。如果你需要按字母顺序排列的术语表，你可以依赖官方 Python 术语表 https://docs.python.org/3/glossary.html 来提供规范定义。

毫无疑问，一些程序员会阅读本章中的定义并提出可以无休止挑剔的特殊情况或例外。与其成为权威指南，本章旨在为你提供易于理解的定义，即使它们并不全面。正如编程中的一切一样，总有更多东西需要学习。

### Python 语言与 Python 解释器

词 python 可以有多种含义。Python 编程语言的名字来源于英国喜剧团体 Monty Python，而不是蛇（尽管 Python 教程和文档同时使用 Monty Python 和蛇的引用）。同样，就计算机编程而言，Python 也可以有两种含义。

当我们说，“Python 运行一个程序”或“Python 将引发异常”时，我们说的是 Python 解释器——实际读取 .py 文件文本并执行其指令的软件。当我们说，“Python 解释器”时，我们几乎总是说的是 CPython，由 Python Software Foundation 维护的 Python 解释器，可在 https://www.python.org 获取。CPython 是 Python 语言的一种实现——即为遵循规范而创建的软件——但还有其他实现。尽管 CPython 用 C 编程语言编写，Jython 是用 Java 编写的，用于运行与 Java 程序互操作的 Python 脚本。PyPy 是一个即时编译器，用于在程序执行时编译 Python，其本身是用 Python 编写的。
