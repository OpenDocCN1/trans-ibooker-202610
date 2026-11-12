```
```
> y2=diff(y1,lag=12)
> ts.plot(y2, main="first differences of order 1 and 12 log CPI Spain")
> acf(y2, main="first differences of order 1 and 12 log CPI Spain")
> sarimaSpec(x1)
Selected differencing order (d,D): 1 1
Selected order (p,d,q,P,D,Q): 0 1 1 0 1 1
minimum criterion: -1457.766
```
```

# ## 2.5 STRUCTURAL AND STATE SPACE MODELS

# ### 2.5.1 Structural Time Series Models

An alternative approach to modeling nonstationary linear time series is to postulate that the series is the sum of a deterministic component, a cyclical component and an irregular component. The deterministic component may contain a time trend, which is often assumed to be linear. The cyclical component consists of some harmonic functions or dummy variables, and the irregular component assumes the form of an ARMA model. This approach is referred to as the *structural approach* and the entertained model *the structural model*. See Harrison and Stevens (1976), West and Harrison (2006), and Harvey (1990), among others. A structural time series model assumes the form

```
$$z_t = \mu_t + S_t + I_t,$$ (2.57)
```

where \(\mu_t\) represents the trend of \(z_t\), \(S_t\) is the cyclical (or seasonal) component and \(I_t\) is the irregular component, which may be a white noise. The functional form of each component of \(z_t\) in Eq. (2.57) depends on the applications, but some general functions are available. A typical formulation of the trend is

```
$$\mu_t = \mu_{t-1} + \beta_{t-1} + v_t,$$ (2.58)

$$\beta_t = \beta_{t-1} + u_t,$$
```

where \(\mu_t\) is the trend level, \(\beta_t\) denotes the trend growth rates, and \(u_t\) and \(v_t\) are two white noise series, which are often assumed to be independent. If the variances of \(u_t\) and \(v_t\) are zero, the trend has a constant growth rate, because \(\beta_t\) is then a constant. In general, the model in Eq. (2.58) represents a stochastic quadratic trend, which is sufficiently flexible in most applications. For instance, if \(\beta_t = 0\) for all \(t\) and the variance of \(v_t\) is positive, then the trend of \(z_t\) follows a random walk.

The cyclical or seasonal component is usually modeled using a stochastic seasonal summation function. Let \(S_t\) be the seasonal component at time index \(t\) and assume that the seasonality is \(s\), then \(S_t\) follows the model

```
$$\sum_{j=0}^{s-1} S_{t-j} = S_s(B)S_t = \epsilon_t,$$ (2.59)
```

where

```
$$S_s(B) = 1 + B + \cdots + B^{s-1},$$
```