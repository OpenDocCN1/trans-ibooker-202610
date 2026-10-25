it's needed. Here's the correct way to write a loop that prints
elements of a list:

Click here to view code image

```
```
a_list = ['Tom', 'Dick', 'Jane']
for s in a_list:
    print(s)
```
```

This prints the following:

```
```
Tom
Dick
Jane
```
```

This approach is more natural and efficient than relying on
indexing, which would be inefficient and slower.

```
```
for i in range(len(a_list)):
    print(a_list[i])
```
```

But what if you want to list the items next to numbers? You
can do that by using index numbers (plus 1, if you want the
indexing to be 1-based), but a better technique is to use the
enumerate function.

![](bbox=[0.1465, 0.7516, 0.2539, 0.8281])

```
```
enumerate(iter, start=0)
```
```