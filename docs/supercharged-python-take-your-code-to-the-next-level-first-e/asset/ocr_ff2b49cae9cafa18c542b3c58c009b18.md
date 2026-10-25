```
vals_list = [ ('pi', 3.14), ('phi', 1.618) ]
```

# A dictionary could be created as follows:

> Click here to view code image

```
my_dict = { i[0]: i[1] for i in vals_list }
```

Note the use of the colon (:) in the key-value expression,
i [0] : i [1]. You can verify that a dictionary was
successfully produced by referring to or printing the following
expression, which should produce the number 3.14:

> Click here to view code image

```
my_dict['pi'] # Produces 3.14.
```

Here’s another example, which combines data from two lists
into a dictionary. It assumes that these two lists are the same
length.

> Click here to view code image

```
keys = ['Bob', 'Carol', 'Ted', 'Alice' ]
vals = [4.0, 4.0, 3.75, 3.9]
grade_dict= { keys[i]: vals[i] for i in
range(len(keys)) }
```

# This example creates a dictionary initialized as follows:

> Click here to view code image

```
grade_dict = { 'Bob':4.0, 'Carol':4.0, 'Ted':3.75,
```