```
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
```