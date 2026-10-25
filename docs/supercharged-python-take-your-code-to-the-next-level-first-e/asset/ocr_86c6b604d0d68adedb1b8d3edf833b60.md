```
else:
    print("Quotient is %s." % quot)
finally:
    print("Execution complete!")

>>> div_me(2, 1)
Quotient is 2.0.
Execution complete!
>>> div_me(2, 0)
Bad division! Text: division by zero
Execution complete!

>> div_me("2", "3")
Execution complete!
Traceback (most recent call last):
  File "<pyshell#21>", line 1, in <module>
    div_me("2", "3")
  File "<pyshell#19>", line 3, in div_me
    quot = x / y
TypeError: unsupported operand type(s) for /: 'str' and 'str'
```