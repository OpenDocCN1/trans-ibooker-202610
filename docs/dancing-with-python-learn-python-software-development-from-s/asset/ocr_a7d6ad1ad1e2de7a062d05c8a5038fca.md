# Exercise 10.1

In the graph of *timeit* times for 2ⁿ with 60 ≤ n ≤ 65 in Figure 10.1, something strange happens when n = 64. Explain why we see the large increase in execution time. (Hint: see section 5.2.)

![](bbox=[0.36, 0.22, 0.71, 0.42])

You can evaluate more code than can easily fit within the `stmt` string by defining a function.

```
def f(n):
    assert n > 6

    the_list = []
    for x in range(n):
        the_list.append(x)
    for _ in range(10):
        the_list += the_list
        the_list = the_list[5:]
```

*f* is an inefficient nonsense function that creates a list, appends it to itself, slices it, and repeats ten times. Use the `globals` keyword argument to allow *timeit* to use your local definitions.

```
count = 1000
timeit.timeit(stmt="f(100)", number=count, globals=globals())
```

0.9215112999999997