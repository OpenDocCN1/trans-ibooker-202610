```
X_1 = np.vstack([X, np.ones(len(X))]).T

# Extract output column (all rows, last column)
Y = df.values[:, -1]

# Calculate coefficients for slope and intercept
b = inv(X_1.transpose() @ X_1) @ (X_1.transpose() @ Y)
print(b) # [1.93939394 4.73333333]

# Predict against the y-values
y_predict = X_1.dot(b)
```

It is not intuitive, but note above we have to stack a “column” of 1’s next to our x column. The reason for this is this will generate the intercept \(\beta_0\) coefficient. Since this column is all 1’s it effectively generates the intercept rather and not just a slope \(\beta_1\).

When you have a lot of data with a lot of dimensions, computers can start to choke and produce unstable results. This is a use case for matrix decomposition which we learned about in Chapter 4 on linear algebra. In this specific case, we take our matrix X, append an additional column of 1’s to generate the intercept \(\beta_0\) just like before, and then decompose it into two component matrices Q and R.

\(X = Q \cdot R\)

Avoiding more calculus rabbit holes, here is how we use Q and R to find the beta coefficient values in the matrix form b.

\(b = R^{-1} \cdot Q^T \cdot y\)

And here is how we use the QR decomposition formula above in Python using NumPy to perform a linear regression.

# Example 5-7. Using QR decomposition to perform a linear regression

```
import pandas as pd
from numpy.linalg import qr, inv
import numpy as np

# Import points
df = pd.read_csv('https://bit.ly/3goOAnt', delimiter=",,")
```