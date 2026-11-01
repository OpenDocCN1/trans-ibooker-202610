
虽然这看起来像一个函数调用 ①，但它实际上是一个带有圆括号包裹的字符串值的打印语句，就像赋值 spam = (2 + 2) 等价于 spam = 2 + 2 一样。在 Python 2 和 3 中，你可以分别向 print 语句或 print() 函数传递多个值。在 Python 3 中，这看起来如下：

```
>>> print('Hello', 'world') # run in Python 3
Hello world
```

但是在 Python 2 中使用相同的代码会被解释为在打印语句中传递两个字符串值的元组，产生以下输出：

```
>>> print('Hello', 'world') # run in Python 2
('Hello', 'world')
```

语句和由函数调用组成的表达式存在细微但真实的差异。

## 块 vs. 子句 vs. 主体

术语 block、clause 和 body 常常可以互换使用，指代一组 Python 指令。块以缩进开始，并在缩进返回到之前的缩进级别时结束。例如，跟在 if 或 for 语句之后的代码被称为该语句的块。以冒号结尾的语句之后需要一个新块，例如 if、else、for、while、def、class 等。

但 Python 确实允许单行块。这是一种有效但不推荐的 Python 语法：

```
if name == 'Zophie': print('Hello, kitty!')
```

通过使用分号，你也可以在 if 语句的块中拥有多条指令：

```
if name == 'Zophie': print('Hello, kitty!'); print('Do you want a treat?')
```

但你不能对需要新块的其他语句使用单行形式。以下不是有效的 Python 代码：

```
if name == 'Zophie': if age < 2: print('Hello, kitten!')
```

if 语句是子句头，而嵌套在 if 中的两个 print() 调用是子句套件或主体。官方 Python 文档使用 block 来指代作为一个单元执行的 Python 代码片段，例如模块、函数或类定义 (https://docs.python.org/3/reference/executionmodel.html)。

## 变量 vs. 属性

变量只是引用对象的名称。属性是，引用官方文档的话，“any name following a dot” (https://docs.python.org/3/tutorial/classes.html#python-scopes-and-namespaces)。属性与对象关联（点号/句点前的名称）。例如，在交互式 shell 中输入以下内容：

```
>>> import datetime
>>> spam = datetime.datetime.now()
>>> spam.year
2018
>>> spam.month
1
```

在此代码示例中，spam 是一个变量，包含一个 datetime 对象（由 datetime.datetime.now() 返回），而 year 和 month 是该对象的属性。即使在 sys.exit() 这样的情况下，exit() 函数也被视为 sys 模块对象的属性。

其他语言将属性称为字段、属性或成员变量。

## 函数 vs. 方法

函数是一段在被调用时运行的代码。方法是一个与类关联的函数（或可调用对象，在下一节描述），就像属性是与对象关联的变量一样。函数包括内置函数或与模块关联的函数。例如，在交互式 shell 中输入以下内容：

```
>>> len('Hello')
5
>>> 'Hello'.upper()
'HELLO'
>>> import math
>>> math.sqrt(25)
5.0
```

在此示例中，len() 是一个函数，而 upper() 是字符串方法。方法也被视为与其关联的对象的属性。请注意，句点并不一定意味着你处理的是方法而不是函数。sqrt() 函数与 math 关联，而 math 是一个模块，而不是类。

## 可迭代对象 vs. 迭代器

Python 的 for 循环非常灵活。语句 for i in range(3): 将运行一段代码三次。range(3) 调用不仅仅是 Python 告诉 for 循环“重复执行某段代码三次”的方式。调用 range(3) 返回一个 range 对象，就像调用 list('cat') 返回一个 list 对象一样。这两种对象都是可迭代对象（或简称为可迭代）的例子。

你在 for 循环中使用可迭代对象。输入以下内容到交互式 shell 中，查看 for 循环如何遍历 range 对象和 list 对象：

```
>>> for i in range(3):
...   print(i) # body of the for loop
...
0
1
2
>>> for i in ['c', 'a', 't']:
...   print(i) # body of the for loop
...
c
a
t
```

可迭代对象还包括所有序列类型，例如 range、list、tuple 和字符串对象，但也包括一些容器对象，例如字典、集合和文件对象。

然而，在这些 for 循环示例中，幕后还有更多操作。在幕后，Python 为 for 循环调用内置的 iter() 和 next() 函数。当在 for 循环中使用时，可迭代对象会传递给内置的 iter() 函数，该函数返回迭代器对象。虽然可迭代对象包含项目，但迭代器对象会跟踪循环中下一个要使用的项目。在循环的每次迭代中，迭代器对象会传递给内置的 next() 函数以返回可迭代对象中的下一个项目。我们可以手动调用 iter() 和 next() 函数，以直接了解 for 循环的工作方式。输入以下内容到交互式 shell 中，以执行与之前循环示例相同的指令：

```
>>> iterableObj = range(3)
>>> iterableObj
range(0, 3)
>>> iteratorObj = iter(iterableObj)
>>> i = next(iteratorObj)
>>> print(i) # body of the for loop
0
>>> i = next(iteratorObj)
>>> print(i) # body of the for loop
1
>>> i = next(iteratorObj)
>>> print(i) # body of the for loop
2
>>> i = next(iteratorObj)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

请注意，如果你在可迭代对象的最后一个项目已被返回后调用 next()，Python 会引发 StopIteration 异常 1。Python 的 for 循环会捕获此异常以知道何时停止循环，而不是用此错误消息导致程序崩溃。

迭代器只能遍历可迭代对象中的项目一次。这类似于你只能使用 open() 和 readlines() 读取文件内容一次，然后必须重新打开文件才能再次读取其内容。如果你想再次遍历可迭代对象，必须再次调用 iter() 以创建另一个迭代器对象。你可以创建任意数量的迭代器对象；每个对象都会独立跟踪其应返回的下一个项目。输入以下内容到交互式 shell 中，查看其工作原理：

```
>>> iterableObj = list('cat')
>>> iterableObj
['c', 'a', 't']
>>> iteratorObj1 = iter(iterableObj)
>>> iteratorObj2 = iter(iterableObj)
>>> next(iteratorObj1)
'c'
>>> next(iteratorObj1)
'a'
>>> next(iteratorObj2)
'c'
```

请记住，可迭代对象作为参数传递给 iter() 函数，而从 iter() 调用返回的对象是迭代器对象。迭代器对象传递给 next() 函数。当你使用类语句创建自己的数据类型时，可以实现 __iter__() 和 __next__() 特殊方法，以便在 for 循环中使用你的对象。

## 语法 vs. 运行时 vs. 语义错误

有许多方法可以对错误进行分类。但在高层次上，你可以将编程错误分为三种类型：语法错误、运行时错误和语义错误。

语法是给定编程语言中有效指令的规则集。语法错误，例如缺少括号、用句点代替逗号或其他拼写错误，会立即生成 SyntaxError。语法错误也称为解析错误，当 Python 解释器无法将源代码文本解析为有效指令时就会发生。在英语中，此错误相当于语法不正确或一串无意义的词，如“by uncontaminated cheese certainly it’s”。计算机需要特定的指令，无法读取程序员的思维来确定程序应做什么，因此带有语法错误的程序甚至无法运行。

运行时错误是指正在运行的程序未能执行某项任务，例如尝试打开不存在的文件或将数字除以零。在英语中，运行时错误相当于给出不可能的指令，如“画一个有三条边的正方形”。如果未处理运行时错误，程序将崩溃并显示追踪信息。但你可以使用 try-except 语句捕获运行时错误并运行错误处理代码。例如，在交互式 shell 中输入以下内容：

```
>>> slices = 8
>>> eaters = 0
>>> print('Each person eats', slices / eaters, 'slices.')
```

运行此代码时会显示以下追踪信息：

```
Traceback (most recent call last):
  File "<pyshell#4>", line 1, in <module>
    print('Each person eats', slices / eaters, 'slices.')
ZeroDivisionError: division by zero
```

请记住，追踪信息中提到的行号只是 Python 解释器检测到错误的位置。错误的真正原因可能在前一行代码，甚至可能在程序更早的地方。

源代码中的语法错误在程序运行前会被解释器捕获，但语法错误也可能在运行时发生。eval() 函数可以接受一段 Python 代码字符串并运行它，这可能在运行时产生 SyntaxError。例如，eval('print("Hello, world)') 缺少一个闭合的双引号，程序直到调用 eval() 时才会遇到它。

语义错误（也称为逻辑错误）是一种更微妙的错误。语义错误不会导致错误消息或崩溃，但计算机以程序员未打算的方式执行指令。在英语中，语义错误的等价物是告诉计算机：“去商店买一盒牛奶，如果他们有鸡蛋，就买一打。”计算机随后会买 13 盒牛奶，因为商店有鸡蛋。好也罢坏也罢，计算机完全按照你的指示执行。例如，在交互式 shell 中输入以下内容：

```
>>> print('The sum of 4 and 2 is', '4' + '2')
```

你会得到以下输出：

```
The sum of 4 and 2 is 42
```

显然，42 不是答案。但请注意程序没有崩溃。因为 Python 的 + 运算符会相加整数值并连接字符串值，错误地使用字符串值 '4' 和 '2' 而不是整数导致了非预期的行为。
