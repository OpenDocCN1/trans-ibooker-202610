```
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
```