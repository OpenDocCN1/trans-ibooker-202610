

>>> a = np.full((2, 2), 3.14)
>>> a
array([[3.14, 3.14],
       [3.14, 3.14]])

Here’s another example; this one creates an array of eight integers, each set to 100.

+ Click here to view code image

>>> a = np.full(8, 100)
>>> a
array([100, 100, 100, 100, 100, 100, 100, 100])

This final example takes advantage of the fact that you can create a **numpy** array of strings—provided that all these strings observe a fixed maximum size.

+ Click here to view code image

>>> a = np.full(5, 'ken')
>>> a
array(['ken', 'ken', 'ken', 'ken', 'ken'],
      dtype='<U3')

After the array is created in this way, with strings of size 3, each such string has, in effect, a maximum size. You can assign a longer string to any of these array elements, but it will be truncated.

+ Click here to view code image

a[0] = 'tommy' # Legal, but only 'tom' is assigned.

## 12.5.9 The “copy” Function

The **numpy copy** function copies all the elements of an existing array. Because data is stored contiguously rather than through references, deep copying versus shallow copying is generally not an issue for **numpy** arrays.

An example should suffice. Suppose you already have an array, `a_arr`, and you want to make a full copy of it and call the copy `b_arr`.

import numpy as np

b_arr = np.copy(a_arr)

## 12.5.10 The “fromfunction” Function

The **numpy fromfunction** function (yes, that’s a mouthful) is among the most powerful ways to create an array; we’ll use it in the next section to create a multiplication table. The **fromfunction** enables you to create and initialize an array by calling another function that works _as if_ it were transforming indexes into arguments.

![](img/6e45221a8147f2442c653a3d4a866570_735_0.png)

Click here to view code image

numpy.fromfunction(func, shape, dtype='float')

The _shape_ is an integer or tuple of integers, just as with some of the other functions. The length of this tuple determines the rank of the array (the number of dimensions); it also determines how many arguments that the _func_—a callable—must accept.

There’s a twist here in that _shape_ must be a tuple and not a scalar, so you may have to use a tuple expression such as (5, ) in order to create one-dimensional data sets.

Here’s a simple example: We want to create a one-dimensional array corresponding to the first five natural numbers. You can do this with **arange**, but **fromfunction** provides another way. It requires us, however, to provide a callable.

Click here to view code image

import numpy as np

def simple(n):
    return n + 1

a = np.fromfunction(simple, (5,), dtype='int32')

The resulting array is displayed as follows in IDLE:

Click here to view code image

array([1, 2, 3, 4, 5], dtype=int32)

This might be better expressed with a lambda function. (See Chapter 3, “Advanced Capabilities of Lists,” for more information on lambdas.)

Click here to view code image

a = np.fromfunction(lambda n:n+1, (5,),
        dtype='int32')

But higher-dimensional arrays are common. Here’s an example that creates a two-dimensional array in which each element is equal to the total of its two indexes.

Click here to view code image

def add_it(r, c):
    return r + c

a = np.fromfunction(add_it, (3, 3), dtype='int32')

This code could also be written with a lambda.

Click here to view code image

a = np.fromfunction(lambda r,c:r+c, (3, 3),
        dtype='int')

In either case, if the resulting array is displayed in IDLE, it has the following representation:

array([[0, 1, 2],
       [1, 2, 3],
       [2, 3, 4]])

At the beginning of this section, we stated that **fromfunction** works _as if_ the function were being called for each element, with the arguments being the index or indexes at that position.

What **fromfunction** actually does is create an array or arrays, in which one of the dimensions (or _axis_) is set to a series of whole numbers. For a one-dimensional array of size 6, this is the numbers 0 through 5.

[0 1 2 3 4 5]

This is an _identity_ array in that each element is equal to its index.

For the two-dimensional 3 × 3 array used in the previous example, **fromfunction** creates two arrays: one for each of the two axes.

[[0 0 0],
 [1 1 1],
 [2 2 2]]

[[0 1 2],
 [0 1 2],
 [0 1 2]]

These are identity arrays along specific axes. In the first array, each element is equal to its row index; in the second array, each element is equal to its column index.

The implementation of **fromfunction** operates on the arrays. As a result, the _callable_ argument (the other function being called) is executed only once! But it is executed on one or more arrays—one for each dimension—enabling batch processing.

If you use **fromfunction** the way it was designed to be used, this underlying implementation works. But if you do unorthodox things, strange results are possible. Consider the following code, which should produce a 3 × 3 array.

```python
a = np.fromfunction(lambda r, c: 1, (3, 3),
                    dtype='int')
```

You’d probably expect the result to be a 3 × 3 array in which each element is set to 1. Yet this function call (you can easily try it out) returns 1 as a scalar value!

## 12.6 EXAMPLE: CREATING A MULTIPLICATION TABLE

Suppose you want to create the classic multiplication table for numbers from 1 to 10. There is more than one way to do that with **numpy**. You could create an empty array, for example, and assign values to the elements.

With **numpy**, you could also do something similar. You could create an array initialized to all-zero values, for example, and then write a nested loop to assign `R * C` to each element—actually, it’s `(R+1) * (C+1)`.

By far the most efficient approach would be to use **fromfunction** to create an array that called a function to generate the values, without writing any loops at all. This is the **numpy** philosophy: As much as possible, let the package do all the work by using batch operations. You should be writing relatively few loops.

Here’s the solution:

Click here to view code image

import numpy as np

def multy(r, c):
    return (r + 1) * (c + 1)

a = np.fromfunction(multy, (10, 10), dtype=np.int16)

You could write this more compactly by using a lambda function. (Lambdas are explained in Chapter 3, “Advanced Capabilities of Lists.”)

Click here to view code image

a = np.fromfunction(lambda r,c: (r+1) * (c+1),
(10, 10), dtype=np.int16)

Printing the result, a, produces a nice-looking multiplication table.

Click here to view code image

>>> print(a)
[[ 1 2 3 4 5 6 7 8 9 10]
 [ 2 4 6 8 10 12 14 16 18 20]
 [ 3 6 9 12 15 18 21 24 27 30]
 [ 4 8 12 16 20 24 28 32 36 40]
 [ 5 10 15 20 25 30 35 40 45 50]
 [ 6 12 18 24 30 36 42 48 54 60]
 [ 7 14 21 28 35 42 49 56 63 70]
 [ 8 16 24 32 40 48 56 64 72 80]
 [ 9 18 27 36 45 54 63 72 81 90]
 [ 10 20 30 40 50 60 70 80 90 100]]

You can improve the appearance by getting rid of the brackets in the display. That's relatively easy to do if we first convert to a string and then use the str class replace method.

s = str(a)
s = s.replace('[', '')
s = s.replace(']', '')
s = ' ' + s

As mentioned in Chapter 4, replacing a character with the empty string is a convenient way to purge all instances of a character. This example calls the replace method to get rid of both kinds of brackets. Finally, a space is inserted at the front of the string to make up for the loss of two open brackets. Now, printing this string produces a pleasing display.

Click here to view code image

>>> print(s)
1 2 3 4 5 6 7 8 9 10
2 4 6 8 10 12 14 16 18 20
3 6 9 12 15 18 21 24 27 30
4 8 12 16 20 24 28 32 36 40
5 10 15 20 25 30 35 40 45 50
6 12 18 24 30 36 42 48 54 60
7 14 21 28 35 42 49 56 63 70
8 16 24 32 40 48 56 64 72 80
9 18 27 36 45 54 63 72 81 90
10 20 30 40 50 60 70 80 90 100

