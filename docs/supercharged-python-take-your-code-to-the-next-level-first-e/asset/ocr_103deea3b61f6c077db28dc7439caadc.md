```
def quad(a, b, c):
    determin = (b * b - 4 * a * c) ** .5
    x1 = (-b + determin) / (2 * a)
    x2 = (-b - determin) / (2 * a)
    return x1, x2
```

This function has three input arguments and two output variables. In calling the function, it’s important to receive both arguments:

```
x1, x2 = quad(1, -1, -1)
```

If you return multiple values to a single variable in this case, that variable will store the values as a tuple. Here’s an example:

```
>>> x = quad(1, -1, -1)
>>> x
(1.618033988749895, -0.6180339887498949)
```

Note that this feature—returning multiple values—is actually an application of the use of tuples in Python.

# ## 4.2.9 Use Loops and the “else” Keyword

The `else` keyword is most frequently used in combination with the `if` keyword. But in Python, it can also be used with `try-except` syntax and with loops.

With loops, the `else` clause is executed if the loop has