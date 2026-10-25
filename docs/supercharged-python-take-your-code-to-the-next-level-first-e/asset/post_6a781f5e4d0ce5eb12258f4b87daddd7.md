

This function is implemented by calling the `__repr__` function of the object's class.

## REVERSED(ITERABLE)

Produces a reverse generator over the elements in the source—that is, it iterates over items in the reverse of the order they have in _iterable_. You can use this generator in a `for` loop, the most typical use. Another thing you can do is to convert it to a list by using the **list** conversion function. For example:

```
>>> print(list(reversed([1, 2, 3])))
[3, 2, 1]
```

Technically, you can get the reverse generator of a string and attempt to display meaningful results. This is difficult to do, however, and requires the use of lists and the `join` function. Otherwise, look at what happens:

```
>>> str(reversed('Wow, Bob, wow!'))
'<reversed object at 0x11124bc88>'
```

The problem is that the **reversed** function, operating on a string, produces a generator object and not a string. But there are alternatives. The easiest solution is to use slicing directly on the string. For example:

```
>>> 'Wow, Bob, wow!'[::-1]
'!wow ,boB ,woW'
```

## ROUND(X [,NDIGITS])

Rounds a numeric value x, using ndigits to indicate at which position to do the rounding: Specifically, ndigits is an integer indicating how many positions to the right of the decimal point to perform the rounding. Negative numbers indicate a position to the left of the decimal point.

An ndigits value of 0 causes rounding to be done to the nearest unit (that is, to the nearest integer). An ndigits value of 1 rounds to the nearest tenth, and a value of −1 rounds to the nearest multiple of 10.

If ndigits is not specified, the function rounds to the nearest unit and returns the result as an integer rather than floating point. (The square brackets in the syntax indicate that this argument is optional.)

For example:

```
>>> round(12.555, 1)
12.6
>>> round(12.555, 2)
12.56
>>> round(12.555, 0)
13.0
>>> round(12.555)
13
>>> round(12.555, -1)
10.0
```

By default, the rounding mechanism rounds up or down, depending on the value of the digit to the right of the least significant digit in the result. If the digit is 5 or higher, then round upward; if 4 or lower, round downward. This behavior works for both positive and negative numbers, so “rounding upward” produces a value farther from 0. For example:

```
>>> round(-55.55)
-56
```

## SET([ITERABLE])

Conversion function for Python sets. If the *iterable* argument is omitted, the result is empty. This is the standard way of representing an empty set in Python, because { } represents an empty dictionary rather than an empty set.

```
empty_set = set()
```

If the *iterable* is not empty, then the resulting set contains all the elements in the argument, but duplicates are dropped and order is not significant. For example:

```
>>> my_list = [11, 11, 3, 5, 3, 3, 3]
>>> my_set = set(my_list)
>>> my_set
{3, 11, 5}
```

## SETATTR(OBJ, NAME_STR, VALUE)

Attribute-setting function. Although most setting of attributes is done directly, this function enables you to set an attribute to be determined later, at run time, rather than hard-coded. In this way, an attribute name can be determined at run time. The attribute might be provided by the user, or it might be determined by some attribute in a database with which the program interacts.

For example, the breed of a Dog object might be set this way:

```
d = Dog()
d.breed = 'Dane'
```

But if the attribute is not known ahead of time, the following statements do the same thing, setting the `breed` attribute to 'Dane' in this case.

```
attr_str = 'breed'
...
setattr(d, attr_str, 'Dane')  # set d.breed = 'Dane'
```

## SORTED(ITERABLE [, KEY] [, REVERSE])

Produces a list that contains all the elements of an _iterable_ but in sorted order. All the elements in the argument must be the same data type, or a compatible data type, in which order can be determined by applying the less-than (<) operator between any two elements. If such comparisons are not supported, a **TypeError** exception is raised.

Here’s a simple example:

```
>>> sorted([5, 0, 10, 7])
[0, 5, 7, 10]
```

This function always produces a list, whereas the **reversed** function produces an iterable. The key argument is a function returning a _key_ to sort by, and _reverse_ indicates high-to-low order if **True**. Each must be a keyword argument if used.

## STR(OBJ="")

Returns a string representation of the object, _obj_. If _obj_ is not specified, this function returns an empty string.

This conversion is implemented by calling the `__str__` method for the object’s class. If the class has no `__str__` method defined, then, by default, its `__repr__` method is called. In many cases, the two methods display the same results; however, the difference is that the `__repr__` method contains the representation of the object as it appears in code—string objects, for example, are returned with quotation marks.

Aside from its role in printing, this string conversion has other uses. For example, you might use this conversion if you want to count the number of o’s in a number.

```
>>> n = 10100140
>>> s = str(n)
>>> s.count('0')
4
```

## STR(OBJ=B" [, ENCODING='UTF-8'])

This version of `str` converts a `bytes` string (which is guaranteed to be made up of individual bytes) to a standard Python string, which may use two or more bytes to store a character. For example:

```
bs = b'Hello!' # Guaranteed to hold exactly six bytes
s = str(bs, encoding='utf-8')
print(s) # Print a normal string, ? bytes per char.
```

_See also_ the previous entry for `str`.

## SUM(ITERABLE [, START])

Produces the sum of the elements in _iterable_. All the elements must be numeric; or they must at least support the __add__ method for objects of that type with each other and with integers. It will not concatenate strings.

This is a super convenient function for use with numeric lists, tuples, and sets. For example, here’s a simple function that gets the average value of a numeric collection:

```
def get_avg(a_list):
    return sum(a_list)/len(a_list)
```

Here’s an example that executes this function:

```
>>> get_avg([1, 2, 3, 4, 10])
4.0
```

The **sum** function can be used on other kinds of iterables, such as generators, as long as they produce a finite sequence. For example:

```
>>> def gen_count(n):
    i = 1
    while i <= n:
        yield i
        i += 1
>>> sum(gen_count(100))
5050
```

The effect of `sum(gen_count(n))`, in this case, is to add up all the numbers from 1 to n.

## SUPER(TYPE)

Returns the superclass of the specified type. This is useful when you’re inheriting from a class and you wish to call the superclass version of a particular method, such as `__init__`.

## TUPLE([ITERABLE])

Tuple conversion: returns an immutable sequence by taking the values from the _iterable_, which must be finite in size. The square brackets indicate that _iterable_ is an optional argument; if omitted, the tuple returned is empty.

## TYPE(OBJ)

Returns the type of _obj_, which can be compared to other types at run time, using either test for equality (==) or **is**. For example:

```
>>> i = 5
>>> type(i) is int
True
>>> type(i) == int
True
```

The **type** function is often useful in Python in determining what the type of an argument is; it enables you to respond in different ways to different types of arguments. However, use of **isinstance** is usually recommended over the use of **type**, because **isinstance** takes subclasses into account.

## ZIP(*ITERABLES)

Returns a sequence of tuples from a series of arguments. For each position, the tuple in the result is (i1, i2, i3... iN), where N is the number of arguments to this function and _i_ is a value produced by the corresponding _iterable_ argument. When the shortest of these arguments is exhausted, the function stops producing tuples.

