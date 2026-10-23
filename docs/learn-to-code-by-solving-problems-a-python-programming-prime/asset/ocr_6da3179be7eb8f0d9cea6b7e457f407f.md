```
if not num in inverted:
    ③ inverted[num] = [key]
else:
    ④ inverted[num].append(key)
return inverted
```

Listing 8-5: Inverting a dictionary

We’re using a for loop over the dictionary d ①, which gives us each key. We index d to obtain the value mapped to by this key ②. Then we add this key:value pair to the inverted dictionary. If num is not yet a key in the inverted dictionary, then we add it and make it map to the associated key in d ③. If num is already a key in the inverted dictionary, then its value is already a list. We can therefore use append to add the key from d as another value ④.

Enter the code for our invert_dictionary function into the Python shell. Let’s give it a try:

```
>>> d = {'a': 1, 'b': 1, 'c': 1}
>>> invert_dictionary(d)
{1: ['a', 'b', 'c']}
>>> d = {'storm': 2, 'cut': 4, 'magma': 1, 'brook': 2,
...     'gully': 3, 'cliff': 1, 'blast': 1}
>>> invert_dictionary(d)
{2: ['storm', 'brook'], 4: ['cut'], 1: ['magma', 'cliff', 'blast'],
3: ['gully']}
```

Now we’re ready to solve Common Words with an inverted dictionary.

# Solving the Problem

If you’d like more practice with top-down design, you might like to solve the problem on your own before continuing. In the interest of space, I won’t follow the steps of top-down design here. Rather, I’ll present the solution in its entirety, and then we’ll discuss each function and how it is used.

# The Code

The solution is in Listing 8-6.