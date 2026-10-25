```
>>> s.count('0')
4
```

# STR(OBJ=B" [, ENCODING='UTF-8'])


This version of `str` converts a `bytes` string (which is guaranteed to be made up of individual bytes) to a standard Python string, which may use two or more bytes to store a character. For example:


> Click here to view code image

```
bs = b'Hello!' # Guaranteed to hold exactly six
bytes
s = str(bs, encoding='utf-8')
print(s) # Print a normal string, ? bytes per
char.

```

_See also_ the previous entry for `str`.


# SUM(ITERABLE [, START])


Produces the sum of the elements in _iterable_. All the elements must be numeric; or they must at least support the __add__ method for objects of that type with each other and with integers. It will not concatenate strings.


This is a super convenient function for use with numeric lists, tuples, and sets. For example, here’s a simple function that gets the average value of a numeric collection:


> Click here to view code image