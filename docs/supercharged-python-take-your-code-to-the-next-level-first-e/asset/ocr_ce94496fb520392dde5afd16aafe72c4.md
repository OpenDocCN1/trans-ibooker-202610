```
def double_it(n):
    n = n * 2

x = 10
double_it(x)
print(x)    # x is still 10!
```

This may at first seem a limitation, because sometimes a programmer needs to create multiple “out” parameters.
However, you can do that in Python by returning multiple values directly. The calling statement must expect the values.

```
def set_values():
    return 10, 20, 30
a, b, c = set_values()
```

The variables a, b, and c are set to 10, 20, and 30,
respectively.

Because Python has no concept of data declarations, an
argument list in Python is just a series of comma-separated
names—except that each may optionally be given a default
value. Here is an example of a function definition with two
arguments but no default values:

```
def calc_hyp(a, b):
    hyp = (a * a + b * b) ** 0.5
    return hyp
```

These arguments are listed without type declaration; Python