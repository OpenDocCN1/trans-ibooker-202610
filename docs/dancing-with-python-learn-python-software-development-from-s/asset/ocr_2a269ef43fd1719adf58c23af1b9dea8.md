# 10.3.6 Don’t do extra work in loops

Consider the function *looper_1*, which accepts a positive integer n. Inside *looper_1*, we execute a loop n times. Inside the loop, we compute 2¹⁰⁰ and then add it to the loop variable j.

```
```python
def looper_1(n):
    for j in range(n):
        a = 2**100
        b = a + j


t_1 = timeit.timeit(stmt="looper_1(100000)",
                    number=3, globals=globals())
print(f"Execution time = {t_1:.10f}")
```
Execution time = 0.0875948000
```

Python evaluates the assignment `a = 2**100` within every iteration of the loop. Nothing happens inside the loop that could change the value! **Move statements and expression evaluations that are not affected by the loop out of the loop.** When we move the assignment before the loop, our function speeds up considerably.

```
```python
def looper_2(n):
    a = 2**100
    for j in range(n):
        b = a + j


t_2 = timeit.timeit(stmt="looper_2(100000)",
                    number=3, globals=globals())
print(f"Execution time = {t_2:.10f}")
```
Execution time = 0.0128910000
```

We now add two numbers inside the loop, but we never use the sum! If we called a function or method that has a side effect, this might make sense, but not in this case. **Don’t execute useless loops unless you are showing examples of useless loops!**

The time to do nothing at all is much less than executing the useless loop.

```
```python
def looper_3(n):
    pass
```
```