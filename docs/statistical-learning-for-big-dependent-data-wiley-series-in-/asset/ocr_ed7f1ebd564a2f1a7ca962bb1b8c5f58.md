# TABLE 3.3 Table of Corner Method for the Pre-whitened Series of Sales and Advertising Expenditures of Example 3.1

| | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|
| 0 | 0.983 12 | 0.966 52 | 0.950 20 | 0.934 16 | 0.918 38 | 0.902 88 | 0.887 63 |
| 1 | 1.000 00 | 0.715 45 | 0.570 78 | 0.673 98 | 0.455 87 | 0.217 40 | -0.335 50 |
| 2 | 0.289 44 | -0.060 95 | -0.164 61 | 0.207 73 | 0.066 74 | 0.221 74 | 0.254 38 |
| 3 | 0.144 73 | 0.071 79 | 0.069 66 | 0.080 33 | -0.091 27 | 0.148 08 | -0.200 47 |
| 4 | -0.175 65 | 0.080 85 | 0.005 55 | 0.061 67 | -0.053 40 | 0.016 38 | 0.150 58 |
| 5 | -0.345 42 | 0.104 64 | -0.071 13 | 0.051 03 | -0.020 18 | 0.056 11 | -0.102 78 |
| 6 | -0.083 55 | -0.168 47 | -0.050 55 | 0.018 96 | 0.046 00 | 0.065 61 | 0.049 85 |

The approximated 2-standard error limit is 0.36

# 3.1.2 Multiple Inputs and Multiple Outputs

In some applications of TFM, the input variable or the output variable or both can be multivariate. In such cases, the pre-whitening process becomes tedious. A more direct and simple approach is to apply multivariate time series analysis. In fact, TFMs can be regarded as special cases of the VARMA models. Details are given below.

# 3.2 VECTOR AR MODELS

The vector AR (VAR) models are, arguably, the most widely used multivariate time series models in practice. A VAR(_p_) model for a _k_-dimensional time series **_z_**_<sub>t</sub>_ is

```
\(z_t = \phi_0 + \sum_{i=1}^p \phi_i z_{t-i} + a_t,\) (3.13)
```

where \(\phi_0\) is a _k_-dimensional vector, \(\phi_i\) are \(k \times k\) real matrices, and **_a_**_<sub>t</sub>_ is a white noise series with mean zero and positive-definite covariance matrix \(\Sigma_a > 0\). Clearly, we assume that the order _p_ is nonnegative and \(\phi_p \neq \mathbf{0}\) if _p_ > 0. Let \(\phi(B) = I - \sum_{i=1}^p \phi_i B^i\) be the AR matrix polynomial. The series **_z_**_<sub>t</sub>_ is weakly stationary if all zeros of the determinant equation \(|\phi(B)| = 0\) are greater than 1 in absolute value. Consider a stationary series. Taking expectation of Eq. (3.13), we have

```
\(E(z_t) \equiv \mu = [\phi(1)]^{-1} \phi_0.\)
```

where \(\phi(1) = I - \sum_{i=1}^p \phi_i\), which is nonsingular. Using the prior equation, a stationary VAR(_p_) model can be rewritten as

```
\(\phi(B)(z_t - \mu) = a_t.\) (3.14)
```

This model is often used in finance because \(z_t - \mu\) is the deviation from the mean and a stationary series **_z_**_<sub>t</sub>_ is mean reverting. Post-multiplying Eq. (3.14) by \((z_{t-j} - \mu)'\) and