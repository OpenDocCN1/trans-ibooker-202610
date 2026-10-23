string’s length. Here’s how we can loop through each character of a string from left to right:

```
>>> s = 'zephyr'
>>> i = 0
① >>> while i < len(s):
...     print('We have ' + s[i])
...     i = i + 1
...
We have z
We have e
We have p
We have h
We have y
We have r
```

The variable i allows us to access each character of the string. It begins at 0 and increases by one each time through the loop.

I used < in the loop’s Boolean expression ① to continue as long as we haven’t reached the length of the string. Had I used <= instead of <, we’d have received an IndexError:

```
>>> i = 0
>>> while i <= len(s):
...     print('We have ' + s[i])
...     i = i + 1
...
We have z
We have e
We have p
We have h
We have y
We have r
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
IndexError: string index out of range
```

The length of the string is 6. We get this error because the loop tries to access s[6], which is not a valid index in the string.

Want to loop through the string jumping by three characters at a time instead of one? No problem; just increase i by 3 instead of 1:

```
>>> i = 0
>>> while i < len(s):
...     print('We have ' + s[i])
...     i = i + 3
```