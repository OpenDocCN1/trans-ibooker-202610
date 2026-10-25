The _shape_ is an integer or tuple of integers, just as with
some of the other functions. The length of this tuple determines
the rank of the array (the number of dimensions); it also
determines how many arguments that the _func_—a callable—
must accept.

There’s a twist here in that _shape_ must be a tuple and not a
scalar, so you may have to use a tuple expression such as (5, )
in order to create one-dimensional data sets.

Here’s a simple example: We want to create a one-
dimensional array corresponding to the first five natural
numbers. You can do this with **arange**, but **fromfunction**
provides another way. It requires us, however, to provide a
callable.

Click here to view code image

```
import numpy as np

def simple(n):
    return n + 1

a = np.fromfunction(simple, (5,), dtype='int32')
```

The resulting array is displayed as follows in IDLE:

Click here to view code image

```
array([1, 2, 3, 4, 5], dtype=int32)
```

This might be better expressed with a lambda function. (See
Chapter 3, “Advanced Capabilities of Lists,” for more
information on lambdas.)