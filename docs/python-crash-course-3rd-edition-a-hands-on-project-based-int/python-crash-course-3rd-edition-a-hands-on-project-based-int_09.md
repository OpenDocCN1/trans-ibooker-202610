## 9 类

![](img/8f15563c16d2a98c4d3115150679b092_275_0.png)

*面向对象编程* (*OOP*) 是编写软件最有效的方法之一。在面向对象编程中，你编写代表现实世界事物和情况的 *类*，并基于这些类创建 *对象*。当你编写一个类时，你定义了整个对象类别可以拥有的通用行为。

当你从类创建单个对象时，每个对象都会自动配备通用行为；然后你可以为每个对象赋予所需的独特特征。你会惊讶于面向对象编程能多好地模拟现实世界的情况。

从类创建对象称为 *实例化*，你操作的是类的 *实例*。在本章中，你将编写类并创建这些类的实例。你将

编写后，我们将用它来创建单个实例，每个实例代表一只特定的狗。

### 创建 Dog 类

从 Dog 类创建的每个实例将存储一个名字和一个年龄，并且我们将赋予每只狗 `sit()` 和 `roll_over()` 的能力：

```python
class Dog:
    """A simple attempt to model a dog."""

    def __init__(self, name, age):
        """Initialize name and age attributes."""
        self.name = name
        self.age = age

    def sit(self):
        """Simulate a dog sitting in response to a command."""
        print(f"{self.name} is now sitting.")

    def roll_over(self):
        """Simulate rolling over in response to a command."""
        print(f"{self.name} rolled over!")
```

这里有很多需要注意的地方，但不用担心。你将在本章中不断看到这种结构，并有充足的时间来熟悉它。我们首先定义一个名为 Dog 的类 ❶。按照惯例，Python 中大写名称指的是类。类定义中没有括号，因为我们是从头开始创建这个类。然后我们编写一个描述该类作用的文档字符串。

### `__init__()` 方法

类的一部分的函数是 *方法*。关于函数的所有内容同样适用于方法；目前唯一的实际区别是我们调用方法的方式。`__init__()` 方法 ② 是 Python 在每次基于 Dog 类创建新实例时自动运行的特殊方法。此方法有两个前导下划线和两个尾随下划线，这是一种约定，有助于防止 Python 的默认方法名称与你的方法名称冲突。请确保在 `__init__()` 的两侧使用两个下划线。如果你每侧只使用一个，方法在你使用类时不会自动调用，这可能导致难以识别的错误。

我们定义 `__init__()` 方法有三个参数：`self`、`name` 和 `age`。`self` 参数在方法定义中是必需的，并且必须排在其他参数之前。它必须包含在定义中，因为当 Python 稍后调用此方法时（以创建 Dog 的实例），方法调用会自动传递 `self` 参数。与实例关联的每个方法调用都会自动传递 `self`，它是对实例本身的引用；它使单个实例能够访问类中的属性和方法。当我们创建 Dog 的实例时，Python 将从 Dog 类调用 `__init__()` 方法。我们将把一个名字和一个年龄作为参数传递给 `Dog()`；`self` 会自动传递，因此我们无需传递它。每当我们想从 Dog 类创建实例时，我们只需为最后两个参数 `name` 和 `age` 提供值。

`__init__()` 方法体中定义的两个变量都以 `self` 为前缀 ③。任何以 `self` 为前缀的变量都可供类中的每个方法使用，并且我们还可以通过从该类创建的任何实例访问这些变量。行 `self.name = name` 取得与参数 `name` 关联的值并将其分配给变量 `name`，然后将其附加到正在创建的实例上。对 `self.age = age` 也是同样的过程。可以通过这种方式通过实例访问的变量称为 *属性*。

`Dog` 类还有两个其他方法定义：`sit()` 和 `roll_over()`。由于这些方法运行不需要额外信息，我们只需将它们定义为一个参数 `self`。稍后我们创建的实例将可以访问这些方法。换句话说，它们将能够坐下和打滚。目前，`sit()` 和 `roll_over()` 并没有做太多。它们只是打印一条消息，说明狗正在坐下或打滚。但这个概念可以扩展到现实情况：如果这个类是计算机游戏的一部分，这些方法将包含使动画狗坐下和打滚的代码。如果这个类是用来控制机器人的，这些方法将指导导致机器狗坐下和打滚的动作。

### 从类创建实例

把类看作是一组制作实例的说明。`Dog` 类是一组说明，告诉 Python 如何制作代表特定狗的单个实例。

让我们创建一个代表特定狗的实例：

```python
class Dog:
    --snip--

my_dog = Dog('Willie', 6)

print(f"My dog's name is {my_dog.name}.")
print(f"My dog is {my_dog.age} years old.")
```

我们在这里使用的 `Dog` 类就是我们在前一个示例中刚编写的类。在这里，我们告诉 Python 创建一只名字为 `'Willie'`、年龄为 6 的狗。当 Python 读取这一行时，它会用参数 `Willie` 和 6 调用 Dog 中的 `__init__()` 方法。`__init__()` 方法创建代表这只特定狗的实例，并使用我们提供的值设置名字和年龄属性。Python 然后返回代表这只狗的实例。我们将该实例分配给变量 `my_dog`。命名约定在这里很有帮助；我们通常可以假设像 `Dog` 这样的大写名称指的是一个类，而像 `my_dog` 这样的小写名称指的是从类创建的单个实例。

### 访问属性

要访问实例的属性，你使用点表示法。我们通过编写以下内容来访问 `my_dog` 的属性 name 的值：

```
my_dog.name
```

点表示法在 Python 中经常使用。这种语法展示了 Python 如何查找属性的值。在这里，Python 查看实例 `my_dog`，然后找到与 `my_dog` 关联的属性 `name`。这就是在类 `Dog` 中称为 `self.name` 的同一个属性。我们使用相同的方法来处理属性 `age`。

输出是我们对 `my_dog` 的了解的总结：

```
My dog's name is Willie.
My dog is 6 years old.
```

### 调用方法

在我们从类 `Dog` 创建实例后，我们可以使用点表示法调用 `Dog` 中定义的任何方法。让我们让我们的狗坐下并打滚：

```python
class Dog:
    --snip--

my_dog = Dog('Willie', 6)
my_dog.sit()
my_dog.roll_over()
```

要调用方法，请给出实例的名称（在本例中为 `my_dog`）和你想要调用的方法，用点分隔。当 Python 读取 `my_dog.sit()` 时，它会在类 `Dog` 中查找方法 `sit()` 并运行该代码。Python 以相同的方式解释行 `my_dog.roll_over()`。

现在 Willie 按照我们的指示行事：

```text
Willie is now sitting.
Willie rolled over!
```

这种语法非常有用。当属性和方法被赋予适当描述性的名称如 `name`、`age`、`sit()` 和 `roll_over()` 时，我们可以很容易地推断出一段代码，即使是以前从未见过的代码， supposed to do。

### 创建多个实例

你可以根据需要从一个类创建任意多个实例。让我们创建一个名为 `your_dog` 的第二只狗：

```python
class Dog:
    --snip--

my_dog = Dog('Willie', 6)
your_dog = Dog('Lucy', 3)

print(f"My dog's name is {my_dog.name}.")
print(f"My dog is {my_dog.age} years old.")
my_dog.sit()

print(f"\nYour dog's name is {your_dog.name}.")
print(f"Your dog is {your_dog.age} years old.")
your_dog.sit()
```




在本示例中，我们创建了一只名为 Willie 的狗和一只名为 Lucy 的狗。每只狗都是一个独立的实例，拥有自己的一组属性，并且能够执行相同的操作：

```
My dog's name is Willie.
My dog is 6 years old.
Willie is now sitting.

Your dog's name is Lucy.
Your dog is 3 years old.
Lucy is now sitting.
```

即使第二只狗使用了相同的名字和年龄，Python 仍会从 Dog 类创建一个独立的实例。你可以根据需要从一个类创建任意多个实例，只要为每个实例指定一个唯一的变量名，或者让它在列表或字典中占据唯一的位置即可。

## 动手试一试

- **9-1. 餐厅：** 创建一个名为 `Restaurant` 的类。`Restaurant` 的 `__init__()` 方法应存储两个属性：`restaurant_name` 和 `cuisine_type`。创建一个名为 `describe_restaurant()` 的方法，打印这两项信息，再创建一个名为 `open_restaurant()` 的方法，打印一条消息，指出餐厅正在营业。

  从你的类创建一个名为 `restaurant` 的实例。分别打印这两个属性，然后调用这两个方法。

- **9-2. 三家餐厅：** 从练习 9-1 中的类开始，创建三个不同的实例，并对每个实例调用 `describe_restaurant()`。

- **9-3. 用户：** 创建一个名为 `User` 的类。创建两个属性 `first_name` 和 `last_name`，然后创建几个用户资料中通常会存储的其他属性。创建一个名为 `describe_user()` 的方法，打印用户信息的摘要。再创建另一个名为 `greet_user()` 的方法，打印一条针对该用户的个性化问候语。

  创建几个表示不同用户的实例，并对每个用户调用这两个方法。

由这个类创建。当我们创建一个新的 `Car` 实例时，我们需要为实例指定品牌、型号和年份。

我们定义了一个名为 `get_descriptive_name()` 的方法，它将汽车的年份、品牌和型号组合成一个整洁地描述汽车的字符串。这样我们就不必逐一打印每个属性的值了。为了在该方法中处理属性值，我们使用了 `self.make`、`self.model` 和 `self.year`。在类外部，我们从 `Car` 类创建一个实例，并将其赋给变量 `my_new_car`。然后调用 `get_descriptive_name()` 来展示我们拥有的是哪种车：

```
2024 Audi A4
```

为了让类更有趣，我们添加一个会随时间变化的属性。我们将添加一个存储汽车总里程的属性。

### 为属性设置默认值

创建实例时，有些属性可以不作为参数传入就进行定义。这些属性可以在 `__init__()` 方法中定义，并在其中被赋予默认值。

让我们添加一个名为 `odometer_reading` 的属性，它的初始值始终为 0。我们还将添加一个名为 `read_odometer()` 的方法，帮助我们读取每辆车的里程表：

```python
class Car:

    def __init__(self, make, model, year):
        """Initialize attributes to describe a car."""
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0
```

```
def get_descriptive_name(self):
    --snip--

def read_odometer(self):
    """Print a statement showing the car's mileage."""
    print(f"This car has {self.odometer_reading} miles on it.")

my_new_car = Car('audi', 'a4', 2024)
print(my_new_car.get_descriptive_name())
my_new_car.read_odometer()
```

这一次，当 Python 调用 `__init__()` 方法创建新实例时，它会像上一个示例一样，将品牌、型号和年份的值存储为属性。然后 Python 会创建一个名为 `odometer_reading` 的新属性，并将其初始值设为 0 ❶。我们还有一个名为 `read_odometer()` 的新方法 ❷，可以方便地读取汽车的里程数。

我们的汽车从 0 里程开始：

```
2024 Audi A4
This car has 0 miles on it.
```

没有多少汽车在出售时里程表上正好是 0 英里，所以我们需要一种方法来修改这个属性的值。

### 修改属性的值

你可以通过三种方式修改属性的值：直接通过实例修改值、通过方法设置值，或者通过方法递增该值（为其增加一定的量）。我们来看看这些方法。

### 直接修改属性的值

修改属性值最简单的方法是通过实例直接访问该属性。这里我们直接将里程表读数设置为 23：

```python
class Car:
    --snip--

my_new_car = Car('audi', 'a4', 2024)
print(my_new_car.get_descriptive_name())

my_new_car.odometer_reading = 23
my_new_car.read_odometer()
```

我们使用点号表示法访问汽车的 `odometer_reading` 属性，并直接设置其值。这一行告诉 Python 获取实例 `my_new_car`，找到与之关联的 `odometer_reading` 属性，并将其值设置为 23：

```text
2024 Audi A4
This car has 23 miles on it.
```

有时你会想这样直接访问属性，但其他时候你可能想编写一个方法来为你更新值。

### 通过方法修改属性的值

拥有一些为你更新特定属性的方法会很有帮助。不必直接访问属性，而是将新值传递给一个在内部处理更新的方法。

这里是一个名为 `update_odometer()` 的方法示例：

```python
class Car:
    --snip--

    def update_odometer(self, mileage):
```

如果里程数大于或等于现有的里程数 `self.odometer_reading`，你就可以将里程表读数更新为新的里程数 ❶。如果新的里程数小于现有的里程数，你会收到一条警告，提示你不能回拨里程表 ❷。

### 通过方法递增属性的值

有时你会希望按一定的量递增属性的值，而不是设置一个全新的值。假设我们买了一辆二手车，从购买到登记之间开了 100 英里。下面是一个方法，允许我们传入这个增量，并将其加到里程表读数上：

```
class Car:
    --snip--

    def update_odometer(self, mileage):
        --snip--

    def increment_odometer(self, miles):
        """Add the given amount to the odometer reading."""
        self.odometer_reading += miles

❶ my_used_car = Car('subaru', 'outback', 2019)
    print(my_used_car.get_descriptive_name())

❷ my_used_car.update_odometer(23_500)
    my_used_car.read_odometer()

    my_used_car.increment_odometer(100)
    my_used_car.read_odometer()
```

新方法 `increment_odometer()` 接收一个英里数，并将该值加到 `self.odometer_reading` 上。首先，我们创建一辆二手车 `my_used_car` ❶。我们调用 `update_odometer()` 并传入 `23_500`，将它的里程表设置为 23 500 ❷。最后，我们调用 `increment_odometer()` 并传入 `100`，把买车和登记之间开的 100 英里加进去：

```
2019 Subaru Outback
This car has 23500 miles on it.
This car has 23600 miles on it.
```

你可以修改这个方法以拒绝负增量，这样也没有人能使用这个函数回拨里程表。

> 注意
> *你可以用这样的方法控制程序用户更新诸如里程表读数的值，但任何能访问该程序的人都可以通过直接访问属性将里程表读数设置为任意值。真正有效的安全措施除了像这里展示的基本检查之外，还需要极其关注细节。*

当你基于现有的类编写新类时，通常会想调用父类的 `__init__()` 方法。这将初始化在父类 `__init__()` 方法中定义的所有属性，并让它们在子类中可用。

例如，让我们模拟一辆电动汽车。电动汽车只是汽车的一种特定类型，所以我们可以基于之前编写的 `Car` 类来创建新的 `ElectricCar` 类。这样，我们只需编写电动汽车特有的属性和行为的代码。

让我们先创建 `ElectricCar` 类的一个简单版本，它能完成 `Car` 类所做的一切：

```python
class Car:
    """A simple attempt to represent a car."""

    def __init__(self, make, model, year):
        """Initialize attributes to describe a car."""
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0

    def get_descriptive_name(self):
        """Return a neatly formatted descriptive name."""
        long_name = f"{self.year} {self.make} {self.model}"
        return long_name.title()

    def read_odometer(self):
        """Print a statement showing the car's mileage."""
        print(f"This car has {self.odometer_reading} miles on it.")

    def update_odometer(self, mileage):
        """Set the odometer reading to the given value."""
        if mileage >= self.odometer_reading:
            self.odometer_reading = mileage
        else:
            print("You can't roll back an odometer!")
```




```
def increment_odometer(self, miles):
    """Add the given amount to the odometer reading."""
    self.odometer_reading += miles
```

```
2 class ElectricCar(Car):
    """Represent aspects of a car, specific to electric vehicles."""
```

```
3     def __init__(self, make, model, year):
        """Initialize attributes of the parent class."""
4         super().__init__(make, model, year)
```

```
5 my_leaf = ElectricCar('nissan', 'leaf', 2024)
print(my_leaf.get_descriptive_name())
```

我们从 Car ① 开始。创建子类时，父类必须是当前文件的一部分，并且必须在文件中位于子类之前。然后我们定义子类 ElectricCar ②。在子类的定义中，父类的名称必须包含在括号内。__init__() 方法接收创建 Car 实例所需的信息 ③。

super() 函数 ④ 是一个特殊函数，允许你调用父类的方法。这一行告诉 Python 调用 Car 中的 __init__() 方法，这会让 ElectricCar 实例获得该方法中定义的所有属性。super 这个名称来自一个约定：将父类称为超类，将子类称为子类。

我们尝试用与创建普通汽车时相同的信息来创建一辆电动汽车，以测试继承是否正常工作。我们创建一个 ElectricCar 类的实例，并将其赋值给 my_leaf ⑤。这一行调用在 ElectricCar 中定义的 __init__() 方法，该方法又告诉 Python 调用父类 Car 中定义的 __init__() 方法。我们提供了参数 'nissan'、'leaf' 和 2024。

除了 __init__() 之外，目前还没有专门针对电动汽车的属性或方法。此时，我们只是在确保电动汽车具有恰当的 Car 行为：

```
2024 Nissan Leaf
```

ElectricCar 实例的工作方式与 Car 实例完全一样，所以现在我们可以开始定义专门针对电动汽车的属性和方法。

## 为子类定义属性和方法

一旦你有了继承自父类的子类，就可以添加任何必要的新属性和方法，以将子类与父类区分开来。

让我们添加一个专门针对电动汽车的属性（例如电池），以及一个报告该属性的方法。我们将存储电池容量，并编写一个打印电池描述的方法：

```
class Car:
    --snip--

class ElectricCar(Car):
    """Represent aspects of a car, specific to electric vehicles."""

    def __init__(self, make, model, year):
        """
        Initialize attributes of the parent class.
        Then initialize attributes specific to an electric car.
        """
```

```
super().__init__(make, model, year)
self.battery_size = 40

def describe_battery(self):
    """Print a statement describing the battery size."""
    print(f"This car has a {self.battery_size}-kWh battery.")

my_leaf = ElectricCar('nissan', 'leaf', 2024)
print(my_leaf.get_descriptive_name())
my_leaf.describe_battery()
```

我们添加一个新属性 `self.battery_size`，并将其初始值设为 40 ❶。该属性将与所有从 `ElectricCar` 类创建的实例相关联，但不会与 `Car` 的任何实例相关联。我们还添加了一个名为 `describe_battery()` 的方法，用于打印电池信息 ❷。当我们调用这个方法时，得到的描述明确是专门针对电动汽车的：

```
2024 Nissan Leaf
This car has a 40-kWh battery.
```

你可以对 `ElectricCar` 类进行任意程度的专门化。你可以根据需要添加任意多的属性和方法，以所需的精度来模拟电动汽车。任何属于任意汽车而非专门针对电动汽车的属性或方法，都应添加到 `Car` 类中，而不是 `ElectricCar` 类中。这样，任何使用 `Car` 类的人也都能使用该功能，而 `ElectricCar` 类将只包含专门针对电动汽车的信息和行为的代码。

## 重写父类的方法

你可以重写父类中任何不符合你试图用子类建模内容的方法。

要做到这一点，请在子类中定义一个与父类中要重写的方法同名的方法。Python 将忽略父类方法，只关注你在子类中定义的方法。

假设 `Car` 类有一个名为 `fill_gas_tank()` 的方法。对于纯电动汽车来说，这个方法毫无意义，因此你可能想要重写它。下面是一种做法：

```
```python
class ElectricCar(Car):
    # --snip--

    def fill_gas_tank(self):
        """Electric cars don't have gas tanks."""
        print("This car doesn't have a gas tank!")
```
```

现在，如果有人试图对电动汽车调用 `fill_gas_tank()`，Python 将忽略 `Car` 中的 `fill_gas_tank()` 方法，并转而运行这段代码。使用继承时，你可以让子类保留你需要的内容，并重写父类中你不需要的任何内容。

## 将实例用作属性

在代码中模拟现实世界的事物时，你可能会发现自己正在向类中添加越来越多的细节。你会发现属性和方法列表越来越长，文件也越来越长。在这些情况下，你可能会意识到，一个类的部分内容可以编写成一个独立的类。你可以将大类拆分成协同工作的小类；这种方法称为*组合*。

例如，如果我们继续向 `ElectricCar` 类添加细节，可能会注意到我们添加了许多专门针对汽车电池的属性和方法。当我们发现这种情况时，可以停下来，将这些属性和方法移动到一个名为 Battery 的独立类中。然后，我们可以在 ElectricCar 类中使用 Battery 实例作为属性：

```
class Car:
    --snip--

class Battery:
    """A simple attempt to model a battery for an electric car."""

    def __init__(self, battery_size=40):
        """Initialize the battery's attributes."""
        self.battery_size = battery_size

    def describe_battery(self):
        """Print a statement describing the battery size."""
        print(f"This car has a {self.battery_size}-kWh battery.")

class ElectricCar(Car):
    """Represent aspects of a car, specific to electric vehicles."""

    def __init__(self, make, model, year):
        """
        Initialize attributes of the parent class.
        Then initialize attributes specific to an electric car.
        """
        super().__init__(make, model, year)
        self.battery = Battery()

my_leaf = ElectricCar('nissan', 'leaf', 2024)
print(my_leaf.get_descriptive_name())
my_leaf.battery.describe_battery()
```

我们定义了一个名为 Battery 的新类，它不继承自任何其他类。__init__() 方法 ① 除了 self 之外，还有一个参数 battery_size。这是一个可选参数，如果未提供值，则会将电池容量设置为 40。方法 `describe_battery()` 也被移到了这个类中 ❷。

在 `ElectricCar` 类中，我们现在添加一个名为 `self.battery` 的属性 ❸。这一行告诉 Python 创建一个新的 `Battery` 实例（默认容量为 40，因为我们没有指定值），并将该实例赋值给属性 `self.battery`。每次调用 `__init__()` 方法时都会发生这种情况；现在，任何 `ElectricCar` 实例都会自动创建一个 `Battery` 实例。

我们创建一辆电动汽车，并将其赋值给变量 `my_leaf`。当我们想要描述电池时，需要通过汽车的 `battery` 属性来实现：

```
my_leaf.battery.describe_battery()
```

这一行告诉 Python 查看实例 `my_leaf`，找到它的 `battery` 属性，并调用与赋值给该属性的 `Battery` 实例相关联的 `describe_battery()` 方法。

输出与之前看到的完全相同：

```
2024 Nissan Leaf
This car has a 40-kWh battery.
```

这看起来像是很多额外的工作，但现在我们可以随心所欲地详细描述电池，而不会使 `ElectricCar` 类显得杂乱。让我们向 `Battery` 再添加一个方法，根据电池容量报告汽车的续航里程：

```
class Car:
    --snip--

class Battery:
    --snip--
```


```
def get_range(self):
    """打印关于此电池续航里程的说明。"""
    if self.battery_size == 40:
        range = 150
    elif self.battery_size == 65:
        range = 225

    print(f"此车充满电后可行驶约 {range} 英里。")
```

```
class ElectricCar(Car):
    --snip--

my_leaf = ElectricCar('nissan', 'leaf', 2024)
print(my_leaf.get_descriptive_name())
my_leaf.battery.describe_battery()
① my_leaf.battery.get_range()
```

新方法 `get_range()` 进行了一些简单的分析。如果电池容量为 40 kWh，`get_range()` 将续航里程设置为 150 英里；如果容量为 65 kWh，则设置为 225 英里。然后它会报告该值。当我们想使用这个方法时，仍然必须通过汽车的 `battery` 属性来调用它 ①。

输出结果根据电池容量告诉我们汽车的续航里程：

```
2024 Nissan Leaf
This car has a 40-kWh battery.
This car can go about 150 miles on a full charge.
```

## 为现实世界对象建模

当你开始对诸如电动汽车之类的更复杂事物进行建模时，你会遇到一些有趣的问题。电动汽车的续航里程是电池的属性还是汽车的属性？如果我们只描述一辆汽车，那么将 `get_range()` 方法与 `Battery` 类关联起来可能没问题。但如果我们描述的是制造商的整个产品线，我们可能希望将 `get_range()` 移到 `ElectricCar` 类中。`get_range()` 方法仍会在确定续航里程之前检查电池容量，但它会报告与其关联的汽车类型所对应的续航里程。或者，我们可以保持 `get_range()` 方法与电池的关联，但向它传递一个参数，例如 `car_model`。然后 `get_range()` 方法将根据电池容量和汽车型号报告续航里程。

这将你带入了作为程序员成长过程中一个有趣的节点。当你思考这类问题时，你是在更高的逻辑层面思考，而不是专注于语法层面。你思考的不是 Python，而是如何在代码中表示现实世界。当你达到这一点时，你会意识到在现实世界情境的建模中，往往没有绝对正确或错误的方法。有些方法比其他的更高效，但找到最有效的表示方法需要练习。如果你的代码能按照你想要的方式工作，那你就做得很好！如果你发现自己反复拆解和重写类，尝试不同的方法，也不要气馁。在追求编写准确、高效代码的过程中，每个人都会经历这个过程。

## 试一试

**9-6. 冰淇淋小摊：** 冰淇淋小摊是一种特定类型的餐馆。编写一个名为 `IceCreamStand` 的类，它继承自你在练习 9-1（第 162 页）或练习 9-4（第 166 页）中编写的 `Restaurant` 类。任何一个版本的类都可以；选择你更喜欢的一个即可。添加一个名为 `flavors` 的属性，用于存储冰淇淋口味的列表。编写一个显示这些口味的方法。创建一个 `IceCreamStand` 的实例，并调用此方法。

**9-7. 管理员：** 管理员是一种特殊类型的用户。编写一个名为 `Admin` 的类，它继承自你在练习 9-3（第 162 页）或练习 9-5（第 167 页）中编写的 `User` 类。添加一个属性 `privileges`，用于存储诸如 "can add post"、"can delete post"、"can ban user" 等字符串的列表。编写一个名为 `show_privileges()` 的方法，列出管理员的权限集。创建一个 `Admin` 的实例，并调用你的方法。

**9-8. 权限：** 编写一个单独的 `Privileges` 类。该类应有一个属性 `privileges`，用于存储练习 9-7 中所述的字符串列表。将 `show_privileges()` 方法移到该类中。在 `Admin` 类中创建一个 `Privileges` 实例作为其属性。创建一个新的 `Admin` 实例，并使用你的方法显示其权限。

**9-9. 电池升级：** 使用本节中 *electric_car.py* 的最终版本。在 `Battery` 类中添加一个名为 `upgrade_battery()` 的方法。此方法应检查电池容量，如果尚未达到 65，则将其设置为 65。制作一个使用默认电池容量的电动汽车，调用一次 `get_range()`，然后在升级电池后再次调用 `get_range()`。你应该会看到汽车续航里程的增加。

## 导入类

随着你为类添加更多功能，即使你正确使用了继承和组合，文件也可能会变得很长。为了遵循 Python 的总体理念，你需要尽可能保持文件的简洁。为此，Python 允许你将类存储在模块中，然后将你需要的类导入到主程序中。

## 导入单个类

让我们创建一个仅包含 `Car` 类的模块。这引出了一个微妙的问题：本章中我们已经有一个名为 car.py 的文件，但这个模块应该命名为 car.py，因为它包含代表汽车的代码。我们将通过将 `Car` 类存储在以 car.py 命名的模块中来解决这个命名问题，替换我们之前使用的 car.py 文件。从现在开始，任何使用此模块的程序都需要一个更具体的文件名，例如 my_car.py。以下是 car.py，只包含 `Car` 类的代码：

# car.py

```
"""一个可用于表示汽车的类。"""

class Car:
    """一次表示汽车的简单尝试。"""

    def __init__(self, make, model, year):
        """初始化描述汽车的属性。"""
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0

    def get_descriptive_name(self):
        """返回格式整洁的描述性名称。"""
        long_name = f"{self.year} {self.make} {self.model}"
        return long_name.title()

    def read_odometer(self):
        """打印一条显示汽车里程的语句。"""
        print(f"This car has {self.odometer_reading} miles on it.")

    def update_odometer(self, mileage):
        """
        将里程表读数设置为给定值。
        如果尝试回调里程表，则拒绝该更改。
        """
        if mileage >= self.odometer_reading:
            self.odometer_reading = mileage
```

```
```python
else:
    print("你不能回调里程表！")

def increment_odometer(self, miles):
    """将给定里程添加到里程表读数中。"""
    self.odometer_reading += miles
```
```

我们包含一个模块级文档字符串，简要描述此模块的内容 ❶。你应该为创建的每个模块编写一个文档字符串。

现在我们创建一个名为 *my_car.py* 的单独文件。此文件将导入 `Car` 类，然后创建该类的实例：

```
**my_car.py**

```python
❶ from car import Car

my_new_car = Car('audi', 'a4', 2024)
print(my_new_car.get_descriptive_name())

my_new_car.odometer_reading = 23
my_new_car.read_odometer()
```
```

导入语句 ❶ 告诉 Python 打开 car 模块并导入 `Car` 类。现在我们可以使用 `Car` 类，就像它是在此文件中定义的一样。输出与我们之前看到的相同：

```
```text
2024 Audi A4
This car has 23 miles on it.
```
```

导入类是一种有效的编程方式。想象一下，如果将整个 `Car` 类包含在内，这个程序文件会有多长。相反，当你将类移到模块并导入该模块时，你仍然可以获得所有相同的功能，但你可以保持主程序文件的干净和易于阅读。你还可以将大部分逻辑存储在单独的文件中；一旦你的类按照你的想法正常工作，你就可以

```
```python
    def __init__(self, make, model, year):
        """
        初始化父类的属性。
        然后初始化电动汽车特有的属性。
        """
        super().__init__(make, model, year)
        self.battery = Battery()
```
```

现在我们可以创建一个名为 *my_electric_car.py* 的新文件，导入 `ElectricCar` 类，并制作一辆电动汽车：

**my_electric_car.py**

```
```python
from car import ElectricCar

my_leaf = ElectricCar('nissan', 'leaf', 2024)
print(my_leaf.get_descriptive_name())
my_leaf.battery.describe_battery()
my_leaf.battery.get_range()
```
```

这与我们之前看到的输出相同，尽管大部分逻辑都隐藏在模块中：

```
```
2024 Nissan Leaf
This car has a 40-kWh battery.
This car can go about 150 miles on a full charge.
```
```

## 从模块导入多个类

你可以根据需要在程序文件中导入任意数量的类。如果我们想在同一文件中制作一辆普通汽车和一辆电动汽车，我们需要同时导入 `Car` 和 `ElectricCar` 类：

**my_cars.py**

```
```python
from car import Car, ElectricCar

my_mustang = Car('ford', 'mustang', 2024)
```
```



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





在第 10 章，你将学习如何处理文件，以便保存你在程序中完成的工作以及允许用户完成的工作。你还将学习关于 *exceptions*，一种特殊的 Python 类，旨在帮助你在出现错误时做出响应。
