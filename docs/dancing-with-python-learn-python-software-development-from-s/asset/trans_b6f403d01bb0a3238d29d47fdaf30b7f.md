
实例变量名以下划线开头，这向我们表明，这些变量应仅由此类及其任何派生类中的其他编码者私有使用。Python 并不强制执行这一点，但这是一个约定。

## 7.13.1 父类和子类

**Guitar** 是一个独立的类，但如果我们查看品牌、型号和制造年份这些属性，它们可以适用于任何商业乐器。所以让我们定义 **MusicalInstrument**：

```
class MusicalInstrument:
    def __init__(self, brand, model, year_built):
        self._brand = brand
        self._model = model
        self._year_built = year_built

    def __str__(self):
        return f"{self._year_built} " \
            + f"{self._brand} {self._model}"
```

在 **MusicalInstrument** 中，我们有
- 实例方法 __init__ 和 __str__，以及
- 实例变量 _brand、_model 和 _year_built。

现在我们让 **Guitar** 从 **MusicalInstrument** 派生。

```
class Guitar(MusicalInstrument):
    def __init__(self, brand, model, year_built, strings=6):
        super().__init__(brand, model, year_built)
        self._strings = strings

    def __str__(self):
        return f"{self._strings}-string " \
            + super().__str__()
```

**Guitar** 添加了实例变量 _strings。

```
isinstance(guitar, MusicalInstrument), isinstance(guitar, Guitar)
```

(True, True)

### 练习 7.31

向 Guitar 添加一个 _number_of_guitars 类变量，以及一个返回该类变量的类函数 get_count。每次我们初始化一个新实例时，都在 __init__ 中递增 _number_of_guitars。

如果我们只想通过 isinstance 将其与父类区分开来，那么我们可以不进行任何额外工作就派生一个子类。

```
class Clarinet(MusicalInstrument):
    pass
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_312_0.png)

图 7.3：MusicalInstrument 类层次结构，包含 Guitar 和 Clarinet

```
help(Clarinet)
```

```
Help on class Clarinet in module src.code.guitar_04:

class Clarinet(MusicalInstrument)
    Clarinet(brand, model, year_built)

    Method resolution order:
        Clarinet
        MusicalInstrument
        builtins.object

    Methods inherited from MusicalInstrument:
```

我们无法创建 **Clarinet**，因为我们没有实现 `__str__`，因此它仍然是抽象的。

```
print(Clarinet("Yamaha", "YCL-650", "2021"))
```

TypeError: Can't instantiate abstract class
Clarinet with abstract methods __str__

**Guitar** 是一个具体类，因为它实现了所有必需的抽象方法。在图中，具体类用实线框表示，抽象基类用虚线框表示。

![](img/4ffeafa3e3f1373f02971e43b0755ea6_316_0.png)

图 7.5：作为抽象基类的 MusicalInstrument

在实践中，您的抽象基类会更大，并且会有更多的方法，开发人员必须在派生的具体类中实现这些方法。

## 7.13.3 多重继承

我现在通过添加 **AcousticGuitar** 来完成我的吉他类层次结构。虽然这不完全正确，但我假设所有原声吉他都是木质的。具体来说，我说的是音板，也就是吉他的面板。

```
guitar = AcousticGuitar("Taylor", "AD27e", "2021", "mahogany")
print(guitar)
```

6-string 2021 Taylor AD27e with mahogany soundboard

![](img/4ffeafa3e3f1373f02971e43b0755ea6_318_0.png)

图 7.7：具有多重继承的 AcousticGuitar

注意我如何编写 __init__ 来调用 Guitar 和 Wooden 的初始化方法。

如果我在 AcousticGuitar 的实例上调用一个方法，Python 会首先在 AcousticGuitar 自身中查找，然后是 Guitar，再然后是 MusicalInstrument，最后在 Wooden 中查找。这被称为方法解析顺序。

如果有第三个类也是 AcousticGuitar 继承的，Python 会在 Wooden 之后搜索它及其父类。

### 练习 7.33

为层次结构中的每个类定义一个 whoami 方法，该方法返回类似“I am an ElectricGuitar object”的字符串。在每种具体类型的实例上调用此方法，以确保其正常工作。

当你从本身有共同祖先的类进行多重继承时，就形成了菱形继承。如果你的代码还能够运行，那么它是错误的一大来源。在图 7.8 中，我用双线展示了这个菱形。

## 7.14 迭代器

当 Python 遍历一个列表时，实际会发生什么？

```
for i in [2, 3, 5]:
    print(i)
```

2
3
5

Python 会跟踪你在列表中的位置，并在你每次请求时返回“下一个”项目。当你到达列表末尾时，循环终止。它的行为类似于这样：

```
the_list = [2, 3, 5]
the_index = 0

while True:
    if the_index < len(the_list):
        print(the_list[the_index])
        the_index += 1
    else:
        break
```

2
3
5

这种迭代看起来像是从列表开头到末尾的正常推进。“迭代”的含义由你决定：你可以定义它来做任何你想做的事情。

```
the_list = [2, 3, 5]
the_index = len(the_list) - 1

while True:
    if the_index >= 0:
        print(the_list[the_index])
        the_index -= 1
    else:
        break
```

5
3
2

在这种情况下，我们设置的迭代框架会从最后一个到第一个选择项目。“下一个”项目位于列表中我们最后访问的项目之前。

*迭代器*对象有一个由它的类定义的 `__next__` 魔术方法。第一次调用 `__next__` 时，它会返回“初始”对象。Python 代码会记住你的位置，随后对 `__next__` 的调用会逐个处理所有项目。

*可迭代对象*有一个 `__iter__` 魔术方法，它返回一个迭代器对象。一个对象可能同时是可迭代对象和迭代器。在这种情况下，`__iter__` 通常只返回 self。

可迭代对象通过 `__iter__` 返回一个迭代器。

### 练习 7.35
*range* 返回的对象是可迭代对象、迭代器，还是两者都是？

Python 在 for 循环中通过 *iter* 和 *next* 自动调用 `__iter__` 和 `__next__`。你也可以自己调用它们。

```
my_iter = iter([2, 3, 5])
print(next(my_iter))
```

2

```
print(next(my_iter))
```

3

```
print(next(my_iter))
```

5

如果你再调用一次，*next* 会引发 **StopIteration** 异常。

```
print(next(my_iter))
```

StopIteration

你应该在 `__next__` 代码中引发这个异常，以便 for 循环正确工作。在 try 块中查找并捕获 **StopIteration**。

如果你在迭代对象时修改了它，你可能会遇到错误，或者至少会得到意外结果。例如，你可能不希望在循环处理列表时向其中插入或删除项目。

### 练习 7.36

设 numbers 为字典：

```
numbers = {1: "one", 2: "two", 3: "three"}
```

解释以下每项中发生的事情：

```
next(iter(numbers))
```

1

```
next(iter(numbers.keys()))
```

1

```
next(iter(numbers.values()))
```

'one'

```
next(iter(numbers.items()))
```

(1, 'one')

哪些是迭代器，哪些是可迭代对象？

对于 UniPoly 实例，我们很自然地使用“字典”一词来表示迭代。

```
def __iter__(self):
    """Iterator returning exponent-coefficient tuple pairs.

    Notes
    -----
    The order of the pairs is not guaranteed to be sorted.

    Returns
    -------
    tuple:
        (exponent, coefficient) in polynomial.
    """

    return iter(self.__terms.items())
```

```
s = UniPoly(1, 's', 1)

for exp_coef in (s - 1)**4:
    print(exp_coef)
```

(4, 1)
(3, -4)
(2, 6)
(1, -4)
(0, 1)

我在 __iter__ 的文档中使用了一个 Notes 部分。

如果一个对象同时是可迭代对象和迭代器，那么该对象很可能将迭代状态存储在其自身内部。这就是为什么 __iter__ 可能返回 self 的原因。不过，正如我们所见，你可能希望以多种方式迭代一个对象。在这种情况下，你会像 Python 对字典所做的那样，创建单独的可迭代对象。

### 练习 7.37

在我们的 UniPoly 类中，我定义了一个属性 is_constant，它告诉我们一个多项式是否为常数。请记住，常数多项式没有自变量指数大于 0 的项。

```
@property
def is_constant(self):
    """True if the polynomial only contains a term of exponent 0."""

    if not self.__terms:
        return True
    if len(self.__terms) != 1:
        return False
    return next(iter(self.__terms)) == 0
```

最后一行做了什么？

