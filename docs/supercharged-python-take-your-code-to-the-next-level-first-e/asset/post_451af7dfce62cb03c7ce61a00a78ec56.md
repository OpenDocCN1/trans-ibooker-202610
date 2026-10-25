

## HEX(N)

Hexadecimal conversion. Returns a string containing a hexadecimal-radix (base 16) representation of integer _n_, including the prefix, `0x`. Inputs other than integer cause Python to raise a `TypeError` exception.

For example:

```
>>> hex(23)
'0x17'
```

## ID(OBJ)

Returns a unique identifier for the object, `obj`. If two variables (`obj1` and `obj2`) have the same identifier, then the expression `obj1 is obj2` returns `True`—meaning that they refer to the same object in memory. (Note: Do not confuse this with test for equality, `==`, which is a less restrictive condition.)

## INPUT([PROMPT_STR])

Input function. Returns a string input by the user after waiting for the user to input zero or more characters and press Enter. The _prompt_str_, if specified, prints a string for prompting the user, such as “Enter name here: ”. No extra space is automatically printed after this prompt string, so you may want to provide it yourself. For example:

Click here to view code image

```
my_name = input('Enter your name, please: ')
my_age = int(input('Enter your age, please: '))
```

## INT(X, BASE=10)

Integer conversion function. This function takes a numeric value or a string containing a valid integer representation and then returns an actual integer value. The *base* argument, if included, determines how to interpret the string of digits given as the argument; by default, decimal radix (base 10) is assumed, but another radix, such as 2 (binary), 8 (octal), or 16 (hexadecimal) may also be used. For example:

```
>>> int('1000', 16)
4096
```

For the first argument, you can specify an object of another numeric type, such as *float*. The *int* conversion truncates the fractional portion. For example:

```
>>> int(3.99), int(-3.99)
(3, -3)
```

To round to the nearest integer (or any significant digit) using a different rounding scheme, see the **round** function.

## INT()

The `int` conversion function for an empty argument list. This version of `int`, with no argument specified, returns the integer value 0.

_See also_ the earlier entry for `int`.

## ISINSTANCE(OBJ, CLASS)

Returns **True** if object _obj_ is an instance of the specified _class_ or any type derived from that class. It’s often recommended that you use this function instead of the **type** function. (The second argument can also be a tuple of types. In that case, this function returns **True** if the object is an instance of any of the specified classes.)

## ISSUBCLASS(CLASS1, CLASS2)

Returns **True** if _class1_ is a subclass of _class2_ or if the two arguments are the same class. A **TypeError** exception is raised if _class1_ is not a class. As with `isinstance`, the second argument can also be a tuple of types.

For example:

Click here to view code image

```
>>> class Floopy(int):
 pass

>>> f = Floopy()
>>> issubclass(Floopy, int)
True
>>> issubclass(int, Floopy)
False
>>> issubclass(int, int)
True
>>> issubclass(f, int) # TypeError: f is not a class
```

## ITER(OBJ)

Iteration function. This function call assumes that _obj_ is an iterable—an object that returns an iterator. This includes standard collections and sequences, as well as generators.

If the argument is not an iterable, then calling `iter(obj)` causes a **TypeError** exception to be raised. If _obj_ is an iterable, the call to `iter` should return an iterator object. Such an object does the actual stepping through a sequence of values by responding to `next`.

A few examples should clarify. First of all, you can’t legally call `iter(obj)` if the object is not an iterable. For example:

Click here to view code image

```
>>> gen = iter(5) # This raises a TypeError
```

However, the call is valid if the target object is a list, even if (as in this case) it is a short list containing only a single member.

```
>>> gen = iter([5])
```

It’s more common, of course, to use `iter` on a longer list containing at least two members. The object returned—called an iterator or generator object—can then be used with `next` to access a stream of values, one at a time. For example:

Click here to view code image

```
>>> gen = iter([1, 2, 3])
>>> next(gen)
1
>>> next(gen)
2
>>> next(gen)
3
>>> next(gen)    # StopIteration exception raised.
```

The `iter` function has the effect of calling the `__iter__` magic method in the iterable object’s class (such as a collection or generator), and the `next` function has the effect of calling the `__next__` method of an iterator object’s class. Remember that it’s a two-step process.

- Calling `iter` on an iterable, such as a collection, sequence, or range, returns an iterator object (such as `gen` in the previous example). Sometimes this happens implicitly; a `for` loop will do this for you automatically.

- Calling `next` on the iterator object gets you the next value yielded by the iteration. A `for` loop also does this automatically.

## LEN(SEQUENCE)

Returns the number of elements currently stored in the `sequence`, which is usually a collection but may also be a sequence generated by the `range` function. In the case of a string, this gives the number of characters in the string.

```
>>> len('Hello')
5
```

This function is usually implemented by calling the `__len__` method for the object’s class.

Note that although the sequence generated by the **range** function supports this function, there is no guarantee that other generators do.

## LIST([ITERABLE])

List conversion function. Takes an argument, which must be some kind of iterable, and returns a list. If a generator object is involved, the source of values must be finite in number.

If *iterable* is a string, the function returns a list in which each element is a one-character string.

```
>>> print(list('cat'))
['c', 'a', 't']
```

The square brackets in the syntax indicate that *iterable* is optional. If *iterable* is not specified, this function returns an empty list, [].

Click here to view code image

```
>>> new_list = list() # Init new_list to empty list.
>>> new_list
[]
```

## LOCALS()

Returns a dictionary containing information on values in the local symbol table. This dictionary should not be altered directly. For example:

```
>>> def foobar() :
    a = 2
    b = 3
    c = 1
    print(locals())
>>> foobar()
{'a':2, 'b':3, 'c':1}
```

## MAP(FUNCTION, ITERABLE1 [, ITERABLE2...])

Takes a series of _iterables_ and then returns another iterable, which may be used in a `for` statement, for example, or may be converted into a list by calling the `list` conversion.

The _function_ argument is a callable that must take a number of arguments equal to the number of _iterables_ provided in the other arguments to this function.

For each argument position in the _iterables_, the function is called and takes one element from each argument; the result is then placed in the resulting sequence generated by the `map` function. The resulting iterable will stop as soon as any of the arguments is exhausted.

For example:

Click here to view code image

```
>>> def multy(a, b, c):
    return a * b * c
>>> m = map(multy, [1, 20], [1, 20], [1, 50])
>>> print(list(m))
[1, 20000]
```

The result in this case is [1, 20000] because 1 * 1 * 1 produces 1, and 20 * 20 * 50 produces 20000.

The map function can be used with as few as one iterable argument; however, in such cases, list comprehension usually offers a better solution.

## MAX(ARG1 [, ARG2]...)

Returns the maximum value from a series of one or more arguments (brackets here are not intended literally). See the other entry for max for more information on how this function works.

```
>>> max(1, 3.0, -100, 5.25)
5.25
```

## MAX(ITERABLE)

Returns the maximum element from a finite iterable (which may be a collection, sequence, or generator object). In Python 3.0, all the elements must be sortable with regard to all the
