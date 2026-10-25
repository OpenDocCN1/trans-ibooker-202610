


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

