that all the 1 values are realized as the Boolean value **True**.

Click here to view code image

```
>>> a = np.ones(6, dtype=np.bool)
>>> a
array([ True,  True,  True,  True,  True,  True])
```

This last kind of array—a Boolean array set to all-**True** values
—will prove useful when running the Sieve of Eratosthenes
benchmark to produce prime numbers.

# ## 12.5.7 The “zeros” Function

The **zeros** function creates an array initialized to all-0 values.
Depending on the data type of the array, each member will be
initialized to either 0, an integer, 0.0, or the Boolean value
**False**.

![](bbox=[0.147, 0.551, 0.257, 0.632])

Click here to view code image

```
zeros(shape, dtype='float', order='C')
```

These are the same common array-creation arguments
described for the **empty** function. Briefly, _shape_ is either an
integer (giving the length of a one-dimensional array) or a
tuple, describing N dimensions. The _dtype_ is one of the values
in Table 12.2. The _order_ is either 'C' (row-major order, as in