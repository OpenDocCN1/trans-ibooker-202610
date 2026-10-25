__str__ function in that it identifies the class as well as
showing its contents.

```
>>> m2 = Money('0.10')
>>> print(m2)
0.10 USD
>>> m2
Money(0.10 USD)
```

# 10.9 OTHER MONETARY OPERATIONS

So far, all we can do with the Money class is create objects and
print them. But to be useful, the class should support, at
minimum, addition operations (+) between Money objects.

If we ignore the role of units for the moment, the __add__
function is easy to write. This version assumes that you only
want to add Money objects to other objects of the same class.

Click here to view code image

```
def __add__(self, other):
    d = self.dec_amt + other.dec_amt
    return Money(str(d))
```

We can expand on this function definition by presupposing
that whatever units are used by the left operand should be used
by the result. Implementing that approach gives us a second
version of the function definition. In the following code, the
item to be added is shown in bold for the sake of illustration.