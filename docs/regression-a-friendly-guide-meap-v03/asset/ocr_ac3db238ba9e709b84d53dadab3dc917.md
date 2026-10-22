Var(A) and Var(B), respectively.

To get the variance-covariance matrix for the logistic model chasm_fit, use its cov_params method:

```
chasm_fit.cov_params()
```

Intercept  Photoperiod
Intercept  147.190287  -10.920255
Photoperiod  -10.920255  0.810638

This matrix tells us three things:

+   - Var(a) = 147.190287, so that SE(a) = √147.190287 = 12.1322

+   - Var(b) = 0.810638, so that SE(b) = √0.810638 = 0.9004

+   - Cov(a, b) = -10.920255

Now we’re in business! To compute the standard errors for the predicted Link values, we’ll use the last formula in the sidebar, replacing C with this covariance matrix. Here are the calculation and the results:

```
links["SE"] = np.sqrt( np.diag( np.dot( np.dot(X, chasm_fit.cov_p X.T)))
```