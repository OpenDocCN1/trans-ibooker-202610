
## 10.11 SETTING THE DEFAULT CURRENCY

To make our Money class friendlier to a wider group of people, we should enable users of the class to set default units other than U.S. dollars. An easy way to implement this feature is to tie it to a class variable and then let the user of the class change it as desired.

To do that, we first need to add a class variable to the Money class, a very easy change to make.

```python
class Money():
    default_curr = 'USD'
```

Then we need to alter the `__init__` function. This is trickier than it sounds, because although you can refer to class variables from within a method definition, you can't use such a reference in the argument list. So the following causes an error:

```python
# This causes an ERROR!
def __init__(self, v='0',
             units=Money.default_curr):
```

It's frustrating that we can't do this. However, the following definition of the `__init__` function works perfectly well, by replacing the default value (an empty string) with the value stored in `default_curr`.

```python
def __init__(self, v='0', units=''):
    self.dec_amt = Decimal(v)
    if not units:
        self.units = Money.default_curr
    else:
        self.units = units
```

With the changes (shown in bold) made to the `__init__` function, the class variable, `default_curr`, now becomes in effect the default value for `units`.

Finally, the `money_calc` function can easily be altered so that the units entered for the first item become the new default setting for the class. One line of code needs to be added, about three-quarters of the way through the loop.

```python
if n == 0:    # If this is first entry...
    amt = m    # Create amt!
    Money.default_curr = m.units
```

With this change, the application now enables the user to specify a default different from U.S. dollars. All they have to do is specify the new default in the first money object they enter.

For example, the user in the following sample session causes Canadian dollars (CAD) to be the default.

```
Enter money value: 1.0 CAD
Enter money value: 2.05
Enter money value: .95
Enter money value: 2
Enter money value:
Total is 6.00 CAD
```

In this case, it's easy to see that both the units used for the total, and the units used as the default currency, are Canadian dollars, and not U.S. dollars.

And in this next sample session, you can see that the default remains Canadian dollars, even if a different currency is entered in the middle of the series.

```
Enter money value: 2.0 CAD
Enter money value: -1
Enter money value: 10 USD
Enter money value: 5.01
Enter money value: -5.01
Enter money value:
Total is 14.30 CAD
```

You can see that all the Canadian amounts cancel out except for one Canadian dollar. A figure of 10 U.S. dollars was also entered. But the final result is printed in Canadian dollars—because the first figure was in CAD. So, although the sum contains 10 U.S. dollars, it's converted to the equivalent in CAD, plus the one Canadian dollar that was not canceled out, giving you 10 U.S. dollars in Canadian dollars (13.30), plus one Canadian dollar (1.00), for a grand total of 14.30.

You should note that changing the default units for the class is a little tricky; such a change affects all subsequent uses of the class as long as the program is running. (However, it does not affect future running of the program.) But if you show a little care, this shouldn't be a problem.

## 10.12 MONEY AND INHERITANCE

What's the best way to get money? Inheritance, of course.

As we mentioned in Section 10.6, “Designing a Money Class,” the more natural way to create a Money class based on an existing object type, `Decimal`, would be to use inheritance—that is, subclassing `Decimal`.

The problem is that the `Decimal` type is immutable. This creates a special challenge; that challenge is solved by a few lines of code, but how to write this code is not at all obvious. Not to worry, though. This section will give you that specialized knowledge.

Normally, inheritance would be easy to implement. Suppose that Money subclassed another class named Thingie, which is not immutable. In that case, you could use the following easy-to-write code:

```python
class Money(Thingie):
    def __init__(self, v, units='USD'):
        super().__init__(v)
        self.units = units
```

What this approach says (and remember that this is the approach you'd use for most classes, but not `Decimal`) is “Call the superclass function to handle initialization of the first argument, but initialize the second argument, units, directly.” Remember that units is the extra attribute that the Money class adds to the Thingie class.

But this approach fails with immutable classes such as Decimal. Instead, it's necessary to write a `__new__` method for the Money class. The allocation of the Decimal portion of the Money class is handled by `__new__`.

```python
from decimal import Decimal

class Money(Decimal):
    def __new__(cls, v, units='USD'):
        return super(Money, cls).__new__(cls, v)

    def __init__(self, v, units='USD'):
        self.units = units

m = Money('0.11', 'USD')
print(m, m.units)
```

This small program prints the following:

```
0.11 USD
```

If you want to apply this coding technique to another situation involving an immutable superclass, here's what you need to remember: Use the `__new__` function to call the superclass version of `__new__`. The arguments should be the subclass name and cls, a reference to the class. Let this method initialize the portion of the class that originates in the superclass (in this case, v). Make sure to pass along the value returned by the superclass version of `__new__`.

```python
def __new__(cls, v, units='USD'):
    return super(Money, cls).__new__(cls, v)
```

For other situations, we can generalize upon this pattern for any given class and superclass named MyClass and MySuperClass, and for superclass data, d:

```python
class MyClass(MySuperClass):
    def __new__(cls, d, other_data):
        return super(MyClass, cls).__new__(cls, d)
```

> **Note**
>
> We can further generalize this code as follows, in which `d` is data in the base class, and `other_data` is data in the subclass, which should be initialized in `__init__`.
>
> ```python
> class MyClass(MySuperClass):
>     def __new__(cls, d, other_data):
>         return super(MyClass, cls).__new__(cls, d)
> ```

Now, let's return to the Money example. An `__init__` method still needs to be written if any additional attributes have been added by the subclass and need to be initialized. The `__init__` method should be used to
