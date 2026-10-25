depending on the value of the digit to the right of the least
significant digit in the result. If the digit is 5 or higher, then
round upward; if 4 or lower, round downward. This behavior
works for both positive and negative numbers, so “rounding
upward” produces a value farther from 0. For example:

```
>>> round(-55.55)
-56
```

# SET([ITERABLE])

Conversion function for Python sets. If the *iterable* argument
is omitted, the result is empty. This is the standard way of
representing an empty set in Python, because { } represents an
empty dictionary rather than an empty set.

```
empty_set = set()
```

If the *iterable* is not empty, then the resulting set contains
all the elements in the argument, but duplicates are dropped
and order is not significant. For example:

Click here to view code image

```
>>> my_list = [11, 11, 3, 5, 3, 3, 3]
>>> my_set = set(my_list)
>>> my_set
{3, 11, 5}
```

# SETATTR(OBJ, NAME_STR,