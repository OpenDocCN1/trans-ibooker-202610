provided that the covariances involved all exist, where \(\ell\) is an integer. We say that the vector process \(\{z_t\}\) is weakly stationary if (a) \(\mu_t = \mu\), a constant vector, and (b) \(\Gamma(t, t - \ell)\) depends only on \(\ell\). In other words, a \(k\)-dimensional stochastic process \(z_t\) is weakly stationary if its first two moments are time-invariant. In this case, we write \(\Gamma(t, t - \ell) = E[(z_t - \mu)(z_{t-\ell} - \mu)'] = \Gamma(\ell) = [\gamma_{ij}(\ell)]\), where \(1 \le i, j \le k\). In particular, the lag-0 autocovariance matrix of a weakly stationary \(k\)-dimensional stochastic process \(z_t\) is given by

\(\Gamma(0) = \begin{bmatrix} \gamma_{11}(0) & \gamma_{12}(0) & \cdots & \gamma_{1k}(0) \\ \gamma_{21}(0) & \gamma_{22}(0) & \cdots & \gamma_{2k}(0) \\ \vdots & \vdots & \ddots & \vdots \\ \gamma_{k1}(0) & \gamma_{k2}(0) & \cdots & \gamma_{kk}(0) \end{bmatrix}\) (1.6)

which is symmetric, because \(\gamma_{ij}(0) = \gamma_{ji}(0)\). Note that the diagonal element \(\gamma_{ii}(0) = \sigma_i^2 = \text{Var}(z_{it})\).

For a stationary vector process \(z_t\), its lag-\(\ell\) autocovariance matrix \(\Gamma(\ell) = [\gamma_{ij}(\ell)]\) measures the linear dependence among the component series \(z_{it}\) and their lagged variables \(z_{j,t-\ell}\). It pays to study the exact meaning of individual element \(\gamma_{ij}(\ell)\) of \(\Gamma(\ell)\). First, the diagonal element \(\gamma_{ii}(\ell)\) is the lag-\(\ell\) autocovariance of the scalar process \(z_{it}\) (\(i = 1, \dots, k\)). Second, the off-diagonal element \(\gamma_{ij}(\ell)\) is

\(\gamma_{ij}(\ell) = E[(z_{it} - \mu_i)(z_{j,t-\ell} - \mu_j)]\),

which measures the linear dependence of \(z_{it}\) on the lagged value \(z_{j,t-\ell}\) for \(\ell > 0\). Thus, \(\gamma_{ij}(\ell)\) quantifies the linear dependence of \(z_{it}\) on the \(\ell\)th past value of \(z_{jt}\). Third, in general, \(\Gamma(\ell)\) is not symmetric if \(\ell > 0\). As a matter of fact, \(\gamma_{ij}(\ell) \neq \gamma_{ji}(\ell)\) for most stochastic process \(z_t\) because, as stated before, \(\gamma_{ij}(\ell)\) denotes the linear dependence of \(z_{it}\) on \(z_{j,t-\ell}\), whereas \(\gamma_{ji}(\ell)\) quantifies the linear dependence of \(z_{jt}\) on \(z_{i,t-\ell}\). Finally, we have

\(\gamma_{ij}(-\ell) = E[(z_{it} - \mu_i)(z_{j,t+\ell} - \mu_j)]\)
\(= E[(z_{j,v} - \mu_j)(z_{i,v-\ell} - \mu_i)] \quad (v = \text{t} + \ell)\)
\(= \gamma_{ji}(\ell). \quad \text{(by stationarity)}\)

Therefore, we have \(\gamma_{ij}(-\ell) = \gamma_{ji}(\ell)\) for all \(\ell\). Consequently, the autocovariance matrices of a stationary vector process \(z_t\) satisfy

\(\Gamma(-\ell) = [\Gamma(\ell)]'\),

and, hence, it suffices to consider \(\Gamma(\ell)\) for \(\ell \ge 0\) in practice. If the dimension of \(\Gamma(\ell)\) is needed to avoid any confusion, we write \(\Gamma(\ell) \equiv \Gamma_k(\ell)\) with the subscript \(k\) denoting the dimension of the underlying stochastic process \(z_t\).

A global measure of the overall variability of the process \(z_t\) is given by its generalized variance, which is the determinant of the lag-0 covariance matrix \(\Gamma(0)\), i.e. \(|\Gamma(0)|\). The standardized, by the dimension, measure is the effective variance, defined by

\(\text{EV}(\mathbf{z}_t) = |\Gamma(0)|^{1/k}.\) (1.7)

For instance, if \(\mathbf{z}_t\) is two dimensional, then \(\text{EV}(\mathbf{z}_t) = [\gamma_{11}(0)\gamma_{22}(0)\{1 - \rho_{12}^2(0)\}]^{1/2}\), where \(\rho_{12}(0)\) is the contemporaneous correlation coefficient between \(z_{1t}\) and \(z_{2t}\).