```
>>> y3 = s*(x1-x3)-y1
>>> print(x3,y3)
18.0 -77.0
```

# Exercise 7

Write the __add__ method when P₁ = P₂.

```
class Point:
    ...
    def __add__(self, other):
        ...
if self == other:
    s = (3 * self.x**2 + self.a) / (2 * self.y)
    x = s**2 - 2 * self.x
    y = s * (self.x - x) - self.y
    return self.__class__(x, y, self.a, self.b)
```

# Chapter 3: Elliptic Curve Cryptography

# Exercise 1

Evaluate whether these points are on the curve y² = x³ + 7 over F₂₂₃:

(192,105), (17,56), (200,119), (1,193), (42,99)

```
>>> from ecc import FieldElement
>>> prime = 223
>>> a = FieldElement(0, prime)
>>> b = FieldElement(7, prime)
>>> def on_curve(x,y):
...     return y**2 == x**3 + a*x + b
>>> print(on_curve(FieldElement(192, prime), FieldElement(105, prime)))
True
>>> print(on_curve(FieldElement(17, prime), FieldElement(56, prime)))
True
>>> print(on_curve(FieldElement(200, prime), FieldElement(119, prime)))
False
>>> print(on_curve(FieldElement(1, prime), FieldElement(193, prime)))
True
>>> print(on_curve(FieldElement(42, prime), FieldElement(99, prime)))
False
```

# Exercise 2

For the curve y² = x³ + 7 over F₂₂₃, find:

+   - (170,142) + (60,139)
+   - (47,71) + (17,56)