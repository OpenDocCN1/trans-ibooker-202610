
```
elif number == 42:
    return 'The answer. '
else:
    return 'Yes, that is a number. '
myLuckyNumber: int = 42
print(describeNumber(myLuckyNumber))
```

正如你所见，对于参数或变量，类型提示使用冒号将名称与类型分隔开，而对于返回值，类型提示使用箭头 (->) 将 def 语句的闭合括号与类型分隔开。describeNumber() 函数的类型提示显示它接受 number 参数的整数值并返回字符串值。

如果你使用类型提示，就不必将其应用于程序中的每一项数据。相反，你可以使用渐进式类型化方法，这是在动态类型灵活性和静态类型安全性之间的一种折中，即仅为某些变量、参数和返回值包含类型提示。但你的程序类型提示越多，静态代码分析工具就能获得越多的信息来发现程序中的潜在错误。

请注意，在前面的示例中，指定类型的名称与 int() 和 str() 构造函数函数的名称相匹配。在 Python 中，类、类型和数据类型具有相同的含义。对于由类创建的任何实例，你应使用类名作为类型：

```
import datetime
① noon: datetime.time = datetime.time(12, 0, 0)

class CatTail:
    def __init__(self, length: int, color: str) -> None:
        self.length = length
        self.color = color

② zophieTail: CatTail = CatTail(29, 'grey')
```

noon 变量具有类型提示 datetime.time ①，因为它是一个时间对象（在 datetime 模块中定义）。同样，zophieTail 对象具有 CatTail 类型提示 ②，因为它是使用 class 语句创建的 CatTail 类的对象。类型提示会自动应用于指定类型的所有子类。例如，具有类型提示 dict 的变量可以设置为任何字典值，也可以设置为任何 collections.OrderedDict 和 collections.defaultdict 值，因为这些类是 dict 的子类。第 16 章更详细地介绍了子类。

静态类型检查工具并不一定需要变量的类型提示。原因在于静态类型检查工具会进行类型推断，从变量的首次赋值语句推断出类型。例如，从 spam = 42 这一行，类型检查器可以推断 spam 应该具有 int 的类型提示。但我仍然建议设置类型提示。将来将其更改为浮点数，如 spam = 42.0，也会改变推断的类型，这可能不是你的本意。最好强制程序员在更改值时同时更改类型提示，以确认他们做的是有意更改而非偶然更改。

## 使用静态分析器

尽管 Python 支持类型提示的语法，但 Python 解释器完全忽略它们。如果你运行一个将类型无效的变量传递给函数的 Python 程序，Python 的行为就好像类型提示不存在一样。换句话说，类型提示不会导致 Python 解释器执行任何运行时类型检查。它们仅为静态类型检查工具服务，这些工具在程序运行之前分析代码，而不是在程序运行时分析。

我们称这些工具为静态分析工具，因为它们在程序运行之前分析源代码，而运行时分析或动态分析工具则分析正在运行的程序。（令人困惑的是，此处的静态和动态指的是程序是否正在运行，而静态类型和动态类型指的是我们如何声明变量和函数的数据类型。Python 是一种动态类型语言，但为其编写了静态分析工具，如 Mypy。）

## 安装和运行 Mypy

尽管 Python 没有官方的类型检查工具，但 Mypy 目前是最受欢迎的第三方类型检查器。你可以通过运行以下命令使用 pip 安装 Mypy：

```
python -m pip install --user mypy
```

在 macOS 和 Linux 上请改用 python3 而不是 python。其他知名类型检查器包括微软的 Pyright、Facebook 的 Pyre 和 Google 的 Pytype。

要运行类型检查器，请打开命令提示符或终端窗口并运行 python -m mypy 命令（以应用程序形式运行模块），并将要检查的 Python 代码的文件名作为参数传递。在此示例中，我正在检查我在一个名为 example.py 的文件中创建的示例程序的代码：

```
C:\Users\Al\Desktop>python -m mypy example.py
Incompatible types in assignment (expression has type "float", variable has type "int")
Found 1 error in 1 file (checked 1 source file)
```

如果没有问题，类型检查器不会输出任何内容；如果有问题则会打印错误信息。在此 example.py 文件中，第 171 行存在问题，因为名为 spam 的变量具有 int 类型提示，但被赋予了 float 值。这可能会导致失败，应进行调查。有些错误信息初读时可能难以理解。Mypy 可以报告大量可能的错误，太多无法在此列出。了解错误含义的最简单方法是在网上搜索。在这种情况下，你可以搜索类似“Mypy incompatible types in assignment”的内容。

每次更改代码时都从命令行运行 Mypy 效率相当低下。为了更好地使用类型检查器，你需要配置你的 IDE 或文本编辑器在后台运行它。这样，编辑器会在你输入代码时不断运行 Mypy，然后在编辑器中显示任何错误。图 11-1 显示了前一个示例中的错误在 Sublime Text 文本编辑器中的情况。

![](img/ab52fe24e511e29be73786864ebe0410_219_0.png)

图 11-1：Sublime Text 文本编辑器中显示来自 Mypy 的错误

配置你的 IDE 或文本编辑器与 Mypy 配合使用的步骤取决于你使用的是哪个 IDE 或文本编辑器。你可以通过搜索“<your IDE> Mypy configure”、“<your IDE> type hints setup”或类似内容在线找到说明。如果一切都失败了，你始终可以从命令提示符或终端窗口运行 Mypy。

## 告诉 Mypy 忽略代码

你可能会编写出于某种原因不想收到类型提示警告的代码。对静态分析工具而言，该行可能看起来使用了错误的类型，但在程序运行时实际上是正常的。你可以通过在行尾添加 # type: ignore 注释来抑制任何类型提示警告。以下是示例：

```python
def removeThreesAndFives(number: int) -> int:
    number = str(number)  # type: ignore
    number = number.replace('3', '').replace('5', '')  # type: ignore
    return int(number)
```

为了从传递给 removeThreesAndFives() 的整数中删除所有 3 和 5 位数字，我们暂时将整数 number 变量设置为字符串。这会导致类型检查器对函数中的前两行发出警告，因此我们在这两行添加 # type: ignore 类型提示以抑制类型检查器的警告。

请谨慎使用 # type: ignore。忽略类型检查器的警告会为错误潜入代码提供机会。你几乎肯定可以重写代码以避免出现警告。例如，如果我们使用 numberAsStr = str(number) 创建一个新变量，或用单行 return int(str(number.replace('3', '').replace('5', ''))) 代码替换所有三行，我们就可以避免对 number 变量重复使用多种类型。我们不想通过将参数的类型提示更改为 Union[int, str] 来抑制警告，因为参数旨在仅允许整数。

## 为多种类型设置类型提示

Python 的变量、参数和返回值可以有多种数据类型。为了适应这一点，你可以通过从内置 typing 模块导入 Union 来指定具有多种类型的类型提示。在 Union 类名后方用方括号指定一系列类型：

```
from typing import Union
spam: Union[int, str, float] = 42
spam = 'hello'
spam = 3.14
```

在此示例中，Union[int, str, float] 类型提示指定你可以将 spam 设置为整数、字符串或浮点数。请注意，最好使用 from typing import X 形式的导入语句，而不是 import typing 形式，然后在整个程序中一致地使用冗长的 typing.X 进行类型提示。

你可能在变量或返回值除了另一种类型外还可能具有 None 值的情况下指定多种数据类型。要在类型提示中包含 NoneType（即 None 值的类型），请将 None 放在方括号内，而不是 NoneType。（从技术上讲，NoneType 不是像 int 或 str 那样的内置标识符。）

更好的做法是，不要使用例如 Union[str, None]，而是可以从 typing 模块导入 Optional 并使用 Optional[str]。此类型提示意味着函数或方法可能返回 None 而不是预期类型的值。以下是示例：

```
from typing import Optional
lastName: Optional[str] = None
lastName = 'Sweigart'
```

在此示例中，你可以将 lastName 变量设置为 None 或 str 值。但最好谨慎使用 Union 和 Optional。你的变量和函数允许的类型越少，代码就越简单，而简单代码比复杂代码更不容易出错。记住 Python 之禅的格言：简单胜于复杂。对于返回 None 以指示错误的函数，请考虑改为引发异常。参见第 178 页的“引发异常 vs. 返回错误代码”。

你可以使用 Any 类型提示（同样来自 typing 模块）来指定变量、参数或返回值可以是任何数据类型：

```
from typing import Any
import datetime
spam: Any = 42
spam = datetime.date.today()
spam = True
```
