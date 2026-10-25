```
my_list = [10, -1, 20, -3, -2.5, 30]
print(list(filter(is_neg, my_list)))
```

The result produced by **filter** is a kind of iterable. You can convert it to a list if desired and print it; so the last of the lines just shown prints the following:

```
[-1, -3, -2.5]
```

The _function_ argument may optionally be **None**. In that case, the items included in the result are those that evaluate to **True** when the **bool** conversion is applied. (Generally, any non-zero value or non-empty collection evaluates as true.)

# # FLOAT([X])

Floating-point conversion. If the optional argument _x_ is specified, the result of converting the value to floating point is returned. Types that may be successfully converted to floating point include numeric types (such as integers) and strings containing a valid representation of a floating-point number.

Strings can include numeric representations such as `'4.105'`, `'-23E01'`, and `'10.5e-5'`. Positive and negative infinity can also be represented as `'Infinity'`, `'-Infinity'`, `'inf'`, and `'-inf'`.

> Click here to view code image

```
n = 1
yyy = float(n)    # Assign 1.0 to yyy.
amt = float('-23E01')  # Assign -23E01 to amt.
```