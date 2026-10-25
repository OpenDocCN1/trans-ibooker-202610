

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

