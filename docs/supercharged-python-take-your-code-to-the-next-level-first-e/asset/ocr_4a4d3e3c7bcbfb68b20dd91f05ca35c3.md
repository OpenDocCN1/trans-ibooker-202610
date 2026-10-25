```
my_list = [1, 2, -10, -500, 33, 21, -1]
neg_list = [i for i in my_list if i < 0 ]
```

The result in this case is to produce the following list and give
it the name `neg_list`:

```
[-10, -500, -1]
```

# ## 3.16 DICTIONARY AND SET COMPREHENSION

The principles of list comprehension extend to sets and
dictionaries. It’s easiest to see this with sets, because a set is a
simple collection of values in which duplicates are ignored and
order doesn’t matter.

For example, suppose we want to get only the positive values
from `a_list` and place them in a set rather than a list. You
could write this using an ordinary loop:

> Click here to view code image

```
a_list = [5, 5, 5, -20, 2, -1, 2]
my_set = set( )
for i in a_list:
    if i > 0:
        my_set.add(i)
```

You can also do this through set comprehension, by using set
braces (“curly braces”) rather than square brackets.