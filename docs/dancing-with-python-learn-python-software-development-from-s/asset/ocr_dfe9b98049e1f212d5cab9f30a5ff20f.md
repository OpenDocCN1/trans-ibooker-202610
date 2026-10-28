```
s = UniPoly(1, 's', 1)

for exp_coef in (s - 1)**4:
    print(exp_coef)
```

(4, 1)
(3, -4)
(2, 6)
(1, -4)
(0, 1)

I used a Notes section within the documentation for __iter__.

If an object is both an iterable and an iterator, the object probably stores the iteration state within itself. That’s why __iter__ might return self. As we have seen, though, there may be multiple ways you want to iterate across an object. In this case, you would create separate iterables as Python does with dictionaries.

# Exercise 7.37

In our UniPoly class, I define a property is_constant that tells us whether a polynomial is constant. Remember that a constant polynomial has no term with the indeterminate exponent greater than 0.

```
@property
def is_constant(self):
    """True if the polynomial only contains a term of exponent 0."""

    if not self.__terms:
        return True
    if len(self.__terms) != 1:
        return False
    return next(iter(self.__terms)) == 0
```

What does the last line do?