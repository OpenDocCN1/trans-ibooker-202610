The ** syntax indicates that the kwargs parameter can handle all keyword arguments passed in a function call. They’ll be stored as key-value pairs in a dictionary assigned to the kwargs parameter. As new chemical elements are discovered, you’d need to update the function’s code but not its def statement, because all keyword arguments are put into kwargs:

```
① >>> def formMolecules(**kwargs):
② ...     if len(kwargs) == 1 and kwargs.get('unobtanium') == 12:
...         return 'aether'
...     # (rest of code for the function goes here)
...
>>> formMolecules(unobtanium=12)
'aether'
```

As you can see, the def statement ① is the same as before, and only the function’s code ② needed updating. When you use the ** syntax, the def statement and the function calls become much simpler to write and still produce readable code.

# Using * and ** to Create Wrapper Functions

A common use case for the * and ** syntax in def statements is to create wrapper functions, which pass on arguments to another function and return that function’s return value. You can use the * and ** syntax to forward any and all arguments to the wrapped function. For example, we can create a printLowercase() function that wraps the built-in print() function. It relies on print() to do the real work but converts the string arguments to lowercase first:

```
① >>> def printLower(*args, **kwargs):
② ...     args = list(args)
...     for i, value in enumerate(args):
...         args[i] = str(value).lower()
③ ...     return print(*args, **kwargs)
...
>>> name = 'Albert'
>>> printLower('Hello, ', name)
hello, albert
>>> printLower('DOG', 'CAT', 'MOOSE', sep=', ')
dog, cat, moose
```

The printLower() function ① uses the * syntax to accept a varying number of positional arguments in a tuple assigned to the args parameter, whereas the ** syntax assigns any keyword arguments to a dictionary in the kwargs parameter. If a function uses *args and **kwargs together, the *args parameter must come before the **kwargs parameter. We pass these on to the wrapped print() function, but first our function modifies some of the arguments, so we create a list form of the args tuple ②.

After changing the strings in args to lowercase, we pass the items in args and key-value pairs in kwargs as separate arguments to print() using the *