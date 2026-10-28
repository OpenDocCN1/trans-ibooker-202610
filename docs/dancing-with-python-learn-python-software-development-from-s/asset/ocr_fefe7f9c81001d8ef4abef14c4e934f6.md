# Exercise 7.17

Is this implementation of __rsub__:

```
def __rsub__(self, other):
    """Computes other - self.

    Returns
    ------
    :class:`UniPoly`
        The difference of other and self.
    """

    # other - self == -(self - other)
    # For efficiency, this should be computed like __sub__
    # so we create fewer intermediate polynomials.

    return self.__sub__(other).__neg__()
```

as efficient as it could be?

```
2 - UniPoly(3, 'x', 7)
```

-3*x**7 + 2

What if you wrote and used an internal-only instance method __destructive_negate that negated the coefficients of self in place?

# 7.10.5 Multiplication

To implement multiplication "*", we code the __mul__ and __rmul__ instance methods. Since our polynomial multiplication is commutative, __rmul__ is straightforward:

```
def __rmul__(self, other):
    """Computes other * self.

    Returns
    ------
    :class:`UniPoly`
        The product of other and self.
    """
```