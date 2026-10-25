```
>>> from math import *
>>> a = 25
>>> dict_1 = {'tan': tan, 'radians': radians, 'a': a }
>>> eval('1000 * tan(radians(a))', dict_1)
176.326980708465

eval('a * a + 100', {}, locals())

def is_neg(x):
    return x < 0

my_list = [10, -1, 20, -3, -2.5, 30]
print(list(filter(is_neg, my_list)))

n = 1
yyy = float(n)      # Assign 1.0 to yyy.
amt = float('-23E01') # Assign -23E01 to amt.

amt = float()       # Assign 0.0 to amt.

>>> frozenset((1, 2, 2, 3, 3, 4, 1))
frozenset({1, 2, 3, 4})

my_name = input('Enter your name, please: ')
my_age = int(input('Enter your age, please: '))
```