```
>>> d = Decimal('0.1')
>>> print(d + d + d)
0.3
```

This example does what you’d expect, but you should already see a twist to it. The **Decimal** variable, `d`, was initialized with a text string. It might seem much more natural to initialize it with a floating-point value. But look what happens if you do.

Click here to view code image

```
>>> d = Decimal(0.1)
>>> print(d)
0.10000000000000000555111512312578...
```

This result must seem strange. But there’s a reason for it. When `0.1` is used to initialize, a floating-point value (type **float**) is converted to **Decimal** format. As stated, **Decimal** can store `0.1` with absolute precision. But in this case, it first has to be converted from floating point; and the problem is, the floating-point value _already contains the rounding error within it_. This is eating the fruit of a poisoned tree.

How do we get around this problem? Initializing from a string is the best solution. Using "`0.01`" as the initializer says, “I want the decimal realization of what this string represents”— that is, the value without rounding errors.

Let’s look at another example.

Click here to view code image

```
>>> d = Decimal('0.1')
>>> print(d + d + d)
```