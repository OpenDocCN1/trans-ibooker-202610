## 类

- 7.1 关于类
- 7.2 方法
- 7.3 创建实例
- 7.4 添加方法
- 7.5 多个实例
- 7.6 继承
- 7.7 子类方法
- 7.8 重写父类方法
- 7.9 在模块中存储类

### 关于类

类将数据与操作组合在一起并组织代码。要使用类，需要创建类的一个实例。属性是与类关联的变量，方法是与类关联的函数。

这个类表示一条路径（例如一条徒步路径），包含两个属性 self.dest 和 self.len，以及一个方法 __init__()：

```python
class Trail():
    """A class to represent trails."""

    def __init__(self, dest, len=0):
        self.dest = dest
        self.len = len
```

Python 类通常使用首字母大写。当创建类的实例时，__init__() 方法会自动运行。在这段代码中，__init__() 需要一个参数 dest，并且有一个可选参数 len。

第一个参数 self 引用类的当前实例。将值 dest 和 len 附加到 self 上，使它们可供类中的所有方法以及类的任何实例使用。

### 创建实例

- 如何创建类的实例？
- 如何通过实例访问属性的值？
- 如何使用实例调用方法？

### 多个实例

- 如何从一个类创建多个实例？

### 继承

- 什么是继承？
- 什么是父类？
- 什么是子类？
- 如何使用继承？

通过使用*继承*，你可以创建一个继承原始类**属性**和**方法**的新类。

我们将创建一个表示自行车道的类：*父类*是 Trail，*子类*是 BikeTrail。定义子类的方式与普通类相同，但要在括号中包含父类名称：

```python
class BikeTrail(Trail):
    """Represent a bike trail."""

    def __init__(self, dest, len=0):
        super().__init__(dest, len)
        self.paved = True
        self.bikes_only = True
```

子类的 __init__() 方法通常需要使用 super() 调用父类的 __init__() 方法。super() 函数引用父类，允许你调用父类中的方法。子类可以定义属于自己的其他属性。例如，self.bikes_only 属性表示该路径是否对非骑行人员关闭。

### 重写父类方法

- 子类如何修改父类方法的行为？

### 使用 `super()` 方法

这个 BikeTrail 类修改了 `run_trail()` 方法的行为。

```python
class BikeTrail(Trail):
    """Represent a bike trail."""

    def __init__(self, dest, len=0):
        --snip--

    def run_trail(self):
        """Simulate running the trail."""
        if self.bikes_only:
            print("You can't run this trail!")
        else:
            super().run_trail()

cross_trail = BikeTrail("Harbor Mountain", 5)
cross_trail.run_trail()
```

该方法首先检查这是否是一条仅供自行车使用的路径；如果是，则打印相应的消息：

```text
You can't run this trail!
```

如果路径允许跑步者，该方法将调用父类方法 `run_trail()`，从而继承普通 `Trail` 的行为。

### 在模块中存储类

这里我们将类 Trail 和 BikeTrail 存储在 trails.py 文件中：

```python
class Trail():
    """A class to represent trails."""
    --snip--

class BikeTrail(Trail):
    """Represent a bike trail."""
    --snip--
```

现在我们可以将这些类导入到另一个文件中：

```python
from trails import Trail, BikeTrail

verst = Trail("Mt. Verstovia", 4)
cross_trail = BikeTrail("Harbor Mt", 5)
```

使用点表示法导入整个模块：

```python
import trails

verst = trails.Trail("Mt. Verstovia", 4)
cross_trail = trails.BikeTrail("Harbor Mt", 5)
```

导入类时可以使用别名。通过存储类来组织代码，并使这些类可供多个程序使用。
