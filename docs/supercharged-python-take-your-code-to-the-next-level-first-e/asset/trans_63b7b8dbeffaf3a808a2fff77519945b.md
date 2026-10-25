


>>> class Dog:
    pass

>>> d = Dog()
>>> setattr(d, 'breed', 'Great Dane')
>>> getattr(d, 'breed')
'Great Dane'

>>> 0.6 + 0.3 + 0.1     # Should produce 1.0
0.9999999999999999
>>> (0.6 + 0.3 + 0.1) / 2   # Should produce 0.5
0.49999999999999994
>>> from decimal import Decimal
>>> my_dec = Decimal()
>>> print(my_dec)
0
>>> d = Decimal('0.1')
>>> print(d + d + d)
0.3
>>> d = Decimal(0.1)
>>> print(d)
0.10000000000000000555111512312578...
>>> d = Decimal('0.1')
>>> print(d + d + d)
0.3
>>> print(0.1 + 0.1 + 0.1)
0.30000000000000004

>>> print(0.1 + 0.1 + 0.1 - 0.3)
5.551115123125783e-17
>>> d1, d3 = Decimal('0.1'), Decimal('0.3')
>>> print(d1 + d1 + d1 - d3)
0.0
>>> d1, d3 = Decimal('0.10'), Decimal('0.30')
>>> d1 + d3
Decimal('0.40')
>>> d1, d2 = Decimal('0.50'), Decimal('0.50')
>>> print(d1 + d2)
1.00
>>> d1, d3 = Decimal('0.020'), Decimal('0.030')
>>> print(d1 * d3)
0.000600
>>> d2 = d      # Save old version of d in d2.
>>> d = d.normalize()  # Now d is normalized.
>>> d2 == d
True
>>> d2 is d
False
>>> d = Decimal('15.0')
>>> d.as_tuple()
DecimalTuple(sign=0, digits=(1, 5, 0), exponent=-1)

>>> d = Decimal((0, (3, 1, 4), -2))
>>> print(d)
3.14

(sign_bit, (digit1, digit2, digit3...), exponent)

>>> decimal.getcontext()
Context(prec=28, rounding=ROUND_HALF_EVEN,
Emin=-999999, Emax=999999, capitals=1, clamp=0,
flags=[DivisionByZero, Inexact, FloatOperation,
Rounded], traps=[InvalidOperation, DivisionByZero,
Overflow])

money_amount = Decimal('1.99')

from decimal import Decimal

total = Decimal('0.00')
while True:
    s = input('Enter amount in dollars and cents (#.##): ')
    if not s:
        break
    d = Decimal(s)
    d = round(d, 2)
    total += d

print('The sum of all these amounts is:', total)

Enter amount in dollars and cents (#.##): 1
Enter amount in dollars and cents (#.##): 3.00
Enter amount in dollars and cents (#.##): 4.50
Enter amount in dollars and cents (#.##): 33.003
Enter amount in dollars and cents (#.##): 12.404
Enter amount in dollars and cents (#.##):
The sum of all these amounts is: 53.90
from decimal import Decimal
class Money():
    def __init__(self, v = '0', units = 'USD'):
        self.dec_amt = Decimal(v)
        self.units = units
>>> m1 = Money('0.10')
>>> print(m1.dec_amt, m1.units)
0.10 USD
>>> print(m1)
<__main__.Money object at 0x103cc6f60>
def __str__(self):
    s = str(self.dec_amt) + ' ' + self.units
    return s

def __repr__(self):
    s = ('Money(' + str(self.dec_amt) + ' ' +
        self.units + '))'
    return s

def __add__(self, other):
    d = self.dec_amt + other.dec_amt
    return Money(str(d))

def __add__(self, other):
    d = self.dec_amt + other.dec_amt
    return Money(str(d), self.units)

exch_dict = {
    'USDCAD': Decimal('0.75'), 'USDEUR': Decimal('1.16'),
    'CADUSD': Decimal('1.33'), 'CADEUR': Decimal('1.54'),
    'EURUSD': Decimal('0.86'), 'EURCAD': Decimal('0.65')
}

def __add__(self, other):
    ''''Money add function.
    Supports two Money objects added together; if
    the second has a different currency unit, then
    exchange rate must be applied before adding the
    two amounts together. Apply rounding of 2.
    '''
    if self.units != other.units:
        r = Money.exch_dict[self.units + other.units]
        m1 = self.dec_amt
        m2 = other.dec_amt * r
        m = Money(m1 + m2, self.units)
    else:
        m = Money(self.dec_amt + other.dec_amt,
                  self.units)
    m.dec_amt = round(m.dec_amt, 2)
    return m

r = Money.exch_dict[self.units + other.units]
m1 = self.dec_amt
m2 = other.dec_amt * r
m = Money(m1 + m2, self.units)

m.dec_amt = round(m.dec_amt, 2)

>>> us_m = Money('1', 'USD')
>>> ca_m = Money('1', 'CAD')
>>> print(us_m + ca_m)
1.75 USD

from decimal import Decimal

class Money():
    """Money Class.
    Stores both a Decimal amount and currency units. When
    objects are added, exchange rate will be applied if
    the currency units differ.
    """

exch_dict = {
    'USDCAD': Decimal('0.75'), 'USDEUR': Decimal('1.16'),
    'CADUSD': Decimal('1.33'), 'CADEUR': Decimal('1.54'),
    'EURUSD': Decimal('0.86'), 'EURCAD': Decimal('0.65')
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
    Supports two Money objects added together; if
    the second has a different currency unit, then
    exchange rate (r) is applied before adding the
    two amounts together. Apply rounding of 2.
    """
    if self.units != other.units:
        r = Money.exch_dict[self.units + other.units]
        m1 = self.dec_amt
        m2 = other.dec_amt * r
        m = Money(m1 + m2, self.units)
    else:
        m = Money(self.dec_amt + other.dec_amt,
                  self.units)
    m.dec_amt = round(m.dec_amt, 2)
    return m

from decimal import Decimal

# Place Money class definition here or import it.
def money_calc():
    '''Money addition calculator.
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

Enter money value: 1.05
Enter money value: 2.00 CAD
Enter money value: 1.5 EUR
Enter money value: 1.00
Enter money value: 2.5 CAD
Enter money value:
Total is 7.16 USD

Enter money value: 1.50 CAD
Enter money value: 1.75 CAD
Enter money value: 2.00 USD
Enter money value: 1.00 USD
Enter money value:
Total is 7.24 CAD

# This causes an ERROR!
def __init__(self, v='0', units=Money.default_curr):

def __init__(self, v='0', units=''):
    self.dec_amt = Decimal(v)
    if not units:
        self.units = Money.default_curr
    else:
        self.units = units

if n == 0:     # If this is first entry...
    amt = m      # Create amt!
    Money.default_curr = m.units

class Money(Thingie):
    def __init__(self, v, units='USD'):
        super().__init__(v)
        self.units = units

from decimal import Decimal

class Money(Decimal):
    def __new__(cls, v, units='USD'):
        return super(Money, cls).__new__(cls, v)

    def __init__(self, v, units='USD'):
        self.units = units

m = Money('0.11', 'USD')
print(m, m.units)

def __new__(cls, v, units='USD'):
    return super(Money, cls).__new__(cls, v)

class MyClass(MySuperClass):
    def __new__(cls, d, other_data):
        return super(MyClass, cls).__new__(cls, d)

class MyClass(MySuperClass):
    def __new__(cls, d, other_data):
        return super(MyClass, cls).__new__(cls, d)

def __init__(self, v, units='USD'):
    self.units = units

def __str__(self):
    return super().__str__() + ' ' + self.units

from fractions import Fraction

fr1 = Fraction(1, 2)
fr2 = Fraction(2, 4)
fr3 = Fraction(100/200)
print('The fractions are %s, %s, & %s.' % (fr1, fr2, fr3))

The fractions are 1/2, 1/2, & 1/2.

>>> if fr1 == fr2 and fr2 == fr3:
...     print('They are all equal!')

They are all equal!

>>> if fr1 == fr2 == fr3:
...     print('They are all equal!')

>>> fr2 = Fraction(0.01)
>>> print(fr2)
5764607523034235/576460752303423488

>>> fr2 = Fraction('0.01')
>>> print(fr2)
1/100

>>> fr1 = Fraction(1, 100)
>>> print(fr1, 'times 50 =', fr1 * 50)
1/100 times 50 = 1/2
>>> fr1 = Fraction('100/300')
>>> print('numerator is', fr1.numerator)
numerator is 1
>>> print('denominator is', fr1.denominator)
denominator is 3
from fractions import Fraction
total = Fraction('0')
while True:
    s = input('Enter fraction (press ENTER to quit): ')
    s = s.replace(' ', '') # Elim. spaces, just in case.
    if not s:
        break
    total += Fraction(s)
print('The total is %s.' % total)


