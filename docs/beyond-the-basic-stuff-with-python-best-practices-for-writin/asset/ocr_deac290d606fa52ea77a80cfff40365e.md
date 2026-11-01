# Put No Spaces Before Separators and a Single Space After Separators

We separate the items lists and dictionaries, as well as the parameters in function def statements, using comma characters. You should place no spaces before these commas and a single space after them, as in this example:

```
YES: def spam(eggs, bacon, ham):
YES:     weights = [42.0, 3.1415, 2.718]
```

Otherwise, you’ll end up with “bunched up” code that is harder to read:

```
NO: def spam(eggs,bacon,ham):
NO:     weights = [42.0,3.1415,2.718]
```

Don’t add spaces before the separator, because that unnecessarily draws the eye to the separator character:

```
NO: def spam(eggs , bacon , ham):
NO:     weights = [42.0 , 3.1415 , 2.718]
```

Black automatically inserts a space after commas and removes spaces before them.

# Don’t Put Spaces Before or After Periods

Python allows you to insert spaces before and after the periods marking the beginning of a Python attribute, but you should avoid doing so. By not placing spaces there, you emphasize the connection between the object and its attribute, as in this example:

```
YES: 'Hello, world'.upper()
```

If you put spaces before or after the period, the object and attribute look like they’re unrelated to each other:

```
NO: 'Hello, world' . upper()
```

Black automatically removes spaces surrounding periods.

# Don’t Put Spaces After a Function, Method, or Container Name

We can readily identify function and method names because they’re followed by a set of parentheses, so don’t put a space between the name and the opening parenthesis. We would normally write a function call like this:

```
YES: print('Hello, world!')
```

But adding a space makes this singular function call look like it’s two separate things:

```
NO: print ('Hello, world!')
```