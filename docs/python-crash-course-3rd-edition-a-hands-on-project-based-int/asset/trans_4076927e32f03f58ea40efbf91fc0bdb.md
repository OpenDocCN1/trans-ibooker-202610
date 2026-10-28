
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
