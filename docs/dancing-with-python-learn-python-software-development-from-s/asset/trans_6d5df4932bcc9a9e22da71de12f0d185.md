

作为 `del` 的替代方法，使用 pop 列表方法。

```
brands.pop(0)
```

'Fender'

```
brands
```

['Gibson']

```
brands.pop()
```

'Gibson'

pop 返回被移除的项，而 del 不会。如果使用 pop 且不带参数，它会返回列表非空时的最后一项。

由于我将展示几种移除项的技术，让我们创建两个具有相同吉他品牌的列表。

```
brands = [
    "Fender",
    "Gibson",
    "Ibanez"
]

original_brands = brands.copy()
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_114_0.png)

图 3.4：引用列表两个副本的变量

如果我们不知道 "Ibanez" 是否在列表中或其位置，可以使用 *index* 方法来定位第一次出现的位置。

```
```python
brands.index("Ibanez")
```

2
```

如果该项不在列表中，*index* 会引发异常：

```
```python
brands.index("Gretsch")
```

ValueError: 'Gretsch' is not in list
```

一种从列表中移除 "Ibanez" 的方法是捕获异常：

```
```python
try:
    del brands[brands.index("Ibanez")]
    print("'Ibanez' was present and was removed")
except ValueError:
    print("'Ibanez' was not present and so not removed")

brands
```

'Ibanez' was present and was removed
['Fender', 'Gibson']
```

```
```python
brands = original_brands.copy()

try:
    del brands[brands.index("Gretsch")]
    print("'Gretsch' was present and was removed")
except ValueError:
    print("'Gretsch' was not present and so not removed")

brands
```

'Gretsch' was not present and so not removed
['Fender', 'Gibson', 'Ibanez']
```

## 练习 3.4

clear 函数会从列表中移除所有项。解释以下行为之间的差异

```
a = [1, 2, 3]
b = a
a = []
print(f"{a = }")
print(f"{b = }")
```

a = []
b = [1, 2, 3]

和

```
a = [1, 2, 3]
b = a
a.clear()
print(f"{a = }")
print(f"{b = }")
```

a = []
b = []

这如何展示列表的可变性？

## 3.2.5 更改列表项

我们可以更改列表，这就是“可变”的含义。我们用来引用列表项的相同语法也可以用在赋值的左侧。

```
brands = original_brands.copy()

brands[2] = "Gretsch"
brands
```

['Fender', 'Gibson', 'Gretsch']

## 3.2.6 切片：引用子列表

切片记法是一种方便的方式来指定列表中零个或多个项。当你第一次开始切片时，它可能看起来既强大又神秘。随着时间的推移，你会欣赏其优雅性，你的舒适度会提高。

我们首先看看如何使用切片从原始列表中提取子列表。项在原始列表中可能并不相邻，但它们是从通常的或反向的项序列中挑选出来的。对于名为 the_list 的列表，切片看起来像

```
the_list[start=0 : end=len(the_list)] : step=1
```

有三个参数由冒号分隔，它们各自都有默认值。

## 练习 3.7

对于以下每个示例，遮住图 3.5 中类似的图示中的项框，以显示切片中的列表项。

![](img/4ffeafa3e3f1373f02971e43b0755ea6_119_0.png)

图 3.5：列表、索引和切片示例

填入 start、end 和 step 的值。

我现在展示几种切片列表的方法。

```
the_list = ["a", "b", "c", "d", "e", "f"]
the_list
```

['a', 'b', 'c', 'd', 'e', 'f']

- the_list[0:] 和 the_list[:] 返回 the_list 的副本。

```
the_list[0:]
```

['a', 'b', 'c', 'd', 'e', 'f']

```
the_list[:]
['a', 'b', 'c', 'd', 'e', 'f']
```

- the_list[the_index:] 返回一个新列表，其中包含 the_list 从位置 the_index 到末尾的项。如果 the_index 为正且 ≥ len(the_list)，我们得到一个空列表。

```
the_list[2:]
['c', 'd', 'e', 'f']
```

```
the_list[10:]
[]
```

- the_list[:the_index] 返回一个新列表，其中包含原始列表中到但不包括 the_index 处项的项。

```
the_list[:2]
['a', 'b']
```

- the_list[the_first_index:the_second_index] 返回一个新列表，其中包含原始列表中从位置 the_first_index 处项开始到但不包括位置 the_second_index 处项的项。

```
the_list[1:3]
['b', 'c']
```

- the_list[::2] 返回一个新列表，其中包含原始列表中从第一个开始，然后每隔一个项的项。

```
the_list[::2]
['a', 'c', 'e']
```

- the_list[1::3] 返回一个新列表，其中包含原始列表中从第二个开始，然后每隔三个项的项。

```
the_list[1::3]
['b', 'e']
```

- the_list[::-1] 返回 the_list 的反向副本。

```
the_list[::-1]
['f', 'e', 'd', 'c', 'b', 'a']
```

我认为反转列表的技术很精彩。

**练习 3.8**

编写一个 Python 函数来生成本书所有可能的问题，其中 `start`、`end` 和 `step` 的取值为负、零和正整数。只是开玩笑，但请多做实验。

**练习 3.9**

为切片与 `start`、`end` 和 `step` 的允许取值如何工作制定正式规则。

## 3.2.7 切片：更改子列表

正如我们看到的，我们可以 **更改** 和 **移除** 列表项，我们可以使用切片记法与赋值一起更改列表的结构。

思路是我们创建一个切片表达式来指定列表的一部分，然后使用赋值用另一个列表替换切片。

```
the_list = ["a", "b", "c", "d", "e", "f"]
```

```
the_list[:1] = [1, 2, 3]
the_list
```

```
[1, 2, 3, 'b', 'c', 'd', 'e', 'f']
```

```
the_list[6:] = [7, 8, 9]
the_list
```

```
[1, 2, 3, 'b', 'c', 'd', 7, 8, 9]
```

```
the_list[4:6] = ["alpha", "beta", "gamma"]
the_list
```

```
[1, 2, 3, 'b', 'alpha', 'beta', 'gamma', 7, 8, 9]
```

**练习 3.10**

虽然这些示例工作正常，但 Python 要求当你使用除一以外的步长时，切片和赋值右侧的列表长度必须相同。尝试正步长和负步长，看看什么有效什么无效。

## 3.2.8 扩展列表

我们如何向现有列表添加项？有几种我们可能想要做的事情：

- 1. 在列表末尾追加一项。
- 2. 在列表开头前置一项。
- 3. 在列表的特定索引处插入一项。
- 4. 用另一个列表的项在末尾扩展我们的列表。
- 5. 在列表开头添加另一个列表的项。
- 6. 在列表的特定索引处添加另一个列表的项。

前四种很容易，但最后两种需要你多做一些工作。我们再次使用吉他品牌。

```
brands = [
    "Fender",
    "Gibson",
    "Ibanez"
]

original_brands = brands.copy()
```

要在列表末尾添加一项，使用 *append*。

```
brands.append("Gretsch")
brands
```

['Fender', 'Gibson', 'Ibanez', 'Gretsch']

要在列表开头添加一项，使用索引 0 的 *insert*。

```
brands = original_brands.copy()
brands.insert(0, "Gretsch")
brands
```

['Gretsch', 'Fender', 'Gibson', 'Ibanez']

更一般地，使用 *insert* 并指定你想要放置新项的索引。

```
brands = original_brands.copy()
brands.insert(1, "Gretsch")
brands
```

['Fender', 'Gretsch', 'Gibson', 'Ibanez']

## 练习 3.11

假设你想在空列表开头放置一项。你应该使用 *append* 还是 *insert*？尝试两种方法。

## 练习 3.12

当你对原始列表使用无效索引时，*insert* 的行为如何？

*extend* 方法类似于 *append*，但它取给定列表的所有项并将它们放在我们的列表末尾。

```
brands = original_brands.copy()
brands.extend(["Epiphone", "Paul Reed Smith"])
brands
```

['Fender', 'Gibson', 'Ibanez', 'Epiphone', 'Paul Reed Smith']

它与此循环做同样的事情：

```
brands = original_brands.copy()

for brand in ["Epiphone", "Paul Reed Smith"]:
    brands.append(brand)

brands
```

['Fender', 'Gibson', 'Ibanez', 'Epiphone', 'Paul Reed Smith']

这种技术为我们在开头插入项提供了提示。让我们试试这个：

```
brands = original_brands.copy()

for brand in ["Epiphone", "Paul Reed Smith"]:
    brands.insert(0, brand)

brands
```

['Paul Reed Smith', 'Epiphone', 'Fender', 'Gibson', 'Ibanez']

