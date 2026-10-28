If you assign again using the same key, the previous value is replaced.

```
numbers[3] = "THREE"
```

```
numbers
```

{1: 'one', 2: 'two', 3: 'THREE'}

If other_numbers is another dictionary, we create a new dictionary, merging it and numbers by using double asterisks "**".

```
other_numbers = {4: "four", 5: "five"}
```

```
{**numbers, **other_numbers}
```

{1: 'one', 2: 'two', 3: 'THREE', 4: 'four', 5: 'five'}

The "**" here means "expand everything out into a collection of key-value pairs."

# Exercise 3.28

Starting with an empty dictionary, iterate across numbers and other_numbers to create a dictionary containing all key-value pairs on both.

In general, does it matter in which order you merge two dictionaries? Why or why not?

# 3.9.9 Removing items from a dictionary

We delete a key-value pair from a dictionary with del.

```
numbers
```

{1: 'one', 2: 'two', 3: 'THREE'}

```
del numbers[2]
numbers
```

{1: 'one', 3: 'THREE'}