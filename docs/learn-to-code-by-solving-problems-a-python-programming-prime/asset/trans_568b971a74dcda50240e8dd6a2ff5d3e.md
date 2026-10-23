

# 概念检测

以下哪项最适合使用字典而不是列表或集合？

+   A. 人们完成比赛的顺序
+   B. 食谱所需的食材
+   C. 国家名称及其首都城市
+   D. 50个随机整数

答案：C。这是唯一包含键值映射关系的选项。这里，键可以是国家名称，值可以是它们的首都城市。

# 概念检测

以下字典中值的类型是什么（忽略键）？

```
{'MLB': {'Bluejays': [1992, 1993],
'Orioles': [1966, 1970, 1983]},
'NFL': {'Patriots': ['too many']}}
```

+   A. 整数
+   B. 字符串
+   C. 列表
+   D. 字典
+   E. 以上不止一种

有时使用 `get` 方法比使用索引查找键对应的值更方便。即使键不存在，`get` 方法也永远不会产生错误：

```
>>> print(d.get('cut'))
4
>>> print(d.get('storm'))
None
```

如果键存在，get 返回其值。否则，它返回 None 表示该键不存在。

除了查找键对应的值，我们还可以使用方括号向字典中添加键或更改键映射的值。以下代码展示了如何执行这些操作，从空字典开始：

```
>>> d = {}
>>> d['gully'] = 1
>>> d
{'gully': 1}
>>> d['cut'] = 1
>>> d
{'gully': 1, 'cut': 1}
>>> d['cut'] = 4
>>> d
{'gully': 1, 'cut': 4}
>>> d['gully'] = d['gully'] + 1
>>> d
{'gully': 2, 'cut': 4}
>>> d['gully'] = d['gully'] + 1
>>> d
{'gully': 3, 'cut': 4}
```

# 概念检测

使用 help({}.get) 了解更多关于字典 get 方法的信息。

以下代码的输出是什么？

```
d = {3: 4}
d[5] = d.get(4, 8)
```

字典有一些方法，允许我们访问键、值或两者。keys 方法给我们键，values 方法给我们值：

```
>>> d.keys()
dict_keys(['cut', 'gully', 'storm', 'brook'])
>>> d.values()
dict_values([4, 3, 2, 2])
```

这些不是列表，但我们可以将它们传递给 list 进行转换：

```
>>> keys = list(d.keys())
>>> keys
['cut', 'gully', 'storm', 'brook']
>>> values = list(d.values())
>>> values
[4, 3, 2, 2]
```

有了可用列表形式的键，我们可以对键进行排序，然后按排序顺序循环遍历它们：

```
>>> keys.sort()
>>> keys
['brook', 'cut', 'gully', 'storm']
>>> for word in keys:
...     print('key', word, 'has value', d[word])
...
key brook has value 2
key cut has value 4
key gully has value 3
key storm has value 2
```

我们也可以循环遍历值：

```
>>> for num in d.values():
...     print('number', num)
...
number 4
number 3
number 2
number 2
```

循环遍历键通常比循环遍历值更受青睐。从键到其值很容易。正如我们将在下一小节中看到的，从值返回其键则不那么容易。

最后一个相关的方法是 items。它让我们同时访问键和值：

```
>>> pairs = list(d.items())
>>> pairs
[('cut', 4), ('gully', 3), ('storm', 2), ('brook', 2)]
```

这为我们提供了另一种循环遍历字典键值对的方法：

```
>>> for pair in pairs:
...   print('key', pair[0], 'has value', pair[1])
...
key cut has value 4
key gully has value 3
key storm has value 2
key brook has value 2
```

仔细看 pairs 的值：

```
>>> pairs
[('cut', 4), ('gully', 3), ('storm', 2), ('brook', 2)]
```

这里有点可疑：每个内部值周围是圆括号，而不是方括号。事实证明，这不是列表的列表，而是元组的列表：

```
>>> type(pairs[0])
<class 'tuple'>
```

元组与列表相似，都存储一系列值。元组和列表最重要的区别在于元组是不可变的。你可以循环遍历它们、索引它们、切片它们，但不能修改它们。如果尝试修改元组，会出错：

```
>>> pairs[0][0] = 'river'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
```

你可以使用圆括号创建自己的元组。对于具有单个值的元组，需要尾随逗号。对于具有多个值的元组，则不需要：

```
>>> (4,)
(4,)
>>> (4, 5)
(4, 5)
>>> (4, 5, 6)
(4, 5, 6)
```

元组也有方法——但只有少数几个，因为会更改元组的方法是不允许的。如果你感兴趣，我鼓励你了解更多关于元组的知识，但在本书中我们不会进一步使用元组。

# 反向字典

我们离使用字典解决“常用词”问题越来越近了。这是计划。我们维护一个将单词映射到其出现次数的字典。每当我们处理一个单词时，我们检查该单词是否已在字典中。如果不在，则将其值设为 1 添加进去。如果在，则将其值增加 1。

以下是添加两个单词的示例，一个是我们见过的，一个是我们没见过的：

```
>>> d = {'storm': 1, 'cut': 1, 'magma': 1}
>>> word = 'cut' # 'cut' 已在字典中
>>> if not word in d:
...     d[word] = 1
... else:
...     d[word] = d[word] + 1
...
>>> d
{'storm': 1, 'cut': 2, 'magma': 1}
>>> word = 'brook' # 'brook' 不在字典中
>>> if not word in d:
...     d[word] = 1
... else:
...     d[word] = d[word] + 1
...
>>> d
{'storm': 1, 'cut': 2, 'magma': 1, 'brook': 1}
```

字典使得从键到值的映射变得容易。例如，给定键 'brook'，我们可以轻松地查找值 1：

```
>>> d['brook']
1
```

参考表 8-1，这就像从左列中的单词到右列中其出现次数的映射。然而，这并不能直接告诉我们具有指定出现次数的单词。我们真正需要能够做的是从右列到左列，从出现次数到单词。然后我们就能将出现次数从多到少排序，以找到我们需要的单词。

也就是说，我们需要从这种字典：

```
{'storm': 2, 'cut': 4, 'magma': 1, 'brook': 2, 'gully': 3, 'cliff': 1, 'blast': 1}
```

转换为这种，*反向字典*：

```
{2: ['storm', 'brook'], 4: ['cut'], 1: ['magma', 'cliff', 'blast'], 3: ['gully']}
```

原始字典将字符串映射到数字。反向字典将数字映射到字符串。嗯，不完全是：反向字典将数字映射到字符串的*列表*。请记住，每个键在字典中只允许出现一次。在反向字典中，我们需要将每个键映射到多个值，因此我们将所有这些值存储在一个列表中。

要反转字典，每个键变成值，每个值变成键。如果反向字典中尚不存在某个键，我们为它的值创建一个列表。如果反向字典中已存在某个键，我们则将它的值添加到列表中。

我们现在可以编写一个函数来返回字典的反向版本。代码见清单 8-5。

```
def invert_dictionary(d):
    """
    d 是一个将字符串映射到数字的字典。

    返回 d 的反向字典。
    """
    inverted = {}
    for key in d:
        num = d[key]
```

```
if not num in inverted:
    ③ inverted[num] = [key]
else:
    ④ inverted[num].append(key)
return inverted
```

清单 8-5：反转字典

我们对字典 d ① 使用 for 循环，这给我们每个键。我们对 d 进行索引以获取此键映射到的值 ②。然后我们将此键值对添加到反向字典中。如果 num 还不是反向字典中的键，我们则添加它并使其映射到 d ③ 中的关联键。如果 num 已经是反向字典中的键，则它的值已是一个列表。因此，我们可以使用 append 将 d 中的键作为另一个值添加 ④。

将我们的 invert_dictionary 函数代码输入 Python shell。让我们试一试：

```
>>> d = {'a': 1, 'b': 1, 'c': 1}
>>> invert_dictionary(d)
{1: ['a', 'b', 'c']}
>>> d = {'storm': 2, 'cut': 4, 'magma': 1, 'brook': 2,
...     'gully': 3, 'cliff': 1, 'blast': 1}
>>> invert_dictionary(d)
{2: ['storm', 'brook'], 4: ['cut'], 1: ['magma', 'cliff', 'blast'],
3: ['gully']}
```

现在我们准备使用反向字典来解决“常用词”问题。

# 解决问题

如果你想要更多自顶向下设计的练习，你可能希望在继续之前自己解决问题。为了篇幅，我在此不遵循自顶向下设计的步骤。而是，我将完整地展示解决方案，然后我们讨论每个函数及其用法。

# 代码

解决方案在清单 8-6 中。

