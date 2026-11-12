is the seasonal sum operator of the order $s$. The variance of the noise $\epsilon_t$, $\sigma_\epsilon^2$, governs the variability of the seasonal coefficients and the seasonal pattern is deterministic if $\sigma_\epsilon^2 = 0$. The irregular component $I_t$ follows a regular ARMA model with innovation $a_t$,

```
\(\phi(B)I_t = \theta(B)a_t.\)
(2.60)
```

The innovations $v_t, u_t, \epsilon_t,$ and $a_t$ are independent white noise processes. Equations (2.57)-(2.60) jointly form the structural time series representation. In the particular case of $I_t = a_t$, the structural model contains only four parameters, which are the variances of the four innovations.

It is easy to see that any structural time series model has an implied ARIMA representation. For instance, assume $I_t = a_t$ for simplicity. Taking the second difference of (2.57) and using Eqs. (2.58)-(2.60), we have

```
\(\nabla^2 z_t = u_t + \nabla v_t + \nabla^2 S_t + \nabla^2 a_t.\)
(2.61)
```

Noting that

```
\(\nabla_s \equiv 1 - B^s = (1-B)(1 + B + \cdots + B^{s-1}) = (1-B)S_s(B),\)
```

and applying $S_s(B)$ to Eq. (2.61), we further obtain

```
\(\nabla \nabla_s z_t = S_s(B)u_t + \nabla_s v_t + \nabla^2 \epsilon_t + \nabla \nabla_s a_t,\)
```

which says that $\nabla \nabla_s z_t$ is the sum of four finite-order MA processes with the maximum order \(s+1\) and, hence, can be written as an MA(\(s+1\)) model. In fact, under certain conditions, the structural model series \(z_t\) would follow an airline model, i.e. an ARIMA(\(0, 1, 1\))_s_(0, 1, 1)_1\) model. See Proietti (2000) for an analysis of the relation between seasonal structural patterns and seasonal ARIMA models.

# 2.5.2 State-Space Models

A general framework for linear time series models is the State-Space Model (SSM). Indeed, the structural time series models are special cases of the SSM and an ARMA model can also be transformed into an SSM. Assume that we observe a vector of time series \(z_t\) that can be represented by means of an observation equation:

```
\(z_t = H_t \alpha_t + \epsilon_t,\)
(2.62)
```

where \(z_t\) is a \(k \times 1\) vector of observations, \(H_t\) is a \(k \times p\) matrix assumed to be known for all \(t\), \(\alpha_t\) is an unobserved \(p \times 1\) state vector and \(\epsilon_t\) is a white noise process that, for simplicity, follows the multivariate distribution \(N(\mathbf{0}, V_t)\), where \(V_t\) denotes the covariance matrix. The state vector \(\alpha_t\) is assumed to evolve over time according to the following state transition equation

```
\(\alpha_t = \Omega_t \alpha_{t-1} + u_t,\)
(2.63)
```

where \(\Omega_t\) is a known \(p \times p\) transition matrix and \(u_t\) is another white noise process with distribution \(N_p(\mathbf{0}, \mathbf{R}_t)\) and independent of \(\epsilon_t\).
Jointly Eqs. (2.62) and (2.63) are referred to as an SSM. The concept of state might be foreign at the first glance, but it will become clear and meaningful once we