long as when you get around to executing `funcA`, `funcB` is also defined.

# 1.14 PYTHON STRINGS

Python has a text string class, `str`, which enables you to use characters of printable text. The class has many built-in capabilities. If you want to get a list of them, type the following into IDLE:

```
>>> help(str)
```

You can specify Python strings using a variety of quotation marks. The only rule is that they must match. Internally, the quotation marks are not stored as part of the string itself. This is a coding issue; what’s the easiest way to represent certain strings?

Click here to view code image

```
s1 = 'This is a string.'
s2 = "This is also a string."
s3 = '''This is a special literal quotation string.'''
```

The last form—using three consecutive quotation marks to delimit the string—creates a literal quotation string. You can also repeat three double quotation marks to achieve the same effect.