## 6

## DICTIONARIES

![](img/8f15563c16d2a98c4d3115150679b092_179_0.png)

在本章中，你将学习如何使用 Python 的字典，它允许你关联相关信息。你将学习如何在信息进入字典后访问该信息以及如何修改该信息。由于字典可以存储几乎无限量的信息，我将向你展示如何遍历字典中的数据。此外，你将学习将字典嵌套在列表中、列表嵌套在字典中，甚至字典嵌套在其他字典中。

理解字典允许你更准确地模拟各种现实世界对象。你将能够创建一个代表人的字典，然后存储你想要的关于这个人的任何信息。你可以存储他们的姓名、年龄、位置、职业以及你能描述的关于这个人的任何其他方面。你可以存储任何可以匹配的两种信息，例如单词及其含义、人们的名字及其最喜欢的数字、山脉及其海拔等等。

### A Simple Dictionary

考虑一个包含外星人的游戏，外星人可以有不同的颜色和点值。这个简单的字典存储有关特定外星人的信息：

```python
alien_0 = {'color': 'green', 'points': 5}

print(alien_0['color'])
print(alien_0['points'])
```

字典 `alien_0` 存储外星人的颜色和点值。最后两行访问并显示该信息，如下所示：

```
green
5
```

与大多数新的编程概念一样，使用字典需要练习。一旦你使用字典一段时间后，你会看到它们如何有效地模拟现实世界的情况。

### Working with Dictionaries

Python 中的 *dictionary* 是一个 *key-value pairs* 的集合。每个键都与一个值相关联，你可以使用键来访问与该键关联的值。键的值可以是数字、字符串、列表，甚至是另一个字典。事实上，你可以在字典中使用你在 Python 中创建的任何对象作为值。

在 Python 中，字典用大括号 (`{}`) 包裹，括号内包含一系列键值对，如前面的示例所示：

```
alien_0 = {'color': 'green', 'points': 5}
```

*key-value pair* 是彼此关联的一组值。当你提供一个键时，Python 会返回与该键关联的值。每个键都通过冒号与其值连接，单独的键值对由逗号分隔。你可以在字典中存储任意多的键值对。

最简单的字典恰好有一个键值对，如下所示是 `alien_0` 字典的修改版本：

```
alien_0 = {'color': 'green'}
```

该字典存储有关 `alien_0` 的一个信息：外星人的颜色。字符串 `'color'` 是该字典中的一个键，其关联的值是 `'green'`。

### Accessing Values in a Dictionary

要获取与键关联的值，请给出字典的名称，然后将键放在一对方括号中，如下所示：

# alien.py

```
alien_0 = {'color': 'green'}
print(alien_0['color'])
```

这会从字典 alien_0 中返回与键 'color' 关联的值：

```
green
```

字典中可以有无限数量的键值对。例如，这里是包含两个键值对的原始 alien_0 字典：

```
alien_0 = {'color': 'green', 'points': 5}
```

现在你可以访问 alien_0 的颜色或点值。如果玩家击落这个外星人，你可以使用如下代码查找他们应获得的分数：

```
alien_0 = {'color': 'green', 'points': 5}

new_points = alien_0['points']
print(f"You just earned {new_points} points!")
```

在定义字典后，我们从字典中提取与键 'points' 关联的值。然后该值被分配给变量 new_points。最后一行打印一条关于玩家刚刚获得多少分的信息：

```
You just earned 5 points!
```

如果每次击落外星人时都运行此代码，外星人的点值将被检索。

### Adding New Key-Value Pairs

字典是动态结构，你可以随时向字典添加新的键值对。要添加新的键值对，你只需给出字典的名称，后跟方括号中的新键以及新值。




## 从空字典开始

有时候，从空字典开始并逐步向其中添加新条目会更方便，甚至很有必要。要开始填充一个空字典，可先定义一个空花括号的字典，然后在单独的行中添加每个键值对。例如，下面演示了如何使用这种方法构建 `alien_0` 字典：

**alien.py**

```python
alien_0 = {}

alien_0['color'] = 'green'
alien_0['points'] = 5

print(alien_0)
```

我们首先定义一个空的 `alien_0` 字典，然后向其中添加颜色和点数。得到的结果就是之前示例中一直在使用的字典：

```text
{'color': 'green', 'points': 5}
```

通常，需要在字典中存储用户提供的数据，或编写代码自动生成大量键值对时，可以使用空字典。

## 修改字典中的值

要修改字典中的值，可依次指定字典名、方括号内的键，以及希望与该键关联的新值。例如，假设随着游戏进度的推进，一个外星人从绿色变为黄色：

**alien.py**

```python
alien_0 = {'color': 'green'}
print(f"The alien is {alien_0['color']}.")

alien_0['color'] = 'yellow'
print(f"The alien is now {alien_0['color']}.")
```

我们首先为 `alien_0` 定义一个只包含外星人颜色的字典；然后将键 `'color'` 关联的值改为 `'yellow'`。输出表明外星人确实从绿色变成了黄色：

```text
The alien is green.
The alien is now yellow.
```

再看一个更有趣的示例：跟踪一个能够以不同速度移动的外星人的位置。我们将存储一个表示外星人当前速度的值，然后据此确定外星人应该向右移动多远：

```python
alien_0 = {'x_position': 0, 'y_position': 25, 'speed': 'medium'}
print(f"Original position: {alien_0['x_position']}")

# Move the alien to the right.
# Determine how far to move the alien based on its current speed.
if alien_0['speed'] == 'slow':
    x_increment = 1
elif alien_0['speed'] == 'medium':
    x_increment = 2
else:
    # This must be a fast alien.
    x_increment = 3

# The new position is the old position plus the increment.
alien_0['x_position'] = alien_0['x_position'] + x_increment

print(f"New position: {alien_0['x_position']}")
```

我们首先定义一个外星人，它带有初始的 x 坐标和 y 坐标，速度为 `'medium'`。为简洁起见，我们省略了颜色和点数，但即使你把这些键值对也包含进来，这个示例的运行方式也相同。我们还打印了 x_position 的原始值，以便查看外星人向右移动了多远。

if-elif-else 语句链决定外星人应向右移动多远，并将这个值赋给变量 x_increment ①。如果外星人的速度是 'slow'，它向右移动 1 个单位；如果速度是 'medium'，它向右移动 2 个单位；如果速度是 'fast'，它向右移动 3 个单位。一旦计算出增量，就把它加到 x_position 的值上 ②，并将结果存储到字典的 x_position 中。

由于这是一个中等速度的外星人，它的位置向右移动了两个单位：

```text
Original position: 0
New position: 2
```

这种技术非常酷：只需更改外星人字典中的一个值，就能改变外星人的整体行为。例如，要将这个中等速度的外星人变成快速外星人，可以添加下面这一行：

```python
alien_0['speed'] = 'fast'
```

下次运行代码时，if-elif-else 块就会为 x_increment 赋一个更大的值。

## 删除键值对

当你不再需要字典中存储的某项信息时，可以使用 `del` 语句彻底删除一个键值对。`del` 语句只需要指定字典名和要删除的键。

例如，下面我们从 `alien_0` 字典中删除键 `points` 及其对应的值：

**alien.py**

```python
alien_0 = {'color': 'green', 'points': 5}
print(alien_0)

del alien_0['points']
print(alien_0)
```

`del` 语句❶告诉 Python 从字典 `alien_0` 中删除键 `points`，并同时删除与该键关联的值。输出显示，键 `points` 及其值 5 已从字典中删除，但字典的其余部分不受影响：

```text
{'color': 'green', 'points': 5}
{'color': 'green'}
```

> **注意**
>
> *请注意，被删除的键值对会被永久移除。*

## 存储相似对象的字典

前面的示例涉及存储一个对象（游戏中的外星人）的不同种类信息。你也可以使用字典来存储多个对象的同一类信息。例如，假设你想对一些人进行调研，询问他们最喜欢的编程语言是什么。字典非常适合存储这种简单调研的结果，如下所示：

**favorite_languages.py**

```python
favorite_languages = {
    'jen': 'python',
    'sarah': 'c',
    'edward': 'rust',
    'phil': 'python',
}
```

如你所见，我们将一个较大的字典拆成了多行。每个键都是受访者的名字，每个值都是他们选择的语言。当你知道定义字典需要多行时，请在左花括号后按回车键。然后将下一行缩进一级（四个空格），写下第一个键值对，并在后面加上逗号。此后每按一次回车键，文本编辑器都应该会自动缩进所有后续键值对，使其与第一个键值对对齐。

定义完字典后，在最后一个键值对之后另起一行添加右花括号，并缩进一级，使其与字典中的键对齐。在最后一个键值对后面也加上逗号是一个好习惯，这样你就可以在下一行继续添加新的键值对。

> **注意**
>
> *大多数编辑器都有一些功能，可帮助你像该示例一样格式化较长的列表和字典。还有其他可接受的格式化长字典的方式，因此你可能会在自己的编辑器中或从其他资料来源看到略有不同的格式。*

要使用这个字典，只要给出受访者的名字，你就能轻松查到他们最喜欢的语言：

**favorite_languages.py**

```python
favorite_languages = {
    'jen': 'python',
    'sarah': 'c',
    'edward': 'rust',
    'phil': 'python',
}

language = favorite_languages['sarah'].title()
print(f"Sarah's favorite language is {language}.")
```

为了查看 Sarah 选择了哪种语言，我们获取该键对应的值：

```python
favorite_languages['sarah']
```

我们使用这种语法从字典中取出 Sarah 最喜欢的语言❸，并将其赋给变量 language。在这里新建一个变量，可以让 print() 调用更简洁。输出显示了 Sarah 最喜欢的语言：

```text
Sarah's favorite language is C.
```

对于字典中表示的任意受访者，你都可以使用相同的语法。

## 使用 get() 访问值

使用方括号中的键从字典中获取你感兴趣的值可能会带来一个潜在问题：如果你请求的键不存在，就会得到一个错误。

我们来看看，当你请求获取一个没有设置 `points` 值的外星人的 `points` 值时，会发生什么：

**alien_no_points.py**

```python
alien_0 = {'color': 'green', 'speed': 'slow'}
print(alien_0['points'])
```

这会生成一个 traceback，并显示 KeyError：

```text
Traceback (most recent call last):
  File "alien_no_points.py", line 2, in <module>
    print(alien_0['points'])
            ~~~~~~~^^^^^^^^
KeyError: 'points'
```

你将在第 10 章更全面地学习如何处理这类错误。具体到字典，你可以使用 `get()` 方法来设置一个默认值，当请求的键不存在时，返回这个默认值。

`get()` 方法要求将一个键作为第一个参数。作为可选的第二个参数，你可以传入当键不存在时要返回的值：

```python
alien_0 = {'color': 'green', 'speed': 'slow'}

point_value = alien_0.get('points', 'No point value assigned.')
print(point_value)
```

如果字典中存在键 `'points'`，你将获得相应的值。如果不存在，你将获得默认值。在本例中，`points` 不存在，因此我们得到一条简洁的消息，而不是错误：

```text
No point value assigned.
```

如果你请求的键有可能不存在，请考虑使用 `get()` 方法而不是方括号表示法。



字典。字典可以以多种方式存储信息；因此，存在几种不同的遍历方式。你可以遍历字典的所有键值对、键或值。

## 遍历所有键值对

在探索不同的遍历方法之前，我们先考虑一个新字典，它用于存储网站上某个用户的信息。下面的字典将存储一个人的用户名、名和姓：

user.py

```
user_0 = {
    'username': 'efermi',
    'first': 'enrico',
    'last': 'fermi',
}
```

你可以根据本章已学的内容访问 user_0 的任何一条信息。但如果你想查看这个用户字典中存储的所有内容呢？为此，你可以使用 for 循环遍历字典：

```
user_0 = {
    'username': 'efermi',
    'first': 'enrico',
    'last': 'fermi',
}

for key, value in user_0.items():
    print(f"\nKey: {key}")
    print(f"Value: {value}")
```

要为字典编写 for 循环，你需要为两个变量命名，它们将分别保存每个键值对中的键和值。你可以为这两个变量选择任意名称。如果使用变量名的缩写，这段代码也同样有效，例如：

```
for k, v in user_0.items()
```

for 语句的后半部分包含字典名，后跟方法 items()，该方法返回一个键值对序列。for 循环随后将每个键值对赋给提供的两个变量。在前面的示例中，我们使用变量打印每个键及其关联值。第一个 print() 调用中的 "\n" 确保在输出中每个键值对之前插入一个空行：

```
Key: username
Value: efermi

Key: first
Value: enrico

Key: last
Value: fermi
```

遍历所有键值对对于像第 96 页的 favorite_languages.py 示例这样的字典尤其有用，该字典为许多不同的键存储同类信息。如果你遍历 favorite_languages 字典，你会得到字典中每个人的名字和他们最喜欢的编程语言。因为键始终对应人名，而值始终是语言，我们将在循环中使用变量 name 和 language，而不是 key 和 value。这样更容易理解循环内部发生的事情：

favorite_languages.py

```
favorite_languages = {
    'jen': 'python',
    'sarah': 'c',
    'edward': 'rust',
    'phil': 'python',
}

for name, language in favorite_languages.items():
    print(f"{name.title()}'s favorite language is {language.title()}.")
```

这段代码告诉 Python 遍历字典中的每个键值对。在处理每个键值对时，键被赋给变量 name，值被赋给变量 language。这些描述性名称使得 print() 调用的作用一目了然。

现在，仅用几行代码，我们就可以显示投票中的所有信息：

```
Jen's favorite language is Python.
Sarah's favorite language is C.
Edward's favorite language is Rust.
Phil's favorite language is Python.
```

如果我们的字典存储的是几千甚至一百万人的投票结果，这种遍历方式同样适用。

## 遍历字典中的所有键

当你不需要处理字典中的所有值时，keys() 方法非常有用。让我们遍历 favorite_languages 字典，并打印所有参与投票的人的名字：

```
favorite_languages = {
    'jen': 'python',
    'sarah': 'c',
    'edward': 'rust',
    'phil': 'python',
}

for name in favorite_languages.keys():
    print(name.title())
```

输出显示了所有参与投票的人的名字：

```
Jen
Sarah
Edward
Phil
```

实际上，遍历字典的键是默认行为，所以如果你写成下面的形式，这段代码的输出将完全相同：

```
for name in favorite_languages:
```

而不是：

```
for name in favorite_languages.keys():
```

你可以选择显式使用 keys() 方法（如果它能让你的代码更易读），也可以根据情况省略它。

在循环中，你可以通过当前键访问与之关联的任何值。让我们向几位朋友打印一条关于他们所选语言的消息。我们将像之前一样遍历字典中的名字，但当名字与某位朋友匹配时，我们会显示一条关于他们最喜欢的语言的消息：

```
favorite_languages = {
    --snip--
}
```

检查 'erin' 是否在这个序列中。由于她不在，会打印一条消息邀请她参加投票：

```
Erin, please take our poll!
```

## 按特定顺序遍历字典的键

遍历字典时，返回的元素顺序与插入顺序相同。不过，有时你会希望以不同的顺序遍历字典。

一种方法是在 for 循环中对返回的键进行排序。你可以使用 sorted() 函数获取按顺序排列的键的副本：

```
favorite_languages = {
    'jen': 'python',
    'sarah': 'c',
    'edward': 'rust',
    'phil': 'python',
}

for name in sorted(favorite_languages.keys()):
    print(f"{name.title()}, thank you for taking the poll.")
```

输出显示了所有参与投票的人，名字按顺序排列：

```
Edward, thank you for taking the poll.
Jen, thank you for taking the poll.
Phil, thank you for taking the poll.
Sarah, thank you for taking the poll.
```

## 遍历字典中的所有值

如果你主要关心字典中包含的值，可以使用 values() 方法来返回一个不包含任何键的值序列。例如，假设我们只想要编程语言投票中所有被选中的语言列表，而不需要选择每种语言的人的名字：

```
favorite_languages = {
    'jen': 'python',
    'sarah': 'c',
    'edward': 'rust',
    'phil': 'python',
}

print("The following languages have been mentioned:")
for language in favorite_languages.values():
    print(language.title())
```

这里的 for 语句从字典中取出每个值，并将其赋给变量 language。打印这些值时，我们会得到所有被选中语言的列表：

```
The following languages have been mentioned:
Python
C
Rust
Python
```

这种方法会从字典中取出所有值，而不检查重复项。对于少量值来说，这可能没问题，但在有大量受访者的投票中，这会导致一个非常重复的列表。为了不重复地查看每种被选中的语言，我们可以使用集合（set）。集合（set）是一种其中每个元素都必须唯一的集合类型：

```
favorite_languages = {
    --snip--
}
```

```
print("The following languages have been mentioned:")
for language in set(favorite_languages.values()):
    print(language.title())
```

当你用 `set()` 包裹一个包含重复项的值的集合时，Python 会识别出集合中的唯一项，并由这些项构建一个集合。这里我们使用 `set()` 来提取 `favorite_languages.values()` 中的唯一语言。

结果是一个不重复的语言列表，其中包含参与投票的人提到的语言：

```
The following languages have been mentioned:
Python
C
Rust
```

随着你继续学习 Python，你经常会发现语言的一个内置特性可以帮助你准确地对数据执行你想要的操作。

- **注意**

  *你可以直接使用花括号并用逗号分隔元素来构建集合：*

  ```
  >>> languages = {'python', 'rust', 'python', 'c'}
  >>> languages
  {'rust', 'python', 'c'}
  ```

  *很容易将集合误认为是字典，因为它们都用花括号括起来。当你看到花括号但没有键值对时，你很可能是在看一个集合。与列表和字典不同，集合不按任何特定顺序保留元素。*





# 亲自动手试一试

**6-4. 术语表 2：** 现在你已经知道如何遍历字典，请通过用一个遍历字典键和值的循环替换一系列 `print()` 调用，来清理练习 6-3（第 99 页）中的代码。当你确定循环有效时，再向你的术语表中添加五个 Python 术语。当你再次运行程序时，这些新单词和含义应会自动包含在输出中。

**6-5. 河流：** 创建一个包含三条主要河流以及每条河流流经的国家的字典。一个键值对可能是 `'nile': 'egypt'`。

- 使用循环打印关于每条河流的句子，例如 *The Nile runs through Egypt.*
- 使用循环打印字典中包含的每条河流的名称。
- 使用循环打印字典中包含的每个国家的名字。

**6-6. 投票：** 使用 *favorite_languages.py*（第 96 页）中的代码。

列出应参与最喜欢语言投票的人。包括一些字典中已有的名字和一些不在其中的名字。遍历应参与投票的人列表。如果他们已经参与过投票，则打印一条感谢他们回复的消息。如果他们尚未参与投票，则打印一条邀请他们参与投票的消息。

## 嵌套

有时你会想在列表中存储多个字典，或将一个列表的项目作为字典中的值。这称为 *嵌套*。你可以将字典嵌套在列表中，将列表的项目嵌套在字典中，甚至将字典嵌套在另一个字典中。嵌套是一个强大的功能，如以下示例所示。

### 字典的列表

`alien_0` 字典包含关于一个外星人的各种信息，但它没有空间存储关于第二个外星人的信息，更不用说一整屏的外星人了。你如何管理一支外星人舰队？一种方法是创建一个外星人列表，其中每个外星人都是关于该外星人的信息的字典。例如，以下代码构建了一个包含三个外星人的列表：

***aliens.py***

```
alien_0 = {'color': 'green', 'points': 5}
alien_1 = {'color': 'yellow', 'points': 10}
alien_2 = {'color': 'red', 'points': 15}

aliens = [alien_0, alien_1, alien_2]

for alien in aliens:
    print(alien)
```

我们首先创建三个字典，每个代表不同的外星人。我们将这些字典存储在一个名为 aliens 的列表中。最后，我们遍历列表并打印出每个外星人：

```
{'color': 'green', 'points': 5}
{'color': 'yellow', 'points': 10}
{'color': 'red', 'points': 15}
```

一个更现实的例子将涉及不止三个外星人，并且代码会自动生成每个外星人。在以下示例中，我们使用 `range()` 创建一支 30 个外星人的舰队：

```
# Make an empty list for storing aliens.
aliens = []

# Make 30 green aliens.
for alien_number in range(30):
    new_alien = {'color': 'green', 'points': 5, 'speed': 'slow'}
    aliens.append(new_alien)

# Show the first 5 aliens.
for alien in aliens[:5]:
```

```
print(alien)
print("...")

# Show how many aliens have been created.
print(f"Total number of aliens: {len(aliens)}")
```
```

此示例以一个空列表开始，用于保存将要创建的所有外星人。`range()` 函数返回一系列数字，这只是告诉 Python 我们希望循环重复多少次。每次循环运行时，我们创建一个新的外星人，然后将每个新外星人附加到列表 `aliens`。我们使用切片打印前五个外星人，最后打印列表的长度以证明我们确实生成了全部 30 个外星人的舰队：

```
{'color': 'green', 'points': 5, 'speed': 'slow'}
{'color': 'green', 'points': 5, 'speed': 'slow'}
{'color': 'green', 'points': 5, 'speed': 'slow'}
{'color': 'green', 'points': 5, 'speed': 'slow'}
{'color': 'green', 'points': 5, 'speed': 'slow'}
...

Total number of aliens: 30
```
```

这些外星人都有相同的特征，但 Python 将每个外星人视为一个单独的对象，这使我们可以单独修改每个外星人。

你如何处理这样一群外星人？想象一下，游戏中一个方面是外星人随着游戏进程改变颜色并移动得更快。当需要改变颜色时，我们可以使用 `for` 循环和 `if` 语句来改变外星人的颜色。例如，要将前三个外星人改为黄色、中速、每个值 10 分的外星人，我们可以这样做：

```
# Make an empty list for storing aliens.
aliens = []

# Make 30 green aliens.
```

```
for alien_number in range(30):
    new_alien = {'color': 'green', 'points': 5, 'speed': 'slow'}
    aliens.append(new_alien)

for alien in aliens[:3]:
    if alien['color'] == 'green':
        alien['color'] = 'yellow'
        alien['speed'] = 'medium'
        alien['points'] = 10

# Show the first 5 aliens.
for alien in aliens[:5]:
    print(alien)
print("...")
```

因为我们想要修改前三个外星人，所以我们遍历只包含前三个外星人的切片。所有外星人现在都是绿色的，但情况并非总是如此，因此我们编写一个 `if` 语句以确保我们只修改绿色外星人。如果外星人是绿色的，我们将颜色改为 `'yellow'`，速度改为 `'medium'`，分数值改为 10，如以下输出所示：

```
{'color': 'yellow', 'points': 10, 'speed': 'medium'}
{'color': 'yellow', 'points': 10, 'speed': 'medium'}
{'color': 'yellow', 'points': 10, 'speed': 'medium'}
{'color': 'green', 'points': 5, 'speed': 'slow'}
{'color': 'green', 'points': 5, 'speed': 'slow'}
...
```

你可以通过添加一个 `elif` 块来扩展此循环，该块将黄色外星人变为红色、快速移动、每个值 15 分的外星人。不再显示整个程序，该循环看起来像这样：

```
for alien in aliens[0:3]:
    if alien['color'] == 'green':
        alien['color'] = 'yellow'
        alien['speed'] = 'medium'
        alien['points'] = 10
```

```
elif alien['color'] == 'yellow':
    alien['color'] = 'red'
    alien['speed'] = 'fast'
    alien['points'] = 15
```
```

当每个字典包含关于一个对象的许多种信息时，将多个字典存储在列表中是很常见的。例如，你可能会为网站上的每个用户创建一个字典，如我们在第 99 页的 *user.py* 中所做的那样，并将各个字典存储在一个名为 users 的列表中。列表中的所有字典都应具有相同的结构，这样你就可以遍历列表并以相同的方式处理每个字典对象。

### 字典中的列表

与其将字典放在列表中，有时将列表放在字典中更有用。例如，考虑你如何描述某人订购的披萨。如果只使用列表，你唯一能存储的就是披萨配料的列表。有了字典，配料列表就可以成为你描述披萨的一个方面。

在以下示例中，为每块披萨存储两种信息：面饼类型和配料列表。配料列表是与键 'toppings' 关联的值。要使用列表中的项目，我们给出字典的名称和键 'toppings'，就像字典中的任何值一样。返回的不是单个值，我们得到一个配料列表：

# pizza.py

```
# Store information about a pizza being ordered.
pizza = {
    'crust': 'thick',
    'toppings': ['mushrooms', 'extra cheese'],
}

# Summarize the order.
```

```
print(f"You ordered a {pizza['crust']}-crust pizza "
      "with the following toppings:")

for topping in pizza['toppings']:
    print(f"\t{topping}")
```
```

我们从一个保存已订购披萨信息的字典开始。字典中的一个键是 `'crust'`，关联的值是字符串 `'thick'`。下一个键 `'toppings'` 的值是一个列表，存储所有请求的配料。我们在制作披萨之前总结订单。当需要在 `print()` 调用中拆分长行时，选择在打印的行中适当的位置拆分行，并在行尾加上引号。缩进下一行，添加开始引号并继续字符串。Python 会自动组合括号内找到的所有字符串。要打印配料，我们编写一个 `for` 循环。要访问配料列表，我们使用键 `'toppings'`，Python 从字典中取出配料列表。

以下输出总结了我们计划制作的披萨：

```
You ordered a thick-crust pizza with the following toppings:
    mushrooms
    extra cheese
```
```

每当你希望将多个值与字典中的单个键关联时，都可以将列表嵌套在字典中。在最喜欢的编程语言的早期示例中，如果我们将每个人的回复存储在列表中，人们可以选择多个最喜欢的语言。当我们遍历字典时，与每个人的关联值将是语言列表而不是单个语言。在字典的 `for` 循环内部，我们使用



再使用另一个 `for` 循环遍历与每个人关联的语言列表：

# ***favorite_languages.py***

```
favorite_languages = {
    'jen': ['python', 'rust'],
    'sarah': ['c'],
    'edward': ['rust', 'go'],
    'phil': ['python', 'haskell'],
}
```

```
for name, languages in favorite_languages.items():
    print(f"\n{name.title()}'s favorite languages are:")
    for language in languages:
        print(f"\t{language.title()}")
```

`favorite_languages` 中每个名称关联的值现在是一个列表。请注意，有些人只有一个最喜欢的语言，而其他人有多个最喜欢的语言。当我们遍历字典时，我们使用变量名 `languages` 来保存字典中的每个值，因为我们知道每个值都将是一个列表。在主字典循环内部，我们使用另一个 `for` 循环遍历每个人的最喜欢语言列表。现在每个人都可以列出任意多的最喜欢语言：

```
Jen's favorite languages are:
    Python
    Rust

Sarah's favorite languages are:
    C

Edward's favorite languages are:
    Rust
    Go

Phil's favorite languages are:
    Python
    Haskell
```

为了进一步完善这个程序，你可以在字典的 `for` 循环开始时加入一个 `if` 语句，通过检查 `len(languages)` 的值来判断每个人是否有不止一个最喜欢的语言。如果某人有不止一个最喜欢的语言，输出保持不变。如果某人只有一个最喜欢的语言，你可以更改措辞以反映这一点。例如，你可以说，“Sarah’s favorite language is C.”

> **NOTE**
> 
> *你不应将列表和字典嵌套得太深。如果你嵌套的项目比前面的示例深得多，或者如果你正在处理他人代码中嵌套层级很深的代码，那么很可能有更简单的方法来解决这个问题。*

## A Dictionary in a Dictionary

你可以在另一个字典中嵌套一个字典，但这样做会让代码很快变得复杂。例如，如果你有一个网站的多个用户，每个用户都有唯一的用户名，你可以使用用户名作为字典中的键。然后，你可以通过使用字典作为与用户名关联的值来存储有关每个用户的信息。在下面的列表中，我们存储了每个用户的三条信息：他们的名字、姓氏和位置。我们将通过遍历用户名和与每个用户名关联的信息字典来访问这些信息：

```
**many_users.py**

```python
users = {
    'aeinstein': {
        'first': 'albert',
```
```

Username: mcurie
    Full name: Marie Curie
    Location: Paris

请注意，每个用户字典的结构是相同的。虽然 Python 并非强制要求，但这种结构使嵌套字典更容易处理。如果每个用户的字典有不同的键，`for` 循环中的代码会更复杂。

# TRY IT YOURSELF

- **6-7. People:** 从你在练习 6-1（第 98 页）中编写的程序开始。创建两个代表不同人的新字典，并将全部三个字典存储在一个名为 `people` 的列表中。遍历你的 `people` 列表。在遍历列表时，打印你所知道的每个人的所有信息。
- **6-8. Pets:** 创建多个字典，其中每个字典代表一只不同的宠物。在每个字典中，包含动物的种类和主人的名字。将这些字典存储在一个名为 `pets` 的列表中。接下来，遍历你的列表，并在遍历时打印你所知道的每只宠物的所有信息。
- **6-9. Favorite Places:** 创建一个名为 `favorite_places` 的字典。想出三个用作字典键的名字，并为每个人存储一个到三个最喜欢的地方。为了让这个练习更有趣，可以请一些朋友说出他们几个最喜欢的地方。遍历字典，并打印每个人的名字和他们的最喜欢的地方。
- **6-10. Favorite Numbers:** 修改你在练习 6-2（第 98 页）中的程序，使每个人可以有多个最喜欢的数字。然后打印每个人的名字以及他们的最喜欢的数字。
- **6-11. Cities:** 创建一个名为 `cities` 的字典。在你的字典中使用三个城市的名字作为键。创建有关每个城市的信息字典，并包含该城市所在的国家、其大致人口以及关于该城市的一个事实。每个城市字典的键应类似于 `country`、`population` 和 `fact`。打印每个城市的名字以及你存储的有关它的所有信息。
- **6-12. Extensions:** 我们现在处理的示例已足够复杂，可以以多种方式扩展。使用本章中的一个示例程序，并通过添加新的键和值、更改程序的上下文或改进输出的格式来扩展它。

# Summary

在本章中，你学习了如何定义字典以及如何处理存储在字典中的信息。你学习了如何访问和修改字典中的单个元素，以及如何遍历字典中的所有信息。你学习了如何遍历字典的键值对、其键和其值。你还学习了如何在列表中嵌套多个字典、在字典中嵌套列表，以及在字典中嵌套字典。

在下一章中，你将学习 `while` 循环以及如何接受使用你的程序的人的输入。这将是一个令人兴奋的章节，因为你将学习如何使你的所有程序具有交互性：它们将能够响应用户输入。

当你的程序需要一个名字列表时，你将能够提示用户输入一系列名字。为此，你将使用 `input()` 函数。

你还将学习如何让程序在用户想要的时间内保持运行，以便他们可以输入所需的信息；然后，你的程序可以处理这些信息。你将使用 Python 的 `while` 循环来让程序在某些条件保持为真时持续运行。

有了处理用户输入的能力以及控制程序运行时间的长短，你将能够编写完全交互式的程序。

# How the input() Function Works

`input()` 函数会暂停你的程序并等待用户输入一些文本。一旦 Python 收到用户的输入，它就会将该输入分配给一个变量，以便你方便地使用它。

例如，下面的程序要求用户输入一些文本，然后将该消息显示回用户：

```
**parrot.py**

```python
message = input("Tell me something, and I will repeat it back
to you: ")
print(message)
```
```

`input()` 函数接受一个参数：我们希望向用户显示的 *prompt*，以便他们知道要输入什么类型的信息。在此示例中，当 Python 运行第一行时，用户会看到提示 `Tell me something, and I will repeat it back to you: `。程序在用户输入响应并在用户按下 ENTER 后继续。响应被分配给变量

message，然后 print(message) 将输入显示回用户：

```
Tell me something, and I will repeat it back to you: Hello everyone!
Hello everyone!
```

**NOTE**
*某些文本编辑器不会运行提示用户输入的程序。你可以使用这些编辑器编写提示输入的程序，但你需要从终端运行这些程序。参见第 11 页的“Running Python Programs from a Terminal”。*

# Writing Clear Prompts

每次使用 input() 函数时，你都应包含一个清晰、易于遵循的提示，告诉用户你正在寻找什么类型的信息。任何告诉用户输入内容的语句都可以。例如：

greeter.py

```
name = input("Please enter your name: ")
print(f"\nHello, {name}!")
```

在你的提示末尾添加一个空格（在前面的示例中冒号之后），以将提示与用户的响应分开，并让用户清楚地知道在哪里输入他们的文本。例如：

```
Please enter your name: Eric
Hello, Eric!
```

有时你希望编写的提示长于一行。例如，你可能希望告诉用户

为什么你会要求某些输入。你可以将提示词赋值给变量，并将该变量传递给`input()`函数。这允许你跨多行构建提示词，然后编写一个干净的`input()`语句。

***greeter.py***

```python
prompt = "If you share your name, we can personalize the messages you see."
prompt += "\nWhat is your first name? "

name = input(prompt)
print(f"\nHello, {name}!")
```

这个示例展示了一种构建多行字符串的方法。第一行将消息的第一部分赋值给变量`prompt`。在第二行中，运算符`+=`获取已赋值给`prompt`的字符串，并将新字符串添加到末尾。

提示词现在跨越两行，问号后面同样有空格，以便清晰：

```text
If you share your name, we can personalize the messages you see.
What is your first name? Eric

Hello, Eric!
```

## 使用 int() 接受数值输入

当使用`input()`函数时，Python会将用户输入的所有内容解释为字符串。请考虑以下解释器会话，它询问用户的年龄：

```text
>>> age = input("How old are you? ")
How old are you? 21
>>> age
'21'
```

用户输入数字21，但当我们向Python询问age的值时，它返回'21'，即所输入数值的字符串表示。我们知道Python将输入解释为字符串，因为该数字现在用引号括起来了。如果你只想打印输入，这没有问题。但如果你尝试将输入用作数字，就会遇到错误：

```text
>>> age = input("How old are you? ")
How old are you? 21
❶ >>> age >= 18
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
❷ TypeError: '>=' not supported between instances of 'str' and 'int'
```

当你尝试使用输入进行数值比较❶时，Python会产生错误，因为它无法将字符串与整数进行比较：赋值给age的字符串'21'无法与数值18进行比较❷。

我们可以通过使用`int()`函数解决此问题，该函数将输入字符串转换为数值。这使比较能够成功运行：

```text
>>> age = input("How old are you? ")
How old are you? 21
❶ >>> age = int(age)
>>> age >= 18
True
```

在此示例中，当我们在提示符处输入21时，Python将数字解释为字符串，但该值随后通过`int()`❶转换为数值表示。现在Python可以运行条件测试：它将age（现在表示数值21）与18进行比较，看age是否大于或等于18。此测试结果为`True`。

如何在实际程序中使用int()函数？考虑一个确定人们是否高到可以乘坐过山车的程序：

***rollercoaster.py***

```python
height = input("How tall are you, in inches? ")
height = int(height)

if height >= 48:
    print("\nYou're tall enough to ride!")
else:
    print("\nYou'll be able to ride when you're a little older.")
```

该程序可以将height与48进行比较，因为height = int(height)在比较之前将输入值转换为数值表示。如果输入的数字大于或等于48，我们告诉用户他们足够高了：

```text
How tall are you, in inches? 71

You're tall enough to ride!
```

当你使用数值输入进行计算和比较时，请务必先将输入值转换为数值表示。

## 模运算符

处理数值信息的一个有用工具是模运算符（%），它将一个数除以另一个数并返回余数：

```text
>>> 4 % 3
1
>>> 5 % 3
2
>>> 6 % 3
0
>>> 7 % 3
1
```

模运算符不会告诉你一个数能整除另一个数多少次；它只告诉你余数是多少。

当一个数能被另一个数整除时，余数为0，因此模运算符总是返回0。你可以利用这一事实来判断一个数是偶数还是奇数：

***even_or_odd.py***

```python
number = input("Enter a number, and I'll tell you if it's even or odd: ")
number = int(number)

if number % 2 == 0:
    print(f"\nThe number {number} is even.")
else:
    print(f"\nThe number {number} is odd.")
```

偶数总是能被2整除，因此如果一个数和2的模为零（这里为`if number % 2 == 0`），则该数是偶数。否则，它是奇数。

```text
Enter a number, and I'll tell you if it's even or odd: 42

The number 42 is even.
```

## 动手试一试

- **7-1. 租车：** 编写一个程序，询问用户想要什么类型的租车。打印一条关于该汽车的消息，例如“让我看看能不能帮你找到一辆斯巴鲁。”
- **7-2. 餐厅订位：** 编写一个程序，询问用户他们的晚餐团体有多少人。如果答案超过八人，打印一条消息说他们需要等位。否则，报告他们的餐桌已准备好。
- **7-3. 10的倍数：** 询问用户一个数字，然后报告该数字是否是10的倍数。

## 引入 while 循环

`for`循环接受一个项目集合，并为集合中的每个项目执行一次代码块。相比之下，`while`循环在某个条件为真时持续运行，或*在*某个条件为真期间运行。

### while 循环的实际应用

你可以使用`while`循环来遍历一系列数字。例如，以下`while`循环从1数到5：

***counting.py***

```python
current_number = 1
while current_number <= 5:
    print(current_number)
    current_number += 1
```

在第一行中，我们通过将`current_number`赋值为1来从1开始计数。然后设置`while`循环，使其在`current_number`的值小于或等于5时持续运行。循环内的代码打印`current_number`的值，然后使用`current_number += 1`将该值加1。（`+=`运算符是`current_number = current_number + 1`的简写。）

只要条件 current_number <= 5 为真，Python就会重复循环。因为1小于5，Python打印1，然后加1，使当前数字为2。因为2小于5，Python打印2并再次加1，使当前数字为3，依此类推。一旦current_number的值大于5，循环停止运行，程序结束：

```text
1
2
3
4
5
```

你每天使用的程序很可能包含while循环。例如，游戏需要一个while循环，只要你想继续玩就持续运行，并且可以在你要求退出时立即停止运行。如果程序在我们告诉它停止之前就停止运行，或者在我们想要退出后仍继续运行，那么程序用起来就没意思了，因此while循环非常有用。

### 让用户选择何时退出

我们可以通过将parrot.py程序的大部分内容放入while循环中，使其在用户想要时持续运行。我们将定义一个退出值，然后在用户尚未输入该退出值之前保持程序运行：

***parrot.py***

```python
prompt = "\nTell me something, and I will repeat it back to you:"
prompt += "\nEnter 'quit' to end the program. "

message = ""
while message != 'quit':
    message = input(prompt)
    print(message)
```

我们首先定义一个提示词，告诉用户他们的两个选项：输入消息或输入退出值（在本例中为`'quit'`）。然后我们设置一个变量`message`来跟踪用户输入的任何值。我们将`message`定义为空字符串`""`，以便Python第一次到达`while`行时有一些东西可以检查。程序第一次运行且Python到达`while`语句时，它需要将`message`的值与`'quit'`进行比较，但此时尚未输入任何用户输入。如果Python没有可比较的东西，它将无法继续运行程序。为了解决这个问题，我们确保赋予`message`一个初始值。虽然它只是一个空字符串，但这对Python来说是有意义的，并允许它执行使`while`循环工作的比较。只要`message`的值不是`'quit'`，这个`while`循环就会持续运行。

第一次循环时，`message`只是一个空字符串，因此Python进入循环。在`message = input(prompt)`处，Python显示提示词并等待用户输入。无论他们输入什么，都被赋值给`message`并打印；然后，Python重新评估`while`语句中的条件。只要用户没有输入单词`'quit'`，就会再次显示提示词，Python等待更多输入。当用户最终输入`'quit'`时，Python停止执行`while`循环，程序结束：

```text
Tell me something, and I will repeat it back to you:
Enter 'quit' to end the program. **Hello everyone!**
Hello everyone!

Tell me something, and I will repeat it back to you:
Enter 'quit' to end the program. **Hello again.**
Hello again.

Tell me something, and I will repeat it back to you:
```


输入 'quit' 以结束程序。**quit**
quit

这个程序运行良好，只是它把单词 'quit' 当作实际消息打印出来。简单的 if 测试可以修复这个问题：

```
prompt = "\nTell me something, and I will repeat it back to you:"
prompt += "\nEnter 'quit' to end the program. "

message = ""
while message != 'quit':
    message = input(prompt)

    if message != 'quit':
        print(message)
```

现在程序在显示消息前会进行快速检查，只有当消息不匹配 quit 值时才打印消息：

```
Tell me something, and I will repeat it back to you:
Enter 'quit' to end the program. **Hello everyone!**
Hello everyone!

Tell me something, and I will repeat it back to you:
Enter 'quit' to end the program. **Hello again.**
Hello again.

Tell me something, and I will repeat it back to you:
Enter 'quit' to end the program. **quit**
```

## 使用标志

在前面的示例中，我们让程序在给定条件为真时执行某些任务。但对于更复杂的程序，如果有许多不同的事件可能导致程序停止运行怎么办？

例如，在游戏中，有几种不同的事件可以结束游戏。当玩家用完飞船、时间用完，或他们应该保护的城市全部被摧毁时，游戏应该结束。只要其中任一事件发生，游戏就需要结束。如果可能有许多事件会导致程序停止，尝试在一个 `while` 语句中测试所有这些条件就会变得复杂且困难。

对于应仅在许多条件都为真时运行的程序，可以定义一个变量来决定整个程序是否处于活动状态。这个变量称为 *flag*，它充当程序的信号。我们可以编写程序使其在标志设置为 `True` 时运行，并在任何几个事件将标志值设置为 `False` 时停止运行。因此，我们的整体 `while` 语句只需要检查一个条件：标志当前是否为 `True`。然后，我们所有的其他测试（查看是否发生了应将标志设置为 `False` 的事件）都可以整齐地组织在程序的其他部分。

让我们为上一节中的 *parrot.py* 添加一个标志。我们将这个标志称为 `active`（你也可以叫它任何名字），它将监控程序是否应继续运行：

```
```python
prompt = "\nTell me something, and I will repeat it back to you:"
prompt += "\nEnter 'quit' to end the program. "

active = True
while active:
    message = input(prompt)

    if message == 'quit':
        active = False
    else:
        print(message)
```
```

我们将变量 `active` 设置为 `True`，以便程序以活动状态开始。这样做使 `while` 语句更简单，因为 `while` 语句本身不进行比较；逻辑在程序的其他部分处理。只要 `active` 变量保持为 `True`，循环就会继续运行 [1]。

在 `while` 循环内部的 `if` 语句中，我们在用户输入后检查 `message` 的值。如果用户输入 'quit'，我们将 `active` 设置为 `False`，`while` 循环停止。如果用户输入除 'quit' 之外的任何内容，我们将输入作为消息打印出来。

此程序的输出与我们在 `while` 语句中直接放置条件测试的先前示例相同。但现在我们有一个标志来指示整个程序是否处于活动状态，很容易添加更多测试（例如 `elif` 语句）用于应导致 `active` 成为 `False` 的事件。这在像游戏这样复杂的程序中很有用，其中可能有许多事件应各自使程序停止运行。当任何这些事件导致活动标志变为 `False` 时，主游戏循环将退出，可以显示游戏结束消息，并可让玩家选择是否再次游戏。

### 使用 break 退出循环

要立即退出 `while` 循环而不运行循环中剩余的任何代码，无论条件测试的结果如何，请使用 `break` 语句。`break` 语句控制程序的流程；你可以使用它来控制执行哪些代码行以及哪些不执行，以便程序仅在你想要时执行你想要的代码。

例如，考虑一个询问用户他们去过哪些地方的程序。我们可以通过在用户输入 'quit' 值时立即调用 break 来停止此程序中的 `while` 循环：

# cities.py

```
prompt = "\nPlease enter the name of a city you have visited:"
prompt += "\n(Enter 'quit' when you are finished.) "

while True:
    city = input(prompt)

    if city == 'quit':
        break
    else:
        print(f"I'd love to go to {city.title()}!")
```

以 while True ① 开始的循环将无限运行，除非它到达 break 语句。此程序中的循环会继续询问用户他们去过的城市名称，直到他们输入 'quit'。当他们输入 'quit' 时，break 语句运行，导致 Python 退出循环：

```
Please enter the name of a city you have visited:
(Enter 'quit' when you are finished.) New York
I'd love to go to New York!

Please enter the name of a city you have visited:
(Enter 'quit' when you are finished.) San Francisco
I'd love to go to San Francisco!

Please enter the name of a city you have visited:
(Enter 'quit' when you are finished.) quit
```

### NOTE

你可以在 Python 的任何循环中使用 break 语句。例如，你可以使用 break 退出正在遍历列表或字典的 for 循环。

## 在循环中使用 continue

而不是完全跳出循环而不执行其余代码，你可以使用 `continue` 语句基于条件测试的结果返回到循环的开头。例如，考虑一个从 1 数到 10 但只打印该范围内奇数的循环：

# counting.py

```
current_number = 0
while current_number < 10:
    current_number += 1
    if current_number % 2 == 0:
        continue

    print(current_number)
```

首先，我们将 `current_number` 设置为 0。因为它小于 10，Python 进入 `while` 循环。进入循环后，我们将计数增加 1 ①，因此 `current_number` 为 1。`if` 语句然后检查 `current_number` 和 2 的模数。如果模数为 0（这意味着 `current_number` 可被 2 整除），`continue` 语句告诉 Python 忽略循环的其余部分并返回开头。如果当前数字不可被 2 整除，则执行循环的其余部分，Python 打印当前数字：

```
1
3
5
7
9
```

## 避免无限循环

每个 `while` 循环都需要一种停止运行的方式，否则它将永远继续运行。例如，这个计数循环应从 1 数到 5：

***counting.py***

```
```python
x = 1
while x <= 5:
    print(x)
    x += 1
```
```

然而，如果你不小心省略了行 x += 1，循环将永远运行：

```
```python
# This loop runs forever!
x = 1
while x <= 5:
    print(x)
```
```

现在 x 的值将从 1 开始但永远不会改变。因此，条件测试 x <= 5 将始终计算为 True，while 循环将永远运行，打印一系列 1，如下所示：

```
```
1
1
1
1
--snip--
```
```

每个程序员偶尔都会不小心写出无限 while 循环，尤其是在程序的循环具有微妙的退出条件时。如果你的程序陷入无限循环，请按 CTRL-C 或直接关闭显示程序输出的终端窗口。

为了避免编写无限循环，请测试每个 while 循环并确保循环在你期望时停止。如果你希望程序在用户输入特定输入值时结束，请运行程序并输入该值。如果程序没有结束，请仔细检查程序处理应导致循环退出的值的方式。确保至少



程序的某个部分可以使循环条件变为 `False`，或者使程序执行到 `break` 语句。

> **注意**
>
> *VS Code 和许多编辑器一样，会在嵌入式终端窗口中显示输出。要取消无限循环，请确保在按下 CTRL-C 之前先点击编辑器的输出区域。*

## 亲自尝试

- **7-4. 披萨配料：**编写一个循环，提示用户输入一系列披萨配料，直到用户输入`'quit'`为止。每当用户输入一种配料时，打印一条消息，说明你会将该配料添加到他们的披萨上。
- **7-5. 电影票：**电影院会根据观众的年龄收取不同的票价。3 岁以下免费；3 到 12 岁收费 10 美元；12 岁以上收费 15 美元。编写一个循环，询问用户的年龄，然后告诉他们电影票的价格。
- **7-6. 三种退出方式：**编写练习 7-4 或 7-5 的不同版本，并至少分别完成以下操作一次：
  - 在`while`语句中使用条件测试来停止循环。
  - 使用一个活动变量控制循环运行的时长。
  - 当用户输入`'quit'`时，使用`break`语句退出循环。
- **7-7. 无限循环：**编写一个永不结束的循环，并运行它。（要结束循环，请按 CTRL-C，或关闭显示输出的窗口。）

## 在列表和字典中使用 while 循环

到目前为止，我们一次只处理一条用户信息。我们接收用户的输入，然后打印该输入或对输入的响应。下一次执行`while`

运行一个`while`循环，直到`'cat'`不再位于列表中，如下所示：

```
**pets.py**

```python
pets = ['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat']
print(pets)

while 'cat' in pets:
    pets.remove('cat')

print(pets)
```
```

我们从一个包含多个`'cat'`实例的列表开始。打印列表后，Python 进入`while`循环，因为它至少在列表中找到了一次值`'cat'`。进入循环后，Python 删除第一个`'cat'`实例，返回到`while`所在行；当它发现列表中仍然有`'cat'`时，再次进入循环。它会逐个删除每个`'cat'`，直到列表中不再存在该值。此时，Python 退出循环，并再次打印列表：

```
```text
['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat']
['dog', 'dog', 'goldfish', 'rabbit']
```
```

## 使用用户输入填充字典

在`while`循环的每次执行过程中，你可以根据需要提示用户输入任意多的信息。下面我们来编写一个民意调查程序，在循环的每次执行过程中，提示参与者输入姓名和回答。我们会将收集到的数据存储在字典中，因为我们希望将每个回答与特定用户关联起来：

```
**mountain_poll.py**

```python
responses = {}
# 设置一个标志，表示调查正在进行。
polling_active = True
```

```
while polling_active:
    # 提示用户输入姓名和回答。
    name = input("\nWhat is your name? ")
    response = input("Which mountain would you like to climb someday? ")

    # 将回答存储在字典中。
    responses[name] = response

    # 了解是否还有其他人要参加调查。
    repeat = input("Would you like to let another person respond? (yes/ no) ")
    if repeat == 'no':
        polling_active = False

# 调查结束。显示结果。
print("\n--- Poll Results ---")
for name, response in responses.items():
    print(f"{name} would like to climb {response}.")
```

程序首先定义一个空字典（responses），并设置一个标志（polling_active），表示调查正在进行。只要 polling_active 为 True，Python 就会运行`while`循环中的代码。

在循环中，程序提示用户输入姓名以及他们将来想攀登的山峰 ①。该信息被存储在 responses 字典中 ②，然后程序询问用户是否要继续进行调查 ③。如果用户输入 yes，程序会再次进入`while`循环。如果用户输入 no，polling_active 标志就会被设置为 False，`while`循环停止运行，最后的代码块 ④ 会显示调查结果。

如果运行该程序并输入示例回答，你应该会看到类似下面的输出：

```
What is your name? Eric
Which mountain would you like to climb someday? Denali
Would you like to let another person respond? (yes/ no) yes
```

```
What is your name? **Lynn**
Which mountain would you like to climb someday? **Devil's Thumb**
Would you like to let another person respond? (yes/ no) **no**

--- Poll Results ---
Eric would like to climb Denali.
Lynn would like to climb Devil's Thumb.
```

## 亲自尝试

- **7-8. 熟食店：**创建一个名为`sandwich_orders`的列表，并在其中填入各种三明治的名称。然后创建一个名为`finished_sandwiches`的空列表。遍历三明治订单列表，并为每个订单打印一条消息，例如`I made your tuna sandwich.`。每制作好一个三明治，就将它移动到已完成三明治列表中。所有三明治制作完成后，打印一条消息，列出制作完成的每种三明治。
- **7-9. 没有熏牛肉：**使用练习 7-8 中的`sandwich_orders`列表，确保`'pastrami'`至少在列表中出现三次。在程序开头附近添加代码，打印一条消息，说明熟食店的熏牛肉已经售罄；然后使用`while`循环从`sandwich_orders`中删除所有`'pastrami'`。确保`finished_sandwiches`中不会出现熏牛肉三明治。
- **7-10. 梦想中的度假：**编写一个程序，调查用户梦想中的度假地点。编写一个类似于 _如果你可以去世界上的一个地方，你会去哪里？_ 的提示语。添加一个代码块，用于打印调查结果。

## 总结

在本章中，你学习了如何使用`input()`让用户在程序中提供自己的信息。你学习了如何处理文本输入和数字输入，以及如何使用`while`循环让程序按照用户的意愿持续运行。你了解了通过设置活动标志、使用`break`语句和使用`continue`语句来控制`while`循环流程的几种方法。你学习了如何使用`while`循环将项目从一个列表移动到另一个列表，以及如何从列表中删除某个值的所有实例。你还学习了如何将`while`循环与字典结合使用。

在第 8 章中，你将学习函数。*函数*可以将程序拆分成多个小部分，每个部分只完成一项特定任务。你可以根据需要多次调用一个函数，也可以将函数存储在单独的文件中。通过使用函数，你将能够编写更高效、更容易排查和维护的代码，并且这些代码还可以在许多不同的程序中重复使用。

[OceanofPDF.com](OceanofPDF.com)
