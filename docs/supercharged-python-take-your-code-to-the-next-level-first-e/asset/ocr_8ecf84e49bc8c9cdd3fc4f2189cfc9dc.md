are no-ops: They don’t really do anything other than return the object as it currently is.
However, some of the methods are of interest to Python programmers generally. One of these is the **normalize** method. The action is to reduce the precision of the object to the minimum necessary, effectively getting rid of trailing zeros.
In the following example, **normalize** takes an object with precision of three places past the decimal point and returns an object with only one place of precision.

```
>>> d = Decimal('15.700')
>>> print(d)
15.700
>>> d2 = d.normalize()
>>> print(d2)
15.7
```

The **normalize** method will even get rid of the decimal point altogether if the fractional portion is zero.

```
>>> d = Decimal('6.00')
>>> print(d)
6.00
>>> d2 = d.normalize()
>>> print(d2)
6
```

However, when you change the precision of a **Decimal** value, you get an object having a different internal state, even though the values are considered equal when tested for equality (`==`). Assume the values of `d` and `d2` in the previous example: