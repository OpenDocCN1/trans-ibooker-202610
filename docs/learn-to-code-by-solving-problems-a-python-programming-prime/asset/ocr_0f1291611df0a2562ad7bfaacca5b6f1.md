You can use the `len` function to determine the number of values in a set:

```
>>> len({2, 4, 6, 8})
4
```

You can also loop over the values in a set:

```
>>> for value in {2, 4, 6, 8}:
... print('I found', value)
...
I found 8
I found 2
I found 4
I found 6
```

You can’t index or slice a set, though. Values in a set don’t have indices.

To create an empty set, you might expect to use an empty pair of curly brackets, {}. In an inconsistency of Python syntax, that doesn’t work:

```
>>> type({2, 4, 6, 8})
<class 'set'>
>>> {}
{}
>>> type({})
<class 'dict'>
```

Using {} gives us the wrong type: a dict (dictionary) instead of a set. We’ll talk about dictionaries later in this chapter.

To make an empty set, we use set(), like this:

```
>>> set()
set()
>>> type(set())
<class 'set'>
```

# Set Methods

Sets are mutable, so we can add and remove values. We can perform these tasks by using methods.