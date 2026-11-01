
接下来，修改 main() 中的这一行，使其实例化 HintBoard 对象而不是 TTTBoard 对象：

```
gameBoard = HintBoard() # Create a TTT board object.
```

除了对 main() 的这一行更改外，其余程序与之前完全一样。现在运行程序时，输出看起来会像这样：

```
Welcome to Tic-Tac-Toe!
--snip--
    X| |   1 2 3
    -+-+
    | |0   4 5 6
    -+-+
    | |X   7 8 9
X can win in one more move.
What is O's move? (1-9)
5

    X| |   1 2 3
    -+-+
    |O|0   4 5 6
    -+-+
    | |X   7 8 9
O can win in one more move.
--snip--
The game is a tie!
Thanks for playing!
```

在方法末尾，如果 xCanWin 或 oCanWin 为 True，则会在 boardStr 字符串中添加一条相应的额外消息。最后返回 boardStr。

并非每个被重写的方法都需要使用 super()！如果一个类的重写方法与父类中被重写的方法完全不同，则无需使用 super() 调用被重写的方法。super() 函数在类具有多个父方法时尤其有用，如本章后面“多重继承”部分所述。

## 优先使用组合而非继承

继承是代码复用的绝佳技术，你可能想立即在所有类中使用它。但你可能并不总是希望基类和子类紧密耦合。创建多层继承带来的更多是官僚主义，而非组织性。

虽然你可以使用继承来处理具有“is a”关系的类（换句话说，当子类是父类的一种时），但对于具有“has a”关系的类，使用一种称为组合的技术往往更有利。组合是一种类设计技术，即在你的类中包含对象，而不是继承这些对象的类。这就是我们在类中添加
属性时所做的事情。在使用继承设计类时，优先考虑组合而非继承。这正是我们在本章和上一章的所有示例中所做的，如以下列表所述：

- 一个 WizCoin 对象“拥有”加隆、锡克尔和纳特硬币的数量。
- 一个 TTTBoard 对象“拥有”九个格子的集合。
- 一个 MiniBoard 对象“是” TTTBoard 对象，因此它也“拥有”九个格子的集合。
- 一个 HintBoard 对象“是” TTTBoard 对象，因此它也“拥有”九个格子的集合。

让我们回到上一章的 WizCoin 类。如果我们创建一个新的 WizardCustomer 类来表示魔法世界中的顾客，这些顾客会携带一定数量的金钱，我们可以用 WizCoin 类来表示。但这两个类之间没有“is a”关系；WizardCustomer 对象不是 WizCoin 对象的一种。如果使用继承，可能会产生一些别扭的代码：

```
import wizcoin

class WizardCustomer(wizcoin.WizCoin):
    def __init__(self, name):
        self.name = name
        super().__init__(0, 0, 0)

wizard = WizardCustomer('Alice')
print(f'{wizard.name} has {wizard.value()} knuts worth of money.')
print(f'{wizard.name}\'s coins weigh {wizard.weightInGrams()} grams.')
```

在这个示例中，WizardCustomer 继承了 WizCoin 对象的 方法，例如 value() 和 weightInGrams()。从技术上讲，继承自 WizCoin 的 WizardCustomer 可以完成与将 WizCoin 对象作为属性包含的 WizardCustomer 相同的任务。但 wizard.value() 和 wizard.weightInGrams() 方法名具有误导性：它们似乎会返回巫师的价值和重量，而不是巫师硬币的价值和重量。此外，如果我们以后想为巫师的体重添加 weightInGrams() 方法，该方法名已经被占用了。

最好将 WizCoin 对象作为属性，因为巫师顾客“拥有”一定数量的巫师硬币：

```
import wizcoin

class WizardCustomer:
    def __init__(self, name):
        self.name = name
        self.purse = wizcoin.WizCoin(0, 0, 0)

wizard = WizardCustomer('Alice')
print(f'{wizard.name} has {wizard.purse.value()} knuts worth of money.')
print(f'{wizard.name}\'s coins weigh {wizard.purse.weightInGrams()} grams.')
```

与其让 WizardCustomer 类继承 WizCoin 的方法，我们给 WizardCustomer 类一个 purse 属性 ①，其中包含一个 WizCoin 对象。当你使用组合时，对 WizCoin 类方法的任何更改都不会改变 WizardCustomer 类的方法。这种技术为两个类的未来设计更改提供了更大的灵活性，并带来更易维护的代码。

## 继承的弊端

继承的主要弊端是，对父类所做的任何未来更改都必然会被其所有子类继承。在大多数情况下，这种紧密耦合正是你想要的。但在某些情况下，你的代码需求无法轻松适应你的继承模型。

例如，假设我们在一个车辆模拟程序中有 Car、Motorcycle 和 LunarRover 类。它们都需要类似的方法，例如 startIgnition() 和 changeTire()。与其将这段代码复制粘贴到每个类中，我们可以创建一个父类 Vehicle，并让 Car、Motorcycle 和 LunarRover 继承它。现在如果我们需要在 changeTire() 方法中修复一个错误，只有一个地方需要更改。这在我们有数十个继承自 Vehicle 的不同车辆相关类时尤其有帮助。这些类的代码看起来像这样：

```
class Vehicle:
    def __init__(self):
        print('Vehicle created.')
    def startIgnition(self):
        pass  # Ignition starting code goes here.
    def changeTire(self):
        pass  # Tire changing code goes here.

class Car(Vehicle):
    def __init__(self):
        print('Car created.')

class Motorcycle(Vehicle):
    def __init__(self):
        print('Motorcycle created.')

class LunarRover(Vehicle):
    def __init__(self):
        print('LunarRover created.')
```

但对 Vehicle 的所有未来更改也会影响这些子类。如果我们需要一个 changeSparkPlug() 方法会怎样？汽车和摩托车有带火花塞的内燃机，但月球漫游车没有。通过优先使用组合而非继承，我们可以创建单独的 CombustionEngine 和 ElectricEngine 类。然后我们设计 Vehicle 类，使其“has an”
engine 属性，可以是 CombustionEngine 或 ElectricEngine 对象，并具有适当的方法：

```
class CombustionEngine:
    def __init__(self):
        print('Combustion engine created.')
    def changeSparkPlug(self):
        pass  # Spark plug changing code goes here.

class ElectricEngine:
    def __init__(self):
        print('Electric engine created.')

class Vehicle:
    def __init__(self):
        print('Vehicle created.')
        self.engine = CombustionEngine()  # Use this engine by default.
--snip--

class LunarRover(Vehicle):
    def __init__(self):
        print('LunarRover created.')
        self.engine = ElectricEngine()
```

这可能需要重写大量代码，特别是如果你有几个类继承自现有的 Vehicle 类：所有 vehicleObj.changeSparkPlug() 调用都需要变为 vehicleObj.engine.changeSparkPlug()，适用于 Vehicle 类或其子类的每个对象。由于如此大规模的更改可能会引入错误，你可能只想让 LunarVehicle 的 changeSparkPlug() 方法什么都不做。在这种情况下，Pythonic 的做法是在 LunarVehicle 类内部将 changeSparkPlug 设置为 None：

```
class LunarRover(Vehicle):
    changeSparkPlug = None
    def __init__(self):
        print('LunarRover created.')
```

changeSparkPlug = None 行遵循本章后面“类属性”部分所述的语法。这会覆盖从 Vehicle 继承的 changeSparkPlug() 方法，因此使用 LunarRover 对象调用它会引发错误：

```
>>> myVehicle = LunarRover()
LunarRover created.
>>> myVehicle.changeSparkPlug()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'NoneType' object is not callable
```

此错误使我们能够快速失败，并在尝试使用 LunarRover 对象调用此不适当方法时立即发现问题。LunarRover 的任何子类也会继承 changeSparkPlug() 的此 None 值。TypeError: 'NoneType' object is not callable 错误消息告诉我们程序员
