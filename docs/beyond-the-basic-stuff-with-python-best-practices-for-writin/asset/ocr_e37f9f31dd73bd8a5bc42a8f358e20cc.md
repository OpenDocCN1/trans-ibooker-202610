```
raise WizCoinException('cannot multiply with negative integers')
return WizCoin(self.galleons * other, self.sickles * other, self.knuts * other)
```

This __mul__() method lets you multiply WizCoin objects by positive integers. If other is an integer, it's the data type the __mul__() method is expecting and we shouldn't return NotImplemented. But if this integer is negative, multiplying the WizCoin object by it would result in negative amounts of coins in our WizCoin object. Because this goes against our design for this class, we raise a WizCoinException with a descriptive error message.

> NOTE
> You shouldn't change the self object in a numeric dunder method. Rather, the method should always create and return a new object. The + and other numeric operators are always expected to evaluate to a new object rather than modifying an object's value in-place.

Enter the following into the interactive shell to see the __mul__() dunder method in action:

```
>>> import wizcoin
>>> purse = wizcoin.WizCoin(2, 5, 10)  # Create a WizCoin object.
>>> purse * 10  # Multiply the WizCoin object by an integer.
WizCoin(20, 50, 100)
>>> purse * -2  # Multiplying by a negative integer causes an error.
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Users\AI\Desktop\wizcoin.py", line 86, in __mul__
    raise WizCoinException('cannot multiply with negative integers')
wizzcoin.WizCoinException: cannot multiply with negative integers
```

Table 17-1 shows the full list of numeric dunder methods. You don't always need to implement all of them for your class. It's up to you to decide which methods are relevant.

# Table 17-1: Numeric Dunder Methods

| Dunder method | Operation | Operator or built-in function |
|---|---|---|
| __add__() | Addition | + |
| __sub__() | Subtraction | - |
| __mul__() | Multiplication | * |
| __matmul__() | Matrix multiplication (new in Python 3.5) | @ |
| __truediv__() | Division | / |
| __floordiv__() | Integer division | // |
| __mod__() | Modulus | % |
| __divmod__() | Division and modulus | divmod() |

(continued)