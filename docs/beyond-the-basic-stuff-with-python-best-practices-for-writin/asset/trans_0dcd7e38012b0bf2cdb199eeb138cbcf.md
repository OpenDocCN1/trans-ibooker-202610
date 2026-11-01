
变量 spam 和 eggs 引用同一个字典对象 ①，因此它们的身份和值相同。但 bacon 引用的是一个单独的字典对象 ②，尽管它包含的数据与 spam 和 eggs 相同。相同的数据意味着 bacon 的值与 spam 和 eggs 相同，但它们是两个不同的对象，具有两个不同的身份。

## 项

在 Python 中，位于容器对象（如列表或字典）内部的对象也称为项或元素。例如，列表 ['dog', 'cat', 'moose'] 中的字符串是对象，但也称为项。

## 可变与不可变

如前所述，Python 中的所有对象都有值、数据类型和身份，其中只有值可以改变。如果可以更改对象的值，则它是可变对象。如果不能更改其值，则它是不可变对象。表 7-2 列出了 Python 中的一些可变和不可变数据类型。

表 7-2：Python 中的一些可变和不可变数据类型

| 可变数据类型 | 不可变数据类型 |
|---|---|
| 列表 | 整数 |
| 字典 | 浮点数 |
| 集合 | 布尔值 |
| 字节数组 | 字符串 |
| 数组 | 冻结集合 |
|  | 字节 |
|  | 元组 |

当你覆盖一个变量时，看起来像是你在更改对象的值，如以下交互式 shell 示例所示：

```
>>> spam = 'hello'
>>> spam
'hello'
>>> spam = 'goodbye'
>>> spam
'goodbye'
```

但在这段代码中，你并没有将 'hello' 对象的值从 'hello' 改为 'goodbye'。它们是两个独立的对象。你只是让 spam 从引用 'hello' 对象切换到引用 'goodbye' 对象。你可以通过使用 id() 函数来显示这两个对象的身份来检查这一点是否正确：

```
>>> spam = 'hello'
>>> id(spam)
40718944
>>> spam = 'goodbye'
>>> id(spam)
40719224
```

这两个字符串对象具有不同的身份（40718944 和 40719224），因为它们是不同的对象。但引用可变对象的变量的值可以在原地修改。例如，将以下内容输入交互式 shell：

```
>>> spam = ['cat', 'dog']
>>> id(spam)
33805576
① >>> spam.append('moose')
② >>> spam[0] = 'snake'
>>> spam
['snake', 'dog', 'moose']
>>> id(spam)
33805576
```

append() 方法 ① 和通过索引进行项赋值 ② 都在原地修改列表的值。尽管列表的值已更改，其身份仍然相同（33805576）。但当你使用 + 运算符连接列表时，你会创建一个新对象（具有新的身份）来覆盖旧列表：

```
>>> spam = spam + ['rat']
>>> spam
['snake', 'dog', 'moose', 'rat']
>>> id(spam)
33840064
```

列表连接会创建一个具有新身份的新列表。此时，旧列表最终将被垃圾回收器从内存中释放。你需要查阅 Python 文档来了解哪些方法和操作在原地修改对象，哪些会覆盖对象。需要记住的一个好规则是，如果你在源代码中看到一个字面量，例如前一个示例中的 ['rat']，Python 很可能创建一个新对象。对对象调用的方法，例如 append()，通常会在原地修改对象。

对于整数、字符串或元组等不可变数据类型的对象，赋值更简单。例如，将以下内容输入交互式 shell：

```
>>> bacon = 'Goodbye'
>>> id(bacon)
33827584
① >>> bacon = 'Hello'
>>> id(bacon)
33863820
② >>> bacon = bacon + ', world!'
>>> bacon
'Hello, world!'
>>> id(bacon)
33870056
③ >>> bacon[0] = 'J'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

字符串是不可变的，因此你无法更改其值。尽管看起来 bacon 中的字符串值正从 'Goodbye' 改为 'Hello' ①，实际上它是由具有新身份的新字符串对象覆盖的。同样，使用字符串连接的表达式会创建具有新身份的新字符串对象 ②。在 Python 中，使用项赋值尝试原地修改字符串是不允许的 ③。

元组的值由其包含的对象及其顺序定义。元组是不变序列对象，用括号括住值。这意味着元组中的项不能被覆盖：

```
>>> eggs = ('cat', 'dog', [2, 4, 6])
>>> id(eggs)
39560896
>>> id(eggs[2])
40654152
>>> eggs[2] = eggs[2] + [8, 10]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
```

但不可变元组内部的可变列表仍然可以在原地修改：

```
>>> eggs[2].append(8)
>>> eggs[2].append(10)
>>> eggs
('cat', 'dog', [2, 4, 6, 8, 10])
>>> id(eggs)
39560896
>>> id(eggs[2])
40654152
```

尽管这是一个晦涩的特殊情况，但很重要，需要记住。如图 7-3 所示，元组仍然引用相同的对象。但如果元组包含一个可变对象并且该对象更改了其值——也就是说，如果对象发生变异——则元组的值也会改变。

我，以及几乎所有 Pythonista，都称元组为不可变。但某些元组是否可以称为可变，取决于你的定义。我在我的 PyCascades 2019 演讲 “The Amazing Mutable, Immutable Tuple” 中更深入地探讨了这个主题，网址为 https://invpy.com/amazingtuple/。你也可以阅读 Luciano Ramalho 在《Fluent Python》第 2 章中的解释。（O'Reilly Media, 2015）

![](img/ab52fe24e511e29be73786864ebe0410_143_0.png)

图 7-3：尽管元组中对象的集合是不可变的，但对象本身可以是可变的。

## 索引、键和哈希

Python 列表和字典是可以包含多个其他值的数值。要访问这些值，你使用索引运算符，它由一对方括号 ([ ]) 和一个称为索引的整数组成，以指定要访问哪个值。输入以下内容到交互式 shell 以查看列表的索引是如何工作的：

```
>>> spam = ['cat', 'dog', 'moose']
>>> spam[0]
'cat'
>>> spam[-2]
'dog'
```

在此示例中，0 是一个索引。第一个索引是 0，而不是 1，因为 Python（像大多数语言一样）使用零基索引。使用一基索引的语言很少见：Lua 和 R 是最主要的。Python 还支持负索引，其中 -1 指列表中的最后一项，-2 指倒数第二项，以此类推。你可以将负索引 spam[-n] 视为与 spam[len(spam) - n] 相同。

> 计算机科学家兼创作歌手 Stan Kelly-Bootle 曾开玩笑说，“Should array indices start at 0 or 1? My compromise of 0.5 was rejected without, I thought, proper consideration。”

你也可以对列表字面量使用索引运算符，尽管所有这些方括号在实际代码中看起来可能令人困惑且不必要：

```
>>> ['cat', 'dog', 'moose'][2]
'moose'
```

索引也可以用于列表之外的数值，例如对字符串进行索引以获取单个字符：

```
>>> 'Hello, world'[0]
'H'
```

Python 字典按键值对组织：

```
>>> spam = {'name': 'Zophie'}
>>> spam['name']
'Zophie'
```

尽管列表索引仅限于整数，Python 字典的索引运算符是键，可以是任何可哈希对象。哈希是一个整数，充当值的某种指纹。对象的哈希在其生命周期内永不改变，并且具有相同值的对象必须具有相同的哈希。在此实例中，字符串 'name' 是值 'Zophie' 的键。hash() 函数如果对象可哈希，将返回对象的哈希。不可变对象，例如字符串、整数、浮点数和元组，可以是可哈希的。列表（以及其他可变对象）不可哈希。输入以下内容到交互式 shell：

```
>>> hash('hello')
-1734230105925061914
>>> hash(42)
42
>>> hash(3.14)
322818021289917443
>>> hash((1, 2, 3))
2528502973977326415
>>> hash([1, 2, 3])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'list' object is unhashable
```

尽管细节超出了本书的范围，但键的哈希用于查找存储在字典和集合数据结构中的项。这就是为什么你不能使用可变列表作为字典的键：

```
>>> d = {}
>>> d[[1, 2, 3]] = 'some value'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'list' object is unhashable
```

哈希与身份不同。具有相同值的两个不同对象将具有不同的身份但相同的哈希。例如，输入以下内容到交互式 shell：

>>> a = ('cat', 'dog', 'moose')
>>> b = ('cat', 'dog', 'moose')
>>> id(a), id(b)
