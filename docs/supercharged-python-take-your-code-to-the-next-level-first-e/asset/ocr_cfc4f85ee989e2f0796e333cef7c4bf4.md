```
>>> a = np.array([[1, 2, 3], [10, 20, 300, 4]])
>>> a
array([list([1, 2, 3]), list([10, 20, 300, 4])],
      dtype=object)
```

Now the array is forced into being a one-dimensional array of objects (each one being a list) rather than a true two-dimensional array.

# 12.5.2 The “arange” Function

The `arange` function creates an array of values from 1 to N, similar to the way the Python `range` function does. This function is limited to generating one-dimensional arrays.

![](bbox=[0.146, 0.505, 0.254, 0.584])

```
arange([beg,] end [,step] [dtype=None])
```

The arguments to `arange` are nearly the same as the arguments to the Python built-in `range` function.

In addition, the `dtype` argument specifies the type of each element. The default argument value is `None`, which causes the function to infer the data type. It uses an integer large enough to accommodate all the values in the range, such as `'int32'`.