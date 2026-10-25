

## 10.9 其他货币操作

到目前为止，我们使用 Money 类只能创建对象并打印它们。但要使该类有用，至少应支持 Money 对象之间的加法操作（+）。

如果暂时忽略单位的作用，`__add__` 函数很容易编写。这个版本假设你只想将 Money 对象添加到同一类的其他对象中。

Click here to view code image

```
def __add__(self, other):
    d = self.dec_amt + other.dec_amt
    return Money(str(d))
```

我们可以扩展这个函数定义，预设左操作数使用的单位应由结果使用。实现这种方法给我们提供了函数定义的第二个版本。在以下代码中，要添加的项目以粗体显示，仅为了说明。

```
def __add__(self, other):
    d = self.dec_amt + other.dec_amt
    return Money(str(d), self.units)
```

更有趣、更有用的是，在先乘以货币汇率后，将右侧使用的单位转换为与左侧匹配。虽然这些数字每天都在变化，但程序可以根据需要进行修改以适应这些变化。一种方法是从按需更新的文件中读取货币汇率。

为了暂时保持简单，我们将选取一些汇率（即撰写本文时的当前汇率），并假设程序可以根据需要进行修改。因为有六种可能的转换（来自我们支持的三种货币），最好的方法是使用字典。键值是连接两个货币符号的结果。值字段显示第二种货币必须乘以什么数字才能产生第一种货币。

```
exch_dict = {
    'USDCAD': Decimal('0.75'), 'USDEUR': Decimal('1.16'),
    'CADUSD': Decimal('1.33'), 'CADEUR': Decimal('1.54'),
    'EURUSD': Decimal('0.86'), 'EURCAD': Decimal('0.65')
}
```

例如，`USDCAD` 键的值为 0.75，这意味着加拿大元数字乘以 0.75 即可得到其等值的美元。现在，函数的最终版本可以在每当两种不同货币相加时应用货币汇率。

字典将汇率存储为 Decimal 对象，从而使后续算术运算更容易执行。

Click here to view code image

```
def __add__(self, other):
    ''''Money add function.
    Supports two Money objects added together;

    if
    the second has a different currency unit,
    then
    exchange rate must be applied before adding
    the
    two amounts together. Apply rounding of 2.
    '''
    if self.units != other.units:
        r = Money.exch_dict[self.units +
                            other.units]
        m1 = self.dec_amt
        m2 = other.dec_amt * r
        m = Money(m1 + m2, self.units)
    else:
        m = Money(self.dec_amt + other.dec_amt,
                  self.units)
    m.dec_amt = round(m.dec_amt, 2)
    return m
```

让我们逐步了解这个函数的工作原理。正如注释（或者更确切地说，文档字符串）所指出的，如果单位不同（例如美元与加拿大元），则在相加金额之前可能会应用汇率。虽然在大多数位置汇率表示为浮点数，但我们在这里将这些汇率存储为 **Decimal** 对象，以便需要进行的转换更少。

Click here to view code image

```
r = Money.exch_dict[self.units +
other.units]
m1 = self.dec_amt
m2 = other.dec_amt * r
m = Money(m1 + m2, self.units)
```

无论哪种情况——无论是应用了汇率还是没有应用——我们也希望应用 2 的舍入因子，以便货币始终以小数点后两位精度表示。

Click here to view code image

```
m.dec_amt = round(m.dec_amt, 2)
```

新的 Money 对象 m 最终由 `__add__` 函数返回。

有了这个函数定义，以及可以作为 Money 类变量的 `exch_dict`，我们现在可以将不同货币相加——只要它们是该程序识别的三种货币之一（尽管该列表可以根据需要扩展）。例如，我们可以将一美元与一加拿大元相加并得到有意义的结果。

Click here to view code image

```
>>> us_m = Money('1', 'USD')
>>> ca_m = Money('1', 'CAD')
>>> print(us_m + ca_m)
1.75 USD
```

> Note
> 当然，只要使用了三种支持的货币，这个函数定义就能正确工作。如果使用了 USD、CAD 或 EUR 以外的单位，每当混合货币相加时都会导致 **KeyError** 异常。

把所有内容放在一起，这里是完整的 Money 类。当然，它并不真正完整，因为我们还可以添加许多操作，例如减法和整数乘法。

Click here to view code image

```
from decimal import Decimal

class Money():
    ''''Money Class.
    Stores both a Decimal amount and currency units.
    When
    objects are added, exchange rate will be applied
    if
    the currency units differ.
    '''
    exch_dict = {
        'USDCAD': Decimal('0.75'), 'USDEUR':
        Decimal('1.16'),
        'CADUSD': Decimal('1.33'), 'CADEUR':
        Decimal('1.54'),
        'EURUSD': Decimal('0.86'), 'EURCAD':
        Decimal('0.65')
    }
    def __init__(self, v = '0', units = 'USD'):
        self.dec_amt = Decimal(v)
        self.units = units
    def __str__(self):
        s = str(self.dec_amt) + ' ' + self.units
        return s
    def __repr__(self):
        s = ('Money(' + str(self.dec_amt) + ' ' +
        str(self.units) + ')')
        return s
    def __add__(self, other):
        """Money add function.
        Supports two Money objects added together;
        if
        the second has a different currency unit,
        then
        exchange rate (r) is applied before adding
        the
        two amounts together. Apply rounding of 2.
        """
        if self.units != other.units:
            r = Money.exch_dict[self.units +
            other.units]
            ml = self.dec_amt
            m2 = other.dec_amt * r
            m = Money(ml + m2, self.units)
        else:
            m = Money(self.dec_amt + other.dec_amt,
            self.units)
        m.dec_amt = round(m.dec_amt, 2)
        return m
```

这就是（目前）完整的类定义——虽然，如前所述，你可能想添加许多操作。

## 10.10 演示：货币计算器

有了完整的 Money 类定义，现在可以编写一个计算器应用程序，它可以将我们目前支持的三种不同货币中的任意数量金额相加，并以共同面额给出答案。

大部分代码很容易编写，但用户输入必须分解为数值和单位部分，这在一定程度上使编码变得复杂。幸运的是，大部分工作由 **str** 类型的 **split** 方法完成。

Click here to view code image

```
from decimal import Decimal

# Place Money class definition here or import it.
def money_calc():
    ''''Money addition calculator.
    Prompt for a series of Money objects until empty
    string is entered; then print results of the
    running total.
    '''

    n = 0
    while True:
        s = input('Enter money value: ')
        s = s.strip()
        if not s:
            break
        a_list = s.split() # Split into amt, units.
        d = a_list[0]
        if len(a_list) > 1:
            m = Money(d, a_list[1])
        else:
            m = Money(d)
        if n == 0:
            amt = m
        else:
            amt += m
        n += 1
    print('Total is', amt)
money_calc()
```

这段代码的最后一行执行该函数，使其成为一个完整的程序。

这个函数有一个微妙之处。最好让第一种货币选择（第一行输入的单位）决定最终答案所使用的货币。这给用户对结果的控制权。例如，也许你希望结果以加拿大元或欧元表示；你只需确保第一个条目使用这些单位即可。

问题是，我们正在保持一个运行总数，而保持运行总数的常用方法是从初始零值开始。这里有一个示例：

```
amt = Money('0')
```

这里的问题是，目前 USD 是单位的默认值；因此，这个初始选择通过程序的逻辑，会预先确定该程序的每个结果都以美元表示。

