that make them poor data for random sampling.

![](bbox=[0.1484, 0.1328, 0.2539, 0.2469])

```
`numpy.empty(shape, dtype='float', order='C')`
```

The _shape_ argument, the only required argument in this case, is either an integer or a tuple. In the former case, a one-dimensional array is created. A tuple specifies a higher-dimensional array. For example, (3, 3) specifies a two-dimensional, 3 × 3 array.

The _dtype_ argument determines the data type of each element. By default, it is set to '**float**'. (See Table 12.2 for a list of _dtype_ settings.)

The _order_ argument determines whether the array is stored in row-major or column-major order. It takes the value '**C**' (row-major order, as in C) or '**F**' (column-major order, as in FORTRAN). C is the default.

The following example creates a 2 × 2 array made up of 16-bit signed integers.

![](bbox=[0.1816, 0.7391, 0.4172, 0.7562])

```
`import numpy as np
a = np.empty((2, 2), dtype='int16')`
```

Displaying this array in IDLE (and thereby getting its