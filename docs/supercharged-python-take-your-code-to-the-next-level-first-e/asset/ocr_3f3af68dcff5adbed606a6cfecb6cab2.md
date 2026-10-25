But what if you want to assign different values to different variables? For example, suppose you want to assign 1 to a, and 0 to b. The obvious way to do that is to use the following statements:

```
a = 1
b = 0
```

But through *tuple assignment*, you can combine these into a single statement.

```
a, b = 1, 0
```

In this form of assignment, you have a series of values on one side of the equals sign (=) and another on the right. They must match in number, with one exception: You can assign a tuple of any size to a single variable (which itself now represents a tuple as a result of this operation).

Click here to view code image

```
a = 4, 8, 12 # a is now a tuple containing three
values.
```

Tuple assignment can be used to write some passages of code more compactly. Consider how compact a Fibonacci-generating function can be in Python.

```
def fibo(n):
    a, b = 1, 0
    while a <= n:
        print(a, end=' ')
```