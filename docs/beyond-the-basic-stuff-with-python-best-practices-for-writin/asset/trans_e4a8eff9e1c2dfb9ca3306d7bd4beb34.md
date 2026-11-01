
## 命名空间真是个了不起的主意——让我们多用一些！

命名空间是用于防止命名冲突的标识符的独立容器。例如，内置函数 open() 和 webbrowser.open() 函数同名，但指的是不同的函数。导入 webbrowser 不会覆盖内置的 open() 函数，因为这两个 open() 函数存在于不同的命名空间中：分别是内置命名空间和 webbrowser 模块的命名空间。但请记住，扁平优于嵌套：命名空间再好，你也应该只在为防止命名冲突时才创建它们，而不是为了添加不必要的分类。

正如所有关于编程的观点一样，你可以反驳我这里列出的观点，或者它们可能与你的情况无关。争论你应该如何编写代码或什么算作“pythonic”很少像你想象的那样有成效。（除非你正在写一本充满编程观点的整本书。）

## 学会热爱显著缩进

我从其他语言转来的程序员那里听到的关于 Python 最常见的担忧是，Python 的显著缩进（常被错误地称为显著空白）很奇怪且不熟悉。代码行开头的缩进量在 Python 中有意义，因为它决定了哪些代码行属于同一个代码块。

使用缩进来分组 Python 代码块可能看起来很奇怪，因为其他语言使用大括号 { 和 } 来开始和结束代码块。但非 Python 语言的程序员通常也会缩进他们的代码块，就像 Python 程序员一样，以使代码更易读。例如，Java 编程语言没有显著缩进。Java 程序员不需要缩进代码块，但他们通常为了可读性而这样做。以下示例有一个名为 main() 的 Java 函数，其中包含对 println() 函数的单个调用：

```
// Java Example
public static void main(String[] args) {
    System.out.println("Hello, world!");
}
```

如果 println() 行没有缩进，这段 Java 代码也能正常运行，因为在大括号标记 Java 中代码块的开始和结束，而不是缩进。Python 不允许缩进是可选的，它强制你的代码始终保持可读性。但请注意，Python 没有显著空白，因为 Python 不限制你如何使用非缩进空白（2 + 2 和 2+2 都是有效的 Python 表达式）。

有些程序员认为大括号应该与打开语句在同一行，而另一些人认为它应该在下一行。程序员会争论他们偏好的风格的优点直到永远。Python 通过完全不使用大括号巧妙地回避了这个问题，让 Python 开发者可以回到更有成效的工作中。我希望所有编程语言都能采用 Python 对分组代码块的方法。

但有些人仍然渴望大括号，并希望将其添加到 Python 的未来版本中——尽管它们是多么不 Pythonic。Python 的 __future__ 模块将特性回溯移植到更早的 Python 版本中，如果你尝试将大括号特性导入 Python，你会发现一个隐藏的彩蛋：

```
>>> from __future__ import braces
SyntaxError: not a chance
```

我不会指望大括号在不久的将来被添加到 Python 中。

## 常见误用语法

如果 Python 不是你的第一门编程语言，你可能会用编写其他语言代码的相同策略来编写 Python 代码。或者，也许你因为不了解更成熟的最佳实践而学会了一种不寻常的 Python 编写方式。这种别扭的代码可以工作，但通过学习更标准的 Pythonic 代码编写方法，你可以节省一些时间和精力。本节解释程序员常犯的错误以及你应该如何改写代码。

### 改用 enumerate() 而不是 range()

在遍历列表或其他序列时，一些程序员使用 range() 和 len() 函数来生成从 0 到序列长度（不包括长度）的索引整数。在这些 for 循环中，使用变量名 i（代表索引）很常见。例如，将以下不 Pythonic 的示例输入到交互式 shell 中：

```
>>> animals = ['cat', 'dog', 'moose']
>>> for i in range(len(animals)):
...     print(i, animals[i])
...
0 cat
1 dog
2 moose
```

range(len()) 惯用法很直接，但并不理想，因为它可能难以阅读。相反，将列表或序列传递给内置的 enumerate() 函数，它将返回索引的整数和该索引处的项目。例如，你可以编写以下 Pythonic 代码。

```
>>> # Pythonic Example
>>> animals = ['cat', 'dog', 'moose']
>>> for i, animal in enumerate(animals):
...     print(i, animal)
...
0 cat
1 dog
2 moose
```

使用 enumerate() 而不是 range(len()) 编写的代码会稍微更简洁一些。如果你只需要项目而不需要索引，你仍然可以直接以 Pythonic 的方式遍历列表：

```
>>> # Pythonic Example
>>> animals = ['cat', 'dog', 'moose']
>>> for animal in animals:
...     print(animal)
...
cat
dog
moose
```

调用 enumerate() 和直接遍历序列比使用老式的 range(len()) 惯用法更可取。

### 改用 with 语句而不是 open() 和 close()

open() 函数会返回一个包含用于读取或写入文件的方法的文件对象。完成后，文件对象的 close() 方法会使文件对其他程序的读写可用。你可以单独使用这些函数。但这样做是不 Pythonic 的。例如，将以下内容输入到交互式 shell 以将文本“Hello, world!”写入名为 spam.txt 的文件：

```
>>> # Unpythonic Example
>>> fileObj = open('spam.txt', 'w')
>>> fileObj.write('Hello, world!')
13
>>> fileObj.close()
```

这样写代码可能会导致文件未关闭，例如，如果在 try 块中发生错误，程序会跳过对 close() 的调用。例如：

```
>>> # Unpythonic Example
>>> try:
...     fileObj = open('spam.txt', 'w')
...     eggs = 42 / 0  # A zero divide error happens here.
...     fileObj.close()  # This line never runs.
... except:
...     print('Some error occurred.')
...
Some error occurred.
```

到达零除错误时，执行会转移到 except 块，跳过 close() 调用并使文件保持打开状态。这可能导致以后难以追溯到 try 块的文件损坏错误。

相反，你可以使用 with 语句在执行离开 with 语句块时自动调用 close()。以下 Pythonic 示例完成了本节第一个示例中的相同任务：

```
>>> # Pythonic Example
>>> with open('spam.txt', 'w') as fileObj:
...     fileObj.write('Hello, world!')
...
```

即使没有显式的 close() 调用，with 语句在执行离开块时也会知道调用它。

### 使用 is 比较 None 而不是 ==

== 等值运算符比较两个对象的数值，而 is 恒等运算符比较两个对象的身份。第 7 章涵盖值和身份。两个对象可以存储等效的值，但作为两个独立的对象意味着它们具有独立的身份。然而，每当你将值与 None 进行比较时，你几乎总是应该使用 is 运算符而不是 == 运算符。

在某些情况下，表达式 spam == None 即使 spam 仅包含 None 也可能评估为 True。这可能是由于重载 == 运算符造成的，第 17 章更详细地介绍了这一点。但 spam is None 将检查 spam 变量中的值是否 literally 为 None。因为 None 是 NoneType 数据类型的唯一值，任何 Python 程序中只有一个 None 对象。如果变量被设置为 None，is None 比较将始终评估为 True。第 17 章描述了重载 == 运算符的具体细节，但以下是这种行为的示例：

```
>>> class SomeClass:
...     def __eq__(self, other):
...         if other is None:
...             return True
...
>>> spam = SomeClass()
>>> spam == None
True
>>> spam is None
False
```
