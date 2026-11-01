The expression valueIfTrue if condition else valueIfFalse ① evaluates to valueIfTrue if the condition variable is True. When the condition variable is False, the expression evaluates to valueIfFalse. Guido van Rossum jokingly described his syntax design as “intentionally ugly.” Most languages with a ternary operator list the condition first, followed by the true value and then the false value. You can use a conditional expression anywhere you can use an expression or value, including as the argument to a function call ②.

Why would Python introduce this syntax in Python 2.5 even though it breaks the first guideline that beautiful is better than ugly? Unfortunately, despite being somewhat unreadable, many programmers use ternary operators and wanted Python to support this syntax. It's possible to abuse Boolean operator short-circuiting to create a sort of ternary operator. The expression condition and valueIfTrue or valueIfFalse will evaluate to valueIfTrue if condition is True, and valueIfFalse if condition is False (except in one important case). Enter the following into the interactive shell:

```
>>> # Unpythonic Example
>>> valueIfTrue = 'Access granted'
>>> valueIfFalse = 'Access denied'
>>> condition = True
>>> condition and valueIfTrue or valueIfFalse
'Access granted'
```

This condition and valueIfTrue or valueIfFalse style of pseudo-ternary operator has a subtle bug: if valueIfTrue is a falsey value (such as 0, False, None, or the blank string), the expression unexpectedly evaluates to valueIfFalse if condition is True.

But programmers continued to use this fake ternary operator anyway, and “Why doesn’t Python have a ternary operator?” became a perennial question to the Python core developers. Conditional expressions were created so programmers would stop asking for a ternary operator and wouldn’t use the bug-prone pseudo-ternary operator. But conditional expressions are also ugly enough to discourage programmers from using them. Although beautiful may be better than ugly, Python’s “ugly” ternary operator is an example of a case when practicality beats purity.

Conditional expressions aren’t exactly pythonic, but they’re not unpythonic, either. If you do use them, avoid nesting conditional expressions inside other conditional expressions:

```
>>> # Unpythonic Example
>>> age = 30
>>> ageRange = 'child' if age < 13 else 'teenager' if age >= 13 and age < 18 else 'adult'
>>> ageRange
'adult'
```

Nested conditional expressions are a good example of how a dense one-liner can be technically correct but frustrating to make sense of when reading.