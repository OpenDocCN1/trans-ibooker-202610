```
t_3 = timeit.timeit(stmt="looper_3(10000)",
                    number=3, globals=globals())
print(f"Execution time = {t_3:.10f}")
```

Execution time = 0.0000008000

Python has a philosophy that coders should be able to do useless and inefficient things if they want to do so. It’s your responsibility to do necessary manual optimizations. If you use a static code checker like pylint or flake8, you can see warnings and suggestions for code improvements. If I put the source for looper_1 in the file looper_1.py, I will see the following when I run flake8 looper_1.py from the operating system command line:

```
looper_1.py:4:9: F841 local variable 'b' is assigned to but never used
```

pylint gives a similar message, together with disparaging comments about how I named my variables. These and other static checkers have many options and allow you to specify which diagnostics they do and the messages they display. Incidentally, the static checking process is called “linting.”

# 10.3.7 Don’t sort until you have to

In section 7.3, we looked at how to represent the terms within our UniPoly polynomial objects. We concluded that there was no need to keep them in a list in sorted order, and we could put them in the right sequence when we printed them.

The general lesson here is that there may be extra implementation work you do along the way as you write your code that is not necessary or optimal. We decided that a dictionary would be a much faster way to represent the polynomial data.

Yes, get your code working, but pause occasionally and ask yourself if you are approaching the problem the correct way. Don’t be afraid to experiment and use functions like timeit to understand if performance is getting better. Is performance speed your primary consideration? Perhaps a better, more elegant, and slightly slower design is your best coding solution.

# Exercise 10.6

What did Knuth mean in his quote at the start of this section?