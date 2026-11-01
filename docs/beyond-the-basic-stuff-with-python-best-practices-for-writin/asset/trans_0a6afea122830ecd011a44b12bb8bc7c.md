
## 第 1 部分
开始使用

## 1
## 处理错误与寻求帮助

![](img/ab52fe24e511e29be73786864ebe0410_29_0.png)

请不要拟人化计算机；它们觉得这非常烦人。当计算机向你展示错误信息时，并不是因为你冒犯了它。计算机是我们大多数人会交互的最复杂的工具，但归根结底，它们只是工具。

即便如此，很容易责怪这些工具。因为学习编程很大程度上是自学的，即使你已经学习 Python 数月，每天仍需多次查阅互联网，也很常见会觉得自己像个失败者。但即使是专业的软件开发者，也会搜索互联网或查阅文档来回答编程问题。

除非你有财力或社会资源聘请私人导师来回答你的编程问题，否则你只能依靠计算机、互联网搜索引擎和自己的毅力。幸运的是，你的问题几乎肯定已经被问过。作为程序员，能够自行找到答案的能力远比任何算法或数据结构知识更重要。本章将指导你培养这一关键技能。

## 如何理解 Python 错误信息

当程序员面对错误信息那一大堆技术术语时，很多人第一反应是完全忽略它。但在这个错误信息内部，就包含了程序出了什么问题的答案。找到这个答案是一个两步过程：检查追溯信息并在互联网上搜索错误信息。

## 检查追溯信息

当代码引发了一个 except 语句未处理的异常时，Python 程序会崩溃。发生这种情况时，Python 会显示异常的消息和追溯信息。也称为堆栈跟踪，追溯信息显示异常在程序中发生的位置以及导致它的函数调用轨迹。

为了练习阅读追溯信息，输入以下有错误的程序并保存为 abcTraceback.py。行号仅供参考，不是程序的一部分。

```
1. def a():
2.     print('Start of a()')
3.     b() # Call b().
4.
5. def b():
6.     print('Start of b()')
7.     c() # Call c().
8.
9. def c():
10.    print('Start of c()')
11.    42 / 0 # This will cause a zero divide error.
12.
13. a() # Call a().
```

在此程序中，a() 函数调用 b() ①，后者调用 c() ②。在 c() 内部，42 / 0 表达式 ③ 导致零除错误。运行此程序时，输出应如下所示：

```
Start of a()
Start of b()
Start of c()
Traceback (most recent call last):
    File "abcTraceback.py", line 13, in <module>
        a()  # Call a().
    File "abcTraceback.py", line 3, in a
        b()  # Call b().
    File "abcTraceback.py", line 7, in b
        c()  # Call c().
```

```
File "abcTraceback.py", line 11, in c
    42 / 0 # This will cause a zero divide error.
ZeroDivisionError: division by zero
```

让我们逐行检查这个追溯信息，从这一行开始：

```
Traceback (most recent call last):
```

这条消息让你知道接下来的是追溯信息。most recent call last 文本表示每个函数调用都按顺序列出，从第一个函数调用开始，到最近的一个结束。

下一行显示追溯信息的第一个函数调用：

```
File "abcTraceback.py", line 13, in <module>
    a()  # Call a().
```

这两行是帧摘要，它们显示了帧对象内部的信息。当函数被调用时，局部变量数据以及函数调用结束后返回代码的位置都会存储在帧对象中。帧对象保存与函数调用相关的局部变量和其他数据。帧对象在函数被调用时创建，在函数返回时销毁。追溯信息为导致崩溃的每个帧显示一个帧摘要。我们可以看到这个函数调用在 abcTraceback.py 的第 13 行，<module> 文本告诉我们这一行在全局作用域中。下一行以两个空格缩进显示第 13 行。

接下来的四行是下一个两个帧摘要：

```
File "abcTraceback.py", line 3, in a
    b()  # Call b().
File "abcTraceback.py", line 7, in b
    c()  # Call c().
```

我们可以从 line 3, in a 文本看出 b() 在 a() 函数的第 3 行被调用，这导致 c() 在 b() 函数的第 7 行被调用。请注意，尽管 print() 调用在函数调用发生前已运行，但第 2、6 和 10 行的 print() 调用不会显示在追溯信息中。只有导致异常的函数调用所在的行才会显示在追溯信息中。

最后一个帧摘要显示导致未处理异常的行，后面跟着异常的名称和异常的消息：

```
File "abcTraceback.py", line 11, in c
    42 / 0 # This will cause a zero divide error.
ZeroDivisionError: division by zero
```

请注意，追溯信息给出的行号是 Python 最终检测到错误的位置。错误的真正来源可能在这一行之前。

错误信息 notoriously 简短且难以理解：除非你知道除以零在数学上是不可能的并且是常见的软件错误，否则 division by zero 这三个词对你来说毫无意义。在此程序中，错误并不难找到。查看帧摘要中的代码行，很清楚 42 / 0 代码中零除错误发生的位置。

但让我们看一个更困难的例子。将以下代码输入到文本编辑器中并保存为 zeroDivideTraceback.py：

```
def spam(number1, number2):
    return number1 / (number2 - 42)

spam(101, 42)
```

运行此程序时，输出应如下所示：

```
Traceback (most recent call last):
    File "zeroDivideTraceback.py", line 4, in <module>
    spam(101, 42)
    File "zeroDivideTraceback.py", line 2, in spam
    return number1 / (number2 - 42)
ZeroDivisionError: division by zero
```

错误信息相同，但 return number1 / (number2 - 42) 中的零除就不那么明显了。你可以从 / 运算符推断出发生了除法，并且表达式 (number2 - 42) 必须求值为 0。这会让你得出结论：当 number2 参数设置为 42 时，spam() 函数就会失败。

有时追溯信息可能会指出错误在 bug 真正原因的下一行。例如，在以下程序中，第一行缺少闭合括号：

```
print('Hello.'
print('How are you?')
```

但此程序的错误信息指出问题出在第二行：

```
File "example.py", line 2
    print('How are you?')
    ^
SyntaxError: invalid syntax
```

原因是 Python 解释器直到读取第二行时才注意到语法错误。追溯信息可以指出问题发生的位置，但这并不总是 bug 实际原因所在的位置。如果帧摘要没有提供足够的信息来找出 bug，或者 bug 的真正原因在追溯信息未显示的上一行，你将不得不使用调试器逐步执行程序或检查任何日志消息来找到原因。这可能需要大量时间。搜索错误信息的互联网可能会更快地为你提供有关解决方案的关键线索。

## 搜索错误信息

通常，错误信息非常简短，甚至不是完整的句子。因为程序员经常遇到它们，所以它们旨在作为提醒而不是完整解释。如果你第一次遇到错误信息，将其复制粘贴到互联网搜索中通常会返回有关错误含义及其可能原因的详细解释。图 1-1 显示了搜索 python “ZeroDivisionError: division by zero” 的结果。在错误信息周围加上引号有助于找到确切的短语，添加单词 python 也能缩小搜索范围。

![](img/ab52fe24e511e29be73786864ebe0410_33_0.png)

图 1-1：将错误信息复制粘贴到互联网搜索工具中可以快速提供解释和解决方案。

搜索错误信息并不算作弊。没人期望记住编程语言的每一种可能的错误信息。专业软件开发者每天都会在互联网上搜索编程答案。

你可能希望排除错误信息中特定于你代码的部分。例如，考虑以下错误信息：

```
>>> print(employeeRecord)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
```
