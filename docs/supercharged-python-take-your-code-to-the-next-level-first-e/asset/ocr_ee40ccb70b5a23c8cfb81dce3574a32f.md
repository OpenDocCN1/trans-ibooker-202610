to provide it yourself. For example:

Click here to view code image

```
my_name = input('Enter your name, please: ')
my_age = int(input('Enter your age, please: '))
```

# # INT(X, BASE=10)

Integer conversion function. This function takes a numeric value
or a string containing a valid integer representation and then
returns an actual integer value. The *base* argument, if included,
determines how to interpret the string of digits given as the
argument; by default, decimal radix (base 10) is assumed, but
another radix, such as 2 (binary), 8 (octal), or 16 (hexadecimal)
may also be used. For example:

```
>>> int('1000', 16)
4096
```

For the first argument, you can specify an object of another
numeric type, such as *float*. The *int* conversion truncates the
fractional portion. For example:

```
>>> int(3.99), int(-3.99)
(3, -3)
```

To round to the nearest integer (or any significant digit) using
a different rounding scheme, see the **round** function.

# # INT()