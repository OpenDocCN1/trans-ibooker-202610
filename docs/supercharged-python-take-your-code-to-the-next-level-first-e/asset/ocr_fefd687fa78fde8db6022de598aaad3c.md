```
>>> n = 9
>>> 'Item 1: %*r, Item 2: %*r' % (n, 'Bob', n,
'Suzanne')
"Item 1:  'Bob', Item 2: ' Suzanne'"
```

You can also create variable-length precision indicators. The
general rule with the format operator (%) is this:

> Where you'd normally put an integer as a formatting code, you can instead place
> an asterisk (*); and for each such asterisk, you must place a corresponding
> integer expression in the argument list.

For example, the following statement formats a number as if
the specifier were `'%8.3f'` :

Click here to view code image

```
>>> '%*.*f' % (8, 3, 3.141592)
' 3.142'
```

# 5.4 THE GLOBAL “FORMAT” FUNCTION

Two closely related features of Python give you even greater
control over specifications. The global `format` function enables
specification of one print field. For example, it provides an easy
way to add commas as thousands place separators.

Click here to view code image