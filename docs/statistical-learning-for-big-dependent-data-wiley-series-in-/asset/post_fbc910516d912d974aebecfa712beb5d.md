
Based on the discussion of multiplicative seasonal ARIMA models, it is clear that the models are derived under some relatively strong assumptions. For instance, the serial dependence of each column in the two-way table is the same for all columns and the serial dependence of each row is also the same for all rows. In addition, the row and column dependences are orthogonal. While the model is parsimonious in parameterization and has been found useful in many applications, one should keep in mind that there exist situations in which such highly structured models may not be adequate. Care needs to be exercised in analyzing seasonal time series.

### 2.4.3.1 The Airline Model

The simplest multiplicative seasonal ARIMA model is the ARIMA(0, 1, 1)\(_s \times (0, 1, 1)\), which assumes the form

```
\(\nabla_s \nabla z_t = (1 - \theta_1 B)(1 - \Theta_1 B^s)a_t, \quad (2.55)\)
```

where \(-1 < \theta_1, \Theta_1 < 1\). This model is referred to as the *Airline model*, because it was first proposed by Box and Jenkins to model monthly number of airlines passengers. See Box et al. (2015). It has been found to be widely applicable in modeling and forecasting seasonal time series. One possible explanation is that the model, in effect, consists simultaneously of a regular and a seasonal exponential smoothing. For simplicity, assume monthly series with \(s = 12\). In this case, the seasonal part of the model in Eq. (2.55) can be written as

```
\((1 - B^{12})u_t = (1 - \Theta_1 B^{12})a_t, \quad (2.56)\)
```

where \(u_t = [(1 - B)/(1 - \theta_1 B)]z_t\). By repeated substitutions of Eq. (2.56), we see that

```
\(u_t = (1 - \Theta_1)(u_{t-12} + \Theta_1 u_{t-24} + \Theta_1^2 u_{t-36} + \cdots) + a_t,\)
```

from which the 1-step ahead prediction of \(u_t\) at the forecast origin \(t-1\) is

```
\(\hat{u}_{t-1}(1) = (1 - \Theta_1) \sum_{i=1}^{\infty} \Theta_1^{i-1} u_{t-12i},\)
```

indicating that \(u_t\) is a weighted average of its past lagged values at lags \(t - 12, t - 24, \dots\). Therefore, \(u_t\) follows a seasonal exponential smoothing model with discounting rate \(\Theta_1\). Next, from \(u_t = [(1 - B)/(1 - \theta_1 B)]z_t\), we have \((1 - B)z_t = (1 - \theta_1 B)u_t\), which shows that \(z_t\) follows an exponential smoothing model with discounting rate \(\theta_1\) and innovation \(u_t\). Consequently, the Airline model in Eq. (2.55) can be thought of as applying a regular exponential smoothing model with discounting rate \(\theta_1\) on top of a seasonal exponential smoothing model with discounting rate \(\Theta_1\). The model is a double-exponential smoothing model.

It is also of interest to study the ACF of the stationary part of the Airline model. Let \(w_t = (1 - B^s)(1 - B)z_t\) be the differenced series. Then, \(w_t = (1 - \theta_1 B)(1 - \Theta_1 B^s)a_t\). It is easy to see that the ACFs of \(w_t\) are given by

```
\(\rho_\ell = \begin{cases} \frac{-\theta_1^2}{1 + \theta_1^2}, & \text{if } \ell = 1, \\ \frac{-\Theta_1}{1 + \Theta_1^2}, & \text{if } \ell = s, \\ \rho_1 \rho_s, & \text{if } \ell = s - 1 \text{ or } s + 1, \\ 0, & \text{otherwise.} \end{cases}\)
```

Therefore, the lag-1 ACF is determined by the regular part of the model, the lag-s ACF is determined by the seasonal part of the model, and the ACFs at lags s − 1 and s + 1 can be thought of as interactions between the regular and the seasonal parts of the model. The ACFs also signify the parsimonious parameterization of the Airline model as it employs two coefficients θ₁ and Θ₁ yet it has four non-zero ACFs.

### Example 2.4

We illustrate seasonal models with data of consumer price indexes in Europe from file CPIEurope2000-15.csv. One of the seasonal time series is the ninth series that corresponds to Spain, and Figure 2.16 presents some plots of the series illustrating the need to apply a regular and a seasonal difference to transform the series into stationarity. The program sarimaSpec indicates that this series is seasonal and can well be modeled by an Airline model. The R commands are given next.

```
> x0 <- read.csv2("CPIEurope2000-15.csv")
> x1=log(x0[, 9])
> y1=diff(x1)
> par(mfrow=c(3,2))
> ts.plot(x1, main="log CPI in Spain")
> ts.plot(y1, main="first difference of log CPI Spain")
> acf(x1, main="ACF log CPI in Spain")
> acf(y1, main="ACF first difference of log CPI Spain")
```

![](img/d4c84ae3c2178f4c506af3dd2ed13122_91_0.png)

Figure 2.16 Plots and ACF of different transformations of the series of Consumer Price Index in Spain.

```
> y2=diff(y1,lag=12)
> ts.plot(y2, main="first differences of order 1 and 12 log CPI Spain")
> acf(y2, main="first differences of order 1 and 12 log CPI Spain")
> sarimaSpec(x1)
Selected differencing order (d,D): 1 1
Selected order (p,d,q,P,D,Q): 0 1 1 0 1 1
minimum criterion: -1457.766
```

## 2.5 STRUCTURAL AND STATE SPACE MODELS

### 2.5.1 Structural Time Series Models

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

which says that $\nabla \nabla_s z_t$ is the sum of four finite-order MA processes with the maximum order \(s+1\) and, hence, can be written as an MA(\(s+1\)) model. In fact, under certain conditions, the structural model series \(z_t\) would follow an airline model, i.e. an ARIMA(\(0, 1, 1\))_s \times (0, 1, 1)_1\) model. See Proietti (2000) for an analysis of the relation between seasonal structural patterns and seasonal ARIMA models.

### 2.5.2 State-Space Models

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

where \(\Omega_t\) is a known \(p \times p\) transition matrix and \(u_t\) is another white noise process with distribution \(N_p(\mathbf{0}, \mathbf{R}_t)\) and independent of \(\epsilon_t\). Jointly Eqs. (2.62) and (2.63) are referred to as an SSM. The concept of state might be foreign at the first glance, but it will become clear and meaningful once we
