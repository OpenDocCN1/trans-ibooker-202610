

## 1.15 PYTHON LISTS (AND A COOL SORTING APP)

Python 最常用的集合类称为 _list_ 集合，它非常灵活且功能强大。

![](img/6e45221a8147f2442c653a3d4a866570_75_0.png)

```
[ items ]
```

这里的方括号是字面意思，而 _items_ 是零个或多个项目的列表，如果有多个项目，则用逗号分隔。下面是一个示例，表示夏季周末的一系列华氏度高温：

```
[78, 81, 81]
```

列表可以包含任何类型的对象（包括其他列表！），并且与 C 或 C++ 不同，Python 允许你混合类型。例如，你可以有字符串列表：

Click here to view code image

```
[ 'John', 'Paul', 'George', 'Ringo' ]
```

你还可以有混合类型的列表：

```
['John', 9, 'Paul', 64 ]
```

但是，在 Python 3.0 中，具有混合类型的列表不能自动排序，而排序是一个重要功能。与其他一些 Python 集合类（字典和集合）不同，列表中的顺序很重要，并且允许重复值。但是，正是内置功能的长列表（全部在第 3 章中介绍）使 Python 列表非常出色。在本节中，我们使用两个功能：**append**，它动态地向列表添加元素，以及前面提到的 **sort** 功能。

下面是一个简洁的小程序，展示了 Python 列表排序功能。将以下内容输入 Python 脚本并运行它。

```
a_list = []

while True:
    s = input('Enter name: ')
    if not s:
        break
    a_list.append(s)
a_list.sort()
print(a_list)
```

哇，这太短了！但它有效吗？下面是一个示例会话：
Click here to view code image

```
Enter name: John
Enter name: Paul
Enter name: George
Enter name: Ringo
Enter name: Brian
Enter name:
['Brian', 'George', 'John', 'Paul', 'Ringo']
```

看到了什么吗？Brian（我相信他是经理）被添加到组中，现在所有人都按字母顺序打印出来。

你应该看到，这个小程序提示用户一次输入一个名字；每输入一个名字，它就通过 **append** 方法添加到列表中。最后，当输入空字符串时，循环中断。之后，它被排序并打印。

## 1.16 THE “FOR” STATEMENT AND RANGES

当你查看上一节中的应用程序时，你可能会想知道是否有一种更优雅的方法，或者至少是一种更灵活的方法，来打印列表的内容。是的，有。在 Python 中，`for` 语句的中心（虽然不是唯一）目的是遍历集合并对每个元素执行相同的操作。

其中一种用法是打印每个元素。上一节应用程序的最后一行可以替换为以下内容，让你更好地控制如何打印输出。

```
for name in a_list:
    print(name)
```

现在输出是

```
Brian
George
John
Paul
Ringo
```

在示例 **for** 语句中，_iterable_ 最常是一个集合，例如列表，但也可以是对 **range** 函数的调用，它是一个生成器，用于生成一系列值的迭代。（你将在第 4 章中了解更多关于生成器的内容。）

![](img/6e45221a8147f2442c653a3d4a866570_78_0.png)

```
for var in iterable:
    indented_statements
```

请再次注意缩进的重要性，以及冒号 (:)。

值以类似于函数参数传递的方式发送到 **for** 循环。因此，给循环变量赋值对原始数据没有影响。

```
my_lst = [10, 15, 25]
for thing in my_lst:
    thing *= 2
```

看起来这个循环应该将 my_lst 的每个元素加倍，但它没有。要以这种方式处理列表，原地更改值，有必要使用索引。

```
my_lst = [10, 15, 25]
for i in [0, 1, 2]:
    my_lst[i] *= 2
```

这产生了预期的效果：将 my_lst 的每个单独元素加倍，因此现在列表数据是 [20, 30, 50]。

要以这种方式索引列表，你需要创建一个形式为

```
0, 1, 2, ... N-1
```

的索引序列，其中 N 是列表的长度。你可以通过使用 **range** 函数来自动生成这样的索引序列。例如，要将长度为 5 的数组中的每个元素加倍，请使用此代码：

Click here to view code image

```
my_lst = [100, 102, 50, 25, 72]
for i in range(5):
    my_lst[i] *= 2
```

这段代码片段不是最优的，因为它将列表长度（即 5）硬编码到代码中。下面是编写此循环的更好方法：

Click here to view code image

```
my_lst = [100, 102, 50, 25, 72]
for i in range(len(my_lst)):
    my_lst[i] *= 2
```

执行此循环后，my\_lst 包含 [200, 204, 100, 50, 144]。

**range** 函数生成一系列整数，如表 1.3 所示，具体取决于你指定一个、两个还是三个参数。

# Table 1.3. Effects of the Range Function

| Syntax | Effect |
|--------|--------|
| `range(end)` | 生成一个从 0 开始，直到但不包括 `end` 的序列。 |
| `range(beg, end)` | 生成一个从 `beg` 开始，直到但不包括 `end` 的序列。 |
| `range(beg, end, step)` | 生成一个从 `beg` 开始，直到但不包括 `end` 的序列；但是，元素每次增加 `step` 的值。如果 `step` 为负，则 range 倒计时。 |

**range** 的另一个用途是创建一个遍历一系列整数的循环。例如，下面的循环计算一个阶乘数。

Click here to view code image

```
n = int(input('Enter a positive integer: '))
prod = 1
for i in range(1, n + 1):
    prod *= i
print(prod)
```

这个循环之所以有效，是因为 `range(1, n + 1)` 生成从 1 开始，直到但不包括 `n + 1` 的整数。因此，这个循环的效果是执行以下计算：

```
1 * 2 * 3 * ... n
```

## 1.17 TUPLES

Python 中的 _tuple_ 概念与列表密切相关；如果说有什么不同的话，元组的概念甚至更基本。下面的代码返回一个整数列表：

```
def my_func():
    return [10, 20, 5]
```

此函数以列表形式返回值。

```
my_lst = my_func()
```

但是，下面的代码返回一系列简单的值，实际上返回一个元组：

```
def a_func():
    return 10, 20, 5
```

它可以这样调用：

```
a, b, c = a_func()
```

请注意，即使元组为了清晰起见被括在括号中，它仍然是一个元组。

Click here to view code image

```
return (10, 20, 5)  # Parens have no effect in
                    # this case.
```

元组和列表的基本属性几乎相同：每个都是有序集合，允许任意数量的重复值。

但是，与列表不同，元组是不可变的；元组值不能原地更改。元组不支持列表支持的所有方法或函数；特别是，元组不支持任何修改元组内容的方法。

## 1.18 DICTIONARIES

Python _dictionary_ 是一个包含一系列键值对关联的集合。与列表不同，字典使用花括号指定，而不是方括号。

![](img/6e45221a8147f2442c653a3d4a866570_83_0.png)

Click here to view code image

```
{ key1: value1, key2: value2, ... }
```

用大白话说，字典就像数据库中的一个平面的两列表。它缺乏现代数据库管理系统的高级功能；它只是一个表。但它仍然可以作为 Python 程序中的丰富数据存储对象。

字典的键是一系列唯一的值；键不能重复。对于每个键，都有一个关联的数据对象，称为值。例如，你可以按以下方式创建一个用于给一班学生评分的字典：

Click here to view code image

```
grade_dict = { 'Bob':3.9, 'Sue':3.9, 'Dick':2.5 }
```

此语句创建一个包含三个条目的字典——字符串“Bob”、“Sue”和“Dick”——它们分别具有关联值 3.9、3.9 和 2.5。请注意，重复值 3.9 完全没问题，因为它不是键。

像往常一样，grade_dict 只是一个名称，你可以给字典起几乎任何你想要的名称（只要名称遵守前面列出的规则）。我选择 grade_dict 这个名称，因为它暗示了这个对象是什么。

创建字典后，你可以随时通过类似下面的语句添加一个值：

