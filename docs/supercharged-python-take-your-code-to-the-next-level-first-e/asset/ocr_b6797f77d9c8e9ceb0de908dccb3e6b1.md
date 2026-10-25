```
>>> a = np.zeros((2, 2, 3), dtype=np.int16)
>>> a
array([[[0, 0, 0],
        [0, 0, 0]],

       [[0, 0, 0],
        [0, 0, 0]]], dtype=int16)
>>> a = np.zeros(5, dtype=np.bool)
>>> a
array([False, False, False, False, False])
numpy.full(shape, fill_value, dtype=None, order='C')
>>> a = np.full(8, 100)
>>> a
array([100, 100, 100, 100, 100, 100, 100, 100])
>>> a = np.full(5,'ken')
>>> a
array(['ken', 'ken', 'ken', 'ken', 'ken'], dtype='<U3')
a[0] = 'tommy'  # Legal, but only 'tom' is assigned.
numpy.fromfunction(func, shape, dtype='float')
```