```
import numpy as np
a = np.array([1, 2, 3])
```

You can just as easily create a two-dimensional array, or
higher, by using a multidimensional Python list (a list of lists):

+   Click here to view code image

```
a = np.array([[1, 2, 3], [10, 20, 30], [0, 0, -1]])
print(a)
```

Printing this array within IDLE produces

```
array([[ 1,  2,  3],
       [10, 20, 30],
       [ 0,  0, -1]])
```

numpy is designed to handle arrays with smooth, rectangular
shapes. If you use higher-dimensional input that is “jagged,” the
array conversion must compensate by constructing as regular
an array as well as it can.
So from within IDLE, you write this code:

+   Click here to view code image

```
>>> import numpy as np
>>> a = np.array([[1, 2, 3], [10, 20, 300]])
>>> a
array([[  1,   2,   3],
       [ 10,  20, 300]])
```

But here’s what happens if the second row is made longer
than the first: