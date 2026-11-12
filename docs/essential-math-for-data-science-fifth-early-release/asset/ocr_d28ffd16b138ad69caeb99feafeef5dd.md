# Diagonal Matrix

Similar to the identity matrix is the **diagonal matrix**, which has a diagonal of non-zero values while the rest of the values are 0. Diagonal matrices are desirable in certain computations because they represent simple scalars being applied to a vector space. It shows up in some linear algebra operations.

```
diagonal:
| 4 0 0 |
| 0 2 0 |
| 0 0 5 |
```

# Triangular Matrix

Similar to the diagonal matrix is the **triangular matrix**, which has a diagonal of non-zero values in front of a triangle of values, while the rest of the values are 0.

```
triangular:
| 4 2 9 |
| 0 1 6 |
| 0 0 5 |
```

Triangular matrices are desirable in many numerical analysis tasks, because they typically are easier to solve in systems of equations. It also shows up in certain decomposition tasks like LU Decomposition.

# Sparse Matrix

Occasionally you will run into matrices that are mostly zeroes and have very few non-zero elements. These are called **sparse matrices**. From a pure mathematical standpoint, they are not terribly interesting. But from a computing standpoint, they provide opportunities to create efficiency. If a matrix has mostly 0’s, a sparse matrix implementation will not bother to waste space storing a bunch of 0’s, and instead only keep track of the cells that are non-zero.