

如果使用相同的键再次赋值，先前的值将被替换。

```
numbers[3] = "THREE"
```

```
numbers
```

{1: 'one', 2: 'two', 3: 'THREE'}

如果 other_numbers 是另一个字典，我们可以使用双星号 "**" 创建一个新字典，将它与 numbers 合并。

```
other_numbers = {4: "four", 5: "five"}
```

```
{**numbers, **other_numbers}
```

{1: 'one', 2: 'two', 3: 'THREE', 4: 'four', 5: 'five'}

这里的 "**" 表示“将所有内容展开为一个键值对集合”。

### 练习 3.28

从一个空字典开始，遍历 numbers 和 other_numbers，创建一个包含两者所有键值对的字典。

一般来说，合并两个字典时顺序重要吗？为什么重要或不重要？

### 3.9.9 从字典中删除项

我们使用 del 从字典中删除一个键值对。

```
numbers
```

{1: 'one', 2: 'two', 3: 'THREE'}

```
del numbers[2]
numbers
```

{1: 'one', 3: 'THREE'}

### 练习 3.29

如果你尝试使用无效键从字典中删除键值对，Python 会引发什么异常？先使用 in 测试键是否有效更好，还是出现问题时捕获异常更好？

使用 clear 清空字典。

```
numbers.clear()
numbers
```

{}

### 3.9.10 字典推导式

在列表推导式中，我们在方括号表达式中使用 for 循环来构造列表。我们对字典也做同样的事情，但使用花括号。请记住，我们需要同时包含新的键和新的值。

```
{model : year for model, year
    in guitars.items() if 1950 < year and year < 1958}
```

{'Fender Stratocaster': 1954, 'Gibson Les Paul': 1952}

### 练习 3.30

这段代码中发生了什么？

```
{year : model.upper() for model, year
    in guitars.items() if 1955 < year}
```

{1958: 'GIBSON FLYING V', 1987: 'IBANEZ RG'}

如果在同一年推出了两种不同的吉他型号，会发生什么？

我的朋友决定专注于班卓琴演奏，于是以 750 欧元把他们的吉他卖给了我。我 *更新* 我的集合以反映这一变化。

```
my_brands.update(their_brands)
my_brands
```

{'Fender', 'Gibson', 'Gretsch', 'Ibanez', 'Ovation'}

### 3.10.4 从集合中删除项

为了完成与朋友的交易，我必须 *清空* 他们的集合。

```
their_brands.clear()
their_brands
```

set()

我妹妹对我很恼火，把 Ibanez 拿了回去。我把它从我的集合中 *丢弃*。

```
my_brands.discard("Ibanez")
my_brands
```

{'Fender', 'Gibson', 'Gretsch', 'Ovation'}

### 练习 3.33

如果项不在集合中，*丢弃* 会做什么？你也可以使用 *移除*。描述当你试图移除一个不是集合成员的项目时，它的行为。

假设 *my_set* 是一个集合。要理解 *my_set.clear()* 和 *my_set = set()* 之间的区别，考虑这个类比。假设你有一盒苹果。*清空* 会把盒子里所有的苹果都移除，但你保留同一个盒子。赋值会拿走你的盒子，给你一个新的空盒子。

### 3.10.5 访问集合成员

对于列表，你可以说“给我这个索引处的项。”对于字典，你可以请求“给我这个键对应的值。”对于集合，什么样的类似问题是有意义的？你如何访问成员？

首先，你可以遍历集合成员。

```
for brand in my_brands: print(brand)
```

Gretsch
Gibson
Fender
Ovation

其次，你可以弹出、移除并返回一个随机成员。

```
my_brands
my_brands.pop()
```

'Gretsch'

```
my_brands
```

{'Fender', 'Gibson', 'Ovation'}

### 练习 3.34

如果你尝试从空集合中弹出，会引发什么类型的异常？

### 3.10.6 子集和超集

让我们恢复原来的吉他收藏。

```
my_brands = my_original_brands.copy()
my_brands
```

{'Fender', 'Gibson', 'Ovation'}

```
their_brands = their_original_brands.copy()
their_brands
```

{'Fender', 'Gibson', 'Gretsch'}

你可以使用 “|” 来代替并集。这种形式的好处是你可以进行运算赋值。

```
guitars = my_brands.copy()
guitars | {"Martin", "Taylor"}
```

{'Fender', 'Gibson', 'Martin', 'Ovation', 'Taylor'}

```
guitars |= {"Martin", "Taylor"}
guitars
```

{'Fender', 'Gibson', 'Martin', 'Ovation', 'Taylor'}

交集 S₁ ∩ S₂ 是同时属于 S₁ 和 S₂ 的所有项的集合。你可以使用交集或 “&”。

```
my_brands.intersection(their_brands)
```

{'Fender', 'Gibson'}

```
my_brands & their_brands
```

{'Fender', 'Gibson'}

差集 S₁ − S₂ 是 S₁ 中不在 S₂ 中的所有项的集合。你可以使用差集或 “-”。

```
my_brands.difference(their_brands)
```

{'Ovation'}

```
my_brands - their_brands
```

{'Ovation'}

### 练习 3.35

S₁ 和 S₂ 的对称差集是 S₁ 和 S₂ 中不同时属于两者的所有项的集合。你可以使用对称差集或 “^”。

仿照上面的例子，给出对称差集的示例。如果 Python 没有提供 symmetric_difference，你会如何使用交集和差集来定义它？

## 4 与字符串同行

> *世界尽是门，尽是机会，是等待拨动的弦。*
> 
> —拉尔夫·沃尔多·爱默生

本章建立在我们在 2.2 节中关于字符串的讨论之上。如果你思考项目列表与字符串之间的相似之处，可能会有所帮助，但你必须记住 **字符串是不可变的**。

### 本章涵盖的主题

| 主题 | 页码 |
| --- | --- |
| 4.1 单引号、双引号和三引号 | 128 |
| 4.2 测试子字符串 | 129 |
| 4.3 访问字符 | 130 |
| 4.4 创建字符串 | 131 |
| 4.5 字符串与迭代 | 134 |
| 4.6 字符串与切片 | 135 |
| 4.7 字符串测试 | 136 |
| 4.8 拆分与去除 | 138 |
| 4.9 总结 | 139 |

### 4.1 单引号、双引号和三引号

你可以使用单引号或双引号来界定字符串字面量。Python 通常用单引号打印字符串。

```
"Franz"
```

'Franz'

```
'Ferdinand'
```

'Ferdinand'

如果你的字符串包含一种引号，请使用另一种引号作为分隔符，或者转义该引号。

```
"That is Franz's cat toy."
```

"That is Franz's cat toy."

```
'The ball is Ferdinand\'s.'
```

"The ball is Ferdinand's."

使用 `\n` 字符在字符串中嵌入换行。

```
"You can see the\nnewline character here."
```

'You can see the\nnewline character here.'

```
print("But 'print' breaks\nthe line at the character.")
```

But 'print' breaks
the line at the character.

对于非常长的字符串，你可以将它们连接在一起，或者使用三引号。你可以使用三个单引号或三个双引号，但 Python 代码风格指南推荐使用后者。[PEP 8]

```
"""
I've broken
this string
over several lines.
"""
```

"\nI've broken\nthis string\nover several lines.\n"

注意我不必显式使用换行符。我还无意中在开头和结尾包含了额外的换行。

```
fix = """This fixes my
    problem with extra newlines."""

fix
```

'This fixes my\n    problem with extra newlines.'

```
print(fix)
```

This fixes my
    problem with extra newlines.

Python 保留了我插入到第二行开头的额外空格。

当使用三引号分隔时，我们不需要转义字符串中的单引号或双引号。在编写代码文档时，我们会使用三引号，正如我们将在 7.8 节中看到的那样。

```
"""A single quote (') and a double quote (")."""
```

'A single quote (\') and a double quote (").'

如果要在字符串中包含反斜杠，可以用另一个反斜杠来转义它。

```
"C:\\src\\code\\chapter-01"
```

'C:\\src\\code\\chapter-01'

或者，在第一个引号前加上一个 r 来使用原始字符串。

```
r"C:\src\code\chapter-01"
```

'C:\\src\\code\\chapter-01'

```
print(r"There is no newline \n in this string.")
```

There is no newline \n in this string.

### 4.2 测试子字符串

使用 in 和 not in 来检查一个字符串的内容是否位于另一个字符串内。

```
"Quantum" in "Quantum computing"
```

True

