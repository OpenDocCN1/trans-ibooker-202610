
```
>>> import operator
>>> operator.eq(42, 42)      # "Equal", same as 42 == 42
True
>>> operator.ne('cat', 'dog')  # "Not Equal", same as 'cat' != 'dog'
True
>>> operator.gt(10, 20)       # "Greater Than ", same as 10 > 20
False
>>> operator.ge(10, 10)       # "Greater than or Equal", same as 10 >= 10
True
>>> operator.lt(10, 20)       # "Less Than", same as 10 < 20
True
>>> operator.le(10, 20)       # "Less than or Equal", same as 10 <= 20
True
```

operator 模块为我们提供了比较运算符的函数版本。它们的实现很简单。例如，我们可以用两行代码编写自己的 operator.eq() 函数：

```
def eq(a, b):
    return a == b
```

拥有比较运算符的函数形式很有用，因为与运算符不同，函数可以作为参数传递给函数调用。我们将这样做来实现比较 dunder 方法的辅助方法。

首先，将以下内容添加到 wizcoin.py 的开头。这些导入使我们能够访问 operator 模块中的函数，并允许我们通过将其与 collections.abc.Sequence 进行比较来检查方法中的另一个参数是否为序列：

```
import collections.abc
import operator
```

然后将以下内容添加到 wizcoin.py 文件的末尾：

```
--snip--
    def _comparisonOperatorHelper(self, operatorFunc, other):
        """A helper method for our comparison dunder methods."""
        if isinstance(other, WizCoin):
            return operatorFunc(self.total, other.total)
        elif isinstance(other, (int, float)):
            return operatorFunc(self.total, other)
        elif isinstance(other, collections.abc.Sequence):
            otherValue = (other[0] * 17 * 29) + (other[1] * 29) + other[2]
            return operatorFunc(self.total, otherValue)
        elif operatorFunc == operator.eq:
            return False
        elif operatorFunc == operator.ne:
            return True
        else:
            return NotImplemented
```

```
def __eq__(self, other):  # eq is "Equal"
    return self._comparisonOperatorHelper(operator.eq, other)

def __ne__(self, other):  # ne is "Not Equal"
    return self._comparisonOperatorHelper(operator.ne, other)

def __lt__(self, other):  # lt is "Less Than"
    return self._comparisonOperatorHelper(operator.lt, other)

def __le__(self, other):  # le is "Less than or Equal"
    return self._comparisonOperatorHelper(operator.le, other)

def __gt__(self, other):  # gt is "Greater Than"
    return self._comparisonOperatorHelper(operator.gt, other)

def __ge__(self, other):  # ge is "Greater than or Equal"
    return self._comparisonOperatorHelper(operator.ge, other)
```

我们的比较 dunder 方法调用 _comparisonOperatorHelper() 方法 ① 并为 operatorFunc 参数传递来自 operator 模块的适当函数。当我们调用 operatorFunc() 时，我们调用的是为 operatorFunc 参数传递的函数——来自 operator 模块的 eq() ⑤、ne() ⑥、lt() ⑦、le() ⑧、gt() ⑨ 或 ge() ⑩。否则，我们就需要在六个比较 dunder 方法中的每一个中重复 _comparisonOperatorHelper() 中的代码。

### 注意
*像 _comparisonOperatorHelper() 这样接受其他函数作为参数的函数（或方法）被称为高阶函数。*

我们的 WizCoin 对象现在可以与其他 WizCoin 对象 ②、整数和浮点数 ③ 以及代表加隆、锡克尔和纳特的三个数值序列 ④ 进行比较。输入以下内容到交互式 shell 中以查看其实际效果：

```
>>> import wizcoin
>>> purse = wizcoin.WizCoin(2, 5, 10)  # Create a WizCoin object.
>>> tipJar = wizcoin.WizCoin(0, 0, 37) # Create another WizCoin object.
>>> purse.total, tipJar.total # Examine the values in knuts.
(1141, 37)
>>> purse > tipJar # Compare WizCoin objects with a comparison operator.
True
>>> purse < tipJar
False
>>> purse > 1000 # Compare with an int.
True
>>> purse <= 1000
False
>>> purse == 1141
True
>>> purse == 1141.0 # Compare with a float.
True
>>> purse == '1141' # The WizCoin is not equal to any string value.
False
```

```
>>> bagOfKnuts = wizcoin.WizCoin(0, 0, 1141)
>>> purse == bagOfKnuts
True
>>> purse == (2, 5, 10) # We can compare with a 3-integer tuple.
True
>>> purse >= [2, 5, 10] # We can compare with a 3-integer list.
True
>>> purse >= ['cat', 'dog'] # This should cause an error.
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "C:\Users\Al\Desktop\wizcoin.py", line 265, in __ge__
    return self._comparisonOperatorHelper(operator.ge, other)
    File "C:\Users\Al\Desktop\wizcoin.py", line 237, in _
    otherValue = (other[0] * 17 * 29) + (other[1] * 29) + other[2]
IndexError: list index out of range
```

我们的辅助方法调用 isinstance(other, collections.abc.Sequence) 来查看 other 是否是序列数据类型，例如元组或列表。通过使 WizCoin 对象可与序列进行比较，我们可以编写诸如 purse >= [2, 5, 10] 这样的代码进行快速比较。

## 序列比较

当比较内置序列类型（如字符串、列表或元组）的两个对象时，Python 会更重视序列中的前几个项目。也就是说，除非前面的项目值相等，否则它不会比较后面的项目。例如，输入以下内容到交互式 shell 中：

```
>>> 'Azriel' < 'Zelda'
True
>>> (1, 2, 3) > (0, 8888, 9999)
True
```

字符串 'Azriel' 在 'Zelda' 之前（换句话说，小于 'Zelda'），因为 'A' 在 'Z' 之前。元组 (1, 2, 3) 在 (0, 8888, 9999) 之后（换句话说，大于），因为 1 大于 0。另一方面，输入以下内容到交互式 shell 中：

```
>>> 'Azriel' < 'Aaron'
False
>>> (1, 0, 0) > (1, 0, 9999)
False
```

字符串 'Azriel' 不在 'Aaron' 之前，因为尽管 'Azriel' 中的 'A' 等于 'Aaron' 中的 'A'，但 'Azriel' 中的后续 'z' 不在 'Aaron' 中的 'a' 之前。同样适用于元组 (1, 0, 0) 和 (1, 0, 9999)：每个元组的前两个项目相等，因此是第三个项目（分别为 0 和 9999）决定了 (1, 0, 0) 在 (1, 0, 9999) 之前。

这迫使我们对 WizCoin 类做出设计决策。WizCoin(0, 0, 9999) 应该在 WizCoin(1, 0, 0) 之前还是之后？如果加隆的数量比锡克尔或纳特的数量更重要，则 WizCoin(0, 0, 9999) 应该在 WizCoin(1, 0, 0) 之前。或者如果我们根据纳特值比较对象，则 WizCoin(0, 0, 9999)（价值 9,999 纳特）在 WizCoin(1, 0, 0)（价值 493 纳特）之后。在 wizcoin.py 中，我决定使用对象的纳特值，因为这种行为与 WizCoin 对象与整数和浮点数比较的方式一致。这些是在设计自己的类时你必须做出的决定类型。

没有你需要实现的反射比较 dunder 方法，例如 __req__() 或 __rne__()。相反，__lt__() 和 __gt__() 互相反射，__le__() 和 __ge__() 互相反射，而 __eq__() 和 __ne__() 自身反射。原因是无论运算符左右两侧的值是什么，以下关系都成立：

- purse > [2, 5, 10] 与 [2, 5, 10] < purse 相同
- purse >= [2, 5, 10] 与 [2, 5, 10] <= purse 相同
- purse == [2, 5, 10] 与 [2, 5, 10] == purse 相同
- purse != [2, 5, 10] 与 [2, 5, 10] != purse 相同

一旦你实现了比较 dunder 方法，Python 的 sort() 函数将自动使用它们对你的对象进行排序。输入以下内容到交互式 shell 中：

```
>>> import wizcoin
>>> oneGalleon = wizcoin.WizCoin(1, 0, 0) # Worth 493 knuts.
>>> oneSickle = wizcoin.WizCoin(0, 1, 0)  # Worth 29 knuts.
>>> oneKnuts = wizcoin.WizCoin(0, 0, 1)   # Worth 1 knut.
>>> coins = [oneSickle, oneKnuts, oneGalleon, 100]
>>> coins.sort() # Sort them from lowest value to highest.
>>> coins
[WizCoin(0, 0, 1), WizCoin(0, 1, 0), 100, WizCoin(1, 0, 0)]
```

## 表 17-3：比较 Dunder 方法和 operator 模块函数

| Dunder 方法 | 操作 | 比较运算符 | operator 模块中的函数 |
|---|---|---|---|
| __eq__() | 等于 | == | operator.eq() |
| __ne__() | 不等于 | != | operator.ne() |
