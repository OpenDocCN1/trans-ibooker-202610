```
>>> intro3()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: intro3() missing 2 required positional arguments: 'message' and 'num_times'
>>> intro3('high')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: intro3() missing 1 required positional argument: 'num_times'
```

We also have to be sure to provide values of the proper types. Wrong types won’t stop us from calling the function, but they will cause an error within the function:

```
>>> intro3('high', 'low')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in intro3
TypeError: 'str' object cannot be interpreted as an integer
```

This TypeError arises because intro3 uses a range for loop on variable num_times. If the argument we provide for num_times is not an integer, the range for loop fails.

# Keyword Arguments

It’s possible to override the left-to-right correspondence between arguments and parameters when calling a function. To do that, we use the names of parameters in whatever order we like. An argument that uses the name of a parameter is called a keyword argument. Here’s how it works:

```
>>> def intro3(message, num_times):
...   for i in range(num_times):
...     print(message)
...
>>> intro3(message='high', num_times=3)
high
high
high
>>> intro3(num_times=3, message='high')
high
high
high
```