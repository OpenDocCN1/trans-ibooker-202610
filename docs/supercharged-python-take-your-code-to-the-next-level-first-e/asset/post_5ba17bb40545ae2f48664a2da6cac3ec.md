

## 10.9 OTHER MONETARY OPERATIONS

So far, all we can do with the Money class is create objects and print them. But to be useful, the class should support, at minimum, addition operations (+) between Money objects.

If we ignore the role of units for the moment, the `__add__` function is easy to write. This version assumes that you only want to add Money objects to other objects of the same class.

Click here to view code image

```
def __add__(self, other):
    d = self.dec_amt + other.dec_amt
    return Money(str(d))
```

We can expand on this function definition by presupposing that whatever units are used by the left operand should be used by the result. Implementing that approach gives us a second version of the function definition. In the following code, the item to be added is shown in bold for the sake of illustration.

```
def __add__(self, other):
    d = self.dec_amt + other.dec_amt
    return Money(str(d), self.units)
```

Even more interesting, and useful, would be to convert the units in use on the right side to match those on the left after first multiplying by the currency-exchange rate. Although such numbers are changed on a daily basis, the program can be revised as needed to accommodate such changes. One way to do that would be to read in the currency-exchange rates from a file that is updated as needed.

To keep things simple for now, we’re going to pick some exchange rates—the current ones as of this writing—and just assume that the program can be revised as needed. Because there are six possible conversions (from our three supported currencies), the best way to do that is with a dictionary. The key value is the result of concatenating two currency symbols. The value field shows what number the second currency must be multiplied by to produce the first.

```
exch_dict = {
    'USDCAD': Decimal('0.75'), 'USDEUR': Decimal('1.16'),
    'CADUSD': Decimal('1.33'), 'CADEUR': Decimal('1.54'),
    'EURUSD': Decimal('0.86'), 'EURCAD': Decimal('0.65')
}
```

So, for example, the value for the `USDCAD` key is 0.75, meaning that a Canadian-dollar figure is multiplied by 0.75 to get its equivalent in U.S. dollars. Now the final version of the function can apply the currency-exchange rate whenever two different currencies are added together.

The dictionary stores the exchange rates as Decimal objects, thereby making the subsequent arithmetic easier to perform.

Click here to view code image

```
def __add__(self, other):
    ''''Money add function.
    Supports two Money objects added together;

    if
    the second has a different currency unit,
    then
    exchange rate must be applied before adding
    the
    two amounts together. Apply rounding of 2.
    '''
    if self.units != other.units:
        r = Money.exch_dict[self.units +
                            other.units]
        m1 = self.dec_amt
        m2 = other.dec_amt * r
        m = Money(m1 + m2, self.units)
    else:
        m = Money(self.dec_amt + other.dec_amt,
                  self.units)
    m.dec_amt = round(m.dec_amt, 2)
    return m
```

Let’s step through how this function works. As the comments (or rather, the doc string) point out, an exchange rate may be applied before the amounts are added together, assuming the units are not the same (such as U.S. dollars versus Canadian dollars). Although exchange rates are expressed as floating point in most locations, we store those rates here as **Decimal** objects, so that fewer conversions need to be done.

Click here to view code image

```
r = Money.exch_dict[self.units +
other.units]
m1 = self.dec_amt
m2 = other.dec_amt * r
m = Money(m1 + m2, self.units)
```

In either case—whether an exchange rate is applied or whether it isn’t—we also want a rounding factor of 2 to be applied, so that the money is always expressed with two digits of precision past the decimal point.

Click here to view code image

```
m.dec_amt = round(m.dec_amt, 2)
```

The new Money object, m, is finally returned by the `__add__` function.

With this function definition in place, along with the `exch_dict`, which can be made a class variable of Money, we can now add different currencies together—as long as they are one of the three currencies recognized by this program (although that list can be expanded as much as you want). So, for example, we can add a U.S. dollar to a Canadian dollar and get a meaningful result.

Click here to view code image

```
>>> us_m = Money('1', 'USD')
>>> ca_m = Money('1', 'CAD')
>>> print(us_m + ca_m)
1.75 USD
```

> Note
> This function definition works correctly, of course, as long as the three supported currencies are used. If units other than USD, CAD, or EUR are used, a **KeyError** exception results whenever mixed currencies are added.

Putting it all together, here’s the complete Money class. It’s not really complete, of course, because there are many operations we still could add, such as subtraction and multiplication by integers.

Click here to view code image

```
from decimal import Decimal

class Money():
    ''''Money Class.
    Stores both a Decimal amount and currency units.
    When
    objects are added, exchange rate will be applied
    if
    the currency units differ.
    '''
    exch_dict = {
        'USDCAD': Decimal('0.75'), 'USDEUR':
        Decimal('1.16'),
        'CADUSD': Decimal('1.33'), 'CADEUR':
        Decimal('1.54'),
        'EURUSD': Decimal('0.86'), 'EURCAD':
        Decimal('0.65')
    }
    def __init__(self, v = '0', units = 'USD'):
        self.dec_amt = Decimal(v)
        self.units = units
    def __str__(self):
        s = str(self.dec_amt) + ' ' + self.units
        return s
    def __repr__(self):
        s = ('Money(' + str(self.dec_amt) + ' ' +
        str(self.units) + ')')
        return s
    def __add__(self, other):
        """Money add function.
        Supports two Money objects added together;
        if
        the second has a different currency unit,
        then
        exchange rate (r) is applied before adding
        the
        two amounts together. Apply rounding of 2.
        """
        if self.units != other.units:
            r = Money.exch_dict[self.units +
            other.units]
            ml = self.dec_amt
            m2 = other.dec_amt * r
            m = Money(ml + m2, self.units)
        else:
            m = Money(self.dec_amt + other.dec_amt,
            self.units)
        m.dec_amt = round(m.dec_amt, 2)
        return m
```

That’s the (for now) complete class definition—although, as mentioned, there are many operations you might want to add.

## 10.10 DEMO: A MONEY CALCULATOR

With the completed Money class definition in place, it’s now possible to write a calculator application that can add up any number of money amounts in the three different currencies we currently support and give the answer in a common denomination.

Most of the code is easy to write, but user input must be broken down into numeric and units portions, something that does complicate the coding a little. Fortunately, much of the work is done by the **split** method of the **str** type.

Click here to view code image

```
from decimal import Decimal

# Place Money class definition here or import it.
def money_calc():
    ''''Money addition calculator.
    Prompt for a series of Money objects until empty
    string is entered; then print results of the
    running total.
    '''

    n = 0
    while True:
        s = input('Enter money value: ')
        s = s.strip()
        if not s:
            break
        a_list = s.split() # Split into amt, units.
        d = a_list[0]
        if len(a_list) > 1:
            m = Money(d, a_list[1])
        else:
            m = Money(d)
        if n == 0:
            amt = m
        else:
            amt += m
        n += 1
    print('Total is', amt)
money_calc()
```

The final line of this code, which executes the function, makes it into a complete program.

There’s a subtlety to this function. It’s desirable to let the first choice of currency (the units entered for the first line) determine the currency used for the final answer. This gives the user control of the results. Perhaps you want the results to be expressed in Canadian dollars or Euros, for example; you simply need to make sure the first entry uses those units.

The problem is, we’re keeping a running total, and the usual way of keeping a running total is to start with an initial zero value. Here’s an example:

```
amt = Money('0')
```

The problem here is that right now, USD is the default value for units; therefore, this initial choice, through the logic of the program, would predetermine that every result of this program is expressed in U.S. dollars.

