

## 9.4 类与前向引用问题

正如 Python 中的函数存在前向引用问题，Python 类也存在同样的问题。问题是，类必须在被实例化之前定义。_实例化_一个类意味着使用它来创建一个对象。

以下是一种在类前向引用中会出现问题的情况。

Click here to view code image

```
class Marriage:
    def __init__(self):
        self.wife = Person('f')
        self.husband = Person('m')
a_marriage = Marriage() # Instantiate the class.

class Person:
    def __init__(self, gender):
        self.gender = gender
```

这个愚蠢的程序会失败，但并非因为它愚蠢。你应该能看出问题所在：前几行代码被执行，导致类 Marriage 被定义，因此作为一个类存在；但第六行随后通过尝试创建一个名为 `a_marriage` 的实际对象来_实例化_该类。

这本身不应该是问题。但是，当对象被创建并调用其 `__init__` 方法时，该方法试图创建名为 `wife` 和 `husband` 的几个对象，即 Person 类的对象。问题就在这里。Person 类尚未定义，无法用于创建新对象。

解决方案很明显：只需将第六行移动到文件末尾。这样，两个类都在被实例化之前定义。

```
`a_marriage = Marriage()`
```

一般来说，如果遵循几条规则，对类的前向引用就不是问题。

- 确保所有类在被实例化之前都已定义。这是主要规则。
- 对于相互实例化的类，或者（天不容）一个类创建自身实例的类，要格外小心。虽然有一种技巧可以让你做到这一点，但这确实是你不应该涉足的领域。愚者总是敢于涉足智者所畏惧之地。
- 但是，包含其他类（单向）的类，或者包含对类实例引用的类，通常不是问题。不过要警惕相互依赖。

## 9.5 方法一般情况

_方法_与普通函数有几点不同。首先，方法是在类定义内定义的；这正是使它们成为方法的原因。

```
my_obj.a_method(12)
```

其次，每次通过实例调用方法时，都会传递一个隐藏参数：对调用它的对象的引用（_self_）。
这里显示的语法对比了方法定义（必须在类定义内发生）与对该同一方法的调用。

![](img/6e45221a8147f2442c653a3d4a866570_540_0.png)

Click here to view code image

```
class class_name:
    def method_name(self, arg1, arg2, arg3...):
        statements

obj_name = class_name()
obj_name.method_name(arg1, arg2, arg3...)
```

请注意，方法的定义——但不是对该方法的_调用_——包含隐藏的第一个参数 _self_，因此定义的参数比函数调用多一个。例如，考虑以下类定义。

```
class Pretty:
    def __init__(self, prefix):
        self.prefix = prefix
    def print_me(self, a, b, c):
        print(self.prefix, a, sep='')
        print(self.prefix, b, sep='')
        print(self.prefix, c, sep='')
```

编写完这个类后，我们可以通过创建一个实例并使用它来测试它。请注意，对方法的调用始终比相应的定义少一个参数，因为定义显式包含 **self**，而对方法的调用则不包含。

```
printer = Pretty('-->')
printer.print_me(10, 20, 30)
```

这将打印

```
-->10
-->20
-->30
```

还要注意，在方法内部，实例本身始终标识为 **self**，实例变量标识为 **self.name**。

## 9.6 公共和私有变量与方法

面向对象编程的传统目标之一是 _封装_，它使类的内部内容对外部世界隐藏。

Python 的哲学与这一观点背道而驰。作为一种脚本语言，Python 倾向于暴露一切，让你尝试任何事情。与其他语言相比，安全性和类型检查较少。

但是，Python 有一个有用的约定。以下划线 (_) 开头的变量和方法名旨在为私有。此外，以双下划线 (__) 开头的名称通过一种称为 _名称改写_ 的过程变得不可访问，假设它们不是“魔术方法”，如本章后面所述。

一个简单的示例可以演示此工作原理。请注意，在以下类定义中，变量 x 和 y 无法从外部访问。但是，在类本身内部，所有三个变量（包括 __z）都是可访问的。

Click here to view code image

```
class Odd:
    def __init__(self):
        self.x = 10
        self.y = 20
        self.__z = 30

def pr(self):
    print('__z = ', self.__z)
```

给定这个类定义，以下语句完全有效，并且完全符合你的预期。

```
o = Odd()
o.x    # 10
o.y    # 20
```

但是，以下表达式会引发异常：

```
o.__z   # Error!
```

最后这个表达式引发错误，因为 Python 将 __z 替换为一个改写名称，该名称由类名和变量名的组合生成。

但是，__z 在同一类的方法定义内仍然可以访问，无需改写，这就是为什么 pr 方法仍然有效的原因。变量和方法名在同一类内始终可访问。但是请记住，在 Python 中，此类类内引用需要用 self 限定。

## 9.7 继承

Python 提供对继承的支持，也称为“子类化”。假设你有一个类 Mammal，它包含你在程序中需要使用的大多数方法。但是，你需要添加或更改其中的几个方法。例如，你可能希望创建一个 Dog 类，其实例可以做 Mammal 实例能做的任何事情，再加上更多事情。

首先显示带有一个基类的单继承语法。

![](img/6e45221a8147f2442c653a3d4a866570_544_0.png)

> Click here to view code image

```
class class_name(base_class):
    statements
```

其效果是创建一个新类 class_name，它继承属于 base_class 的所有类变量和方法。语句可以添加新变量和方法定义，以及覆盖现有定义。

Python 中的每个变量和方法名都是多态的。名称直到运行时才被解析。因此，你可以调用任何对象的任何方法，并且它将被正确解析。

例如，以下类层次结构涉及一个基类 Mammal 和两个子类 Dog 和 Cat。子类从 Mammal 继承 __init__ 和 call_out 方法，但每个子类都实现了 speak 方法的自己的版本。

> Click here to view code image

```
class Mammal:
    def __init__(self, name, size):
        self.name = name
        self.size = size

def speak(self):
    print('My name is', name)

def call_out(self):
    self.speak()
    self.speak()
    self.speak()

class Dog(Mammal):
    def speak(self):
        print('ARF!!')

class Cat(Mammal):
    def speak(self):
        print('Purrrrrrr!!!!')
```

这些定义使以下语句成为可能：

> Click here to view code image

```
my_cat = Cat('Precious', 17.5)
my_cat.call_out()
```

最后一个语句打印

```
Purrrrrrr!!!!
Purrrrrrr!!!!
Purrrrrrr!!!!
```

Dog 和 Cat 类继承 __init__ 方法，与 C++ 语言中的构造函数不同。但这引发了一个问题：如果子类需要进行额外初始化，同时我们又希望尽可能利用基类的 __init__ 定义，该怎么办？解决方案是为子类编写一个新的 __init__ 方法定义，但调用基类的

