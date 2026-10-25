

initialize these other attributes.

Click here to view code image

```
def __init__(self, v, units='USD'):
    self.units = units
```

Even with these definitions in place, it’s still necessary to print both the object itself (which inherits directly from `Decimal`) and the `units`, which is the attribute added by Money.

```
print(m, m.units)
```

But we can improve this situation by overriding the `__str__` method, to print a Money object in a more natural and direct way. Notice that the superclass version of this method is called to do much of the work.

Click here to view code image

```
def __str__(self):
    return super().__str__() + ' ' + self.units
```

This is a typical example of how you’d override the `__str__` method, regardless of whether you’re subclassing an immutable class or a mutable class.

> **Note**
> It may seem unreasonable that Python doesn’t let you use the easy approach to subclassing another type, as shown earlier with the hypothetical superclass Thingie.

There are a number of reasons that's not feasible in Python. For one thing, if a superclass is immutable, that means its data can never be changed after it's created. Also, some built-in classes make use of the `__new__` function to initialize values, in addition to other actions, so that calling upon the superclass's `__init__` function is inadequate. The basic rule is this: If subclassing a built-in type the ordinary way doesn't work, you might need to subclass `__new__`.

## 10.13 分数类

Decimal 和 Money 类可以以绝对精度存储十进制数字，例如 0.53。但这些类也有其局限性。

如果你想存储 1/3 的值，该值无法在二进制基数下表示而不产生舍入误差。但在十进制基数下表示这个数量同样不可能！从数学上讲，你需要无限多的数字来在任一基数中存储 1/3（三分之一）。

0.33333333333333333333333333333333333333333...

幸运的是，整数来拯救了我们。整数以绝对精度存储数字，通过创建具有两部分的对象——分子（上半部分）和分母（下半部分）——我们可以表示任何可以表示为两个整数之比的数字（参见图 10.2）。

![](img/6e45221a8147f2442c653a3d4a866570_642_0.png)

### 分数类

图 10.2. 分数类的结构

会出现一些问题，但这些问题都由该类平滑处理。例如，1/2、2/4 和 100/200 在数学上都是等价的。但得益于内部方法，这些都会自动减少到相同的内部表示。下面是一个示例。首先，我们需要导入该类。

Click here to view code image

```
from fractions import Fraction
```

请务必按所示输入此语句。单词 fractions 是小写复数；单词 Fraction 是大写单数！为什么不一致，我们不确定。无论如何，导入该类后，可以以一致且非常方便的方式处理 Fraction 对象。让我们再看看处理 1/2、2/4 和 100/200 的问题。

Click here to view code image

```
fr1 = Fraction(1, 2)
fr2 = Fraction(2, 4)
fr3 = Fraction(100/200)
print('The fractions are %s, %s, & %s.' % (fr1, fr2, fr3))
```

此示例打印

```
The fractions are 1/2, 1/2, & 1/2.
```

所有这些 **Fraction** 对象都显示为相同的数量，因为它们会自动减少到最简形式。

```
>>> if fr1 == fr2 and fr2 == fr3:
    print('They are all equal!')
They are all equal!
```

> Note
> 通过使用第 4 章中指出的快捷方式之一，你可以将此示例中的条件替换为链式比较，从而产生更短的版本。
> >>> if fr1 == fr2 == fr3:
>     print('They are all equal!')

分数可以以其他方式指定。例如，如果初始化时只给出一个整数，该类将其存储为该整数除以 1（当然，这是一个比率）。下面是一个示例：

```
>>> fr1 = Fraction(5)
>>> print(fr1)
5
```

分数也可以从 **Decimal** 对象和浮点值转换。有时这工作良好，如下所示。

```
>>> fr1 = Fraction(0.5)
>>> print(fr1)
1/2
```

但有时则不然。

```
>>> fr2 = Fraction(0.01)
>>> print(fr2)
5764607523034235/576460752303423488
```

哇，这里发生了什么？答案是，我们的老对头，浮点舍入误差，再次抬头了。**Fraction** 类尽力适应浮点值 0.01 中的那个微小舍入误差，因此得出了这个丑陋的比率。

有几种解决方案。一种是直接从字符串初始化，就像我们对 **Decimal** 对象所做的那样。

```
>>> fr2 = Fraction('0.01')
>>> print(fr2)
1/100
```

这就好多了！另一种选择是使用 `limit_denominator` 方法。该方法表示分母只能变得这么大。考虑到这一限制，`Fraction` 类会生成尽可能接近的近似值……而该近似值通常正好就是我们想要的数字。

```
>>> fr2 = Fraction(0.01).limit_denominator(1000)
>>> print(fr2)
1/100
```

成功！但该类的真正强大之处在于它支持所有 `Fraction` 类型对象的标准操作，并且结果保证绝对正确。下面是一个示例：

```
>>> fr1 = Fraction(1, 2)
>>> fr2 = Fraction(1, 3)
>>> fr3 = Fraction(5, 12)
>>> print(fr1 + fr2 + fr3)
5/4
```

因此，1/2、1/3 和 5/12 相加得到 5/4。你可以自己验证这个答案是否正确。其他算术运算，如乘法、除法、减法等，均受支持，并可以与整数平滑结合。

```
>>> fr1 = Fraction(1, 100)
>>> print(fr1, 'times 50 =', fr1 * 50)
1/100 times 50 = 1/2
```

考虑到你可以从指定浮点表达式的字符串（例如 `'0.1'`）初始化 **Fraction** 对象，那么你能否从字符串 `'1/7'` 初始化，这是我们真正想做的？

是的。这尤其方便，我们将在即将推出的应用程序中使用它。

```
>>> fr1 = Fraction('1/7')
>>> print(fr1)
1/7
>>> fr1 += Fraction('3/4')
>>> print(fr1)
25/28
```

这种转换只有在分子、正斜杠 (/) 和分母之间没有空格时才有效。很少有用户会愚蠢到加入额外空格。但如果你担心用户会这样做，你可以始终通过以下方式消除空格：

```
s = s.replace(' ', '')
```

最后，你始终可以访问 Fraction 对象的 **numerator** 和 denominator 成员。但请记住，这些对象在输入后会立即被简化。下面是一个示例：

```
>>> fr1 = Fraction('100/300')
>>> print('numerator is', fr1.numerator)
numerator is 1
>>> print('denominator is', fr1.denominator)
denominator is 3
```

现在，让我们创建另一个加法器应用程序——这次是针对分数的。分数可以以 'x/y' 形式的字符串输入，这使得这个应用程序易于编写。

```
from fractions import Fraction
total = Fraction('0')
while True:
    s = input('Enter fraction (press ENTER to quit): ')
    s = s.replace(',', '') # Elim. spaces, just in case.
    if not s:
        break
    total += Fraction(s)
print('The total is %s.' % total)
```

哇，这是一个短程序！之所以这么容易编写，是因为用户可以以他们通常使用的形式输入分数（例如 '1/3' 表示三分之一），而无需引入任何额外代码来词法分析此结果。是的，Fraction 类为你完成了所有工作！但是，用户不能以 '2 1/3' 的形式输入分数。那个特定的数量必须输入为 '7/3'。下面是一个示例会话。注意该应用程序如何平滑地处理负数和整数，例如 2。

```
Enter fraction (press ENTER to quit): 2
Enter fraction (press ENTER to quit): 1
Enter fraction (press ENTER to quit): 1/2
Enter fraction (press ENTER to quit): 1/3
Enter fraction (press ENTER to quit): -3
Enter fraction (press ENTER to quit):
The total is 5/6.
```

处理整数没有问题，因为像 2 这样的输入会被转换为分数 2/1。

## 10.14 复数类

在结束本章之前，我们将再看一个 Python 内置类型：复数类。像 int 和 float 一样，它是一个完全内置的不可变类；你甚至不需要导入任何东西。复数到底是什么？幸运的是，如果你不知道它是什么，你几乎肯定不是需要知道它的人。复数理论为

