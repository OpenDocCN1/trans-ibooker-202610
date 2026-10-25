Displays a number as a percentage: Multiply by 100 and then add a percent sign (%).

The next five sections illustrate specific uses of the _type_ specifier.

# ## 5.8.9 Displaying in Binary Radix

To print an integer in binary radix (base 2), use the `b` specifier. The result is a series of 1’s and 0’s. For example, the following statement displays 5, 6, and 16 in binary radix:

Click here to view code image

```
```
print('{:b} {:b} {:b}'.format(5, 6, 16))
```
```

This prints the following:

```
```
101  110  10000
```
```

You can optionally use the # specifier to automatically put in radix prefixes, such as **0b** for binary. This formatting character is placed after the _fill_, _align_, and _sign_ characters if they appear but before the _type_ specifier. (It also precedes _width_ and _precision_.) Here’s an example:

```
```
print('{:#b}'.format(7))
```
```

This prints