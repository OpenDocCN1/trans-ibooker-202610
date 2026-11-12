walk with drift, (2.43). By repeated substitutions of \(z_j = c + z_{j-1} + a_j\), for \(1 \le j \le t\), we obtain

```
\(z_t = ct + \sum_{i=1}^t a_t.\)
```

where \(z_0\), the initial value of the process, is assumed to be zero. If \(c \neq 0\), we see that, from Eq. (2.44), \(E(z_t) = ct\), indicating that the mean of \(z_t\) is time-varying with slope \(c\). Furthermore,

```
\(\text{Var}(z_t) = E(a_t + a_{t-1} + a_{t-2} + \cdots + a_1)^2 = \sigma^2 t,\)
```

which converges to infinity as \(t\) increases, indicating that the uncertainty of \(z_t\) increases with \(\sqrt{t}\), which is unbounded. Finally, using

```
\(z_{t+h} = c(t+h) + a_{t+h} + a_{t+h-1} + \cdots + a_1,\)
```

and \(\text{Cov}(t, t+h)=\text{Cov}(z_t, z_{t+h})\) for \(h > 0\), we have

```
\(\text{Cov}(t, t+h) = E[(z_t - ct)(z_{t+h} - c(t+h))] = \sigma^2 t,\)
```

which is also time-varying. As a matter of fact, one can easily see that

```
\(\text{Cov}(t, s) = \text{Cov}(z_t, z_s) = E \left[ \left( \sum_{i=1}^t a_i \right) \left( \sum_{j=1}^s a_j \right) \right] = \sigma^2 \wedge (t, s),\)
```

where \(\wedge(t,s) = \min\{t,s\}\). Consequently, for \(h > 0\), we have

```
\(\text{Cov}(t, t-h) = \sigma^2 (t-h) \neq \text{Cov}(t, t+h).\)
```

The aforementioned properties show that the random walk process \(z_t\) is nonstationary. Furthermore, the autocorrelation coefficients of \(z_t\) are, \(h > 0\),

```
\(\rho(t, t-h) = \frac{t-h}{\sqrt{t(t-h)}} = \sqrt{1 - \frac{h}{t}}.\)
```

The above expression indicates that, for large \(t\), the autocorrelation coefficients of \(z_t\) would approach 1 and they decay approximately in a linear manner as \(h\) increases. Indeed, if we assume that the process starts at the remote past so that \(h/t\) is small, then the function \((1 - h/t)^{1/2}\) can be approximated, using a first-order Taylor expansion, by

```
\(\rho(t, t-h) \approx 1 - \frac{2h}{t},\)
```

indicating that, assuming fixed \(t\), the autocorrelation coefficients is a function of \(h\) and follows roughly a straight line with slope \((-1/2t)\). The property shown in Eq. (2.47) continues to hold for integrated processes, even though the derivation is a bit more complicated. We provide further details in the next section.

# 2.4.2 ARIMA Models

This idea of transforming a nonstationary series into a stationary one by differencing can be generalized to any ARMA process, leading to the ARIMA\((p, d, q)\) processes,