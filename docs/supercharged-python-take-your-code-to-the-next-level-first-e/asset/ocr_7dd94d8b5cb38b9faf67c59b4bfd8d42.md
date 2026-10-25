looking at the coding techniques, remember that a **Decimal**
object is created most naturally and efficiently by initialization
from a string.

Click here to view code image

```
money_amount = Decimal('1.99')
```

Here’s the full application code. The action is to prompt the
user for a number and—if the user did not enter an empty string
—add the specified string of digits as a **Decimal** object. If the
user presses Enter without typing input, the program breaks,
stops, and prints the total.

Click here to view code image

```
from decimal import Decimal
total = Decimal('0.00')
while True:
    s = input('Enter amount in dollars and cents
(##.##): ')
    if not s:
        break
    d = Decimal(s)
    d = round(d, 2)
total += d
print('The sum of all these amounts is:', total)
```

The program could have used one of two different strategies
in handling numbers with fractions smaller than 0.01. You
could add all the smaller fractions together in the running total
and do the rounding at the end. Instead, the approach here is to