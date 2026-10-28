

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

# 8
函数

![](img/8f15563c16d2a98c4d3115150679b092_234_0.png)

在本章中，你将学习编写*函数*。函数是带有名称的代码块，旨在完成某一项特定任务。当你想执行在函数中定义的某项任务时，就可以*调用*负责该任务的函数。如果需要在程序中多次执行该任务，就不必一遍又一遍地重新输入同样的代码；只需调用专门处理该任务的函数即可，而这次调用会告诉 Python 运行函数内部的代码。你会发现，使用函数可以让程序更容易编写、阅读、测试和修复。

在本章中，你还将学习向函数传递信息的各种方式。你将学习如何编写主要用于显示信息的函数，以及如何编写用于处理数据并返回一个值或一组值的其他函数。最后，你将学习如何将函数存储在名为*模块*的独立文件中，从而帮助组织主程序文件。

## 定义函数

下面是一个名为`greet_user()`的简单函数，它会打印一条问候语：

```
greeter.py

```python
def greet_user():
    """Display a simple greeting."""
    print("Hello!")


greet_user()
```
```

这个示例展示了函数最简单的结构。第一行使用关键字`def`告诉 Python 你正在定义一个函数。这就是*函数定义*，它告诉 Python 函数的名称，以及函数完成任务时（如果需要）所需的信息。括号中包含这些信息。在本例中，函数名是`greet_user()`，它不需要任何信息就能完成任务，因此括号为空。（即便如此，括号仍然是必需的。）最后，函数定义以冒号结尾。

紧跟在`def greet_user():`之后的所有缩进行都构成函数的*函数体*。第二行中的文本是一条称为*文档字符串*的注释，用于描述函数的功能。当 Python 为程序中的函数生成文档时，它会查找紧跟在函数定义之后的字符串。这些字符串是

