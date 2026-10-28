Since the loop and the action are so small, I put them both on the same line. I can define a function and do everything in the nested list comprehension.

```
def one_or_zero(x, y):
    if x == y:
        return 1
    return 0

[[one_or_zero(i, j) for i in range(4)] for j in range(4)]
```

[[1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0], [0, 0, 0, 1]]

I can be more Pythonic and improve the function:

```
def one_or_zero(x, y): return 1 if x == y else 0

[[one_or_zero(i, j) for i in range(4)] for j in range(4)]
```

[[1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0], [0, 0, 0, 1]]

We haven’t seen this before! Python allows you to put a conditional in the middle of an expression. Finally, I skip the function entirely.

```
[[1 if i == j else 0 for i in range(4)] for j in range(4)]
```

[[1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0], [0, 0, 0, 1]]

Using a conditional in this way is an additional technique that allows you to write concise and readable code.

# Exercise 3.23

What is happening in the following comprehension?

```
[f"i + j = {i+j} for {i = } and {j =}"
    for i in range(2) for j in range(3)]
```

['i + j = 0 for i = 0 and j =0',
'i + j = 1 for i = 0 and j =1',
'i + j = 2 for i = 0 and j =2',
'i + j = 1 for i = 1 and j =0',
'i + j = 2 for i = 1 and j =1',
'i + j = 3 for i = 1 and j =2']

Is this nested or are the for loops running in parallel?