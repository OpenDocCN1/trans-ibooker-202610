each element in A to its corresponding element in B, and then
sum those products, producing a single scalar value.

Click here to view code image

```
D. P. = A[0]*B[0] + A[1]*B[1] + ... + A[N-1] * B[N-
1]
```

# Here’s an example:

Click here to view code image

```
>>> import numpy as np
>>> A = np.ones(5)
>>> B = np.arange(5)
>>> print(A, B)
[1. 1. 1. 1. 1.] [0 1 2 3 4]
>>> np.dot(A, A)
5.0
>>> np.dot(A, B)
10.0
>>> np.dot(B, B)
30
```

You should be able to see that the dot product of B with B is
equal to 30, because that product is equal to the sum of the
squares of its members:

Click here to view code image

```
D. P. = 0*0 + 1*1 + 2*2 + 3*3 + 4*4
= 0 + 1 + 4 + 9 + 16
= 30
```

# We can generalize this: