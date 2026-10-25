
There is a sign bit (1 indicates negative; 0 indicates non-negative).
The decimal digits (1, 5, 0) are stored individually.
The precision is stored, as a negative exponent in this case, showing how many places to shift the decimal point to the right (or left if negative).

And in fact, you can use this same information, if you choose, to construct a `Decimal` object directly. Place a tuple inside parentheses, and then use the information to initialize an object:

```
>>> d = Decimal((0, (3, 1, 4), -2))
>>> print(d)
3.14
```

The general structure of such a tuple—a tuple that fully describes the state of a Decimal object—is shown here.

![](img/6e45221a8147f2442c653a3d4a866570_616_0.png)

```
(sign_bit, (digit1, digit2, digit3...), exponent)
```

Another operation that is sometimes of practical use is the `getcontext` function, defined in the decimal package. Here’s an example of use.

```
>>> decimal.getcontext()
Context(prec=28, rounding=ROUND_HALF_EVEN,
Emin=-999999, Emax=999999, capitals=1, clamp=0,
flags=[DivisionByZero, Inexact, FloatOperation,
Rounded], traps=[InvalidOperation, DivisionByZero,
Overflow])
```

This is a lot of information to wade through, but much of it’s useful. First, prec=28 indicates that there’s a maximum precision of 28 places. Second, the rounding technique, ROUND_HALF_EVEN, means that the final significant digit is rounded, if necessary, by checking whether the digit on its right is 5 or greater, in which case it’s rounded up. The traps indicate what kinds of operations cause an exception to be raised.

## 10.5 A DECIMAL CLASS APPLICATION

We can use the `Decimal` class to add up a column of figures in which the precision is two places to right of the decimal point.

That’s an appropriate way to add dollar and cents figures, so you can expect all amounts to be in the form 1.00 (one dollar), 1.50, 9.95, and so on. In such a situation, if we get fractions of a penny, we’ll round them up or down to the nearest cent rather than just throwing all remainders away thoughtlessly.

Finally, the application will present the result in dollars-and-cents format—without, however, printing the currency symbols. (That’s a feature we’ll add in the next section.) When you’re looking at the coding techniques, remember that a **Decimal** object is created most naturally and efficiently by initialization from a string.

Click here to view code image

```
money_amount = Decimal('1.99')
```

Here’s the full application code. The action is to prompt the user for a number and—if the user did not enter an empty string—add the specified string of digits as a **Decimal** object. If the user presses Enter without typing input, the program breaks, stops, and prints the total.

Click here to view code image

```
from decimal import Decimal
total = Decimal('0.00')
while True:
    s = input('Enter amount in dollars and cents (##.##): ')
    if not s:
        break
    d = Decimal(s)
    d = round(d, 2)
total += d
print('The sum of all these amounts is:', total)
```

The program could have used one of two different strategies in handling numbers with fractions smaller than 0.01. You could add all the smaller fractions together in the running total and do the rounding at the end. Instead, the approach here is to round amounts as each is entered.

Here’s a sample session.

Click here to view code image

```
Enter amount in dollars and cents (#.##): 1
Enter amount in dollars and cents (#.##): 3.00
Enter amount in dollars and cents (#.##): 4.50
Enter amount in dollars and cents (#.##): 33.003
Enter amount in dollars and cents (#.##): 12.404
Enter amount in dollars and cents (#.##):
The sum of all these amounts is: 53.90
```

Because of how this application applies rounding—after each entry—the result is 53.90. But we could have done all the rounding at the end, which would in this case produce a slightly different result: 53.91.

## 10.6 DESIGNING A MONEY CLASS

Money talks. So now we’re going to create a Money class. You could download such a class. But it’s instructive to create such a class yourself. In the process, we’ll use a good many of the concepts from Chapter 9 in a practical example.

It might be useful to store decimal figures along with a *units* figure that represents the type of currency. We’ll use three kinds, indicated by the value stored in an additional string field. The abbreviations in Table 10.1, by the way, are recognized internationally as standard names for these currencies.

# Table 10.1. Abbreviations for Three Currencies

| Symbol (to store in units, a str instance variable) | Description |
|---|---|
| 'USD' | U.S. dollars |
| 'EUR' | European dollars |
| 'CAD' | Canadian dollars |

Now there’s a decision to make: Which of the following two approaches should we use?

- Containment. This approach views a Money object as a container for a `Decimal` object along with another object (the `units` field). The drawback is that for every operation you want the Money class to support, you need to write a separate magic method.
- Inheritance. This approach views a Money object as a specialized kind of `Decimal` object, in which the `units` field is added as an additional attribute.

Given this choice, inheritance is probably the better way to go; it’s also more in keeping with the spirit of object orientation, which says that the relationship “A is a kind of B, only more specialized,” is really an inheritance relationship.

Containment, on the other hand, is appropriate when the relationship is “A has a B.” Containment works better in that case.

But there’s a quirk in the Python language that makes inheritance difficult to use in this situation. A general guideline is that if you want to keep things simple, avoid inheriting from an immutable or built-in class. Unfortunately, that includes inheriting from `Decimal`.

Therefore, in this chapter we’ll use object containment as a way of building a Money class around the `Decimal` class. Later, we’ll show how to use inheritance to create a Money class, in Section 10.12, “Money and Inheritance.”

Figure 10.1 shows the containment architecture. Each Money object contains two parts: a `Decimal` object, `dec_amt`, and a string called `units`.

![](img/6e45221a8147f2442c653a3d4a866570_621_0.png)

Figure 10.1. A Money class using containment

## 10.7 WRITING THE BASIC MONEY CLASS (CONTAINMENT)

Building a Money class around a `Decimal` object is easy to do in the beginning. Here’s how we get started:

Click here to view code image

```
from decimal import Decimal

class Money():
    def __init__(self, v = '0', units = 'USD'):
        self.dec_amt = Decimal(v)
        self.units = units
```

With this simple class definition, you can create Money objects and display their attributes, although other operations and methods need to be added. The following example takes advantage of the default units being U.S. dollars.

Click here to view code image

```
>>> m1 = Money('0.10')
>>> print(m1.dec_amt, m1.units)
0.10 USD
```

But if this is all you can do, it’s not impressive. The next thing to add is the ability to print Money objects in a meaningful and automatic way. Right now, if you print m1, it’s not very useful.

Click here to view code image

```
>>> print(m1)
<__main__.Money object at 0x103cc6f60>
```

## 10.8 DISPLAYING MONEY OBJECTS ("__STR__", "__REPR__")

To determine how a Money object is printed, write a `__str__` method for the class. Here’s a working version of the function, to be added to the class definition.

Click here to view code image

```
def __str__(self):
    s = str(self.dec_amt) + ' ' + self.units
    return s
```

And here’s a sample session that takes advantage of this method.

```
>>> m1 = Money('5.01', 'CAD')
>>> print(m1)
5.01 CAD
```

As you can see, it’s now easy to initialize and display Money objects, with the default type of units being USD, indicating the use of U.S. dollars.

But we also want to print a good canonical representation of the class. This requires a `__repr__` method definition, in addition to `__str__`.

Click here to view code image

```
def __repr__(self):
    s = ('Money(' + str(self.dec_amt) + ', ' +
         self.units + ')')
    return s
```

A class’s `__repr__` function typically differs from a `__str__` function.
