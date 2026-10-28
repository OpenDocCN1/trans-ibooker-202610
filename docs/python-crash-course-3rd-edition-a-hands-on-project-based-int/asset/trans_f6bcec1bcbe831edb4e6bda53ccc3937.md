
当你第一次使用循环时，请记住，无论列表中有多少项，这组步骤都会针对列表中的每一项重复执行一次。如果你的列表中有100万个项，Python会重复这些步骤100万次——而且通常非常快。

另外，在编写你自己的`for`循环时，请记住，你可以为与列表中每个值关联的临时变量选择任意名称。不过，选择一个能表示列表中单个项的有意义的名称会很有帮助。例如，下面是针对猫列表、狗列表和通用项目列表启动`for`循环的好方法：

```python
for cat in cats:
for dog in dogs:
for item in list_of_items:
```

这些命名约定可以帮助你跟踪`for`循环中对每个项执行的操作。使用单数和复数名称可以帮助你识别一段代码是在处理列表中的单个元素还是整个列表。

## 在 for 循环中做更多工作

你可以对`for`循环中的每个项执行几乎任何操作。让我们在前一个示例的基础上，向每位魔术师打印一条消息，告诉他们表演了一个很棒的魔术：

**magicians.py**

```python
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
```

这段代码的唯一区别在于，我们为每位魔术师编写了一条以该魔术师姓名开头的消息。第一次循环时，`magician`的值是`'alice'`，因此Python以名字`'Alice'`开头。第二次循环时，消息将以`'David'`开头；第三次循环时，消息将以`'Carolina'`开头。

输出显示了对列表中每位魔术师的个性化消息：

```text
Alice, that was a great trick!
David, that was a great trick!
Carolina, that was a great trick!
```

你还可以在`for`循环中编写任意多行代码。跟在`for magician in magicians`这一行后面的每个缩进行都被视为*循环内部*，并且每个缩进行都会针对列表中的每个值执行一次。因此，你可以对列表中的每个值做尽可能多的工作。

让我们在消息中添加第二行，告诉每位魔术师我们期待他们的下一个魔术：

```python
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
    print(f"I can't wait to see your next trick, {magician.title()}.\n")
```

因为我们对两次`print()`调用都进行了缩进，所以每一行都会针对列表中的每一位魔术师执行一次。第二个`print()`调用中的换行符（`"\n"`）会在每次循环结束后插入一个空行。这样可以为列表中的每个人创建一组整齐排列的消息：

```text
Alice, that was a great trick!
I can't wait to see your next trick, Alice.

David, that was a great trick!
I can't wait to see your next trick, David.

Carolina, that was a great trick!
I can't wait to see your next trick, Carolina.
```

你可以在for循环中使用任意多行。实际上，你通常会发现，使用for循环时，对列表中的每个项进行一些不同的操作会很有用。

## for 循环结束后要做的事

当for循环执行完毕后会发生什么？通常，你会想要总结一段输出，或者继续执行程序必须完成的其他工作。

for循环之后任何未缩进的代码行都会执行一次，不会重复。让我们向整个魔术师团队写一封感谢信，感谢他们奉献了一场精彩的表演。要在所有单独消息打印完之后显示这条团队消息，我们将感谢信放在for循环之后，且不缩进：

```python
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
    print(f"I can't wait to see your next trick, {magician.title()}.\n")

print("Thank you, everyone. That was a great magic show!")
```

前两次`print()`调用针对列表中的每一位魔术师重复执行一次，正如你之前看到的那样。但是，由于最后一行没有缩进，它只打印一次：

```text
Alice, that was a great trick!
I can't wait to see your next trick, Alice.

David, that was a great trick!
I can't wait to see your next trick, David.

Carolina, that was a great trick!
I can't wait to see your next trick, Carolina.

Thank you, everyone. That was a great magic show!
```

当你使用for循环处理数据时，你会发现这是总结对整个数据集执行的操作的好方法。例如，你可能会使用`for`循环遍历角色列表并在屏幕上显示每个角色来初始化游戏。然后，你可能会在此循环之后编写一些额外的代码，在所有角色绘制到屏幕上后显示一个*立即开始*按钮。

## 避免缩进错误

Python使用缩进来确定一行或一组行与程序其余部分的关系。在前面的示例中，向个别魔术师打印消息的行是for循环的一部分，因为它们被缩进了。Python对缩进的使用使代码非常易于阅读。基本上，它利用空白强制你编写格式整洁、视觉结构清晰的代码。在较长的Python程序中，你会注意到一些代码块在不同级别缩进。这些缩进级别有助于你大致了解整个程序的组织结构。

当你开始编写依赖正确缩进的代码时，需要注意几个常见的*缩进错误*。例如，人们有时会缩进不需要缩进的代码行，或者忘记缩进需要缩进的代码行。现在看看这些错误的示例，将有助于你将来避免它们，并在它们出现在你自己的程序中时纠正它们。

```python
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
print(f"I can't wait to see your next trick, {magician.title()}.\n")
```

第二个`print()`调用本应缩进，但因为Python在`for`语句之后发现至少一个缩进行，所以它不会报告错误。因此，第一个`print()`调用针对列表中的每个名字执行一次，因为它被缩进了。第二个`print()`调用没有缩进，所以它只在循环运行完成后执行一次。由于与`magician`关联的最终值是`'carolina'`，因此只有她收到了“期待下一个魔术”的消息：

```text
Alice, that was a great trick!
David, that was a great trick!
Carolina, that was a great trick!
I can't wait to see your next trick, Carolina.
```

这是一个*逻辑错误*。语法是有效的Python代码，但代码没有产生预期的结果，因为其逻辑中出现了问题。如果你期望某个操作对列表中的每个项重复执行一次，但它只执行了一次，请判断你是只需要缩进一行还是一组行。

### 不必要的缩进

如果你不小心缩进了不需要缩进的行，Python会告知你意外的缩进：

**hello_world.py**

```python
message = "Hello Python world!"
    print(message)
```

我们不需要缩进`print()`调用，因为它不是循环的一部分；因此，Python会报告该错误：

```text
File "hello_world.py", line 2
    print(message)
    ^
IndentationError: unexpected indent
```

你可以避免意外的缩进错误，方法是只在有特定原因时才缩进。在目前你正在编写的程序中，唯一应该缩进的行是你希望在`for`循环中为每个项重复执行的操作。

### 循环结束后不必要的缩进

如果你不小心缩进了本应在循环结束后运行的代码，该代码将对列表中的每个项重复执行一次。有时这会促使Python报告错误，但通常这会导致逻辑错误。

例如，让我们看看当我们不小心缩进了感谢魔术师团队奉献了一场精彩表演的那一行时会发生什么：

**magicians.py**

```python
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
    print(f"I can't wait to see your next trick, {magician.title()}.\n")

    print("Thank you everyone, that was a great magic show!")
```

因为最后一行❶被缩进了，所以它会针对列表中的每个人打印一次。
