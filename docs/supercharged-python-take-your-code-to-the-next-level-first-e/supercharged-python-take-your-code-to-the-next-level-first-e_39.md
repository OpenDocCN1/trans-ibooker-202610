## E. 语句参考

本附录涵盖了 Python 语言中的一些基本语法问题，但运算符、重要方法和内置函数除外，这些内容将在其他地方介绍。此处涵盖的内容包括

- 变量和赋值
- Python 中的间距问题
- 语句按字母顺序参考

## 变量和赋值

Python 中没有数据声明，即使对于多维列表等非简单类型也是如此。类和函数是在运行时定义的，但除此之外，列表、集合和字典等对象必须被 _构建_ 而不是声明。

变量是通过赋值创建的。它们也可以通过 `for` 循环创建。在函数定义中，参数会创建具有局部作用域的变量。但赋值仍然是创建变量的基本工具。

最简单的赋值形式是

```
variable_name = expression
```

你也可以通过多重赋值创建任意数量的变量。

> Click here to view code image

```
var1 = var2 = var2 = ... varN = expression
```

以下是每种形式的示例。

> Click here to view code image

```
a = 5.5
b = 5.5 * 100
x = y = z = 0    # Assign x, y, z the same val
var1 = var2 = 1000 / 3   # Assign var1, var2 the same val
```

最后，支持元组赋值，其中变量的数量和表达式的数量必须匹配，或者两边之一必须是表示元组的变量。

```
x, y, z = 10, 20, 1000 / 3
```

无论如何，变量都是作为指向值的名称创建的。更准确地说，它成为全局或局部级别符号表中的一个条目。每个作用域级别都将其自身的符号表作为字典维护。考虑以下程序。

```
def main():
    a, b = 100, 200
    print(locals())
main()
```

结果是打印函数局部作用域的数据字典。

```
{'b': 200, 'a': 100}
```

现在，每当在该函数的表达式中引用变量 `a` 或 `b` 时，相应的值（可以是任何类型的对象）会在表中查找，然后 Python 使用与变量名关联的值。如果找不到名称，Python 则查看全局符号表。最后，它查看内置列表。如果名称在这些地方都找不到，则会引发 **NameError** 异常。

因此，Python 变量本质上是名称。它们可以随时被重新赋予新值（即对象）；它们甚至可以在不同时间被赋予不同类型的对象，尽管这大多不被鼓励——多态参数和鸭子类型的情况除外。（“鸭子类型”在《Python Without Fear》等书中有讨论。）

因此，变量不像 C 和 C++ 等其他编程语言中的变量那样占据内存中的固定位置。变量本身没有属性，只有它所引用的对象的属性。

变量像对象的引用一样工作。将新对象赋给现有变量会替换其在数据字典中的条目，取消旧的关联。反例是对列表使用赋值运算符，例如 `+=`，它作为原地修改工作。有关更多信息，请参见第 4.2.3 节，“了解组合运算符赋值（+= 等）”。

有效的符号名称以下划线 (_) 或字母开头。此后，每个字符必须是字母、下划线或数字字符。

## Python 中的间距问题

间距和缩进在 Python 中非常重要。每当一行代码相对于前一行代码缩进时，它就表示另一层嵌套。

顶级代码行必须左对齐，从第 1 列开始。

一行代码的缩进必须与属于同一块的行保持一致。这意味着，例如，以下是有效的：

Click here to view code image

```
a, b = 1, 100
if a < b:
    print('a is less than b')
    print('a < b')
else:
    print('a not less than b')
```

然而，以下是无效的。

Click here to view code image

```
a, b = 1, 100
if a < b:
    print('a < b')
else:    # ERROR!
print('a not less than b')
```

在这种情况下，问题在于 **else** 的位置。它与前面的相应 **if** 语句不对齐，要有效就必须对齐。

你可以这样做，尽管不推荐。

```
a, b = 1, 100
if a < b:
    print('a < b')
else:
    print('a not less than b')
```

在这种情况下，使用了不一致的缩进，但逻辑关系很清晰。但是，强烈建议一旦你选择了缩进的空格数，就始终在整个程序中使用它。PEP-8 标准推荐每级使用四个空格。

> **Note**
> 要小心制表符，Python 不认为制表符等同于任何数量的空格。如果可能，请让你的文本编辑器插入空格而不是制表符 (\t)。

作为一般规则，语句由行的物理结束终止。有几个例外。你可以使用分号 (;) 在同一物理行上放置多个语句。这些是语句分隔符而不是终止符。

Click here to view code image

```
a = 1; b = 100; a += b; print(a)
```

你还可以将整个 **for** 或 **while** 循环放在同一行上，如第 4.2.17 节“使用单行 ‘for’ 循环”中所述。

## 语句按字母顺序参考

本附录的其余部分描述了 Python 语言支持的语句。这不涵盖内置函数，这些内容将在附录 B 中介绍。

### 表 E.1. 最常用的 Python 语句。

| 要执行以下任务： | 使用这些关键字： |
|---|---|
| 跳出（退出）循环 | **break** |
| 捕获并处理异常 | **try**, **except** |
| 继续循环的下一个周期 | **continue** |
| 定义类 | **class** |
| 定义函数 | **def** |
| 全局变量，创建或操作 | **global** |
| 导入包 | `import` |
| 如果假设被违反则打印错误消息 | `assert` |
| 提供 “for” 循环 | `for` |
| 提供 “while” 循环 | `while` |
| 提供 if/else 结构 | `if`, `elif`, `else` |
| 从函数返回值，或提前退出 | `return` |
| 生成值（创建生成器） | `yield` |

### assert 语句

此语句作为调试工具很有帮助。它具有以下语法：

Click here to view code image

```
assert expression, error_msg_str
```

Python 通过求值 `expression` 来响应。如果为真，则什么都不发生。如果表达式为假，则打印 `error_msg_str` 并终止程序。目的是捕获已被违反的重要假设。例如：

Click here to view code image

```
def set_list_vals(list_arg):
    assert isinstance(list_arg, list), 'arg must be a list'
    assert len(list_arg) >= 3, 'list argument too short'
    list_arg[0] = 100
    list_arg[1] = 200
    list_arg[2] = 150
```

当 **assert** 条件失败时，Python 打印错误消息并标识断言失败的模块和行号。

当你打开优化（命令行中的 -O）时，Python 会关闭 **assert** 语句，因为此语句仅用于调试目的。

### break 语句

此语句具有简单的语法。

```
break
```

**break** 语句的效果是退出最近的封闭 **for** 或 **while** 循环，将控制权转移到循环后的第一条语句（如果有）。例如：

Click here to view code image

```
total = 0.0
while True:
    s = input('Enter number: ')
    if not s:     # Break on empty-string input.
        break
    total += float(s)  # Only executed on non-empty s.
print(total)
```

在这种情况下，涉及 **break** 语句的条件的效果是在输入空字符串后退出循环。

在循环外使用 **break** 会导致语法错误。

### class 语句

此语句在运行时创建或“编译”类定义。定义必须在语法上正确，但 Python 不需要在类用于实例化对象之前解析定义中的所有符号。（因此，类可以互相引用，如果两者都在定义之前都不实例化。）

此关键字具有以下语法，其中方括号表示可选项：`base_classes`，如果有多个类，则是由逗号分隔的零个或多个类的列表。

Click here to view code image

```
class class_name [(base_classes)]:
    statements
```

这些语句由一个或多个语句组成；这些是




通常是变量赋值和函数定义。一个 **pass** 语句，即空操作，也可用作稍后添加的语句的占位符。

```python
class Dog:
    pass
```

在类定义中创建的变量成为类变量。类定义中的函数成为方法。常见的此类方法是 `__init__`。与其他方法一样，如果要通过实例调用它，定义必须以额外参数 **self** 开头，该参数指实例本身。例如：

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

定义类后，可以使用它来实例化对象。对象创建时给出的参数会传递给 `__init__` 方法。请注意，此方法提供了一种为类的对象创建一组统一实例变量的方式。（由于此方法，所有 Point 对象都将具有 x 和 y 元素。）

Click here to view code image

```python
my_pt = Point(10, 20)  # my_pt.x = 10, my_pt.y = 20
```

类定义内的函数定义可以使用 @classmethod 和 @staticmethod 进行装饰，分别创建类方法和静态方法。

类方法可以访问类中定义的符号，并以额外参数开头，默认情况下为 cls，指类本身。

静态方法定义在类中，但无法访问类或实例变量。

例如，以下代码定义了一个类方法 set_xy 和一个静态方法 bar。两者都是类 foo 的方法，并且都通过类名调用。它们也可以通过 foo 的任何实例调用。

```python
>>> class foo:
... x = y = 0 # class vars
...
... @classmethod
... def set_xy(cls, n, m):
... cls.x = n
... cls.y = m
...
... @staticmethod
... def bar():
... return 100
...
>>> foo.set_xy(10, 20)
>>> foo.x, foo.y
(10, 20)
>>> foo.bar()
100
```

有关更多信息，请参见第 9 章，“类和魔术方法”。

## continue 语句

此语句语法简单。

```python
continue
```

**continue** 的效果是将执行转移到封闭的 **for** 或 **while** 循环顶部，并进入下一次迭代。如果在 **for** 循环中执行 **continue**，则循环变量的值会推进到可迭代序列中的下一个值，除非该序列已耗尽，在这种情况下循环终止。

例如，以下示例打印“Python”中的每个字母，但不包括大写或小写的“d”。

Click here to view code image

```python
for let in 'You moved Dover!':
    if let == 'D' or let == 'd':
        continue
    print(let, end='')
```

此代码的效果是打印：

```
You move over!
```

在循环外使用 **continue** 会导致语法错误。

## def 语句

此语句在运行时创建或“编译”函数定义。定义必须在语法上正确，但 Python 不需要在函数被调用之前解析定义中的所有符号。（只要两个函数在任一被调用之前都已定义，这就能实现互相自引用。）

```python
def function_name(args):
    statements
```

在此语法中，args 是零个或多个参数的列表，如果有多个参数，则用逗号分隔：

```
[arg1 [,arg2]...]
```

例如：

Click here to view code image

```python
def hypotenuse(side1, side2):
    total = side1 * side1 + side2 * side2
    return total ** 0.5   # Return square root of total.
```

定义函数后，可以随时执行（调用）它，但在函数调用中始终需要括号，无论是否有参数。

Click here to view code image

```python
def floopy():
    return 100

print(floopy())       # Call floopy, no args! Print 100
print(hypotenuse(3, 5))
```

函数还有其他功能，将在第 1 章“基础回顾”中解释。此外，函数可以嵌套，这在装饰器中特别有用，如第 4.9 节“装饰器和函数分析器”中所述。

## del 语句

此语句从当前上下文中删除一个或多个符号。它具有以下语法：

```python
del sym1 [, sym2]...
```

其效果是删除指定的符号或符号，但不一定会销毁任何其他对象，只要对其还有引用即可。例如：

Click here to view code image

```python
a_list = [1, 2, 3]
b_list = a_list  # Create alias for the list
del a_list      # Remove a_list from symbol table
print(b_list)   # List referred to still exists.
```

## elif 子句

`elif` 关键字不是单独的语句，而是用作 `if` 语句语法的一部分。有关更多信息，请参见 `if` 语句。

## else 子句

`else` 关键字不是单独的语句，而是用作 `if`、`for`、`while` 或 `try` 语句语法的一部分。

## except 子句

`except` 子句不是单独的语句，而是用于 `try` 语句。有关更多信息，请参见 `try`。

## for 语句

此语句的语法如下所示。它本质上是一个“for each”循环。如果希望它像传统的 FORTRAN “for” 那样工作（只有在必要时或你非常固执时才应这样做），则需要使用 `range` 内置函数。括号并非字面意思，而是表示可选项。

Click here to view code image

```python
for loop_var in iterable:
    statements
[else:
    statements] # Executed if first block of statements finished successfully, without break
```

一个效果是创建 `loop_var` 作为变量，引用 `iterable`（它是集合或生成的序列）生成的第一个项目。该变量在当前作用域级别继续存在。循环完成后，该变量应引用 `iterable` 中的最后一个项目，假设没有提前退出。

`for` 循环反复执行 `statements`，就像 **while** 循环一样；但它还会在每次迭代（周期）开始时将 `loop_var` 设置为 `iterable` 生成的下一个值。当迭代耗尽时，循环终止。以下是一些示例。

Click here to view code image

```python
# Print members of the Beatles on separate lines

beat_list = ['John', 'Paul', 'George', 'Ringo']

for guy in beat_list:
    print(guy)

# Define function to calculate 1 * 2 * ... n

def factorial(n):
    prod = 1
    for n in range(1, n + 1):
        prod *= n
    return (prod)
```

有关将 **else** 子句与 **for** 一起使用的示例，请参见第 4.2.9 节，“循环和‘else’关键字”。

## global 语句

此语句具有以下语法，涉及一个或多个变量名。

```python
global var1 [, var2]...
```

**global** 语句的效果是：“不要将此变量或这些变量视为当前函数作用域内的局部变量。” 但是，它不会创建全局变量；需要单独的语句来完成此操作。

有时此语句是必要的，因为否则，从函数内部对全局变量进行赋值会被解释为创建局部变量。如果未对全局变量进行赋值，则没有问题。但是，如果从函数内部对全局变量进行赋值，代码会创建一个局部变量。这就是我们所说的“局部变量陷阱”。

Click here to view code image

```python
account = 1000
def clear_account():
    account = 0  # Oops, create new var, as a local

clear_account()
print(account)  # Prints 1000, this is an error!
```

这个简单程序本应创建一个变量，将其重置为 0，然后打印 0。但它未能做到，因为语句 account=0 出现在函数内部。执行时，函数将 account 创建为局部变量，因此与全局 account 副本不相连。

解决方案是使用 **global** 语句，这会导致函数不将 account 视为局部变量；因此它强制 Python 引用全局版本。

Click here to view code image

```python
account = 1000
```






def clear_account():
    global account  # 不要将 account 设为局部变量。
    account = 0     # 将全局副本重置为 0！

clear_account()
print(account)      # 打印 0，而不是 1000。

## if 语句

该语句有简单版本和更完整的版本，此处（虽然有些冗余）包含两者，以便于理解。最简单的版本是

```
if condition:
    statements
```

条件可以是任何 Python 对象——或求值为对象的表达式——或者是一系列链式比较，如下所示。

Click here to view code image

```
age = int(input('Enter your age: '))
if 12 < age < 20:
    print('You are a teenager.')
```

在此上下文中，所有 Python 对象都会转换为 **True** 或 **False**。statements 是一个或多个 Python 语句。
下面是完整语法。在此处，方括号表示可选项。你可以有任意数量的 **elif** 子句。

```
if condition:
    statements
[elif condition:
    statements ]...
[else:
    statements]
```

下面是一个示例。此示例仅包含一个 `elif` 子句，
但你可以拥有任意数量的它们。

Click here to view code image

```
age = int(input('Enter age: '))
if age < 13:
    print('Hello, spring chicken!')
elif age < 20:
    print('You are a teenager.')
    print('Do not trust x, if x > 30.')
else:
    print('My, my. ')
    print('We are not getting any younger are we?')
```

## import 语句

`import` 语句会暂停当前模块的执行，并在指定的包或模块尚未执行时执行它。这是必要的，因为在 Python 中，
函数和类定义是在运行时动态（“编译”）执行的。

另一个效果是，根据所使用的版本，使指定模块或包中的符号对当前模块可访问。

Click here to view code image

```
import module
import module as short_name
from module import sym1 [, sym2]...
from module import *
```

前两个版本使符号可访问，但仅限于限定名称，如 `math.pi` 或 `math.e`。第三个版本使符号无需限定即可访问，但仅限于列出的符号。第四个版本使模块中的所有符号无需限定即可使用。

此最后一个版本最为方便，但如果指定的模块或包定义了许多符号，则存在命名冲突的风险。对于大型模块和具有大型命名空间的包，建议使用 `import` 语句的其他版本。

有关更多信息，请参见第 14 章，“多个模块和 RPN 示例”。

## nonlocal 语句

该语句的语法与 `global` 语句的语法相似。

```
nonlocal var1 [, var2]...
```

`nonlocal` 语句的目的与 `global` 语句的目的相似，只有一个区别：`nonlocal` 用于拒绝局部作用域，并优先使用封闭但非全局的作用域。这种情况仅发生在函数定义嵌套在另一个函数定义内部时。因此，`nonlocal` 的使用很少。
有关更多信息，请参见 `global` 语句。

## pass 语句

该语句具有简单的语法：

```
pass
```

该语句本质上是一个空操作。它在运行时不执行任何操作，其主要用途是在类或函数定义中作为占位符或替代符——也就是说，它保留一个位置，以便稍后用其他语句填充。

Click here to view code image

```
class Dog:
    pass      # 此类目前没有方法。
```

## raise 语句

该语句具有以下语法，其中方括号并非字面意思，而是表示可选项。

Click here to view code image

```
raise [exception_class [(args)]]
```

其效果是引发指定的异常，并带有可选参数。一旦引发异常，必须由程序处理，否则会导致粗鲁且突然的终止。异常处理程序可以通过使用

**raise** 来重新抛出异常。不带 `exception_class` 使用该语句会重新抛出异常而不更改它，实际上是在说，“我决定不再处理这个了”并将其传递下去。然后 Python 必须查找另一个处理程序。

```
raise
```

有关更多信息，请参见 **try** 语句。

## return 语句

该语句有一个可选部分。此处的方括号并非字面意思。

```
return [return_val]
```

其操作是退出当前函数并将值返回给函数的调用者。如果省略 `return_val`，则默认返回值为 **None**。可以通过返回元组来返回多个值。

Click here to view code image

```
return a, b, c # 退出并返回三个值。
```

如果 **return** 在所有函数之外使用，结果是语法错误。有关 **return** 在上下文中的示例，请参见 **def** 语句。

## try 语句

该语句具有相当复杂的语法，因此我们将其分解为两个主要部分：首先是整体语法。方括号表示可选项。

```
try:
    statements
[except exception_specifier:
    statements]...
[else:
    statements]
[finally:
    statements]
```

第一个 `statements` 块会直接执行。但是，如果在执行该块期间引发异常——即使是在由这些语句中的一个直接或间接调用的函数期间——Python 会检查异常处理程序，如本节后面所述。可能有任意数量的异常处理程序。

可选 `else` 子句中的语句会在第一个语句块在未被异常中断的情况下完成时执行。可选 `finally` 子句中的语句会在该语法中的其他语句之后无条件执行。

每个 `except` 子句具有以下语法：

```
except [exception [as e]]:
    statements
```

如果省略 `exception`，则该子句处理所有异常。如果指定了 `exception`，当异常与 `exception` 具有相同的类或派生类时，Python 认为它匹配——或者如果它是一个其类匹配的对象。可选符号是一个提供信息的参数。Python 按给定顺序逐个检查每个异常处理程序。

**Exception** 类是所有错误类的基类，但它不会捕获所有异常，例如 **StopIteration**。

处理异常意味着执行关联的语句，然后将执行转移到 **finally** 子句（如果存在），否则转移到整个 **try/except** 块的末尾。

Click here to view code image

```
>>> def div_me(x, y):
    try:
        quot = x / y
    except ZeroDivisionError as e:
        print("Bad division! Text:", e)
    else:
        print("Quotient is %s." % quot)
    finally:
        print("Execution complete!")
>>> div_me(2, 1)
Quotient is 2.0.
Execution complete!
>>> div_me(2, 0)
Bad division! Text: division by zero
Execution complete!
>>> div_me("2", "3")
Execution complete!
Traceback (most recent call last):
  File "<pyshell#21>", line 1, in <module>
    div_me("2", "3")
  File "<pyshell#19>", line 3, in div_me
    quot = x / y
TypeError: unsupported operand type(s) for /: 'str'
```

and 'str'

在这些示例中，最后一个示例显示如果发生未捕获的异常会怎样。程序会突然终止并打印堆栈跟踪，但无论发生什么，`finally` 子句都会执行，即使在这种情况下也是如此。

下一个示例展示了 `try/except` 结构如何可以有多个异常处理程序；理论上，你可以拥有任意数量。如果一个块没有捕获异常，另一个块可以捕获。较窄的异常类别应首先处理。在以下示例中，任何未被第一个处理程序捕获的错误都会被第二个处理程序捕获。

请记住，`as e` 部分始终是可选的，但对于打印错误文本很有帮助。

Click here to view code image

```
try:
    f = open('silly.txt', 'r')
    text = f.read()
    print(text)
except IOError as e:
    print('Problem opening file:', e)
except Exception as e:
    print('That was a Bozo no-no.')
    print('Error text:', e)
```

## while 语句

该语句是一个简单的循环，只有一个版本。没有“do-while”。方括号并非字面意思，而是表示可选项。





## while 语句

```python
while condition:
    statements
[else:
    statements] # Executed if first block of
statements      # finished successfully, without
break
```

在循环顶部，`condition` 会被求值：任何“真”值都会导致执行 `statements`；然后控制权转移回顶部，再次对 `condition` 求值。“假”值会导致退出循环。

（非布尔条件的真/假值是通过应用 `bool` 转换并得到 `True` 或 `False` 来确定的。所有 Python 对象都支持这种转换。Python 对象的默认行为是转换为 `True`。一般来说，对象都是“真”的，除了零值、`None` 和空集合。）

因此，例如，下面的示例从 10 倒数到 1，每次都会打印该数字。（发射！）

```python
n = 10
while n > 0:
    print(n, end=' ')
    n -= 1  # Decrement n by 1
```

由于求值条件的规则，你也可以按如下方式编写代码，尽管它稍微不可靠一些，因为如果 `n` 初始化为负数，结果将是一个无限循环。

```python
n = 10
while n:
    print(n, end=' ')
    n -= 1  # Decrement n by 1
```

请参阅 **continue** 和 **break**，了解控制循环执行的方法。

## with 语句

该语句具有以下语法，其中可选部分显示在方括号中。

```
with expression [as var_name]
    statements
```

其效果是求值表达式并生成一个对象。Python 执行该对象的 `__enter__` 方法。如果该对象的类未定义该方法，Python 会生成一个 `AttributeError`。假设未引发此异常，然后执行这些语句。

最后，执行该对象的 `__exit__` 方法。如果执行提前终止——例如，由于异常——该方法仍然会被执行。

with 语句的常见用途是打开和关闭文件。文件对象的 `__exit__` 方法在执行完该块后会自动关闭文件。

单击此处查看代码图像

```
with open('stuff.txt', 'r') as f:
    print(f.read())  # Print contents of a file.
```

## yield 语句

该语句的语法与 **return** 语句类似，但其效果完全不同。

```
yield [yielded_val]
```

`yielded_val` 的默认设置是 **None**。在函数外使用 **yield** 是语法错误。

**yield** 的效果是将当前函数转换为 _生成器工厂_ 而不是普通函数。生成器工厂的实际返回值是一个 _生成器对象_，然后按工厂定义的方式产生值。

诚然，这是 Python 中最令人困惑的部分之一，因为在创建生成器对象之前，实际上不会产生任何值。（谁说所有语言设计师都逻辑严密？）事实上，这可能是 Python 中最反直觉的功能之一。

有关澄清信息，请参见第 4.10 节，“生成器。”
