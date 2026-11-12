```
>lx=log(x)
>out1=SummaryModel(lx) All series are nonstationary
Number of first-differenced series: 99
```

# ## 2.7.4 Model Identification

This is an important step of the proposed automatic modeling procedure. Its goal is to specify the order (p, q) for nonseasonal series and the orders (p, q) and (P, Q) for seasonal series. Ideally, we would like to try as many candidate models as possible, but due to various issues such as the difficulty of over-fitting and computational time constraint, some restrictions are needed to narrow down the entertained models. To this end, we specify the maximum values of p and q, say p_max and q_max, respectively, and estimate all possible models with 0 <= p <= p_max and 0 <= q <= q_max. Then, we use an information criterion such as Bayesian information criterion (BIC) to perform model selection. For ease in computation, especially for large k, we set (p_max, q_max) = (5,3). For seasonal time series, we also specify the maximum values of P and Q. More specifically, for seasonal time series, we set (p_max, d, q_max) = (2, 1, 3) and (P_max, D, Q_max) = (1, 1, 1) in our procedure. These maximum orders are default values, and they can be changed if needed.

# ## 2.8 ESTIMATION AND INFORMATION CRITERIA

# ### 2.8.1 Conditional Likelihood

Estimation of a specified ARIMA model is often carried out via the conditional ML method under the normality assumption. For AR models, the LS method can be used and is faster. Here we briefly outline the likelihood function approach. For simplicity, we define z'_t = (z_s, z_{s+1}, ..., z_n)' as the vector of data collection from time index s to n of a time series z_t. Suppose we want to estimate a zero-mean AR(p) model for a stationary Gaussian series omega'_1 = (omega_1, ..., omega_T)'. In practice, omega_t might be a differenced series of z_t as omega_t = \nabla^d z_t. For an AR(p) model, we have that, for t > p, the conditional density function f(omega_t|omega'_{t-1}) is normal with mean phi'_p omega^{t-p}_{t-1} and variance sigma^2, where phi_p = (phi_1, ..., phi_p)'. Therefore,

```
$$f(\omega^T_1) = f(\omega_1, ..., \omega_p) \prod_{t=p+1}^T f(\omega_t | \omega^{t-p}_{t-1}).$$
```

Since p is fixed, the likelihood function would be dominated by the second term of the above equation for large T. One can then focus on the conditional likelihood function, namely f(\omega^T_{p+1} | \omega^p_1) to perform estimation. The log likelihood function then becomes

```
$$L_C(\phi_p, \sigma^2) = -\frac{(T-p)}{2} \ln \sigma^2 - \frac{1}{2} \sum_{t=p+1}^T \frac{(\omega_t - \phi'_p \omega^{t-p}_{t-1})^2}{\sigma^2}.$$
```

Define W = [\omega^T_{p+1}, ..., \omega^T_{1-T}]. We can express the conditional log likelihood function in a matrix form as

```
$$L_C(\phi_p, \sigma^2) = -\frac{(T-p)}{2} \ln \sigma^2 - \frac{1}{2\sigma^2}(\omega^T_{p+1} - W\phi_p)'(\omega^T_{p+1} - W\phi_p).$$
```