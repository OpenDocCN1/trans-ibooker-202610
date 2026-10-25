```
23 % 5    # Result is 3
```

Note that in remainder division, a division is carried out first
and the quotient is thrown away. The result is whatever is left
over after division. So 5 goes into 23 four times but results in a
remainder of 3.

In Python 2.0, the rules are as follows:

+   • Division between two integers is automatically ground division, so
+   the remainder is thrown away:
+   
+   Click here to view code image

```
7 / 2    # Result is 3 (in Python
2.0)
```

+   • To force a floating-point result, convert one of the operands to
+   floating-point format.

```
7 / 2.0    # Result is 3.5
7 / float(2) # Ditto
```

+   • Remember that you can always use modulus division (%) to get the
+   remainder.

Python also supports a **divmod** function that returns
quotient and remainder as a *tuple* (that is, an ordered group) of
two values. For example:

```
quot, rem = divmod(23, 10)
```