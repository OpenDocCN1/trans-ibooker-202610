# Exercise 9

Write the corresponding `__truediv__` method that defines the division of two field elements.

Note that in Python 3, division is separated into `__truediv__` and `__floordiv__`. The first does normal division and the second does integer division.

# Redefining Exponentiation

One last thing that we need to take care of before we leave this chapter is the `__pow__` method, which needs to handle negative exponents. For example, $a^{-3}$ needs to be a finite field element, but the current code does not take care of this case. We want, for example, something like this to work:

```
>>> from ecc import FieldElement
>>> a = FieldElement(7, 13)
>>> b = FieldElement(8, 13)
>>> print(a**-3==b)
True
```

Unfortunately, the way we’ve defined `__pow__` simply doesn’t handle negative exponents, because the second parameter of the built-in Python function `pow` is required to be positive.

Thankfully, we can use some math we already know to solve this. We know from Fermat’s little theorem that:

$$a^{p-1}=1$$

This fact means that we can multiply by $a^{p-1}$ as many times as we want. So, for $a^{-3}$, we can do:

$$a^{-3}=a^{-3}\cdot a^{p-1}=a^{p-4}$$

This is a way we can do negative exponents. A naive implementation would do something like this:

```
class FieldElement:
    ...
    def __pow__(self, exponent):
        n = exponent
        while n < 0:
            n += self.prime - 1
        num = pow(self.num, n, self.prime)
        return self.__class__(num, self.prime)
```

① We add until we get a positive exponent.