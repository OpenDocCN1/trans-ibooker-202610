### 10 编写有效的函数

![](img/ab52fe24e511e29be73786864ebe0410_187_0.png)

函数就像我们程序中的迷你程序，允许我们将代码拆分为更小的单元。这让我们免于编写重复代码，而重复代码可能会引入错误。但编写有效的函数需要就命名、大小、参数和复杂性做出许多决策。

本章探讨了我们可以用不同方式编写函数的不同方法以及不同权衡的利弊。我们将深入探讨如何在小型函数和大型函数之间权衡，参数数量如何影响函数的复杂性，以及如何使用 * 和 ** 运算符编写具有可变数量参数的函数。我们还将探讨函数式编程范式以及按照该范式编写函数的益处。

### 函数名称

函数名应遵循我们在第 4 章中描述的一般标识符约定。但它们通常应包含一个动词，因为函数通常执行某些操作。你也可以包含一个名词来描述被操作的对象。例如，名称 refreshConnection()、setPassword() 和 extract_version() 阐明了函数的作用以及作用对象。

对于类或模块的一部分的方法，你可能不需要名词。SatelliteConnection 类中的 reset() 方法或 webbrowser 模块中的 open() 函数已经提供了必要的上下文。你可以知道卫星连接是被重置的对象，而网页浏览器是被打开的对象。

使用长而描述性的名称比使用缩写或过短的名称更好。数学家可能会立即理解名为 gcd() 的函数返回两个数的最大公约数，但其他人会认为 getGreatestCommonDenominator() 更具信息性。

请记住不要使用 Python 的任何内置函数或模块名称，例如 all、any、date、email、file、format、hash、id、input、list、min、max、object、open、random、set、str、sum、test 和 type。

### 函数大小权衡

有些程序员说函数应尽可能短，并且不超过单屏所能容纳的长度。一个只有十几行的函数相对容易理解，至少与数百行的函数相比是如此。但通过将代码拆分为多个较小的函数来使函数变短也会有其缺点。让我们来看一下小函数的一些优点：

- 函数的代码更容易理解。
- 函数可能需要的参数更少。
- 函数不太可能产生副作用，如第 172 页“Functional Programming”所述。
- 函数更容易测试和调试。
- 函数可能引发的异常种类更少。

但短函数也有一些缺点：

- 编写短函数通常意味着程序中函数数量更多。
- 函数越多，程序就越复杂。
- 函数越多，也意味着需要想出更多描述性、准确的名称，这是一项困难的任务。
- 使用更多函数需要你编写更多文档。
- 函数之间的关系变得更加复杂。

有些人将“越短越好”这一指导原则推向极端，声称所有函数都应最多为三到四行代码。这是疯狂的。例如，这里是第 14 章汉诺塔游戏中 getPlayerMove() 函数。这段代码的工作细节并不重要。只需查看函数的总体结构：

```
def getPlayerMove(towers):
    """Asks the player for a move. Returns (fromTower, toTower)."""

    while True:  # Keep asking player until they enter a valid move.
        print('Enter the letters of "from" and "to" towers, or QUIT.')
        print("(e.g. AB to move a disk from tower A to tower B.)")
        print()
        response = input("> ").upper().strip()

        if response == "QUIT":
            print("Thanks for playing!")
            sys.exit()

    # Make sure the user entered valid tower letters:
    if response not in ("AB", "AC", "BA", "BC", "CA", "CB"):
        print("Enter one of AB, AC, BA, BC, CA, or CB.")
        continue  # Ask player again for their move.

    # Use more descriptive variable names:
    fromTower, toTower = response[0], response[1]

    if len(towers[fromTower]) == 0:
        # The "from" tower cannot be an empty tower:
        print("You selected a tower with no disks.")
        continue  # Ask player again for their move.
    elif len(towers[toTower]) == 0:
        # Any disk can be moved onto an empty "to" tower:
        return fromTower, toTower
    elif towers[toTower][-1] < towers[fromTower][-1]:
        print("Can't put larger disks on top of smaller ones.")
        continue  # Ask player again for their move.
    else:
        # This is a valid move, so return the selected towers:
        return fromTower, toTower
```

此函数长 34 行。尽管它涵盖了多个任务，包括允许玩家输入移动、检查该移动是否有效，以及在移动无效时再次要求玩家输入移动，但这些任务都属于获取玩家移动的范畴。另一方面，如果我们致力于编写短函数，我们可以将 getPlayerMove() 中的代码拆分为更小的函数，如下所示：

```python
def getPlayerMove(towers):
    """Asks the player for a move. Returns (fromTower, toTower)."""

    while True:  # Keep asking player until they enter a valid move.
        response = askForPlayerMove()
        terminateIfResponseIsQuit(response)
        if not isValidTowerLetters(response):
            continue # Ask player again for their move.

        # Use more descriptive variable names:
        fromTower, toTower = response[0], response[1]

        if towerWithNoDisksSelected(towers, fromTower):
            continue # Ask player again for their move.
        elif len(towers[toTower]) == 0:
            # Any disk can be moved onto an empty "to" tower:
            return fromTower, toTower
        elif largerDiskIsOnSmallerDisk(towers, fromTower, toTower):
            continue # Ask player again for their move.
        else:
            # This is a valid move, so return the selected towers:
            return fromTower, toTower

def askForPlayerMove():
    """Prompt the player, and return which towers they select."""
    print('Enter the letters of "from" and "to" towers, or QUIT.')
    print("(e.g. AB to move a disk from tower A to tower B.)")
    print()
    return input("> ").upper().strip()

def terminateIfResponseIsQuit(response):
    """Terminate the program if response is 'QUIT'"""
    if response == "QUIT":
        print("Thanks for playing!")
        sys.exit()

def isValidTowerLetters(towerLetters):
    """Return True if `towerLetters` is valid."""
    if towerLetters not in ("AB", "AC", "BA", "BC", "CA", "CB"):
        print("Enter one of AB, AC, BA, BC, CA, or CB.")
        return False
    return True

def towerWithNoDisksSelected(towers, selectedTower):
    """Return True if `selectedTower` has no disks."""
    if len(towers[selectedTower]) == 0:
        print("You selected a tower with no disks.")
        return True
    return False

def largerDiskIsOnSmallerDisk(towers, fromTower, toTower):
    """Return True if a larger disk would move on a smaller disk."""
    if towers[toTower][-1] < towers[fromTower][-1]:
        print("Can't put larger disks on top of smaller ones.")
        return True
    return False
```

这六个函数长 56 行，几乎是原始代码行数的两倍，但它们执行相同的任务。尽管每个函数比原始的 getPlayerMove() 函数更容易理解，但它们组合在一起代表了复杂性的增加。你的代码的读者可能难以理解它们如何协同工作。getPlayerMove() 函数是程序其他部分唯一调用的函数；其他五个函数仅被调用一次，即从 getPlayerMove() 调用。但大量函数的存在并不能传达这一事实。

我还必须为每个新函数想出新的名称和 docstrings（每个 def 语句下的三引号字符串，在第 11 章中进一步解释）。这导致了名称令人困惑地相似的函数，例如 getPlayerMove() 和 askForPlayerMove()。此外，getPlayerMove() 仍然比三到四行长，所以如果我遵循“越短越好”的指导原则，我就需要将其拆分为更小的函数！

在这种情况下，只允许极短函数的策略可能导致了更简单的函数，但程序的整体复杂性急剧增加。在我看来，函数理想情况下应少于 30 行，并且绝对不超过 200 行。让你的函数尽可能短，但不要更短。

### 函数参数和参数

函数的参数是函数 def 语句括号之间的变量名，而参数是函数调用括号之间的值。函数的参数越多，其代码就越可配置和通用。但参数越多也意味着更大的复杂性。

一个好的规则是零到三个参数是可以的，但超过五或六个可能就太多了。一旦函数变得过于复杂，最好考虑如何将其拆分为参数更少的较小函数。

### 默认参数

减少函数参数复杂性的一种方法是为参数提供默认值。默认参数是在函数调用未指定参数时使用的值。如果大多数函数调用使用特定的参数值，我们可以将该值设为默认值，以避免在函数调用中反复输入。

我们在 def 语句中指定默认值，位于参数名和等号之后。例如，在这个 introduction() 函数中，如果函数调用未指定，名为 greeting 的参数的值为 'Hello'：

```
>>> def introduction(name, greeting='Hello'):
...     print(greeting + ', ' + name)
...
>>> introduction('Alice')
```

```
Hello, Alice
>>> introduction('Hiro', 'Ohayo gozaimasu')
Ohayo gozaimasu, Hiro
```

当 introduction() 函数在没有第二个参数的情况下被调用时，它默认使用字符串 'Hello'。请注意，带有默认参数的参数必须始终放在没有默认参数的参数之后。

回顾第 8 章，你应避免使用可变对象（如空列表 [] 或空字典 {}）作为默认值。第 143 页的“Don’t Use Mutable Values for Default Arguments”解释了这种做法导致的问题及其解决方案。

## 使用 * 和 ** 传递参数给函数

你可以使用 * 和 ** 语法（通常读作 star 和 star star）将一组参数分别传递给函数。* 语法允许你传入可迭代对象（如列表或元组）中的项。** 语法允许你将映射对象（如字典）中的键值对作为单独的参数传入。

例如，print() 函数可以接受多个参数。它默认在它们之间放置一个空格，如以下代码所示：

```
>>> print('cat', 'dog', 'moose')
cat dog moose
```

这些参数称为位置参数，因为它们在函数调用中的位置决定了哪个参数被分配给哪个形参。但如果你将这些字符串存储在列表中并尝试传递该列表，print() 函数会认为你试图将列表作为单个值打印：

```
>>> args = ['cat', 'dog', 'moose']
>>> print(args)
['cat', 'dog', 'moose']
```

将列表传递给 print() 会显示该列表，包括方括号、引号和逗号字符。

打印列表中各个项的一种方法是将列表拆分为多个参数，通过分别传递每个项的索引给函数来实现，这会导致代码更难阅读：

```
>>> # An example of less readable code:
>>> args = ['cat', 'dog', 'moose']
>>> print(args[0], args[1], args[2])
cat dog moose
```

更简单的方法是将这些项传递给 print()。你可以使用 * 语法将列表（或其他任何可迭代数据类型）中的项解释为单独的位置参数。将以下示例输入到交互式 shell 中。

```
>>> args = ['cat', 'dog', 'moose']
>>> print(*args)
cat dog moose
```

* 语法允许你将列表项单独传递给函数，无论列表中有多少项。

你可以使用 ** 语法将映射数据类型（如字典）作为单独的关键字参数传递。关键字参数前面有参数名和等号。例如，print() 函数有一个 sep 关键字参数，用于指定在它显示的参数之间放置的字符串。默认情况下它被设置为单个空格字符串 ' '。你可以使用赋值语句或 ** 语法将关键字参数赋给不同的值。要了解其工作原理，请在交互式 shell 中输入以下内容：

```
>>> print('cat', 'dog', 'moose', sep='-')
cat-dog-moose
>>> kwargsForPrint = {'sep': '-'}
>>> print('cat', 'dog', 'moose', **kwargsForPrint)
cat-dog-moose
```

请注意，这些指令产生相同的输出。在示例中，我们只用一行代码设置了 kwargsForPrint 字典。但对于更复杂的情况，你可能需要更多代码来设置关键字参数的字典。** 语法允许你创建一个自定义的配置设置字典以传递给函数调用。这对于接受大量关键字参数的函数和方法尤其有用。

通过在运行时修改列表或字典，你可以使用 * 和 ** 语法为函数调用提供可变数量的参数。

## 使用 * 创建可变参数函数

你也可以在 def 语句中使用 * 语法创建可变参数或 varargs 函数，使其接收可变数量的位置参数。例如，print() 是一个可变参数函数，因为你可以向它传递任意数量的字符串：例如 print('Hello!') 或 print('My name is', name)。请注意，虽然我们在上一节的函数调用中使用了 * 语法，但在本节中我们使用 * 语法进行函数定义。

让我们通过创建一个 product() 函数来举例，该函数接受任意数量的参数并将它们相乘：

```
>>> def product(*args):
...     result = 1
...     for num in args:
...         result *= num
...     return result
...
>>> product(3, 3)
9
>>> product(2, 1, 2, 3)
12
```

函数内部，args 只是一个包含所有位置参数的普通 Python 元组。技术上，你可以给这个参数命名为任何名称，只要它以星号 (*) 开头即可，但按惯例通常命名为 args。

何时使用 * 需要一些思考。毕竟，使函数变为可变参数的替代方案是拥有一个接受列表（或其他可迭代数据类型）的单个参数，其中包含可变数量的项。这就是内置 sum() 函数的做法：

```
>>> sum([2, 1, 2, 3])
8
```

sum() 函数期望一个可迭代参数，因此传递多个参数会导致异常：

```
>>> sum(2, 1, 2, 3)
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
TypeError: sum() takes at most 2 arguments (4 given)
```

与此同时，内置的 min() 和 max() 函数用于查找多个值的最小值或最大值，它们接受单个可迭代参数或多个单独参数：

```
>>> min([2, 1, 3, 5, 8])
1
>>> min(2, 1, 3, 5, 8)
1
>>> max([2, 1, 3, 5, 8])
8
>>> max(2, 1, 3, 5, 8)
8
```

所有这些函数都接受可变数量的参数，那么为什么它们的形参设计不同呢？我们又应在什么时候设计函数以接受单个可迭代参数或使用 * 语法接受多个单独参数？

我们如何设计参数取决于我们预测程序员将如何使用我们的代码。print() 函数接受多个参数，因为程序员更常向它传递一系列字符串，或包含字符串的变量，如 print('My name is', name)。将这些字符串在多个步骤中收集到列表中然后再传递给 print() 的情况并不常见。此外，如果你传递一个列表给 print()，函数会完整打印该列表值，因此你无法用它来打印列表中的各个值。

因为 Python 已经使用 + 运算符来实现这一点，所以没有理由用单独的参数调用 sum()。因为你可以编写像 2 + 4 + 8 这样的代码，所以你不需要能够编写像 sum(2, 4, 8) 这样的代码。因此，将可变数量的参数仅作为列表传递给 sum() 是有意义的。

min() 和 max() 函数允许两种风格。如果程序员传递一个参数，函数假定它是一个要检查的值的列表或元组。如果程序员传递多个参数，它假定这些是要检查的值。这两个函数通常在程序运行时处理值列表，如函数调用 min(allExpenses) 所示。它们也处理程序员在编写代码时选择的单独参数，如 max(0, someNumber)。因此，这些函数被设计为接受两种类型参数。下面的 myMinFunction() 是我自己对 min() 函数的实现，演示了这一点：

```
def myMinFunction(*args):
    if len(args) == 1:
        ① values = args[0]
    else:
        ② values = args

    if len(values) == 0:
        ③ raise ValueError('myMinFunction() args is an empty sequence')

    ④ for i, value in enumerate(values):
        if i == 0 or value < smallestValue:
            smallestValue = value
    return smallestValue
```

myMinFunction() 使用 * 语法以元组形式接受可变数量的参数。如果该元组只包含一个值，我们假定它是要检查的值序列 ①。否则，我们假定 args 是要检查的值的元组 ②。无论哪种情况，values 变量都将包含供其余代码检查的值序列。与实际的 min() 函数一样，如果调用者没有传递任何参数或传递了空序列，我们会引发 ValueError ③。其余代码遍历 values 并返回找到的最小值 ④。为了保持示例简单，myMinFunction() 只接受列表或元组等序列，而不是任何可迭代值。

你可能会想，为什么我们不总是编写函数以接受传递可变数量参数的两种方式。答案是最好让你的函数尽可能简单。除非两种调用方式都很常见，否则选择其中一种即可。如果函数通常处理程序运行时创建的数据结构，最好让它接受单个参数。如果函数通常处理程序员在编写代码时指定的参数，最好使用 * 语法接受可变数量的参数。

## 使用 ** 创建可变参数函数

可变参数函数也可以使用 ** 语法。虽然 def 语句中的 * 语法表示可变数量的位置参数，但 ** 语法表示可变数量的可选关键字参数。

如果在不使用 ** 语法的情况下定义一个可能接受大量可选关键字参数的函数，你的 def 语句可能会变成

难以处理。考虑一个假设的 formMolecule() 函数，它的参数涵盖全部 118 种已知元素：

```
>>> def formMolecule(hydrogen, helium, lithium, beryllium, boron, --snip--
```

将氢参数设为 2、氧参数设为 1 以返回 'water' 也会很繁琐且难以阅读，因为你必须将所有无关元素都设为零：

```
>>> formMolecule(2, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0 --snip--
'water'
```

你可以通过使用命名关键字参数来使函数更易管理，每个参数都有默认值，这样你就不必在函数调用中为该参数传递参数了。

> NOTE 虽然 argument 和 parameter 术语有明确定义，但程序员倾向于将 keyword argument 和 keyword parameter 互换使用。

例如，此 def 语句为每个关键字参数都设置了默认参数 0：

```
>>> def formMolecule(hydrogen=0, helium=0, lithium=0, beryllium=0, --snip--
```

这样调用 formMolecule() 更容易，因为你只需为与默认值不同的参数指定参数。你也可以按任意顺序指定关键字参数：

```
>>> formMolecule(hydrogen=2, oxygen=1)
'water'
>>> formMolecule(oxygen=1, hydrogen=2)
'water'
>>> formMolecule(carbon=8, hydrogen=10, nitrogen=4, oxygen=2)
'caffeine'
```

但你仍然有一个难以处理的 def 语句，其中包含 118 个参数名。如果发现了新元素呢？你必须更新函数的 def 语句以及函数参数的任何文档。

相反，你可以使用关键字参数的 ** 语法将所有参数及其参数作为键值对收集到字典中。技术上，你可以为 ** 参数命名为任何名称，但按惯例通常命名为 kwargs：

```
>>> def formMolecules(**kwargs):
...     if len(kwargs) == 2 and kwargs['hydrogen'] == 2 and
...         kwargs['oxygen'] == 1:
...         return 'water'
...     # (rest of code for the function goes here)
...
>>> formMolecules(hydrogen=2, oxygen=1)
'water'
```

** 语法表示 kwargs 参数可以处理函数调用中传递的所有关键字参数。它们将作为键值对存储在分配给 kwargs 参数的字典中。随着新化学元素的发现，你需要更新函数的代码，但不需要更新其 def 语句，因为所有关键字参数都放入 kwargs 中：

```
① >>> def formMolecules(**kwargs):
② ...     if len(kwargs) == 1 and kwargs.get('unobtanium') == 12:
...         return 'aether'
...     # (rest of code for the function goes here)
...
>>> formMolecules(unobtanium=12)
'aether'
```

如你所见，def 语句 ① 与之前相同，只有函数代码 ② 需要更新。当你使用 ** 语法时，def 语句和函数调用变得更简单易写，同时仍能产生可读的代码。

## 使用 * 和 ** 创建包装函数

在 def 语句中使用 * 和 ** 语法的常见用例是创建包装函数，这些函数将参数传递给另一个函数并返回该函数的返回值。你可以使用 * 和 ** 语法将任意参数转发给被包装的函数。例如，我们可以创建一个 printLowercase() 函数来包装内置的 print() 函数。它依赖 print() 执行实际工作，但首先将字符串参数转换为小写：

```
① >>> def printLower(*args, **kwargs):
② ...     args = list(args)
...     for i, value in enumerate(args):
...         args[i] = str(value).lower()
③ ...     return print(*args, **kwargs)
...
>>> name = 'Albert'
>>> printLower('Hello, ', name)
hello, albert
>>> printLower('DOG', 'CAT', 'MOOSE', sep=', ')
dog, cat, moose
```

printLower() 函数 ① 使用 * 语法接受可变数量的位置参数，这些参数被分配给 args 参数的元组，而 ** 语法将任何关键字参数分配给 kwargs 参数的字典。如果一个函数同时使用 *args 和 **kwargs，*args 参数必须在 **kwargs 参数之前。我们将这些参数传递给被包装的 print() 函数，但在传递之前我们的函数会修改部分参数，因此我们将 args 元组创建为列表形式 ②。

在将 args 中的字符串改为小写后，我们使用 *
和 ** 语法将 args 中的项和 kwargs 中的键值对作为单独参数传递给 print() ③。print() 的返回值也会作为 printLower() 的返回值返回。这些步骤有效地包装了 print() 函数。

## 函数式编程

函数式编程是一种编程范式，强调编写在不修改全局变量或任何外部状态（如硬盘上的文件、互联网连接或数据库）的情况下执行计算的函数。一些编程语言，如 Erlang、Lisp 和 Haskell，是围绕函数式编程概念 heavily 设计的。虽然不受该范式束缚，Python 也有一些函数式编程特性。Python 程序可主要使用的特性是无副作用函数、高阶函数和 lambda 函数。

## 副作用

副作用是函数对存在于其自身代码和局部变量之外的程序部分所做的任何更改。为了说明这一点，让我们创建一个 subtract() 函数来实现 Python 的减法运算符 (-) ：

```
>>> def subtract(number1, number2):
...     return number1 - number2
...
>>> subtract(123, 987)
-864
```

这个 subtract() 函数没有副作用。也就是说，它不会影响程序中不属于其代码的部分。从程序或计算机的状态无法判断 subtract() 函数之前是否被调用过一次、两次或一百万次。函数可能会修改函数内部的局部变量，但这些更改仍与程序其余部分隔离。

现在考虑一个 addToTotal() 函数，它将数值参数加到名为 TOTAL 的全局变量上：

```
>>> TOTAL = 0
>>> def addToTotal(amount):
...     global TOTAL
...     TOTAL += amount
...     return TOTAL
...
>>> addToTotal(10)
10
>>> addToTotal(10)
20
>>> addToTotal(9999)
10019
>>> TOTAL
10019
```

addToTotal() 函数确实有副作用，因为它修改了函数外部存在的元素：TOTAL 全局变量。副作用不只是对全局变量的更改。它们包括更新或删除文件、在屏幕上打印文本、打开数据库连接、向服务器进行身份验证，或对函数外部做任何其他更改。函数调用返回后留下的任何痕迹都是副作用。

副作用还可能包括对函数外部引用的可变对象进行原地更改。例如，以下 removeLastCatFromList() 函数就原地修改了列表参数：

```
>>> def removeLastCatFromList(petSpecies):
...     if len(petSpecies) > 0 and petSpecies[-1] == 'cat':
...         petSpecies.pop()
...
>>> myPets = ['dog', 'cat', 'bird', 'cat']
>>> removeLastCatFromList(myPets)
>>> myPets
['dog', 'cat', 'bird']
```

在此示例中，myPets 变量和 petSpecies 参数引用同一个列表。对函数内部列表对象所做的任何原地修改也会存在于函数外部，使此修改成为副作用。

一个相关概念，确定性函数，在给定相同参数时总是返回相同的返回值。subtract(123, 987) 函数调用总是返回 −864。Python 的内置 round() 函数在传入 3.14 作为参数时总是返回 3。非确定性函数在传入相同参数时不总是返回相同的值。例如，调用 random.randint(1, 10) 会返回 1 到 10 之间的随机整数。time.time() 函数没有参数，但它返回的值取决于函数被调用时计算机时钟的设置。在 time.time() 的情况下，时钟是一个外部资源，实际上是函数的一个输入，就像参数一样。依赖于函数外部资源（包括全局变量、硬盘上的文件、数据库和互联网连接）的函数不被视为确定性的。

确定性函数的一个好处是你可以缓存它们的值。如果 subtract() 能记住第一次用这些参数调用时的返回值，就无需多次计算 123 和 987 的差值。因此，确定性函数允许我们进行空间-时间权衡，通过在内存中使用空间缓存先前结果来加快函数的运行时间。

一个既确定性又无副作用的函数称为纯函数。函数式程序员力求在程序中只创建纯函数。除了已提及的之外，纯函数还提供多种好处：

- 它们非常适合单元测试，因为它们不需要你设置任何外部资源。



- 在纯函数中，通过使用相同的参数调用函数很容易复现 bug。
- 纯函数可以调用其他纯函数并且仍然保持纯净。
- 在多线程程序中，纯函数是线程安全的并且可以安全地并发运行。（多线程超出了本书的范围。）
- 对纯函数的多次调用可以在并行的 CPU 核心上运行或在多线程程序中运行，因为它们不必依赖于任何需要按特定顺序运行的外部资源。

你可以在 Python 中尽可能编写纯函数并且应该这样做。Python 函数仅通过约定才是纯的；没有任何设置会导致 Python 解释器强制执行纯度。使函数保持纯净最常见的方法是避免在其中使用全局变量，并确保它们不与文件、互联网、系统时钟、随机数或其他外部资源交互。

## 高阶函数

高阶函数可以接受其他函数作为参数或返回函数作为返回值。例如，让我们定义一个名为 callItTwice() 的函数，它将调用给定的函数两次：

```
>>> def callItTwice(func, *args, **kwargs):
...     func(*args, **kwargs)
...     func(*args, **kwargs)
...
>>> callItTwice(print, 'Hello, world!')
Hello, world!
Hello, world!
```

callItTwice() 函数适用于传递给它的任何函数。在 Python 中，函数是第一类对象，这意味着它们就像其他任何对象一样：你可以将函数存储在变量中、作为参数传递，或用作返回值。

## Lambda 函数

Lambda 函数，也称为匿名函数或无名函数，是简化后的函数，它们没有名称且代码仅由一个返回语句组成。我们经常在将函数作为参数传递给其他函数时使用 lambda 函数。

例如，我们可以创建一个接受包含 4 x 10 矩形宽度和高度的列表的普通函数，如下所示：

```
>>> def rectanglePerimeter(rect):
...     return (rect[0] * 2) + (rect[1] * 2)
...
>>> myRectangle = [4, 10]
>>> rectanglePerimeter(myRectangle)
28
```

等效的 lambda 函数看起来像这样：

```
lambda rect: (rect[0] * 2) + (rect[1] * 2)
```

要定义 Python lambda 函数，请使用 lambda 关键字，后跟以逗号分隔的参数列表（如果有），冒号，然后是一个作为返回值的作用的表达式。由于函数是第一类对象，你可以将 lambda 函数分配给变量，有效地复制了 def 语句所做的工作：

```
>>> rectanglePerimeter = lambda rect: (rect[0] * 2) + (rect[1] * 2)
>>> rectanglePerimeter([4, 10])
28
```

我们将此 lambda 函数分配给名为 rectanglePerimeter 的变量，本质上给了我们一个 rectanglePerimeter() 函数。正如你所见，由 lambda 语句创建的函数与由 def 语句创建的函数相同。

> 注意
> *在实际代码中，使用 def 语句而不是将 lambda 函数分配给常量变量。Lambda 函数专门用于函数不需要名称的情况。*

lambda 函数语法有助于指定小型函数作为其他函数调用的参数。例如，sorted() 函数有一个名为 key 的关键字参数，让你可以指定一个函数。它不是根据列表中项目的数值对项目进行排序，而是根据函数的返回值对它们进行排序。在下面的示例中，我们向 sorted() 传递一个返回给定矩形周长的 lambda 函数。这使得 sorted() 函数根据其 [width, height] 列表计算出的周长进行排序，而不是直接基于 [width, height] 列表：

```
>>> rects = [[10, 2], [3, 6], [2, 4], [3, 9], [10, 7], [9, 9]]
>>> sorted(rects, key=lambda rect: (rect[0] * 2) + (rect[1] * 2))
[[2, 4], [3, 6], [10, 2], [3, 9], [10, 7], [9, 9]]
```

而不是例如对值 [10, 2] 或 [3, 6] 进行排序，函数现在基于返回的周长整数 24 和 18 进行排序。Lambda 函数是一个方便的语法快捷方式：你可以指定一个小的单行 lambda 函数，而不是使用 def 语句定义一个新的命名函数。

## 使用列表推导式进行映射和过滤

在早期 Python 版本中，map() 和 filter() 函数是常见的可转换和过滤列表的高阶函数，通常借助 lambda 函数。映射可以根据另一个列表的值创建值列表。过滤可以创建仅包含与某些条件匹配的另一个列表的值的列表。

例如，如果你想创建一个新列表，其中包含字符串而不是整数 [8, 16, 18, 19, 12, 1, 6, 7]，你可以将该列表和 lambda n: str(n) 传递给 map() 函数：

```
>>> mapObj = map(lambda n: str(n), [8, 16, 18, 19, 12, 1, 6, 7])
>>> list(mapObj)
['8', '16', '18', '19', '12', '1', '6', '7']
```

map() 函数返回一个 map 对象，我们可以通过将其传递给 list() 函数以列表形式获取它。映射后的列表现在包含基于原始列表整数值的字符串值。filter() 函数类似，但在这里，lambda 函数参数决定列表中哪些项目保留（如果 lambda 函数返回 True）或被过滤掉（如果它返回 False）。例如，我们可以传递 lambda n: n % 2 == 0 来过滤掉任何奇数整数：

```
>>> filterObj = filter(lambda n: n % 2 == 0, [8, 16, 18, 19, 12, 1, 6, 7])
>>> list(filterObj)
[8, 16, 18, 12, 6]
```

filter() 函数返回一个 filter 对象，我们再次可以将其传递给 list() 函数。只有偶数整数保留在过滤后的列表中。

但 map() 和 filter() 函数是创建 Python 中映射或过滤列表的过时方法。相反，你现在可以用列表推导式创建它们。列表推导式不仅让你免于编写 lambda 函数，而且比 map() 和 filter() 更快。

这里我们使用列表推导式复制 map() 函数示例：

```
>>> [str(n) for n in [8, 16, 18, 19, 12, 1, 6, 7]]
['8', '16', '18', '19', '12', '1', '6', '7']
```

注意列表推导式中的 str(n) 部分与 lambda n: str(n) 相似。

这里我们使用列表推导式复制 filter() 函数示例：

```
>>> [n for n in [8, 16, 18, 19, 12, 1, 6, 7] if n % 2 == 0]
[8, 16, 18, 12, 6]
```

注意列表推导式中的 if n % 2 == 0 部分与 lambda n: n % 2 == 0 相似。

许多语言都有函数作为第一类对象的概念，从而允许存在高阶函数，包括映射和过滤函数。

## 返回值应始终具有相同的数据类型

Python 是一种动态类型语言，这意味着 Python 函数和方法可以自由返回任何数据类型的值。但为了使你的函数更可预测，你应努力让它们仅返回单一数据类型的值。

例如，这里有一个函数，根据随机数返回整数值或字符串值：

```
>>> import random
>>> def returnsTwoTypes():
...     if random.randint(1, 2) == 1:
...         return 42
...     else:
...         return 'forty two'
```

当你编写调用此函数的代码时，很容易忘记你必须处理几种可能的数据类型。为了继续这个示例，假设我们调用 returnsTwoTypes() 并想将其返回的数字转换为十六进制：

```
>>> hexNum = hex(returnsTwoTypes())
>>> hexNum
'0x2a'
```

Python 的内置 hex() 函数返回传递给它的整数值的十六进制数字的字符串。只要 returnsTwoTypes() 返回整数，此代码就能正常工作，给我们留下此代码无 bug 的印象。但当 returnsTwoTypes() 返回字符串时，它会引发异常：

```
>>> hexNum = hex(returnsTwoTypes())
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object cannot be interpreted as an integer
```

当然，我们应该始终记住处理返回值可能拥有的每种可能数据类型。但在现实世界中，很容易忘记这一点。为了防止这些 bug，我们应始终尝试使函数返回单一数据类型的值。这不是严格的要求，有时无法避免函数返回不同数据类型的值。但你越接近只返回一种类型，你的函数就越简单、越不易出现 bug。

特别需要注意一种情况：除非你的函数始终返回 None，否则不要从函数返回 None。None 值是 NoneType 数据类型的唯一值。让人很想让函数返回 None 来表示发生了错误（我在下一节“引发异常与返回错误代码”中讨论这种做法），但你应将返回 None 保留给没有有意义返回值的函数。



原因在于，使用返回 None 来表示错误是导致未捕获的 'NoneType' object has no attribute 异常的常见来源：

```
>>> import random
>>> def sometimesReturnsNone():
...     if random.randint(1, 2) == 1:
...         return 'Hello!'
...     else:
...         return None
...
>>> returnVal = sometimesReturnsNone()
>>> returnVal.upper()
'HELLO!'
>>> returnVal = sometimesReturnsNone()
>>> returnVal.upper()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'NoneType' object has no attribute 'upper'
```

此错误信息相当模糊，且可能需要花费一些精力才能追溯其原因，追溯到一个通常返回预期结果但在发生错误时也可能返回 None 的函数。问题发生的原因是 sometimesReturnsNone() 返回了 None，而我们随后将其赋给了 returnVal 变量。但错误信息会让你以为问题出现在对 upper() 方法的调用中。

在 2009 年的一次会议演讲中，计算机科学家 Tony Hoare 为在 1965 年发明空引用（与 Python 的 None 值大致对应的通用值）道歉，他说：“I call it my billion dollar mistake. [...] I couldn’t resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last 40 years.” 你可以在 https://autor.com/billiondollarmistake 在线观看他的完整演讲。

## 引发异常 vs. 返回错误代码

在 Python 中，术语 exception 和 error 的含义大致相同：程序中通常表示问题的异常情况。

异常作为编程语言特性在 1980 年代和 1990 年代随着 C++ 和 Java 而流行起来。它们取代了错误代码的使用，错误代码是函数返回的值，用来指示问题。异常的好处在于，返回值仅与函数的功能相关，而不需同时表示错误的出现。

错误代码也可能在你的程序中引起问题。例如，Python 的 find() 字符串方法通常返回找到子字符串的索引，如果找不到，则返回 -1 作为错误代码。但由于我们也可以使用 -1 来指定字符串末尾的索引，不小心将 -1 用作错误代码可能会引入 bug。请在交互式 shell 中输入以下内容以查看其工作原理。

```
>>> print('Letters after b in "Albert":', 'Albert'['Albert'.find('b') + 1:])
Letters after b in "Albert": ert
>>> print('Letters after x in "Albert":', 'Albert'['Albert'.find('x') + 1:])
Letters after x in "Albert": Albert
```

代码中 'Albert'.find('x') 部分求值为错误代码 -1。这使得表达式 'Albert'['Albert'.find('x') + 1:] 求值为 'Albert'[-1 + 1:]，进而求值为 'Albert'[0:]，最后为 'Albert'。显然，这不是代码的预期行为。如果改用 index() 而不是 find()，例如 'Albert'['Albert'.index('x') + 1:]，就会引发异常，使问题变得明显且无法忽视。

另一方面，index() 字符串方法如果无法找到子字符串，则会引发 ValueError 异常。如果不处理此异常，程序将会崩溃——这种行为通常比未注意到错误更可取。

当异常表示实际错误时，异常类的名称通常以 “Error” 结尾，例如 ValueError、NameError 或 SyntaxError。表示不一定是错误的异常情况的异常类包括 StopIteration、KeyboardInterrupt 或 SystemExit。

## 摘要

函数是将程序代码分组的常见方式，它们要求你做出某些决定：如何命名它们、大小如何、应有多少参数，以及为这些参数传递多少参数。def 语句中的 * 和 ** 语法允许函数接收可变数量的参数，使其成为可变参数函数。

虽然不是函数式编程语言，但 Python 具有许多函数式编程语言使用的特性。函数是第一类对象，这意味着你可以将它们存储在变量中并作为参数传递给其他函数（在此上下文中称为高阶函数）。Lambda 函数提供了一种简短的语法，用于指定作为高阶函数参数的无名匿名函数。Python 中最常见的高阶函数是 map() 和 filter()，尽管你可以用列表推导式更快地执行它们提供的功能。

你的函数的返回值应始终为相同的数据类型。你不应将返回值用作错误代码：异常用于指示错误。特别是 None 值经常被错误地用作错误代码。
