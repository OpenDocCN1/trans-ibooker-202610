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


通常用三引号括起来，这样可以编写多行内容。

行 `print("Hello!")` 是这个函数体中唯一的一行实际代码，所以 `greet_user()` 只有一项工作：`print("Hello!")`。

当你想使用这个函数时，必须调用它。*函数调用*会告诉 Python 执行函数中的代码。要*调用*函数，需要写出函数的名字，并在括号中提供任何必要的信息。由于这里不需要任何信息，调用我们的函数只需输入 `greet_user()`。正如预期，它会打印 Hello!：

```
Hello!
```

## 向函数传递信息

如果稍微修改函数 `greet_user()`，它就能按名字问候用户。要让函数做到这一点，你需要在 `def greet_user()` 的函数定义括号中输入 `username`。在这里添加 `username`，函数就可以接受你指定的任何 `username` 值。现在，每次调用该函数时，都需要为 `username` 提供一个值。调用 `greet_user()` 时，你可以在括号内传入一个名字，例如 'jesse'：

```
def greet_user(username):
    """Display a simple greeting."""
    print(f"Hello, {username.title()}!")

greet_user('jesse')
```

输入 `greet_user('jesse')` 会调用 `greet_user()`，并为函数提供执行 `print()` 调用所需的信息。函数接受你传入的名字，并显示对该名字的问候：

## 动手试一试

**8-1. 消息：** 编写一个名为 `display_message()` 的函数，它打印一句告诉每个人你在本章学习什么的话。调用该函数，并确保消息正确显示。

**8-2. 最喜欢的书：** 编写一个名为 `favorite_book()` 的函数，它接受一个参数 `title`。该函数应打印一条消息，例如 `One of my favorite books is Alice in Wonderland`。调用该函数，并确保在函数调用中包含一个书名作为实参。

## 传递实参

由于函数定义可以有多个形参，因此函数调用可能需要多个实参。你可以通过多种方式向函数传递实参：位置实参（需要与形参的编写顺序一致）、关键字实参（每个实参由一个变量名和一个值组成），以及值的列表和字典。接下来我们逐一介绍。

## 位置实参

当你调用函数时，Python 必须将函数调用中的每个实参与函数定义中的形参匹配。最简单的方法是基于所提供实参的顺序。以这种方式匹配的值称为位置实参。

为了理解其工作原理，来看一个显示宠物信息的函数。该函数会告诉我们每只宠物是什么动物以及宠物的名字，如下所示：

*pets.py*

```
❶ def describe_pet(animal_type, pet_name):
    """Display information about a pet."""
```

```
print(f"\nI have a {animal_type}.")
print(f"My {animal_type}'s name is {pet_name.title()}.")
```

+   2. `describe_pet('hamster', 'harry')`

这个定义显示，函数需要一种动物类型和动物的名字❶。当我们调用 describe_pet() 时，需要按顺序提供动物类型和名字。例如，在函数调用中，实参 'hamster' 被赋给形参 animal_type，实参 'harry' 被赋给形参 pet_name❷。在函数体中，这两个形参用于显示所描述宠物的信息。

输出描述了一只名为 Harry 的仓鼠：

```
I have a hamster.
My hamster's name is Harry.
```

## 多次函数调用

你可以根据需要多次调用函数。描述第二只不同的宠物只需再调用一次 describe_pet()：

```
def describe_pet(animal_type, pet_name):
    """Display information about a pet."""
    print(f"\nI have a {animal_type}.")
    print(f"My {animal_type}'s name is {pet_name.title()}.")

describe_pet('hamster', 'harry')
describe_pet('dog', 'willie')
```

在第二个函数调用中，我们向 describe_pet() 传递实参 'dog' 和 'willie'。与之前使用的实参一样，Python 将 'dog' 与形参 animal_type 匹配，将 'willie' 与形参 pet_name 匹配。和之前一样，函数完成了它的工作，但这次

打印的是名为 Willie 的狗的信息。现在我们有一只名为 Harry 的仓鼠和一只名为 Willie 的狗：

```
I have a hamster.
My hamster's name is Harry.

I have a dog.
My dog's name is Willie.
```

多次调用函数是一种非常高效的工作方式。描述宠物的代码在函数中只编写一次。之后，任何时候你想描述一只新宠物，只需用新宠物的信息调用该函数。即使描述宠物的代码扩展到 10 行，你仍然可以通过再次调用函数，仅用一行代码描述一只新宠物。

## 位置实参的顺序很重要

使用位置实参时，如果混淆了函数调用中实参的顺序，可能会得到意想不到的结果：

```
def describe_pet(animal_type, pet_name):
    """Display information about a pet."""
    print(f"\nI have a {animal_type}.")
    print(f"My {animal_type}'s name is {pet_name.title()}.")

describe_pet('harry', 'hamster')
```

在这个函数调用中，我们先列出名字，再列出动物类型。由于这次实参 'harry' 排在第一位，因此该值被赋给了形参 animal_type。同样，'hamster' 被赋给了 pet_name。现在我们有一只名为“Hamster”的“harry”：

```
I have a harry.
My harry's name is Hamster.
```

如果你得到这样奇怪的结果，请检查函数调用中实参的顺序是否与函数定义中形参的顺序一致。

## 关键字实参

*关键字实参*是传递给函数的一个名称-值对。你在实参中直接将名称和值关联起来，因此在将实参传递给函数时不会产生混淆（你不会得到一只名为 Hamster 的 harry）。关键字实参让你不必担心函数调用中实参的正确顺序，并且明确了函数调用中每个值的作用。

让我们使用关键字实参来重写 *pets.py*，以调用 `describe_pet()`：

```
```python
def describe_pet(animal_type, pet_name):
    """Display information about a pet."""
    print(f"\nI have a {animal_type}.")
    print(f"My {animal_type}'s name is {pet_name.title()}.")

describe_pet(animal_type='hamster', pet_name='harry')
```
```

函数 `describe_pet()` 并没有改变。但在调用该函数时，我们明确告诉 Python 每个实参应匹配哪个形参。当 Python 读取函数调用时，它知道将实参 `'hamster'` 赋给形参 `animal_type`，将实参 `'harry'` 赋给 `pet_name`。输出正确显示我们有一只名为 Harry 的仓鼠。

关键字实参的顺序无关紧要，因为 Python 知道每个值应该放在哪里。下面两个函数调用是等价的：

```
```python
describe_pet(animal_type='hamster', pet_name='harry')
describe_pet(pet_name='harry', animal_type='hamster')
```
```

注意

> 使用关键字实参时，请务必使用函数定义中形参的确切名称。

## 默认值

编写函数时，可以为每个形参定义*默认值*。如果在函数调用中为形参提供了实参，Python 将使用该实参值；否则，它将使用形参的默认值。因此，当你为形参定义默认值时，可以省略通常在函数调用中编写的相应实参。使用默认值可以简化函数调用，并明确函数的典型使用方式。

例如，如果你注意到大多数对 `describe_pet()` 的调用都是在描述狗，就可以将 `animal_type` 的默认值设置为 `'dog'`。现在，任何为狗调用 `describe_pet()` 的人都可以省略该信息：

```
def describe_pet(pet_name, animal_type='dog'):
    """Display information about a pet."""
    print(f"\nI have a {animal_type}.")
    print(f"My {animal_type}'s name is {pet_name.title()}.")

describe_pet(pet_name='willie')
```

我们修改了 `describe_pet()` 的定义，为 `animal_type` 添加了默认值 `'dog'`。现在，当调用函数时没有指定 `animal_type`，Python 就会知道对该形参使用值 `'dog'`：

```
I have a dog.
My dog's name is Willie.
```


请注意，函数定义中参数的顺序必须改变。因为默认值使得在调用函数时无需指定动物类型，函数调用中唯一剩下的参数就是宠物的名字。Python 仍会将其解释为位置实参，因此如果只用宠物名字来调用函数，该实参会与函数定义中列出的第一个形参匹配。这就是第一个形参需要是 `pet_name` 的原因。

现在使用这个函数最简单的方式，就是在函数调用中只提供狗的名字：

```
describe_pet('willie')
```

这个函数调用的输出与之前的示例相同。唯一的实参是 `'willie'`，所以它会与定义中的第一个形参 `pet_name` 匹配。由于没有为 `animal_type` 提供实参，Python 会使用默认值 `'dog'`。

要描述狗以外的动物，你可以使用这样的函数调用：

```
describe_pet(pet_name='harry', animal_type='hamster')
```

由于为 `animal_type` 提供了显式实参，Python 会忽略该形参的默认值。

> **注意**
> 
> *使用默认值时，任何带默认值的形参都必须列在所有不带默认值的形参之后。这样 Python 才能继续正确解释位置实参。*

## 等价的函数调用

由于位置实参、关键字实参和默认值可以同时使用，你通常会有几种等价的函数调用方式。考虑下面这个带有一个默认值的 `describe_pet()` 定义：

```
```python
def describe_pet(pet_name, animal_type='dog'):
```
```

对于这个定义，始终需要为 `pet_name` 提供实参，这个值可以使用位置格式或关键字格式提供。如果所描述的动物不是狗，则必须在调用中包含 `animal_type` 的实参，这个实参也可以使用位置格式或关键字格式指定。

以下所有调用都适用于这个函数：

```
```python
# 一只名叫 Willie 的狗。
describe_pet('willie')
describe_pet(pet_name='willie')

# 一只名叫 Harry 的仓鼠。
describe_pet('harry', 'hamster')
describe_pet(pet_name='harry', animal_type='hamster')
describe_pet(animal_type='hamster', pet_name='harry')
```
```

这些函数调用中的每一个都会产生与前面示例相同的输出。

你使用哪种调用风格其实并不重要。只要你的函数调用能产生你想要的输出，就使用你觉得最容易理解的风格。

### 避免实参错误

当你开始使用函数时，遇到关于实参不匹配的错误不要感到意外。当提供的实参少于或多于函数完成工作所需的数量时，就会出现实参不匹配的情况。例如，如果我们尝试不带任何实参调用 `describe_pet()`，会发生如下情况：

```
def describe_pet(animal_type, pet_name):
    """显示宠物的信息。"""
    print(f"\nI have a {animal_type}.")
    print(f"My {animal_type}'s name is {pet_name.title()}.")

describe_pet()
```

Python 会识别出函数调用中缺少一些信息，回溯信息会告诉我们：

```
Traceback (most recent call last):
①   File "pets.py", line 6, in <module>
②       describe_pet()
        ^^^^^^^^^^^^^^
③ TypeError: describe_pet() missing 2 required positional arguments:
        'animal_type' and 'pet_name'
```

回溯首先告诉我们问题的位置❶，以便我们回过头来看到函数调用中出了什么问题。接下来，违规的函数调用被写出来供我们查看❷。最后，回溯告诉我们调用缺少两个实参，并报告了缺失实参的名称❸。如果这个函数在单独的文件中，我们很可能无需打开该文件并阅读函数代码就能正确重写这个调用。

Python 很有帮助，它会为我们读取函数代码并告诉我们需要提供的实参名称。这也是为变量和函数起描述性名称的另一个动机。如果你这样做了，Python 的错误信息对你和任何可能使用你代码的人来说都会更有用。

如果你提供了太多实参，你应该会得到类似的回溯信息，这可以帮助你正确地将函数调用与函数定义匹配起来。

## 动手试一试

**8-3. T 恤：** 编写一个名为 `make_shirt()` 的函数，它接受一个尺码以及应印在 T 恤上的文字。该函数应打印一个句子，概述 T 恤的尺码和印在其上的文字。

使用位置实参调用该函数一次来制作一件 T 恤。使用关键字实参再次调用该函数。

**8-4. 大号 T 恤：** 修改 `make_shirt()` 函数，使 T 恤默认是大号，并印有 *I love Python* 的文字。使用默认文字制作一件大号 T 恤和一件中号 T 恤，并使用不同的文字制作一件任意尺码的 T 恤。

**8-5. 城市：** 编写一个名为 `describe_city()` 的函数，它接受一个城市名称及其所属国家。该函数应打印一个简单的句子，例如 Reykjavik is in Iceland。为国家参数设置默认值。调用你的函数处理三个不同的城市，其中至少有一个不在默认国家内。

## 返回值

函数不必总是直接显示其输出。相反，它可以处理一些数据，然后返回一个值或一组值。函数返回的值称为*返回值*。`return` 语句从函数内部取出一个值，并将其发送回调用函数的那一行。返回值允许你将程序中的大量繁重工作移到函数中，从而简化程序主体。

### 返回简单值

让我们看一个接受名和姓，并返回格式整洁的完整姓名的函数：

```
```python
def get_formatted_name(first_name, last_name):
    """返回整洁格式的完整姓名。"""
    full_name = f"{first_name} {last_name}"
    return full_name.title()

musician = get_formatted_name('jimi', 'hendrix')
print(musician)
```
```

`get_formatted_name()` 的定义将名和姓作为形参。该函数将这两个名字组合起来，在它们之间添加一个空格，并将结果赋给 `full_name`❶。`full_name` 的值被转换为标题格式，然后返回给调用行❷。

当你调用一个返回值的函数时，你需要提供一个变量来接收该返回值。在这个例子中，返回值被赋给变量 `musician`❸。输出显示了一个由人名各部分组成的格式整洁的姓名：

```
```
Jimi Hendrix
```
```

这可能看起来为了得到一个格式整洁的姓名要做很多工作，而我们本来可以直接写：

```
```python
print("Jimi Hendrix")
```
```

然而，当你考虑一个需要分别存储大量名和姓的大型程序时，像 `get_formatted_name()` 这样的函数就变得非常有用。你可以分别存储名和姓，然后在需要显示完整姓名时调用这个函数。

## 让实参变为可选

有时让实参变为可选的很有意义，这样使用函数的人可以只在需要时才选择提供额外信息。你可以使用默认值来让实参变为可选。

例如，假设我们想要扩展 `get_formatted_name()` 以处理中间名。第一次尝试包含中间名可能看起来像这样：

```
def get_formatted_name(first_name, middle_name, last_name):
    """返回整洁格式的完整姓名。"""
    full_name = f"{first_name} {middle_name} {last_name}"
    return full_name.title()

musician = get_formatted_name('john', 'lee', 'hooker')
print(musician)
```

这个函数在给定名、中间名和姓时能正常工作。函数接收名字的三个部分，然后用它们构建一个字符串。函数在适当的位置添加空格，并将完整姓名转换为标题格式：

```
John Lee Hooker
```

但中间名并不总是需要的，而这个按原样编写的函数如果只给定名和姓来调用，就无法工作。要使中间名变为可选，我们可以给 `middle_name` 形参一个空的默认值，并在用户未提供值的情况下忽略该实参。要让 `get_formatted_name()` 在没有中间名的情况下也能工作，我们将 `middle_name` 的默认值设置为空字符串，并将其移到形参列表的末尾：

```
def get_formatted_name(first_name, last_name, middle_name=''):
    """返回整洁格式的完整姓名。"""
    if middle_name:
        full_name = f"{first_name} {middle_name} {last_name}"





```
else:
    full_name = f"{first_name} {last_name}"
    return full_name.title()

musician = get_formatted_name('jimi', 'hendrix')
print(musician)
```

```
musician = get_formatted_name('john', 'hooker', 'lee')
print(musician)
```

在这个示例中，姓名由三个可能的部分组成。因为名和姓总是必需的，所以这两个参数列在函数定义的最前面。中间名是可选的，因此列在定义的最后，其默认值为空字符串。

在函数体中，我们检查是否提供了中间名。Python 将非空字符串解释为 True，因此如果函数调用中传入了中间名参数，条件测试 `if middle_name` 的结果便为 True ①。如果提供了中间名，就将名、中间名和姓组合起来构成完整姓名。然后将这个姓名转换为标题格式并返回到函数调用行，在那里它被赋给变量 `musician` 并打印出来。如果没有提供中间名，空字符串会使 `if` 测试失败，于是执行 `else` 代码块 ②。完整姓名仅由名和姓构成，格式化后的姓名被返回到调用行，赋给 `musician` 并打印出来。

用名和姓调用这个函数很直接。然而，如果我们要使用中间名，就必须确保中间名作为最后一个实参传入，这样 Python 才能正确地匹配位置实参 ③。

这个修改后的函数既适用于只有名和姓的人，也适用于有中间名的人：

## 可选值让函数能够处理各种使用场景，同时让函数调用尽可能保持简单。

## 返回字典

函数可以返回你需要的任何类型的值，包括列表和字典等更复杂的数据结构。例如，下面的函数接收姓名的各个部分，并返回一个表示人的字典：

### person.py

```
def build_person(first_name, last_name):
    """Return a dictionary of information about a person."""
    person = {'first': first_name, 'last': last_name}
    return person

musician = build_person('jimi', 'hendrix')
print(musician)
```

函数 `build_person()` 接收名和姓，并将这些值放入一个字典 ①。`first_name` 的值与键 `'first'` 一起存储，`last_name` 的值与键 `'last'` 一起存储。然后，返回表示这个人的整个字典 ②。返回值被打印出来 ③，原来的两条文本信息现在存储在一个字典中：

```
{'first': 'jimi', 'last': 'hendrix'}
```

这个函数接收简单的文本信息，并将其放入更有意义的数据结构中，让你能够处理这些信息，而不仅仅是打印它们。字符串

`'jimi'` 和 `'hendrix'` 现在被标记为名和姓。你可以轻松地扩展这个函数，使其接受可选值，如中间名、年龄、职业或你想存储的关于一个人的任何其他信息。例如，下面的修改还允许你存储一个人的年龄：

```python
def build_person(first_name, last_name, age=None):
    """Return a dictionary of information about a person."""
    person = {'first': first_name, 'last': last_name}
    if age:
        person['age'] = age
    return person

musician = build_person('jimi', 'hendrix', age=27)
print(musician)
```

我们在函数定义中添加了一个新的可选参数 `age`，并赋予该参数特殊值 `None`，当变量没有指定具体值时就会使用它。你可以把 `None` 视为一个占位值。在条件测试中，`None` 的求值结果为 False。如果函数调用中包含 `age` 的值，该值就会存储在字典中。这个函数始终会存储一个人的姓名，但也可以修改它以存储你想保存的关于这个人的任何其他信息。

## 在 while 循环中使用函数

你可以将函数与你目前学到的所有 Python 结构结合使用。例如，我们结合 `while` 循环使用 `get_formatted_name()` 函数，以更正式的方式向用户问候。下面是使用用户的名和姓向人们问候的第一次尝试：

**greeter.py**

```python
def get_formatted_name(first_name, last_name):
    """Return a full name, neatly formatted."""
```

```
full_name = f"{first_name} {last_name}"
return full_name.title()

# This is an infinite loop!
while True:
    print("\nPlease tell me your name:")
    f_name = input("First name: ")
    l_name = input("Last name: ")

    formatted_name = get_formatted_name(f_name, l_name)
    print(f"\nHello, {formatted_name}!")
```
```

在这个示例中，我们使用一个不涉及中间名的简单版 `get_formatted_name()`。`while` 循环要求用户输入姓名，我们分别提示输入名和姓 ❶。

但这个 `while` 循环有一个问题：我们没有定义退出条件。当你要求用户进行一系列输入时，把退出条件放在哪里？我们希望用户能够尽可能轻松地退出，因此每个提示都应提供退出的方式。`break` 语句提供了一种在任一提示处直接退出循环的方式：

```python
def get_formatted_name(first_name, last_name):
    """Return a full name, neatly formatted."""
    full_name = f"{first_name} {last_name}"
    return full_name.title()

while True:
    print("\nPlease tell me your name:")
    print("(enter 'q' at any time to quit)")

    f_name = input("First name: ")
    if f_name == 'q':
        break

    l_name = input("Last name: ")
    if l_name == 'q':
        break
```

```
formatted_name = get_formatted_name(f_name, l_name)
print(f"\nHello, {formatted_name}!")
```

我们添加了一条告知用户如何退出的消息，然后如果用户在任一提示处输入退出值，就跳出循环。现在程序会一直向人们问候，直到有人在输入名或姓时输入 q：

```
Please tell me your name:
(enter 'q' at any time to quit)
First name: eric
Last name: matthes

Hello, Eric Matthes!

Please tell me your name:
(enter 'q' at any time to quit)
First name: q
```

## 动手试一试

- **8-6. 城市名：** 编写一个名为 `city_country()` 的函数，它接收城市名及其所属国家名。该函数应返回一个格式如下的字符串：

  ```
  "Santiago, Chile"
  ```

  用至少三对城市—国家调用你的函数，并打印返回的值。

- **8-7. 专辑：** 编写一个名为 `make_album()` 的函数，用于创建一个描述音乐专辑的字典。该函数应接收艺术家名和专辑名，并返回一个包含这两项信息的字典。使用该函数创建三个表示不同专辑的字典。打印每个返回值，以表明字典正确地存储了专辑信息。

  使用 `None` 给 `make_album()` 添加一个可选参数，以便存储专辑中的歌曲数量。如果调用行中包含歌曲数量的值，就将该值添加到专辑字典中。至少再进行一次包含专辑歌曲数量的函数调用。

- **8-8. 用户专辑：** 以练习 8-7 中的程序为基础。编写一个 `while` 循环，让用户输入专辑的艺术家和专辑名。获得这些信息后，用用户的输入调用 `make_album()`，并打印所创建的字典。务必在 `while` 循环中包含退出值。

## 传递列表

你经常会发现将列表传递给函数很有用，无论这个列表是名字、数字还是字典等更复杂的对象。当你将列表传递给函数时，函数可以直接访问列表的内容。让我们使用函数使列表处理更高效。

假设我们有一个用户列表，想向每个人打印问候语。下面的示例将一个名字列表发送给名为 `greet_users()` 的函数，该函数逐个向列表中的每个人问候：

***greet_users.py***





```python
dron']
completed_models = []

# Simulate printing each design, until none are left.
# Move each design to completed_models after printing.
while unprinted_designs:
    current_design = unprinted_designs.pop()
    print(f"Printing model: {current_design}")
    completed_models.append(current_design)

# Display all completed models.
print("\nThe following models have been printed:")
for completed_model in completed_models:
    print(completed_model)
```

此程序以需要打印的设计列表开始，并有一个名为 completed_models 的空列表，每个设计在打印后将被移至该列表。只要 unprinted_designs 中仍有设计，while 循环就会通过从列表末尾移除一个设计、将其存储在 current_design 中并显示当前设计正在打印的消息来模拟打印每个设计。然后它将该设计添加到已完成模型列表中。循环运行结束后，会显示已打印的设计列表：

```text
Printing model: dodecahedron
Printing model: robot pendant
Printing model: phone case

The following models have been printed:
dodecahedron
robot pendant
phone case
```

我们可以通过编写两个函数来重新组织此代码，每个函数执行一项特定工作。大多数代码不会改变；我们只是更仔细地对其进行结构化。第一个函数将处理打印设计，第二个函数将汇总已完成的打印：

```python
def print_models(unprinted_designs, completed_models):
    """
    Simulate printing each design, until none are left.
    Move each design to completed_models after printing.
    """
    while unprinted_designs:
        current_design = unprinted_designs.pop()
        print(f"Printing model: {current_design}")
        completed_models.append(current_design)

def show_completed_models(completed_models):
    """Show all the models that were printed."""
    print("\nThe following models have been printed:")
    for completed_model in completed_models:
        print(completed_model)

unprinted_designs = ['phone case', 'robot pendant', 'dodecahedron']
completed_models = []

print_models(unprinted_designs, completed_models)
show_completed_models(completed_models)
```

我们使用两个参数定义函数 print_models()：需要打印的设计列表和已完成模型列表 ❶。给定这两个列表，该函数通过清空未打印设计列表并填满已完成模型列表来模拟打印每个设计。然后我们使用一个参数定义函数 show_completed_models()：已完成模型列表 ❷。给定此列表，show_completed_models() 会显示已打印的每个模型名称。

此程序的输出与无函数版本相同，但代码更有条理。执行大部分工作的代码已移至两个独立的函数中，这使得程序的主体部分更容易理解。查看程序体，注意你能多么轻松地跟随正在发生的事情：

```python
unprinted_designs = ['phone case', 'robot pendant', 'dodecahedron']
completed_models = []

print_models(unprinted_designs, completed_models)
show_completed_models(completed_models)
```

我们设置了一个未打印设计的列表和一个将保存已完成模型的空列表。然后，由于我们已经定义了两个函数，我们所要做的就是调用它们并传递正确的参数。我们调用 `print_models()` 并传递它所需的两个列表；不出所料，`print_models()` 模拟打印设计。然后我们调用 `show_completed_models()` 并传递已完成模型的列表，以便它报告已打印的模型。描述性的函数名称使其他人能够阅读此代码并理解它，即使没有注释。

此程序比没有函数的版本更容易扩展和维护。如果我们以后需要打印更多设计，我们只需再次调用 `print_models()` 即可。如果我们意识到打印代码需要修改，我们可以一次性更改代码，我们的更改将在函数被调用的每个地方生效。这种技术比在程序的多个地方分别更新代码更高效。

此示例还演示了每个函数应有一项特定工作的思想。第一个函数打印每个设计，第二个函数显示已完成的模型。这比使用一个函数完成两项工作更有益。如果你在编写函数时注意到该函数执行了太多不同的任务，请尝试将代码拆分为两个函数。记住，你可以从另一个函数中调用一个函数，这在将复杂任务拆分为一系列步骤时很有帮助。

## 防止函数修改列表

有时你会希望防止函数修改列表。例如，假设你从未打印设计的列表开始，并编写一个函数将其移至已完成模型的列表，如前例所示。你可能会决定，即使你已打印了所有设计，你也想保留原始的未打印设计列表以供记录。但由于你已将所有设计名称移出 `unprinted_designs`，列表现在为空，并且空列表是你拥有的唯一版本；原始列表已消失。在这种情况下，你可以通过将列表的副本而不是原始列表传递给函数来解决此问题。函数对列表所做的任何更改都只会影响副本，从而保持原始列表完整。

你可以像这样向函数发送列表的副本：

```python
function_name(list_name[:])
```

切片表示法 `[:]` 会制作一份列表的副本以发送给函数。如果我们不想清空 *printing_models.py* 中的未打印设计列表，我们可以像这样调用 `print_models()`：

```python
print_models(unprinted_designs[:], completed_models)
```

函数 `print_models()` 仍能完成工作，因为它仍然接收所有未打印设计名称。但这次它使用的是原始未打印设计列表的副本，而不是实际的 `unprinted_designs` 列表。列表 `completed_models` 将像以前一样填满已打印模型名称，但原始未打印设计列表不会受到函数的影响。

尽管你可以通过向函数传递其副本来保留列表的内容，但除非你有特定原因传递副本，否则应将原始列表传递给函数。对现有列表进行操作对函数来说更高效，因为这避免了制作单独副本所需的时间和内存。这在处理大型列表时尤其如此。

### 自己试试

- **8-9. 消息：** 创建一个包含一系列简短文本消息的列表。将列表传递给名为 `show_messages()` 的函数，该函数打印每条文本消息。
- **8-10. 发送消息：** 从练习 8-9 的程序开始。编写一个名为 `send_messages()` 的函数，该函数打印每条文本消息并在打印时将其移至名为 `sent_messages` 的新列表中。调用函数后，打印两个列表以确保消息已正确移动。
- **8-11. 归档消息：** 从练习 8-10 的工作开始。使用消息列表的副本调用函数 `send_messages()`。调用函数后，打印两个列表以显示原始列表已保留其消息。

## 传递任意数量的参数

有时你事先不知道函数需要接受多少个参数。幸运的是，Python 允许函数从调用语句中收集任意数量的参数。

例如，考虑一个制作披萨的函数。它需要接受一定数量的配料，但你无法事先知道一个人想要多少配料。以下示例中的函数有一个参数 `*toppings`，但此参数会收集调用行提供的任意多个参数：

**pizza.py**

```python
def make_pizza(*toppings):
    """Print the list of toppings that have been requested."""
```



print(toppings)

make_pizza('pepperoni')
make_pizza('mushrooms', 'green peppers', 'extra cheese')
```

参数名 *toppings 中的星号告诉 Python 创建一个名为 toppings 的元组，其中包含该函数接收的所有值。函数体中的 print() 调用产生的输出表明，Python 可以处理传入一个值的函数调用，也可以处理传入三个值的调用。它对不同的调用会做类似的处理。请注意，即使函数只接收一个值，Python 也会将实参打包成一个元组：

```
('pepperoni',)
('mushrooms', 'green peppers', 'extra cheese')
```

现在，我们可以用一个循环替换 print() 调用，遍历配料列表并描述正在订购的披萨：

```
def make_pizza(*toppings):
    """Summarize the pizza we are about to make."""
    print("\nMaking a pizza with the following toppings:")
    for topping in toppings:
        print(f"- {topping}")

make_pizza('pepperoni')
make_pizza('mushrooms', 'green peppers', 'extra cheese')
```

无论函数接收一个值还是三个值，它都能做出恰当的响应：

```
Making a pizza with the following toppings:
- pepperoni

Making a pizza with the following toppings:
- mushrooms
- green peppers
- extra cheese
```

无论函数接收多少实参，这种语法都有效。

## 混合使用位置参数和任意数量参数

如果你想让函数接受几种不同类别的实参，那么接受任意数量实参的形参必须在函数定义中放在最后。Python 先匹配位置实参和关键字实参，然后在最后一个形参中收集其余实参。

例如，如果函数需要接收披萨的尺寸，那么该形参必须放在形参 *toppings 之前：

```
def make_pizza(size, *toppings):
    """Summarize the pizza we are about to make."""
    print(f"\nMaking a {size}-inch pizza with the following toppings:")
    for topping in toppings:
        print(f"- {topping}")

make_pizza(16, 'pepperoni')
make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')
```

在函数定义中，Python 将接收到的第一个值赋给形参 size。随后传入的所有其他值都存储在元组 toppings 中。函数调用先包含一个尺寸实参，后面再按需要跟任意数量的配料。

现在，每个披萨都有尺寸和若干配料，每条信息都会在适当的位置打印出来，先显示尺寸，再显示配料：

```
Making a 16-inch pizza with the following toppings:
- pepperoni

Making a 12-inch pizza with the following toppings:
- mushrooms
- green peppers
- extra cheese
```

### 注意

> 你经常会看到通用形参名 *args，它像这样收集任意的位置实参。

## 使用任意数量的关键字参数

有时，你需要接受任意数量的实参，但事先并不知道函数会接收到什么样的信息。在这种情况下，你可以编写这样的函数：它能够接受调用语句提供的任意数量的键值对。一个例子是构建用户资料：你知道会获得关于用户的信息，但不确定会获得哪类信息。下面示例中的函数 build_profile() 总是接收名和姓，但也接受任意数量的关键字实参：

### user_profile.py

```
def build_profile(first, last, **user_info):
    """Build a dictionary containing everything we know about a user."""
    user_info['first_name'] = first
    user_info['last_name'] = last
    return user_info

user_profile = build_profile('albert', 'einstein',
                            location='princeton',
                            field='physics')
print(user_profile)
```

build_profile() 的定义要求提供名和姓，然后允许用户传入任意数量的名称-值对。参数 `**user_info` 前的两个星号让 Python 创建一个名为 `user_info` 的字典，其中包含函数接收到的所有额外的名称-值对。在函数体内，你可以像访问任何字典一样访问 `user_info` 中的键值对。

在 `build_profile()` 的函数体中，我们将名和姓加入到 `user_info` 字典，因为我们总是会从用户那里收到这两条信息 ❶，而它们还没有被放入字典。然后，我们将 `user_info` 字典返回到函数调用行。

我们调用 `build_profile()`，向它传递名 `'albert'`、姓 `'einstein'`，以及两个键值对 `location='princeton'` 和 `field='physics'`。我们将返回的 `profile` 赋值给 `user_profile`，并打印 `user_profile`：

```
{'location': 'princeton', 'field': 'physics',
'first_name': 'albert', 'last_name': 'einstein'}
```

返回的字典包含用户的名和姓，以及位置和研究领域。不管函数调用中提供了多少额外的键值对，该函数都能正常工作。

在编写自己的函数时，你可以用多种方式混合使用位置值、关键字值和任意数量值。了解所有这些实参类型的存在是很有用的，因为当你开始阅读别人的代码时，会经常看到它们。要正确地使用不同类型并知道何时使用哪种类型，需要多加练习。目前，请记住使用最简单的方法来完成任务。随着不断深入学习，你将学会每次都使用最高效的方法。

### 注意

> 你经常会看到形参名 **kwargs，它用于收集非特定的关键字实参。

## 动手试一试

- **8-12. 三明治：** 编写一个函数，它接受一个列表，其中包含顾客想要在三明治中添加的食材。这个函数应只有一个形参，用于收集函数调用提供的尽可能多的食材，并打印所订购三明治的概要。调用这个函数三次，每次提供不同数量的实参。
- **8-13. 用户资料：** 从第 148 页的 *user_profile.py* 副本开始。通过调用 `build_profile()` 来构建你自己的用户资料，使用你的名和姓以及另外三个描述你的键值对。
- **8-14. 汽车：** 编写一个函数，将汽车的信息存储在字典中。这个函数应始终接收制造商和型号名称。然后，它应接受任意数量的关键字实参。使用必需的信息以及另外两个名称-值对（如颜色或可选功能）调用这个函数。你的函数应能处理像下面这样的调用：

```
car = make_car('subaru', 'outback', color='blue', tow_package=True)
```

打印返回的字典，以确保所有信息都正确存储。

## 将函数存储在模块中

函数的一个优点在于，它们能将代码块与主程序分离。当你为函数使用描述性名称时，程序会更容易理解。你还可以更进一步，将函数存储在称为*模块*的独立文件中，然后将该模块*导入*到主程序中。`import` 语句告诉 Python，让模块中的代码在当前运行的程序文件中可用。

将函数存储在独立文件中，可以让你隐藏程序代码的细节，专注于更高层次的逻辑。它还允许你在许多不同的程序中重用函数。当你将函数存储在独立文件中时，你可以与其他程序员共享这些文件，而不必共享整个程序。了解如何导入函数，还使你能够使用其他程序员编写的函数库。

导入模块有几种方法，我将逐一简要介绍。

### 导入整个模块

要开始导入函数，我们首先需要创建一个模块。*模块*是以 .py 结尾的文件，其中包含你想要导入到程序中的代码。我们来创建一个包含函数 `make_pizza()` 的模块。为此，我们将从文件 *pizza.py* 中删除除函数 `make_pizza()` 之外的所有内容：

```
pizza.py

def make_pizza(size, *toppings):
    """Summarize the pizza we are about to make."""
    print(f"\nMaking a {size}-inch pizza with the following toppings:")
    for topping in toppings:
        print(f"- {topping}")
```

现在，我们将在与 *pizza.py* 相同的目录中创建一个名为 *making_pizzas.py* 的独立文件。这个文件导入我们刚刚创建的模块，然后对 `make_pizza()` 进行两次调用：

```
making_pizzas.py

import pizza

pizza.make_pizza(16, 'pepperoni')
pizza.make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')
```





当 Python 读取此文件时，行 `import pizza` 告诉 Python 打开文件 *pizza.py* 并将其中所有的函数复制到此程序中。你实际上看不到代码在文件之间被复制，因为 Python 会在程序运行前在幕后复制代码。你只需要知道的是，*pizza.py* 中定义的任何函数现在都可在 *making_pizzas.py* 中使用。

要从导入的模块调用函数，输入你导入的模块名，`pizza`，后跟函数名，`make_pizza()`，两者用点分隔。此代码产生的输出与未导入模块的原始程序相同：

```
Making a 16-inch pizza with the following toppings:
- pepperoni

Making a 12-inch pizza with the following toppings:
- mushrooms
- green peppers
- extra cheese
```

这种导入的第一种方法，即简单地写 `import` 后跟模块名，会使模块中的每个函数都可在你的程序中使用。如果你使用这种 `import` 语句导入整个名为 *module_name.py* 的模块，模块中的每个函数都可通过以下语法使用：

```
*module_name.function_name()*
```

## Importing Specific Functions

你也可以从模块中导入特定的函数。下面是这种方法的通用语法：

```
from *module_name* import *function_name*
```

你可以通过用逗号分隔每个函数名，从模块中导入任意多个函数：

```
from module_name import function_0, function_1, function_2
```

如果我们只想导入将要使用的函数，*making_pizzas.py* 示例将如下所示：

```
from pizza import make_pizza

make_pizza(16, 'pepperoni')
make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')
```

使用这种语法，调用函数时不需要使用点符号。因为我们在导入语句中显式导入了函数 `make_pizza()`，所以在使用函数时可以直接按名称调用它。

## Using as to Give a Function an Alias

如果要导入的函数名可能与程序中已有的名称冲突，或者函数名过长，你可以使用简短且唯一的 *alias*——类似于函数昵称的替代名称。你将在导入函数时为函数提供这个特殊昵称。

这里我们为函数 `make_pizza()` 提供了一个别名 `mp()`，方法是导入 `make_pizza as mp`。`as` 关键字使用你提供的别名重命名函数：

```
from pizza import make_pizza as mp

mp(16, 'pepperoni')
mp(12, 'mushrooms', 'green peppers', 'extra cheese')
```

这里显示的 `import` 语句将函数 `make_pizza()` 重命名为 `mp()`，在此程序中。只要我们想调用 `make_pizza()`，就可以简单地写 `mp()`，Python 将运行 `make_pizza()` 中的代码，同时避免与你在本程序文件中可能编写的另一个 `make_pizza()` 函数产生混淆。

提供别名的通用语法是：

```
from *module_name* import *function_name* as fn
```

## Using as to Give a Module an Alias

你也可以为模块名提供别名。为模块提供简短的别名，如 `pizza` 的 `p`，可以让你更快地调用模块的函数。调用 `p.make_pizza()` 比调用 `pizza.make_pizza()` 更简洁：

```
import pizza as p

p.make_pizza(16, 'pepperoni')
p.make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')
```

模块 `pizza` 在 `import` 语句中被赋予别名 `p`，但模块的所有函数都保留其原始名称。通过写 `p.make_pizza()` 调用函数不仅比 `pizza.make_pizza()` 更简洁，而且还能将你的注意力从模块名转移，允许你专注于其函数的描述性名称。这些函数名称清楚地告诉你每个函数的作用，对代码可读性的重要性超过使用完整的模块名。

这种方法的通用语法是：

```
import *module_name* as mn
```

## Importing All Functions in a Module

## Styling Functions

你的代码试图做什么。模块名也应使用这些约定。

每个函数都应有一个注释，简洁地说明函数的作用。该注释应出现在函数定义之后立即出现，并使用 docstring 格式。在一个文档完善的函数中，其他程序员只需阅读 docstring 中的描述即可使用该函数。他们应该能够信任代码按描述工作，并且只要他们知道函数的名称、所需参数以及返回值的类型，就应该能够在他们的程序中使用它。

如果你为参数指定默认值，则在等号两侧不应使用空格：

```python
def function_name(parameter_0, parameter_1='default value')
```

在函数调用中的关键字参数也应使用相同的约定：

```python
function_name(value_0, parameter_1='value')
```

PEP 8 (https://www.python.org/dev/peps/pep-0008) 建议将代码行限制在 79 个字符内，以便每行都能在合理大小的编辑器窗口中显示。如果一组参数导致函数定义超过 79 个字符，请在定义行的左括号后按 ENTER。在下一行，按两次 TAB 键将参数列表与函数体分开，函数体仅缩进一级。

大多数编辑器会自动将任何额外的参数行与你在第一行建立的缩进对齐：

```python
def function_name(
        parameter_0, parameter_1, parameter_2,
        parameter_3, parameter_4, parameter_5):
    function body...
```

如果你的程序或模块有多个函数，你可以通过两个空行分隔每个函数，以便更容易看出一个函数何时结束，下一个函数何时开始。

所有 import 语句都应写在文件的开头。唯一的例外是如果你在文件开头使用注释来描述整个程序。

## TRY IT YOURSELF

- **8-15. Printing Models:** 将示例 *printing_models.py* 的函数放入单独的文件 *printing_functions.py* 中。在 *printing_models.py* 顶部写一个 import 语句，并修改文件以使用导入的函数。

- **8-16. Imports:** 使用你编写的一个包含一个函数的程序，将该函数存储在单独的文件中。将函数导入到你的主程序文件中，并使用以下每种方法调用函数：

  ```python
  import module_name
  from module_name import function_name
  from module_name import function_name as fn
  import module_name as mn
  from module_name import *
  ```

- **8-17. Styling Functions:** 选择本章中你编写的任意三个程序，并确保它们遵循本节所述的样式指南。

## Summary

在本章中，你学习了如何编写函数并传递参数，以便你的函数能够访问完成工作所需的信息。你学习了如何使用位置参数和关键字参数，以及如何接受任意数量的参数。你看到了显示输出的函数和返回值函数。你学习了如何使用函数与列表、字典、if 语句和 `while` 循环。你还看到了如何将函数存储在称为 *modules* 的单独文件中，以便你的程序文件更简单、更易于理解。最后，你学习了如何为函数设置样式，以便你的程序保持良好的结构，并尽可能易于你和其他人阅读。

作为程序员，你的目标之一应该是编写简单且按你想要的方式工作的代码，而函数可以帮助你实现这一点。它们允许你编写代码块，并在确认其工作后将其搁置。一旦你知道函数能正确完成工作，你就可以信任它会继续工作，然后继续下一个编码任务。

函数允许你编写一次代码，然后按你想要的次数重复使用它。当你需要运行函数中的代码时，只需写一行调用，函数就会完成工作。当你需要修改函数的行为时，你只需修改一个代码块，你的更改就会在你调用该函数的所有地方生效。

使用函数使你的程序更易于阅读，良好的函数名称总结了程序每个部分的作用。阅读一系列函数调用比阅读长串代码块更能快速了解程序的作用。

函数还使你的代码更易于测试和调试。当程序的大部分工作由一组各有特定任务的函数完成时，测试和维护你编写的代码会容易得多。你可以编写一个单独的程序调用每个函数并测试每个函数在可能遇到的所有情况下是否正常工作。这样做时，你可以确信每次调用它们时函数都会正常工作。



