```
a equals 10, b equals 50, c equals 100.
```

You can also use the positioning techniques to repeat values
in your output. Here’s an example:

```
print('{0}, {0}, {1}, {1}'.format(100, 200))
```

This example prints

```
100, 100, 200, 200
```

Position ordering has an advanced feature that’s occasionally
useful for certain applications. By changing the format string
itself, you can change which parts of an argument get selected
for inclusion in the print string.

For example, {0[0]:} means “Select the first element of the
first argument.” {0[1]:} means “Select the second element of
the first argument.” And so on.

Here’s a more complete example. Remember that zero-based
indexing is used, as usual.

```
>>> a_list = [100, 200, 300]
>>> '{0[1]:}, {0[2]:}'.format(a_list)
'200, 300'
```

This technology works with named positions as well.