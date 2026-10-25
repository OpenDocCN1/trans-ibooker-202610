
that make them poor data for random sampling.

![](img/6e45221a8147f2442c653a3d4a866570_726_0.png)

```
`numpy.empty(shape, dtype='float', order='C')`
```

The _shape_ argument, the only required argument in this case, is either an integer or a tuple. In the former case, a one-dimensional array is created. A tuple specifies a higher-dimensional array. For example, (3, 3) specifies a two-dimensional, 3 × 3 array.

The _dtype_ argument determines the data type of each element. By default, it is set to '**float**'. (See Table 12.2 for a list of _dtype_ settings.)

The _order_ argument determines whether the array is stored in row-major or column-major order. It takes the value '**C**' (row-major order, as in C) or '**F**' (column-major order, as in FORTRAN). C is the default.

The following example creates a 2 × 2 array made up of 16-bit signed integers.

![](img/6e45221a8147f2442c653a3d4a866570_726_1.png)

```
`import numpy as np
a = np.empty((2, 2), dtype='int16')`
```

Displaying this array in IDLE (and thereby getting its canonical representation) produces

```
array(([0, 0],
[0, -3]], dtype=int16)
```

Your results may vary, because the data in this case is uninitialized and therefore unpredictable. Here’s another example. Remember that although the numbers may look random, don’t rely on this “randomness.” It’s better to consider such uninitialized values to be “garbage.” This means don’t use them.

Click here to view code image

```
a = np.empty((3, 2), dtype='float32')
```

Displaying this array in IDLE produces

Click here to view code image

```
array(([1.4012985e-45, 2.3509887e-38],
[9.1835496e-41, 3.5873241e-43],
[1.4012985e-45, 2.3509887e-38]],
dtype=float32)
```

## 12.5.5 The “eye” Function

The **eye** function is similar to the **identity** function in **numpy**. Both create the same kind of array—specifically, an “identity” array, which places 1’s in the positions [0,0], [1,1], [2,2], [3,3] and so on, while placing 0’s everywhere else. This function produces a two-dimensional array only.

![](img/6e45221a8147f2442c653a3d4a866570_728_0.png)

Click here to view code image

```
numpy.eye(N, M=None, [k,] dtype='float', order='C')
```

The _N_ and _M_ arguments, respectively, specify the number of rows and columns. If _M_ is not specified or is specified as **None**, then it’s automatically set to the value of _N_.

The _k_ argument, which is optional, can be used to move the diagonal. The default, 0, utilizes the main diagonal (see the upcoming example). Positive and negative integer values, respectively, move this diagonal up and down.

The _dtype_ argument determines the data type of each element. By default, it is set to **'float'**. See Table 12.2 for a list of settings.

The _order_ argument determines whether the array is stored in row-major or column-major order, and it takes the value **C** (row-major, as in the C language) or **F** (column-major, as in FORTRAN). C is the default.

Here’s an example:

```
a = np.eye(4, dtype='int')
```

Displaying this array in IDLE produces

```
array([[1, 0, 0, 0],
       [0, 1, 0, 0],
       [0, 0, 1, 0],
       [0, 0, 0, 1]])
```

Or we can create a floating-point version, using the _dtype_ default, `'float'`, and making it somewhat larger: 6 × 6 instead of 4 × 4.

```
a = np.eye(6)
```

The result looks like this when displayed in IDLE:

```
array([[1., 0., 0., 0., 0., 0.],
       [0., 1., 0., 0., 0., 0.],
       [0., 0., 1., 0., 0., 0.],
       [0., 0., 0., 1., 0., 0.],
       [0., 0., 0., 0., 1., 0.],
       [0., 0., 0., 0., 0., 1.]])
```

Arrays like this have a number of uses, but basically, they provide a way to do batch processing on large arrays when you want to do something special with coordinate pairs that match the identity relationship, R = C.

## 12.5.6 The “ones” Function

The ones function creates an array initialized to all 1 values. Depending on the data type of the array, each member will be initialized to either 1, an integer, 1.0, or the Boolean value **True**.

![](img/6e45221a8147f2442c653a3d4a866570_729_0.png)

```
numpy.ones(shape, dtype='float', order='C')
```

These are the same arguments described for the **empty** function. Briefly, _shape_ is either an integer (giving the length of a one-dimensional array) or a tuple describing N dimensions. The _dtype_ is one of the values in Table 12.2. The _order_ is either 'C' (row-major order, as in the C language) or 'F' (column-major order, as in FORTRAN).

Here’s a simple example creating a \(3 \times 3\) two-dimensional array using the default **float** type.

```
>>> import numpy as np
>>> a = np.ones((3,3))
>>> a
array([[1., 1., 1.],
       [1., 1., 1.],
       [1., 1., 1.]])
```

Here is another example, this time creating a \(2 \times 2 \times 3\) array of integers.

```
>>> a = np.ones((2, 2, 3), dtype=np.int16)
>>> a
array([[[1, 1, 1],
        [1, 1, 1]],
       [[1, 1, 1],
        [1, 1, 1]]], dtype=int16)
```

Finally, here’s a one-dimensional array of Booleans. Notice that all the 1 values are realized as the Boolean value **True**.

Click here to view code image

```
>>> a = np.ones(6, dtype=np.bool)
>>> a
array([ True,  True,  True,  True,  True,  True])
```

This last kind of array—a Boolean array set to all-**True** values—will prove useful when running the Sieve of Eratosthenes benchmark to produce prime numbers.

## 12.5.7 The “zeros” Function

The **zeros** function creates an array initialized to all-0 values. Depending on the data type of the array, each member will be initialized to either 0, an integer, 0.0, or the Boolean value **False**.

![](img/6e45221a8147f2442c653a3d4a866570_731_0.png)

Click here to view code image

```
zeros(shape, dtype='float', order='C')
```

These are the same common array-creation arguments described for the **empty** function. Briefly, _shape_ is either an integer (giving the length of a one-dimensional array) or a tuple, describing N dimensions. The _dtype_ is one of the values in Table 12.2. The _order_ is either 'C' (row-major order, as in the C language) or 'F' (column-major order, as in FORTRAN).

> Note
> 
> The name of this function is tricky, because the English word "zeros" can also be spelled "zeroes." Remember to use the shorter spelling, zeros, only. Ah, English spelling—never mastered even by native English speakers!

Here is a simple example creating a 3 × 3 two-dimensional array using the default float type.

```
>>> import numpy as np
>>> a = np.zeros((3,3))
>>> a
array([[0., 0., 0.],
       [0., 0., 0.],
       [0., 0., 0.]])
```

Here’s another example, this time creating a 2 × 2 × 3 array of integers.

Click here to view code image

```
>>> a = np.zeros((2, 2, 3), dtype=np.int16)
>>> a
array([[[0, 0, 0],
        [0, 0, 0]],
       [[0, 0, 0],
        [0, 0, 0]]], dtype=int16)
```

Finally, here’s a one-dimensional array of Booleans. Notice that all the zero values are realized as the Boolean value False.

Click here to view code image

```
>>> a = np.zeros(5, dtype=np.bool)
>>> a
array([False, False, False, False, False])
```

## 12.5.8 The “full” Function

The `full` function creates a `numpy` array using the same arguments shown earlier for `empty`, `ones`, and `zeros`; however, `full` has one additional argument: a value to assign to each and every element.

![](img/6e45221a8147f2442c653a3d4a866570_733_0.png)

Click here to view code image

```
numpy.full(shape, fill_value, dtype=None, order='C')
```

Briefly, _shape_ is either an integer (giving the length of a one-dimensional array) or a tuple, describing N dimensions. The _dtype_ is one of the values in Table 12.2. The _order_ is either 'C' (row-major order, as in the C language) or 'F' (column-major order, as in FORTRAN).

If the _dtype_ argument is either omitted or set to `None`, the function uses the data type of the _fill_value_, which is required for this function.

Here’s a simple example creating a 2 × 2 array in which each element is set to 3.14.

```
>>> import numpy as np
```
