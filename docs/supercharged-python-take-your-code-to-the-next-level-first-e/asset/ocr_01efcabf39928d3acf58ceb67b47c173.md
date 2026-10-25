position up to but not including the `end` position. These arguments are zero-based indexes, as usual.

If either or both of the arguments (`begin`, `end`) are out of range, the `count` method does not raise an exception but works on as many characters as it can.

Similar rules apply to the **find** method. A simple call to this method finds the first occurrence of the substring argument and returns the nonnegative index of that instance; it returns –1 if the substring isn’t found.

Click here to view code image

```
frank_str = 'doo be doo be doo...'

print(frank_str.find('doo'))   # Print 0
print(frank_str.find('doob'))  # Print -1
```

If you want to find the positions of all occurrences of a substring, you can call the **find** method in a loop, as in the following example.

Click here to view code image

```
frank_str = 'doo be doo be doo...'
n = -1
while True:
    n = frank_str.find('doo', n + 1)
    if n == -1:
        break
    print(n, end=' ')
```

This example prints every index at which an instance of 'doo' can be found.