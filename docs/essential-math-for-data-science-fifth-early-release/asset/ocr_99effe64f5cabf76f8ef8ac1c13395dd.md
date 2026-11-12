= \(\begin{bmatrix} 1 & -1 \\ 1 & 0 \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix}\)

To execute this in Python using NumPy, you can combine the two matrices simply using the `matmul()` or `@` operator (Example 4-11). We will then turn around and use this consolidated tranformation and apply it to a vector `[1 2]`.

# Example 4-11.

```
```python
from numpy import array

# Transformation 1
i_hat1 = array([0, 1])
j_hat1 = array([-1, 0])
transform1 = array([i_hat1, j_hat1]).transpose()

# Transformation 2
i_hat2 = array([1, 0])
j_hat2 = array([1, 1])
transform2 = array([i_hat2, j_hat2]).transpose()

# Combine Transformations
combined = transform2 @ transform1

# Test
print("COMBINED MATRIX:\n {}".format(combined))

v = array([1, 2])    # [-1 1]
print(combined.dot(v)) # [-1 1]
```
```

> # USING DOT() VS MATMULT() AND @
> 
> In general, you want to prefer `matmul()` and its shorthand `@` to combine matrices rather than the `dot()` operator in NumPy. The former generally has a preferable policy for higher-dimensional matrices and how the elements are broadcasted.
> 
> If you like diving into these kinds of implementation details, this [StackOverflow](https://stackoverflow.com/) question is a good place to start.

Note that we could have also applied each tranformation individually to vector \(\vec{v}\) and still have gotten the same result. If you replace the last line