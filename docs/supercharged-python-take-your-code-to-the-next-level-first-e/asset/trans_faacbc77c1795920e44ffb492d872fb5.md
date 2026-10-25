

```python
import numpy as np
a = np.empty((2, 2), dtype='int16')
a = np.empty((3, 2), dtype='float32')
array([[1.4012985e-45, 2.3509887e-38],
       [9.1835496e-41, 3.5873241e-43],
       [1.4012985e-45, 2.3509887e-38]], dtype=float32)
numpy.eye(N, M=None, [k,] dtype='float', order='C')
numpy.ones(shape, dtype='float', order='C')
>>> a = np.ones((2, 2, 3), dtype=np.int16)
>>> a
array([[[1, 1, 1],
        [1, 1, 1]],

       [[1, 1, 1],
        [1, 1, 1]]], dtype=int16)
>>> a = np.ones(6, dtype=np.bool)
>>> a
array([ True,  True,  True,  True,  True,  True])
zeros(shape, dtype='float', order='C')
```

```python
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

```python
import numpy as np

def simple(n):
    return n + 1

a = np.fromfunction(simple, (5,), dtype='int32')
array([1, 2, 3, 4, 5], dtype=int32)
a = np.fromfunction(lambda n:n+1, (5,), dtype='int32')

def add_it(r, c):
    return r + c

a = np.fromfunction(add_it, (3, 3), dtype='int32')
a = np.fromfunction(lambda r,c:r+c, (3, 3), dtype='int')
a = np.fromfunction(lambda r, c: 1, (3, 3), dtype='int')

import numpy as np

def multy(r, c):
    return (r + 1) * (c+1)

a = np.fromfunction(multy, (10, 10), dtype=np.int16)
a = np.fromfunction(lambda r,c: (r+1) * (c+1),
                    (10, 10), dtype=np.int16)
```

```python
>>> print(a)
[[ 1  2  3  4  5  6  7  8  9 10]
 [ 2  4  6  8 10 12 14 16 18 20]
 [ 3  6  9 12 15 18 21 24 27 30]
 [ 4  8 12 16 20 24 28 32 36 40]
 [ 5 10 15 20 25 30 35 40 45 50]
 [ 6 12 18 24 30 36 42 48 54 60]
 [ 7 14 21 28 35 42 49 56 63 70]
 [ 8 16 24 32 40 48 56 64 72 80]
 [ 9 18 27 36 45 54 63 72 81 90]
 [10 20 30 40 50 60 70 80 90 100]]
```

```python
>>> print(s)
   1   2   3   4   5   6   7   8   9  10
   2   4   6   8  10  12  14  16  18  20
   3   6   9  12  15  18  21  24  27  30
   4   8  12  16  20  24  28  32  36  40
   5  10  15  20  25  30  35  40  45  50
   6  12  18  24  30  36  42  48  54  60
   7  14  21  28  35  42  49  56  63  70
   8  16  24  32  40  48  56  64  72  80
   9  18  27  36  45  54  63  72  81  90
  10  20  30  40  50  60  70  80  90 100
```

my_array *= 2   # Double each element of my_array

```python
import numpy as np

A = np.array([[0, 1, 2, 3], [4, 5, 6, 7],
           [8, 9, 10, 11], [12, 13, 14, 15]])
print(A)

A = np.arange(16).reshape((4,4))

A = np.fromfunction(lambda r, c: r*4 + c, (4, 4))

>>> print(C.reshape((2,8)))

[[  1.   4.   9.  16.  25.  36.  49.  64.]
 [ 81. 100. 121. 144. 169. 196. 225. 256.]]

>>> A = np.arange(51)
>>> print(A)
[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16
 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33
 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50]

>>> A[1] = 0
>>> A[2 * 2::2] = 0
>>> print(A)
[ 0  0  2  3  0  5  0  7  0  9  0 11  0 13  0 15  0
 17  0 19  0 21  0 23  0 25  0 27  0 29  0 31  0 33
  0 35  0 37  0 39  0 41  0 43  0 45  0 47  0 49  0]
```

```python
>>> A[3 * 3::3] = 0
[ 0 0 2 3 0 5 0 7 0 0 0 11 0 13 0 0 0
17 0 19 0 0 0 23 0 25 0 0 0 29 0 31 0 0
0 35 0 37 0 0 0 41 0 43 0 0 0 47 0 49 0]
[ 0 0 2 3 0 5 0 7 0 0 0 11 0 13 0 0 0
17 0 19 0 0 0 23 0 0 0 0 0 29 0 31 0 0
0 0 0 37 0 0 0 41 0 43 0 0 0 47 0 0 0]
my_prime_list = [i for i in A if i > 0]
>>> P = A[A > 0]
>>> print(P)
[ 2 3 5 7 11 13 17 19 23 29 31 37 41 43 47]
>>> A = np.arange(1,17).reshape((4,4))
>>> print(A)
[[ 1 2 3 4]
 [ 5 6 7 8]
 [ 9 10 11 12]
 [13 14 15 16]]
neighbor_count = np.sum(G[1:4, 1:4]) - G[2, 2]
array_name[bool_array] # Use bool_array as a mask.
B2 = (B > 2) & (B < 7) # "AND" operation
>>> print(B[ (B == 1) | (B > 6)]) # "OR" operation
[1 7 8 9]
```

```python
def sieve(n):
    b_list = [True] * (n + 1)
    for i in range(2, n+1):
        if b_list[i]:
            for j in range(i*i, n+1, i):
                b_list[j] = False
    primes = [i for i in range(2, n+1) if b_list[i]]
    return primes

import numpy as np

def np_sieve(n):
    # Create B, setting all elements to True.
    B = np.ones(n + 1, dtype=np.bool)
    B[0:2] = False
    for i in range(2, n + 1):
        if B[i]:
            B[i*i: n+1: i] = False
    return np.arange(n + 1)[B]
```

```python
import numpy as np
import time
def np_sieve(n):
    t1 = time.time() * 1000
    B = np.ones(n + 1, dtype=np.bool)
    B[0:2] = False
    for i in range(2, n + 1):
        if B[i]:
            B[i*i: n+1: i] = False
    P = np.arange(n + 1)[B]
    t2 = time.time() * 1000
    print('np_sieve took', t2-t1, 'milliseconds.')

[0.49353738 0.88551261 0.69064065 0.93626092
 0.17818198 0.16637079 0.55144244 0.16262533
 0.36946706 0.61859074]
>>> np.sum(A)
93
>>> np.std(A)     # standard deviation
4.780603169754489
B = np.fromfunction(lambda r,c: c, (4, 5),
                    dtype=np.int32)
```

```python
>>> np.sum(B, axis = 0)    # row, totaling cols.
array([ 0, 4, 8, 12, 16])
>>> np.sum(B, axis = 1)    # col, totaling rows.
array([10, 10, 10, 10])

B = np.fromfunction(lambda r,c:c, (4, 5), dtype=np.int32)

B_rows = np.sum(B, axis = 1)

>>> arr = np.arange(15).reshape(3, 5)
>>> print(arr)
[[ 0  1  2  3  4]
 [ 5  6  7  8  9]
 [10 11 12 13 14]]
```

```python
import numpy as np
A = np.linspace(0, np.pi, 10)
B = np.sin(A)
```

# numpy.info(numpy.function_name)

```python
>>> A = np.linspace(0, np.pi, 10)
>>> B = np.sin(A, dtype='float16')
>>> print(B)
[0.000e+00 3.420e-01 6.431e-01 8.657e-01 9.849e-01
 9.849e-01 8.662e-01 6.431e-01 3.416e-01 9.675e-04]
```

```python
>>> B = np.sin(A)
>>> print(' '.join(format(x, '5.3f') for x in B))
0.000 0.342 0.643 0.866 0.985 0.985 0.866 0.643 0.342
0.000
```

```python
import numpy as np
import matplotlib.pyplot as plt
```

```python
import numpy as np
import matplotlib.pyplot as plt
```

```python
A = np.linspace(0, 2 * np.pi)
plt.plot(A, np.sin(A))
plt.show()
```

```python
import numpy as np
import matplotlib.pyplot as plt

A = np.linspace(0, 2 * np.pi)

import numpy as np
import matplotlib.pyplot as plt

A = np.linspace(0, 2 * np.pi)
plt.plot(A, np.cos(A))
plt.show()

(0.1, 10.0), (0.2, 5.0), (0.3, 3.3)...
plt.plot([0, 1, 2, 3, 4], [1, 2, 4, 5, 3])
plt.plot([3, 4, 1, 5, 2, 3], [4, 1, 3, 3, 1, 4])
(3, 4), (4, 1), (1, 3), (5, 3), (2, 1), (3, 4)
import numpy as np
import matplotlib.pyplot as plt

A = np.linspace(-15, 20)
plt.plot(A, A ** 3 - (15 * A ** 2) + 25)
plt.show()
```

```python
import numpy as np
import matplotlib.pyplot as plt

A = np.linspace(0, 2 * np.pi)
plt.plot(A, np.sin(A))
plt.plot(A, np.cos(A))
plt.show()

plt.plot(A, np.sin(A), A, np.cos(A))

plt.plot( X1, Y1, [fmt1,] X2, Y2, [fmt2,] ... )

plt.plot(A, np.sin(A), A, np.cos(A), 'o')
plt.show()

plt.plot(A, np.sin(A), '^', A, np.cos(A), 'o')
plt.title('Sine and Cosine')
plt.xlabel('X Axis')
plt.ylabel('Y Axis')
plt.show()

'b^'   # Use blue triangles.

A = np.linspace(0, 60, 60)

2 * A   # Formula for increase of $2 a year
1.1 ** A   # Formula for growth of 10% a year

plt.plot(A, 2 * A, 'o', A, 1.1 ** A)
```

```python
plt.plot(A, 2 * A, 'o') # +$2 a year (with circles)
plt.plot(A, 1.1 ** A) # Compound 10% a year

plt.title('Compounded Interest v. Linear')
plt.xlabel('Years')
plt.ylabel('Value of Funds')
plt.show()

IQ_list = [91, 110, 105, 107, 135, 127, 92, 111, 105,
    106, 130, 145, 145, 128, 109, 108, 98, 129, 100,
    108, 114, 119, 99, 137, 142, 145, 112, 113 ]

import numpy as np
import matplotlib.pyplot as plt

IQ_A = np.array(IQ_list)

plt.hist(IQ_A)
plt.title('IQ Distribution of Development Team.')
plt.show()

plt.hist(A [, bins=10] [, keyword_args] )

plt.hist(A, bins=50) # Place results into 50 bins.

A = np.random.standard_normal(200000)
A = A * 10 + 100
```

