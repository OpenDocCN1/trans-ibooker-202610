taking expectation, we have

```
\(\Gamma_z(j) = \left\{ \begin{array}{ll} \sum_{i=1}^p \phi_i \Gamma_z(j-i) + \Sigma_a & \text{if } j = 0, \\ \sum_{i=1}^p \phi_i \Gamma_z(j-i) & \text{if } j > 0, \end{array} \right.\)
```

where, as before, \(\Gamma_z(j)\) is the lag-\(j\) autocovariance matrix of \(z_t\). Eq. (3.15) is the moment equation of a stationary VAR(\(p\)) model. In particular, considering the equations jointly for \(j = 1, \dots, p\), we have

```
[\Gamma_z(1), \dots, \Gamma_z(p)] = [\phi_1, \dots, \phi_p] \left[ \begin{array}{cccc} \Gamma_z(0) & \Gamma_z(1) & \cdots & \Gamma_z(p-1) \\ \Gamma_z(-1) & \Gamma_z(0) & \cdots & \Gamma_z(p-2) \\ \vdots & \vdots & \ddots & \vdots \\ \Gamma_z(1-p) & \Gamma_z(2-p) & \cdots & \Gamma_z(0) \end{array} \right]
```

which is referred to as the multivariate Yule-Walker Equation and can be used to obtain the AR-coefficient matrices \(\phi_j\) given the autocovariance matrices \(\Gamma_z(j)\). For instance, for an AR(1) we obtain

```
\(\phi_1 = \Gamma_z(1)[\Gamma_z(0)]^{-1}.\)
```

On the other hand, given a stationary VAR(\(p\)) model, we can compute the autocovariance matrices \(\Gamma_z(j)\) recursively via the moment equation in (3.15). The initial values of the recursion can be obtained by solving a system of linear equations. To illustrate, consider a stationary (zero-mean) VAR(1) model, \(z_t = \phi_1 z_{t-1} + a_t\). Taking the covariance on both sides of the model and noting that \(a_t\) is uncorrelated with \(z_{t-1}\), we have

```
\(\Gamma_z(0) = \phi_1 \Gamma_z(0) \phi_1' + \Sigma_a.\)
```

Consequently,

```
\(\text{vec}[\Gamma_z(0)] = (\phi_1 \otimes \phi_1)\text{vec}[\Gamma_z(0)] + \text{vec}(\Sigma_a),\)
```

and

```
\(\text{vec}[\Gamma_z(0)] = [I - \phi_1 \otimes \phi_1]^{-1} \text{vec}(\Sigma_a),\)
```

where \(I\) is a \(k^2 \times k^2\) identity matrix and \(\otimes\) denotes the Kronecker product. This is a generalization of the scalar case for which \(\text{var}(z_t) = \sigma_a^2 / (1 - \phi_1^2)\). With \(\Gamma_z(0)\) given, one can compute \(\Gamma_z(j)\) recursively via the moment equation in (3.15) for a VAR(1) model.

Since a \(k\)-dimensional VAR(\(p\)) model can always be rewritten as a \(kp\)-dimensional VAR(1) model, the prior discussion applies to all stationary VAR(\(p\)) models. Specifically, let \(Z_t = (z_t', z_{t-1}', \dots, z_{t-p+1}')'\) be a \(kp\)-dimensional staggered time series. Then, it is easy to see that

```
Z_t = \left[ \begin{array}{ccccc} \phi_1 & \phi_2 & \cdots & \phi_{p-1} & \phi_p \\ I & 0 & \cdots & 0 & 0 \\ 0 & I & \cdots & 0 & 0 \\ \vdots & \vdots & \ddots & \vdots & \vdots \\ 0 & 0 & \cdots & I & 0 \end{array} \right] Z_{t-1} + \left[ \begin{array}{c} a_t \\ 0 \\ \vdots \\ 0 \\ 0 \end{array} \right],
```