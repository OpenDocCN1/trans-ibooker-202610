

## BOOL(OBJ)

Boolean conversion. This is an important conversion because it's implicitly called as needed by `if` and `while` statements, as well as by the `filter` function.

This function returns either `True` or `False`, depending on the value of _obj_. Each class can determine how to evaluate this function by implementing a `__bool__` method; but otherwise, the default behavior is to return `True`. Python classes tend to observe the following general guidelines. An object usually converts to `True` if it contains any of the following:

- A non-zero numeric value. (For complex numbers, this is any value other than `0+0j`.)
- Non-empty collections, including lists and strings that are not empty.

Objects that usually convert to `False` include:

- Any numeric value equal to zero
- A zero-length collection or sequence, or a zero-length string
- The special value `None`

For example:

```
>>> class Blah() :
 pass
```

```
>>> b = Blah()
>>> b.egg = 0
>>> bool(b)
True
>>> bool(b.egg)
False
```

In this case, the object `b` automatically converted to **True**, because no `__bool__` method was defined for the class, but `b.egg` is equal to 0 and therefore **False**.

## BYTES(SOURCE, ENCODING)

Byte-string conversion function. Converts a source, typically a string, into a `bytes` string, in which each element has a byte value between 0 and 255, inclusive, and is stored in a single byte. In Python 3.0, it's typical for Python strings to use Unicode or UTF-8 representation, so that regular strings may occupy more than one byte per character. Therefore, ordinary strings need to be converted to produce `bytes` strings, if they need to contain one byte per character.

For example:

```
>>> bs = bytes('Hi there!', encoding='utf-8')
>>> bs
b'Hi there!'
```

## CALLABLE(OBJ)

Returns **True** if the object _obj_ can be called as a function. A true value indicates one or more of the following: _obj_ is the name of a function, is created by a function definition, or is an instance of a class that implements `__call__`.

## CHR(N)

Returns the one-character string in which that character has Unicode value _n_. The lower part of this range includes ASCII characters. This is the inverse of the **ord** function. The domain of this function is 0x10FFFF. Values of _n_ outside that domain cause a **ValueError** to be raised.

For example:

```
>>> chr(97)
'a'
```

## COMPILE(CMD_STR, FILENAME, MODE_STR, FLAGS=0, DONT_INHERIT=FALSE, OPTIMIZE=-1)

The **compile** function takes a string containing an expression, statement, or block of code (depending on _mode_) and returns a code object, which can then be executed by calling the **eval** or **exec** function. The _mode_str string contains 'exec', 'single', or 'eval', to compile a module, single statement, or expression, respectively.

Because it enables execution of arbitrary strings of Python code, giving outsiders access through **exec** creates major security holes. In general, avoid it unless you have very good reason to use it. But here’s a simple demonstration:

```
>>> command_str = '''
pi = 3.141592
print('pi/2 = ', pi/2)
'''
>>> cmd = compile(command_str, 'my_mod', 'exec')
>>> exec(cmd)
pi/2 = 1.570796
```

For more information, search online help. Remember that few people should use this function. If you execute an arbitrary user-input string, you’re handling over control of everything in your program and the system.

## COMPLEX(REAL=0, IMAG=0)

Complex-number conversion. Both _real_ and _imag_ arguments are optional, and each has a default value of 0. Given numeric input (as opposed to string input, which is explained in the next entry), the function responds by doing the following:

- Multiply the _imag_ argument by 1j—that is, by the imaginary number, _i_.
- Add that to the _real_ argument.
- Ensure that the result is a complex number, by adding or subtracting `0j` if needed. (This usually follows from the first two rules.)

These simple rules encompass everything the conversion does. For example, a zero-value complex number is returned by default:

```
>>> complex()
0j
```

You can also provide an argument to the real portion only, creating a value that includes `0j` to indicate its status as a complex rather than a real number.

```
>>> complex(5.5)
(5.5+0j)
```

The most common way to use this conversion is to use a real number or integer for each argument.

```
>>> complex(3, -2.1)
(3-2.1j)
```

You can also specify a complex argument for the first argument only, and the conversion will simply return the number as is.

```
>>> complex(10-3.5j)
(10-3.5j)
```

A quirk of this function is that you can specify a complex number for both arguments! The rules outlined earlier still apply. In the following example, the _imag_ argument is 5j. This is multiplied by 1j, as usual, and the result is 5j * 1j = -5. That value is then added to the _real_ argument, 1, to produce -4.

```
>>> complex(1, 5j)
(-4+0j)
```

## COMPLEX(COMPLEX_STR)

The complex-number conversion accepts a lone string argument of the form ' a+bj '—but it must have no internal spaces, and no second argument is allowed. Optional parentheses are accepted around a+bj. For example:

```
>>> complex('10.2+5j')
(10.2+5j)
>>> complex(' (10.2+5j) ')
(10.2+5j)
```

This function accepts strings that are valid complex numbers even if such numbers have only a real or only an imaginary portion. 0j will always be present in the result even if there is no non-zero imaginary portion. For example:

```
>>> complex('2')
(2+0j)
>>> complex('3j')
3j
```

Another quirk of this function is that _complex_str is the only context in which you can use `j` in place of `1j`; usually a number must be combined with `j` to distinguish it from any variable named `j`.

```
>>> complex('1+j')
(1+1j)
```

> Note
> It’s possible to produce values that use `-0j` instead of the usual `+0j`. For example, `complex(1, -1j)` produces `(2-0j)` even though `complex(2, 0)` produces `(2+0j)`. The two results can be compared successfully with `==` but not with `is`. This phenomenon arises from floating-point representation including a sign bit; `0.0` and `-0.0` are distinct objects, even though numerically equal.

See also the previous entry on `complex`.

## DELATTR(OBJ, NAME_STR)

Delete-attribute function. Removes an attribute from object `obj`, in which _name_str is a string containing the name of the attribute. For example:

```
my_dog.breed = 'St Bernard'
...
a_str = 'breed'
delattr(my_dog, a_str)
```

After this statement is executed, the object `my_dog` no longer has a ‘breed’ attribute.

An `AttributeError` exception is raised if the `obj` does not already have the named attribute before the deletion. See also the `hasattr` and `setattr` functions.

## DIR([OBJ])

Directory function. Returns a list of attributes for the optional argument _obj_. If this object is a class, it shows all the attributes of the class. If the object is not a class, it gets the object’s class and shows all that class’s attributes.

If this argument is omitted, `dir` returns a list of attributes for the current context—either the current function or module. For example:

```
dir()   # Get attributes of the module
```

Or, from within a function definition:

```
def demo():
    i = 1
    j = 2
    print(dir())
```

The call to `demo` prints the following:

```
['i', 'j']
```

The innocent-looking `print(dir(i))`, would print a rather long list, because `i` is an integer, and the integer class (`int`) has a fairly long list of attributes. Such a call has the same output as

```
print(dir(int))
```

## DIVMOD(A, B)

Divides _a_ by _b_ and then returns a tuple containing a/b, rounded downward to the nearest integer, along with the result, a % b, the remainder. This function is typically used with integers. For example:

```
quot, rem = divmod(203, 10) # Result is 20,
remainder 3
```

Either or both of the arguments _a_ and _b_ may be **float** values; but in that case, both of the return values will be in floating-point format. The quotient will have no fractional portion, but the remainder may. For example:

```
>>> divmod(10.6, 0.5)
(21.0, 0.09999999999999964)
```

The resulting tuple in this case should be `(21.0, 0.1)`, but there is a small rounding error, as can happen with floating-point values. You can use the **round** function to help correct it.

## ENUMERATE(ITERABLE, START=0)

Enumeration. Takes an _iterable_ as input and returns a
