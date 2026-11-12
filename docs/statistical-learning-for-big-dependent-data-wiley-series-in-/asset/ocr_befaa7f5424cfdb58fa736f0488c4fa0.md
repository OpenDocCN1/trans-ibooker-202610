Clearly, EV($z_t$) assumes its largest value when the two series are uncorrelated, i.e. \(\rho_{12}(0) = 0\). For a \(k\)-dimensional process $z_t$, as the determinant of a square-matrix is the product of its eigenvalues, the effective variance is the geometrical mean of the eigenvalues of \(\Gamma(0)\).

We can summarize the properties of a stationary vector process $z_t$ by using linear combinations of its components. Consider, for example, the linear process

```
$$y_t = c'z_t = \sum_{i=1}^k c_i z_{it}$$
```

where $c = (c_1, ..., c_k)' \neq 0$. This process is stationary because it is a finite linear combination of the stationary components $z_{it}$ ($i = 1, ..., k$). More specifically, the expectation is $E(y_t) = c_1 E(z_{1t}) + ... + c_k E(z_{kt}) = c'E(z_t) = c'\mu$, which is time-invariant. Similarly, the variance of $y_t$ is given by $\text{Var}(y_t) = \text{Var}(c'z_t) = c'\text{Var}(z_t)c = c'\Gamma(0)c$, which is also time-invariant. In general, we have $\text{Cov}(y_t, y_{t-\ell}) = c'\Gamma(\ell)c$, which depends only on $\ell$. Thus, $y_t$ is stationary. In addition, since $\text{Var}(y_t)$ is nonnegative and $c$ is an arbitrary non-zero vector, we conclude that $\Gamma(0)$ is nonnegative definite.

Similar to the scalar case, the autocovariance matrices in Eq. (1.5) of a stationary vector process $z_t$ must satisfy certain conditions. To see this, we consider a non-zero linear combination of $z_t$ and its $m$ lagged values, say,

```
$$y_t = \sum_{i=1}^m b'_i z_{t-i+1} = b'_1 z_t + b'_2 z_{t-1} + \cdots + b'_m z_{t-m+1}$$
```

where $b_j = (b_{j1}, ..., b_{jk})'$ is a $k$-dimensional real-valued vector for $j = 1, ..., m$. Defining $b = (b'_1, b'_2, ..., b'_m)'$, which is a non-zero $km$-dimensional constant vector, and $z_{t,m} = (z'_t, z'_{t-1}, ..., z'_{t-m+1})'$, we have $\text{Var}(y_t) = b'\text{Var}(z_{t,m})b = b'M_z(m)b$, where

```
$$M_z(m) = \begin{bmatrix} \Gamma(0) & \Gamma(1) & \cdots & \Gamma(m-1) \\ \Gamma(-1) & \Gamma(0) & \cdots & \Gamma(m-2) \\ \vdots & \vdots & \ddots & \vdots \\ \Gamma(-m+1) & \Gamma(-m+2) & \cdots & \Gamma(0) \end{bmatrix} \quad (1.8)$$
```

is a nonnegative block Toeplitz matrix. This $M_z(m)$ matrix is the generalization of $M_z(m)$ in Eq. (1.3) for the scalar series $z_t$.

The cross-correlation matrices (CCMs) of $z_t$ are defined as the autocovariances of the standardized process $x_t = D^{-1/2}(z_t - \mu)$, where the matrix $D$ is defined as $\text{diag}(\gamma_{11}(0), ..., \gamma_{kk}(0))$, consisting of the variances of the components of $z_t$. Specifically, the lag-\(\ell\) CCM of $z_t$ is

```
$$R_z(\ell) = \text{Cov}(x_t, x_{t-\ell}) = D^{-1/2} \text{Cov}(z_t, z_{t-\ell}) D^{-1/2}$$
```

```
$$= \begin{bmatrix} \rho_{11}(\ell) & \rho_{12}(\ell) & \cdots & \rho_{1k}(\ell) \\ \rho_{21}(\ell) & \rho_{22}(\ell) & \cdots & \rho_{2k}(\ell) \\ \vdots & \vdots & \ddots & \vdots \\ \rho_{k1}(\ell) & \rho_{k2}(\ell) & \cdots & \rho_{kk}(\ell) \end{bmatrix} \quad (1.9)$$
```