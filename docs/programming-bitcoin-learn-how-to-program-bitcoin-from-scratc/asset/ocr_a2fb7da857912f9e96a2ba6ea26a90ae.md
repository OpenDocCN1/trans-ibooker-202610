# Coding Point Addition over Finite Fields

Because we coded `FieldElement` in such a way as to define `__add__`, `__sub__`, `__mul__`, `__truediv__`, `__pow__`, `__eq__`, and `__ne__`, we can simply initialize `Point` with `FieldElement` objects and point addition will work:

```
```python
>>> from ecc import FieldElement, Point
>>> prime = 223
>>> a = FieldElement(num=0, prime=prime)
>>> b = FieldElement(num=7, prime=prime)
>>> x1 = FieldElement(num=192, prime=prime)
>>> y1 = FieldElement(num=105, prime=prime)
>>> x2 = FieldElement(num=17, prime=prime)
>>> y2 = FieldElement(num=56, prime=prime)
>>> p1 = Point(x1, y1, a, b)
>>> p2 = Point(x2, y2, a, b)
>>> print(p1+p2)
Point(170,142)_0_7 FieldElement(223)
```
```

# Exercise 2

For the curve $y^2 = x^3 + 7$ over $F_{223}$, find:

+   - $(170,142) + (60,139)$
+   - $(47,71) + (17,56)$
+   - $(143,98) + (76,66)$

# Exercise 3

Extend `ECCTest` to test for the additions from the previous exercise. Call this `test_add`.

# Scalar Multiplication for Elliptic Curves

Because we can add a point to itself, we can introduce some new notation:

$(170,142) + (170,142) = 2 \cdot (170,142)$

Similarly, because we have associativity, we can actually add the point again:

$2 \cdot (170,142) + (170,142) = 3 \cdot (170, 142)$

We can do this as many times as we want. This is what we call *scalar multiplication*. That is, we have a *scalar* number in front of the point. We can do this because we have defined point addition and point addition is associative.