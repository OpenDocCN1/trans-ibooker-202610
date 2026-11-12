```
j_hat = np.array([2, 2])
basis = np.array([i_hat,j_hat]).transpose()
determinant = det(basis)
print(determinant) # 2.0
```

4. Yes, because matrix multiplication allows us to combine several matrices into a single matrix representing one consolidated transformation.

5. x=19.8, y=-5.4, z=-6. Code is below.

# Example B-14.

```
from numpy import array
from numpy.linalg import inv

A = array([
  [3, 1, 0],
  [2, 4, 1],
  [3, 1, 8]
])

B = array([
  54,
  12,
  6
])

X = inv(A).dot(B)

print(X) # [19.8 -5.4 -6. ]
```

6. Yes it is linearly dependent. Although we have some floating point imprecision with NumPy, the determinant is effectively 0.

# Example B-15.

```
from numpy.linalg import det
from numpy import array

i_hat = array([2, 6])
j_hat = array([1, 3])

basis = array([i_hat, j_hat]).transpose()
```