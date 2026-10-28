

```python
print(my_mustang.get_descriptive_name())
my_leaf = ElectricCar('nissan', 'leaf', 2024)
print(my_leaf.get_descriptive_name())
```

你可以通过用逗号分隔每个类来从一个模块导入多个类 ❶。一旦导入了所需的类，你就可以根据需要创建每个类的任意多个实例。

在此示例中，我们先创建一个汽油动力福特 Mustang ❷，然后再创建一个电动日产 Leaf ❸：

```text
2024 Ford Mustang
2024 Nissan Leaf
```

## Importing an Entire Module

你也可以导入整个模块，然后使用点表示法访问所需的类。这种方法简单，并且生成的代码易于阅读。由于每次创建类实例的调用都包含模块名，因此不会与当前文件使用的任何名称发生命名冲突。

下面是导入整个 car 模块然后创建一个普通汽车和一辆电动汽车的示例：

```python
# my_cars.py

import car

my_mustang = car.Car('ford', 'mustang', 2024)
print(my_mustang.get_descriptive_name())

my_leaf = car.ElectricCar('nissan', 'leaf', 2024)
print(my_leaf.get_descriptive_name())
```

首先我们导入整个 car 模块 ❶。然后我们通过 *module_name.ClassName* 语法访问所需的类。

## Importing a Module into a Module

当你将类分散到多个模块中时，你可能会发现一个模块中的类依赖于另一个模块中的类。这种情况下，你可以将所需的类导入到第一个模块中。

例如，让我们将 `Car` 类存储在一个模块中，将 `ElectricCar` 和 `Battery` 类存储在另一个模块中。我们将创建一个名为 *electric_car.py* 的新模块——替换我们之前创建的 *electric_car.py* 文件——并仅将 `Battery` 和 `ElectricCar` 类复制到此文件中：

**electric_car.py**

```python
"""A set of classes that can be used to represent electric cars."""

from car import Car

class Battery:
    --snip--

class ElectricCar(Car):
    --snip--
```

类 `ElectricCar` 需要访问其父类 `Car`，因此我们直接将 `Car` 导入到该模块中。如果我们忘记这一行，Python 在尝试导入 `electric_car` 模块时会引发错误。我们还需要更新 `Car` 模块，使其仅包含 `Car` 类：

**car.py**

```python
"""A class that can be used to represent a car."""

class Car:
    --snip--
```

现在我们可以分别从每个模块导入并创建所需的任何类型的汽车：

**my_cars.py**

```python
from car import Car
from electric_car import ElectricCar

my_mustang = Car('ford', 'mustang', 2024)
print(my_mustang.get_descriptive_name())

my_leaf = ElectricCar('nissan', 'leaf', 2024)
print(my_leaf.get_descriptive_name())
```

我们从其模块导入 `Car`，并从其模块导入 `ElectricCar`。然后我们创建一个普通汽车和一辆电动汽车。两辆车都已正确创建：

```text
2024 Ford Mustang
2024 Nissan Leaf
```

## Using Aliases

正如你在第 8 章中所看到的，在使用模块组织项目代码时，別名会非常有帮助。你在导入类时也可以使用別名。

例如，考虑一个你想要制作大量电动汽车的程序。反复输入（和阅读）`ElectricCar` 可能会很乏味。你可以在导入语句中为 `ElectricCar` 指定別名：

```python
from electric_car import ElectricCar as EC
```

现在你可以在想要制作电动汽车时使用此別名：

```python
my_leaf = EC('nissan', 'leaf', 2024)
```

你也可以为模块指定別名。下面是如何使用別名导入整个 `electric_car` 模块：

```python
import electric_car as ec
```

现在你可以使用此模块別名和完整的类名：

```python
my_leaf = ec.ElectricCar('nissan', 'leaf', 2024)
```

## The Python Standard Library

现在你对函数和类的工作原理有了基本的了解，你就可以开始使用其他程序员编写的模块了。你可以通过在文件顶部包含一个简单的 `import` 语句来使用标准库中的任何函数或类。让我们看一个模块 `random`，它在模拟许多现实世界情况时很有用。

`random` 模块中的一个有趣函数是 `randint()`。此函数接受两个整数参数，并返回介于（包括）这两个数字之间的随机整数。

下面是如何生成 1 到 6 之间的随机数：

```python
>>> from random import randint
>>> randint(1, 6)
3
```

另一个有用的函数是 `choice()`。此函数接受一个列表或元组并返回一个随机选择的元素：

```python
>>> from random import choice
>>> players = ['charles', 'martina', 'michael', 'florence', 'eli']
>>> first_up = choice(players)
>>> first_up
'florence'
```

`random` 模块不应用于构建与安全相关的应用程序，但它适用于许多有趣且引人入胜的项目。

**NOTE**

*你也可以从外部来源下载模块。你将在第二部分看到许多这些示例，在那里我们需要外部模块来完成每个项目。*

## Try It Yourself

- **9-13. Dice:** 创建一个名为 `Die` 的类，具有一个名为 `sides` 的属性，默认值为 6。编写一个名为 `roll_die()` 的方法，打印 1 到骰子面数之间的随机数。创建一个 6 面骰子并掷 10 次。

  创建一个 10 面骰子和一个 20 面骰子。分别掷每个骰子 10 次。

- **9-14. Lottery:** 创建一个包含一系列 10 个数字和 5 个字母的列表或元组。从列表中随机选择 4 个数字或字母并打印一条消息，说明与这 4 个数字或字母匹配的任何彩票都可赢得奖品。

- **9-15. Lottery Analysis:** 你可以使用循环来了解赢得你刚刚模拟的这种彩票可能有多困难。创建一个名为 `my_ticket` 的列表或元组。编写一个循环，不断抽取号码，直到你的彩票获胜。打印一条消息，报告循环运行了多少次才给你一张中奖彩票。

- **9-16. Python Module of the Week:** 探索 Python 标准库的一个优秀资源是名为 *Python Module of the Week* 的网站。访问 [https://pymotw.com](https://pymotw.com) 并查看目录。找到一个看起来有趣的模块并阅读它，也许从 random 模块开始。

## Styling Classes

与类相关的几个样式问题值得澄清，尤其是在你的程序变得更复杂时。

类名应以 *CamelCase* 编写。为此，将名称中每个单词的首字母大写，并且不要使用下划线。实例和模块名应以小写编写，单词之间用下划线分隔。

每个类都应在类定义后立即有一个文档字符串。文档字符串应是对类作用的简要描述，并且你应遵循为函数编写文档字符串时使用的相同格式约定。每个模块也应有一个文档字符串，描述该模块中的类可用于什么。

你可以使用空行来组织代码，但不要过度使用。在类内部，你可以在方法之间使用一行空行，在模块内部，你可以使用两行空行来分隔类。

如果你需要从标准库导入模块并导入你自己编写的模块，请先放置标准库模块的导入语句。然后添加一个空行和你编写的模块的导入语句。在具有多个导入语句的程序中，此约定使更容易看出程序中使用的不同模块来自何处。

## Summary

在本章中，你学习了如何编写自己的类。你学习了如何使用属性在类中存储信息，以及如何编写方法为类提供所需的行为。你学习了编写 __init__() 方法，使用你想要的确切属性从类创建实例。你看到了如何直接通过方法和通过方法修改实例的属性。你学习了继承可以简化相关类的创建，并且你学习了将一个类的实例用作另一个类的属性以保持每个类简单。

你看到了将类存储在模块中并将所需的类导入到它们将被使用的文件中可以保持项目的组织。你开始学习 Python 标准库，并看到了基于 random 模块的示例。最后，你学习了使用 Python 惯例来设置类的样式。
