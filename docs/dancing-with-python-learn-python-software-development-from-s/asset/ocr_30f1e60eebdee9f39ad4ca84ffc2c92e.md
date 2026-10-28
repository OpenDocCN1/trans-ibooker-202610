Using summation notation and generalizing to $n$ points, we have

$$m = \frac{\sum_{j=1}^{n}(x_j-\mu_x)(y_j-\mu_y)}{\sum_{j=1}^{n}(x_j-\mu_x)^2} = \frac{n\sum_{j=1}^{n}x_jy_j-\left(\sum_{j=1}^{n}x_j\right)\left(\sum_{j=1}^{n}y_j\right)}{n\sum_{j=1}^{n}x_j^2-\left(\sum_{j=1}^{n}x_j\right)^2}$$

$$b = \mu_y-m\mu_x$$

Admittedly, that looks complicated. The second definition of $m$ is easy to translate to a Python function using **numpy**:

```
def simple_linear_regression(xs, ys):
    # return the slope and y-intercept of the simple
    # linear regression line

    n = xs.size
    assert n == ys.size

    numer = n * (xs * ys).sum() - xs.sum() * ys.sum()
    denom = n * (xs * xs).sum() - (xs.sum())**2
    slope = numer / denom
    y_intercept = ys.mean() - slope * xs.mean()
    return slope, y_intercept
```

Note how easy it is to express the summations in **numpy**. Now we compute $m$ and $b$, and hence $L$:

```
xs = np.array([2.0, 4.0, 8.0])
ys = np.array([2.0, 5.0, 5.0])

simple_linear_regression(xs, ys)
```

(0.42857142857142855, 2.0)

+   **Exercise 15.27**
+   
+   Why do I have the `assert` in the definition of `simple_linear_regression`?

+   **Exercise 15.28**
+   
+   Does the point $(\mu_x,\mu_y)$ lie on $L$?