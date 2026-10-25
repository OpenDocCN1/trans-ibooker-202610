functions produce new lists.
These functions work on tuples and strings as well as lists,
but the **sorted** function always produces a list. Here's an
example:

![](bbox=[0.181, 0.219, 0.416, 0.234])

```
```
a_tup = (30, 55, 15, 45)
print(sorted(a_tup))   # Print [15, 30, 45, 55]
```
```

The **reversed** function is unusual because it produces an
iterable but not a collection. In simple terms, this means you
need a **for** loop to print it or else use a **list** or **tuple**
conversion. Here's an example:

```
```
a_tup = (1, 3, 5, 0)
for i in reversed(a_tup):
    print(i, end=' ')
```
```

This prints

```
```
0 5 3 1
```
```

Alternatively, you can use the following:

```
```
print(tuple(reversed(a_tup)))
```
```

This produces

```
```
(0, 5, 3, 1)
```
```