earlier.
This solution—inheritance—works only when you choose to
build your collection class on top of an existing class, such as
`list` or `dict`.

# 9.10.10 Implementing “`__iter__`” and “`__next__`”

The `__iter__` and `__next__` methods, along with the
`next` function, require some elaboration. These methods enable
objects of your class to produce a _generator_ (or _iterator_), which
in turn makes it usable in certain special contexts, such as a `for`
loop. Let’s start with some terminology.

+   - An _iterable_ is an objected that can be iterated—or rather, stepped
+   through—one element at a time. To be an iterable, an object must
+   return an object in response to `__iter__`.
+   
+   - An _iterator_ is what a call to `__iter__` must return. An iterator
+   is an object that can be used to step through a collection object.
+   
+   - This iterator, which can be the same class or a separate class
+   written just for this purpose, must respond to the `__next__`
+   method—that is, it must implement this magic method, even if it
+   does nothing else.

These abilities are important, because they enable instances
of a class to be used with the Python `for` keyword. For
example, suppose you have a four-dimensional Point object. If
the iterator for such an object gets one of the four coordinates at
a time, then the user of the class could use code like this: