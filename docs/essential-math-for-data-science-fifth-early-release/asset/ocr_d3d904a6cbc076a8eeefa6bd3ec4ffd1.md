```
# Extract input variables (all rows, all columns but last column)
X = df.values[:, :-1].flatten()

# Add placeholder "1" column to generate intercept
X_1 = np.vstack([X, np.ones(len(X))]).transpose()

# Extract output column (all rows, last column)
Y = df.values[:, -1]

# calculate coefficients for slope and intercept
# using QR decomposition
Q, R = qr(X_1)
b = inv(R) . dot(Q.transpose() ) . dot(Y)

print(b) # [1.93939394 4.73333333]
```

Typically, QR decomposition is the method used by many scientific libraries for linear regression because it copes with large amounts of data more easily and is more stable. What do I mean by _stable_? **Numerical stability** is how well an algorithm keeps errors minnimzed, rather than amplifying errors in approximations. Remember that computers only work to so many decimal places and have to approximate, so it becomes important our algorithms do not deterioriate with compounding errors in those approximations.

https://en.wikipedia.org/wiki/Numerical_stability

> > **OVERWHELMED?**
> > If you find these linear algebra examples of linear regression overwhelming, do not worry! I just wanted to provide exposure to a practical use case for linear algebra. Going forward we will focus on other techniques you can utilize.

# # Gradient Descent

**Gradient descent** is an optimization technique that uses derivatives and iterations to minimize/maximize a set of parameters against an objective. To learn about gradient descent, let’s do a quick thought experiment and then apply it on a simple example.