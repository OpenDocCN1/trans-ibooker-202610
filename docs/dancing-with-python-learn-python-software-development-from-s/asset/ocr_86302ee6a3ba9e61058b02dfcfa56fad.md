# string.startswith(substring_or_tuple, start=0, end=len(string))

If `substring_or_tuple` is a `str`, return `True` if string begins with substring. If `substring_or_tuple` is a tuple of strings, return `True` if string begins with any of them. Return `False` otherwise. See section 4.7.

```
"# This is a Python comment".startswith('#')

True
```

```
"abcdef".startswith(('b', 'B'), 1)

True
```

# string.endswith(substring_or_tuple, start=0, end=len(string))

If `substring_or_tuple` is a `str`, return `True` if string terminates with substring. If `substring_or_tuple` is a tuple of strings, return `True` if string terminates with any of them. Return `False` otherwise. See section 4.7.

```
"Is this a question?".endswith('?')

True
```

```
"This could be a sentence!".endswith(('.', '!', '?'))

True
```

You might want to call one of *strip*, *lstrip*, or *rstrip*, before using *startswith* or *endswith*.

> **Exercise 12.1**
> 
> Use *startswith* and *endswith* to test whether a string is a palindrome. Show the code you would use to make the test case-insensitive.

# string.replace(old_substring, new_substring, count=-1)

Without the `count` argument, *replace* substitutes all instances of `old_substring` with `new_substring` in `string`. When `count` is given and is a non-negative `int`, *replace* does, at most, `count` substitutions, beginning at the start of `string`. See section 4.4.2.

```
"-A-A-A-A-A-A-A-".replace("-A-", "=B=")

'=B=A=B=A=B=A=B='
```

```
"-A-A-A-A-A-A-A-".replace("-A-", "=B=", 2)

'=B=A=B=A-A-A-A-'
```