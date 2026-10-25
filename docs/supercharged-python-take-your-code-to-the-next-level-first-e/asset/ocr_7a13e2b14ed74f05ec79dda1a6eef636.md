You can make this a complete program by running it from
within IDLE or by adding these module-level lines below it:

```
n = int(input('Input n: '))
pr_fibo(n)
```

New features, by the way, are contained in these lines of the
function definition:

```
a, b = 1, 0
a, b = a + b, a
```

These two statements are examples of _tuple assignment_,
which we return to in later chapters. In essence, it enables a list
of values to be used as inputs, and a list of variables to be used
as outputs, without one assignment interfering with the other.
These assignments could have been written as

```
a = 1
b = 0
...
temp = a
a = a + b
b = temp
```

Simply put, `a` and `b` are initialized to 1 and 0, respectively.
Then, later, `a` is set to the total `a + b`, while simultaneously, `b`
is set to the old value of `a`.

The second app (try it yourself!) is a complete computer
game. It secretly selects a random number between 1 and 50
and then requires you, the player, to try to find the answer