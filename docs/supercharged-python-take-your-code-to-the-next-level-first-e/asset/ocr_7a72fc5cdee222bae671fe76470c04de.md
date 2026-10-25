```
print('{:.2%} of {:.2%} of 40...'.format(0.231,
0.5))
```

This prints

```
23.10% of 50.00% of 40...
```

As with fixed-point format, if you want to print percentages so that they line up nicely in a table, then specify both _width_ and _precision_ specifiers.

# ## 5.8.12 Binary Radix Example

The `format` method provides the tools to print numeric output in binary, octal, or hex radix. You can combine that capability with `int` conversions to create a binary calculator that uses both binary input and output—that is to say, its input and output features strings of 1’s and 0’s.

This next example performs binary addition, displaying results in both decimal and binary.

Click here to view code image

```
def calc_binary():
    print('Enter values in binary only!')
    b1 = int(input('Enter b1:'), 2)
    b2 = int(input('Enter b2:'), 2)
    print('Total is: {:#b}'.format(b1 + b2))
    print('{} + {} = {}'.format(b1, b2, b1 + b2))
```

Here’s a sample session with user input in bold.