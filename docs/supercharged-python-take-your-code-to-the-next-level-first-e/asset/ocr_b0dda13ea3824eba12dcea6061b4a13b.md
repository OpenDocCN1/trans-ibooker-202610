This example prints

```
22.70
3.14
555.50
29.00
1010.01
```

# ## 5.8.7 “Precision” Used with Strings (Truncation)

When used with strings, the _precision_ specifier potentially causes truncation. If the length of the string to be printed is greater than the _precision_, the text is truncated. Here’s an example:

Click here to view code image

```
print('{:.5}'.format('Superannuated.')) # Prints
'Super'
print('{:.5}'.format('Excellent!')) # Prints
'Excel'
print('{:.5}'.format('Sam')) # Prints
'Sam'
```

In these examples, if the string to be printed is shorter than the _precision_, there is no effect. But the next examples use a combination of _fill_ character, _alignment_, _width_, and _precision_.

```
fss = '{:*<6.6}'
```