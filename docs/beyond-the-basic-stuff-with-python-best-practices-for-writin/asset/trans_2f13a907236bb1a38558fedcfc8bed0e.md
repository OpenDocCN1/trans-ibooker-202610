
```
>>> greetings = ['hello', 'hello', 'mello', 'yello', 'hello']
>>> newGreetings = []
>>> for word in greetings:
...     if word == 'hello':  # Copy everything that is 'hello'.
...         newGreetings.append(word)
...
>>> greetings = newGreetings  # Replace the original list.
>>> print(greetings)
['hello', 'hello', 'hello']
```

此代码执行的可视化位于 https://autobox.com/deletingloopfixed/。

请记住，因为这段代码只是一个创建列表的简单循环，你可以用列表推导式来替换它。列表推导式运行速度不会更快，也不会使用更少的内存，但输入更简短，同时不会损失太多可读性。将以下内容输入到交互式 shell 中，它等价于前一个示例中的代码：

```
>>> greetings = ['hello', 'hello', 'mello', 'yello', 'hello']
>>> greetings = [word for word in greetings if word == 'hello']
>>> print(greetings)
['hello', 'hello', 'hello']
```

列表推导式不仅更简洁，还避免了在迭代时修改列表时出现的陷阱。

## 参考、内存使用与 SYS.GETSIZEOF()

看起来创建新列表而不是修改原列表会浪费内存。但请记住，就像变量在技术上包含对值的引用而不是实际值一样，列表也包含对值的引用。前面显示的 newGreetings.append(word) 行并不是复制 word 变量中的字符串，只是复制了对字符串的引用，而引用要小得多。

你可以通过使用 sys.getsizeof() 函数来看到这一点，该函数返回传递给它的对象在内存中占用的字节数。在此交互式 shell 示例中，我们可以看到短字符串 'cat' 占用 52 字节，而更长的字符串占用 85 字节：

```
>>> import sys
>>> sys.getsizeof('cat')
52
>>> sys.getsizeof('a much longer string than just "cat"')
85
```

（在我使用的 Python 版本中，字符串对象的开销占用 49 字节，而字符串中的每个实际字符占用 1 字节。）但包含其中任一字符串的列表占用 72 字节，无论字符串有多长：

```
>>> sys.getsizeof(['cat'])
72
>>> sys.getsizeof(['a much longer string than just "cat"'])
72
```

原因在于列表在技术上并不包含字符串，而是只包含对字符串的引用，而引用的大小与所引用数据的大小无关。像 newGreetings.append(word) 这样的代码并不是复制 word 中的字符串，而是复制对字符串的引用。如果你想知道一个对象以及它所引用的所有对象占用多少内存，Python 核心开发者 Raymond Hettinger 为此编写了一个函数，你可以在 https://code.activestate.com/recipes/577504-compute-memory-footprint-of-an-object-and-its-cont/ 访问。

因此，你不必觉得通过创建新列表而不是在迭代时修改原列表而浪费内存。即使你的修改列表的代码似乎有效，它也可能是导致难以发现和修复的微妙错误的原因。浪费程序员的时间比浪费计算机内存昂贵得多。

虽然你不应该在迭代时从列表（或任何可迭代对象）中添加或删除项目，但修改列表的内容是可以的。例如，假设我们有一个字符串形式的数字列表：['1', '2', '3', '4', '5']。我们可以在迭代列表的同时将这个字符串列表转换为整数列表 [1, 2, 3, 4, 5]：

```
>>> numbers = ['1', '2', '3', '4', '5']
>>> for i, number in enumerate(numbers):
...     numbers[i] = int(number)
...
>>> numbers
[1, 2, 3, 4, 5]
```

此代码执行的可视化位于 https://autobox.com/convertstringnumbers。修改列表中的项目是可以的；改变列表中的项目数量才是容易出错的。

另一种安全地在列表中添加或删除项目的方法是从列表末尾向开头反向迭代。这样，你可以在迭代时从列表中删除项目，或者只要将项目添加到列表末尾就可以向列表中添加项目。例如，输入以下代码，它会从 someInts 列表中删除偶数整数。

```
>>> someInts = [1, 7, 4, 5]
>>> for i in range(len(someInts)):
...     if someInts[i] % 2 == 0:
...         del someInts[i]
...
Traceback (most recent call last):
    File "<stdin>", line 2, in <module>
IndexError: list index out of range
>>> someInts = [1, 7, 4, 5]
>>> for i in range(len(someInts) - 1, -1, -1):
...     if someInts[i] % 2 == 0:
...         del someInts[i]
...
>>> someInts
[1, 7, 5]
```

此代码之所以有效，是因为循环未来将迭代的项目索引永远不会改变。但删除值后值重复上移使得这种技术对于长列表效率低下。此代码执行的可视化位于 https://autobox.com/iteratebackwards1。你可以在图 8-3 中看到向前迭代和向后迭代的差异。

### 向前迭代

### 向后迭代

![](img/ab52fe24e511e29be73786864ebe0410_165_0.png)

图 8-3：迭代时向前（左）和向后（右）从列表中删除偶数

同样，你也可以在反向迭代列表时向列表末尾添加项目。将以下内容输入到交互式 shell 中，它会将 someInts 列表中的任何偶数整数的副本追加到列表末尾：

```
>>> someInts = [1, 7, 4, 5]
>>> for i in range(len(someInts) - 1, -1, -1):
...     if someInts[i] % 2 == 0:
...         someInts.append(someInts[i])
...
>>> someInts
[1, 7, 4, 5, 4]
```

此代码执行的可视化位于 https://autobox.com/iteratebackwards2。通过反向迭代，我们可以向列表追加项目或从列表中删除项目。但这样做正确起来可能很棘手，因为对这种基本技术的细微改动可能会引入错误。创建新列表而不是修改原列表要简单得多。正如 Python 核心开发者 Raymond Hettinger 所说：

Q. 修改列表时在循环中进行的最佳实践是什么？
A. 不要。

## 不要在没有 copy.copy() 和 copy.deepcopy() 的情况下复制可变值

最好将变量视为引用对象的标签或名牌，而不是包含对象的盒子。这种心智模型在修改可变对象时尤其有用：可变对象是指值可以突变（即改变）的对象，例如列表、字典和集合。常见的陷阱是复制引用可变对象的变量到另一个变量，并认为实际对象被复制了。在 Python 中，赋值语句永远不会复制对象；它们只复制对对象的引用。（Python 开发者 Ned Batchelder 有一个关于这个想法的精彩 PyCon 2015 演讲，标题为“Facts and Myths about Python Names and Values”。在 https://youtu.be/_AEJHKgK9ns 观看。）

例如，将以下代码输入到交互式 shell 中，注意即使我们只更改 spam 变量，cheese 变量也会发生变化：

```
>>> spam = ['cat', 'dog', 'eel']
>>> cheese = spam
>>> spam
['cat', 'dog', 'eel']
>>> cheese
['cat', 'dog', 'eel']
>>> spam[2] = 'MOOSE'
>>> spam
['cat', 'dog', 'MOOSE']
>>> cheese
['cat', 'dog', 'MOOSE']
>>> id(cheese), id(spam)
2356896337288, 2356896337288
```

此代码执行的可视化位于 https://autobox.com/listcopygotcha1。如果你认为 cheese = spam 复制了列表对象，你可能会惊讶于 cheese 似乎发生了变化，即使我们只修改了 spam。但赋值语句永远不会复制对象，只会复制对对象的引用。赋值语句 cheese = spam 使 cheese 引用计算机内存中与 spam 相同的列表对象。它不会复制列表对象。这就是为什么更改 spam 也会更改 cheese：两个变量引用同一个列表对象。

同样的原则也适用于传递给函数调用的可变对象。将以下内容输入到交互式 shell 中，注意全局变量 spam 和局部参数（记住，参数是在函数的 def 语句中定义的变量）theList 都引用同一个对象：

```
>>> def printIdOfParam(theList):
...     print(id(theList))
...
>>> eggs = ['cat', 'dog', 'eel']
>>> print(id(eggs))
2356893256136
>>> printIdOfParam(eggs)
2356893256136
```

此代码执行的可视化位于 https://autobox.com/listcopygotcha2。请注意，eggs 和 theList 返回的 id() 标识相同，这意味着这些变量引用同一个列表对象。eggs 变量的列表对象并未复制到 theList；复制的是引用，这就是为什么两个变量引用同一个列表。引用只有几个字节大小，但想象一下如果 Python 复制整个列表而不是只复制引用。如果 eggs 包含十亿个项目而不是仅仅三个，将其传递给 printIdOfParam() 函数就需要复制这个巨大的列表。这只是一个简单的函数调用就会消耗数 GB 的内存！这就是为什么 Python 赋值只复制引用，从不复制对象。

防止这种陷阱的一种方法是使用 copy.copy() 函数制作列表对象的副本（而不是仅仅引用）。将以下内容输入到交互式 shell 中：

```
>>> import copy
>>> bacon = [2, 4, 8, 16]
>>> ham = copy.copy(bacon)
>>> id(bacon), id(ham)
(2356896337352, 2356896337480)
>>> bacon[0] = 'CHANGED'
>>> bacon
['CHANGED', 4, 8, 16]
>>> ham
[2, 4, 8, 16]
>>> id(bacon), id(ham)
(2356896337352, 2356896337480)
```

此代码执行的可视化位于 https://autobox.com/copycopy1。ham 变量引用的是复制的列表对象，而不是 bacon 引用的原始列表对象，因此它不会受到这种陷阱的影响。

但正如变量更像是标签或名牌而不是包含对象的盒子一样，列表也包含引用对象的标签或名牌
