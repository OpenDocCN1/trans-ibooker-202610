
Using the get() method to specify a default value to use for nonexistent keys is shorter and more readable than using if-else statements.

Conversely, you might want to set a default value if a key doesn't exist. For example, if the dictionary numberOfPets doesn't have a 'cats' key, the instruction numberOfPets['cats'] += 10 would result in a KeyError error. You might want to add code that checks for the key's absence and sets a default value:

```
>>> # Unpythonic Example
>>> numberOfPets = {'dogs': 2}
>>> if 'cats' not in numberOfPets:
...     numberOfPets['cats'] = 0
...
>>> numberOfPets['cats'] += 10
>>> numberOfPets['cats']
10
```

But because this pattern is also common, dictionaries have a more pythonic setdefault() method. The following code is equivalent to the previous example:

```
>>> # Pythonic Example
>>> numberOfPets = {'dogs': 2}
>>> numberOfPets.setdefault('cats', 0) # Does nothing if 'cats' exists.
0
>>> numberOfPets['cats'] += 10
>>> numberOfPets['cats']
10
```

If you're writing if statements that check whether a key exists in a dictionary and sets a default value if the key is absent, use the setdefault() method instead.

## Use collections.defaultdict for Default Values

You can use the collections.defaultdict class to eliminate KeyError errors entirely. This class lets you create a default dictionary by importing the collections module and calling collections.defaultdict(), passing it a data type to use for a default value. For example, by passing int to collections.defaultdict(), you can make a dictionary-like object that uses 0 for a default value of nonexistent keys. Enter the following into the interactive shell:

```
>>> import collections
>>> scores = collections.defaultdict(int)
>>> scores
defaultdict(<class 'int'>, {})
>>> scores['Al'] += 1 # No need to set a value for the 'Al' key first.
>>> scores
defaultdict(<class 'int'>, {'Al': 1})
>>> scores['Zophie'] # No need to set a value for the 'Zophie' key first.
0
>>> scores['Zophie'] += 40
>>> scores
defaultdict(<class 'int'>, {'Al': 1, 'Zophie': 40})
```

Note that you're passing the int() function, not calling it, so you omit the parentheses after int in collections.defaultdict(int). You can also pass list to use an empty list as the default value. Enter the following into the interactive shell:

```
>>> import collections
>>> booksReadBy = collections.defaultdict(list)
>>> booksReadBy['Al'].append('Oryx and Crake')
>>> booksReadBy['Al'].append('American Gods')
>>> len(booksReadBy['Al'])
2
>>> len(booksReadBy['Zophie']) # The default value is an empty list.
0
```

If you need a default value for every possible key, it's much easier to use collections.defaultdict() than use a regular dictionary and constantly call the setdefault() method.

## Use Dictionaries Instead of a switch Statement

Languages such as Java have a switch statement, which is a kind of if-elif-else statement that runs code based on which one of many values a specific variable contains. Python doesn't have a switch statement, so Python programmers sometimes write code like the following example, which runs a different assignment statement based on which one of many values the season variable contains:

```
# All of the following if and elif conditions have "season ==":
if season == 'Winter':
    holiday = 'New Year\'s Day'
elif season == 'Spring':
    holiday = 'May Day'
elif season == 'Summer':
    holiday = 'Juneteenth'
elif season == 'Fall':
    holiday = 'Halloween'
else:
    holiday = 'Personal day off'
```

This code isn't necessarily unpythonic, but it's a bit verbose. By default, Java switch statements have “fall-through” that requires each block to end with a break statement. Otherwise, the execution continues on to the next block. Forgetting to add this break statement is a common source of bugs. But all the if-elif statements in our Python example can be repetitive.

Some Python programmers prefer to set up a dictionary value instead of using if-elif statements. The following concise and pythonic code is equivalent to the previous example:

```
holiday = {'Winter': 'New Year\'s Day',
    'Spring': 'May Day',
    'Summer': 'Juneteenth',
    'Fall': 'Halloween'}.get(season, 'Personal day off')
```

This code is just a single assignment statement. The value stored in holiday is the return value of the get() method call, which returns the value for the key that season is set to. If the season key doesn't exist, get() returns 'Personal day off'. Using a dictionary will result in more concise code, but it can also make your code harder to read. It's up to you whether or not to use this convention.

## Conditional Expressions: Python’s “Ugly” Ternary Operator

Ternary operators (officially called conditional expressions, or sometimes ternary selection expressions, in Python) evaluate an expression to one of two values based on a condition. Normally, you would do this with a pythonic if-else statement:

```
>>> # Pythonic Example
>>> condition = True
>>> if condition:
...     message = 'Access granted'
... else:
...     message = 'Access denied'
...
>>> message
'Access granted'
```

Ternary simply means an operator with three inputs, but in programming it's synonymous with conditional expression. Conditional expressions also offer a more concise one-liner for code that fits this pattern. In Python, they're implemented with an odd arrangement of the if and else keywords:

```
>>> valueIfTrue = 'Access granted'
>>> valueIfFalse = 'Access denied'
>>> condition = True
>>> message = valueIfTrue if condition else valueIfFalse
>>> message
'Access granted'
>>> print(valueIfTrue if condition else valueIfFalse)
'Access granted'
>>> condition = False
>>> message = valueIfTrue if condition else valueIfFalse
>>> message
'Access denied'
```

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
