A fill (or padding) character is recognized as such only if
there is an `align` character just after it (<, >, ^, or =).

Click here to view code image

```
print('{:>7}'.format('Tom'))   # Print '   Tom'
print('{:@>7}'.format('Lady'))   # Print '@@@Lady'
print('{:*>7}'.format('Bill'))   # Print '***Bill'
```

In the first of these examples, no _fill_ character is specified,
so a default value of a blank space is used to pad the print field.
In the second and third cases, fill characters of an ampersand
(@) and an asterisk (*) are used.

If we were to instead use < to specify left justification,
padding would be placed on the right (although note that left
justification is the default for strings). So the previous examples
would be revised:

Click here to view code image

```
print('{:<7}'.format('Tom'))   # Print 'Tom   '
print('{:@<7}'.format('Lady'))   # Print 'Lady@@@'
print('{:*<7}'.format('Bill'))   # Print 'Bill***'
```

The next few examples demonstrate the use of ^ to specify
centering of the data; padding appears on either side of the text.

Click here to view code image

```
fss = '{:^10}Jones'
print(fss.format('Tom'))   # Print '   Tom
Jones'
fss = '{:@^10}'
print(fss.format('Lady'))   # Print '@@Lady@@@'
```