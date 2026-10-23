And here’s a while loop counting down from 4 to 0:

```
>>> num = 4
① >>> while num >= 0:
...   print(num)
...   num = num - 1
...
4
3
2
1
0
```

Notice that I’ve used >= here rather than > ①. This way, the while loop runs when num refers to 0, as desired.

# CONCEPT CHECK

What is the output of the following code?

```
n = 3
while n > 0:
    if n == 5:
        n = -100
    print(n)
    n = n + 1
```

A.

3
4

B.

3
4
5

C.