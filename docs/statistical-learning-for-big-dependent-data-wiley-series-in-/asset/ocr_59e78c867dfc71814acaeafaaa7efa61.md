expected value or mean of $z_t$ as

$$\mu_t = E(z_t) = \int_R z_t f_t(z) dz$$

provided that the integral exists, where $R$ denotes the real line. The sequence $\mu_1^T = (\mu_1, \mu_2, \dots, \mu_T)$ is the mean function of $\mathbf{z}_1^T$. Define the variance of $z_t$ as

$$\sigma_t^2 = E(z_t - \mu_t)^2 = \int_R (z - \mu_t)^2 f_t(z) dz$$

provided that the integral exists. The sequence $\{\sigma_1^2\}_1^T = (\sigma_1^2, \dots, \sigma_T^2)$ is the variance function of $\mathbf{z}_1^T$. Higher order moments can be defined similarly, assuming that they exist.

A special case of interest is that all elements of $\mathbf{z}_1^T$ have the same mean. In this case, $\mu_1^T$ is a constant function. If all elements of $\mathbf{z}_1^T$ have the same variance, then $\{\sigma_t^2\}_1^T$ is a constant function. If both $\mu_1^T$ and $\{\sigma_t^2\}_1^T$ are constants for all finite $T$, then the stochastic process $z_t$ is stable and we say that it is a homogeneous process. In some applications, $\mu_1^T$ or $\{\sigma_t^2\}_1^T$ may not exist.

In what follows, we assume that $\sigma_t^2$ exists for all $t$. The linear dynamic dependence of a stochastic process $z_t$ is determined by its autocovariance or autocorrelation function (ACF). For $z_t$ and $z_{t-j}$, where $j$ is a given integer, we define their autocovariance as

$$\gamma(t, t-j) = \text{Cov}(z_t, z_{t-j}) = E[(z_t - \mu_t)(z_{t-j} - \mu_{t-j})]. \quad (1.1)$$

When $j=0$, $\gamma(t, t) = \text{Var}(z_t) = \sigma_t^2$. Also, from the definition, it is clear that $\gamma(t, t-j) = \gamma(t-j, t)$. The autocorrelation between $z_t$ and $z_{t-j}$ is defined as

$$\rho(t, t-j) = \frac{\gamma(t, t-j)}{\sigma_t \sigma_{t-j}}. \quad (1.2)$$

Of particular importance in real applications is the case in which the autocovariance and, hence, the autocorrelation between $z_t$ and $z_{t-j}$ is a function of the lag $j$, and not a function of $t$. In this case, the autocovariance and autocorrelation are time-invariant and we write $\gamma(t, t-j) = \gamma_j$ and $\rho(t, t-j) = \rho_j$.

# 1.2.1.1 Stationarity

A stochastic process $z_t$ is strictly stationary if the joint distribution of the $m$-dimensional random vectors $(z_{t_1}, z_{t_2}, \dots, z_{t_m})$ and $(z_{t_1+h}, z_{t_2+h}, \dots, z_{t_m+h})$ are the same, where $m$ is an arbitrary positive integer, $\{t_1, \dots, t_m\}$ are arbitrary $m$ time indexes, and $h$ is an arbitrary integer. In other words, the joint distribution of arbitrary $m$ random variables is time-invariant. In particular, the marginal probability density $f_t(z)$ of a strictly stationary process $z_t$ does not depend on $t$, and we simply denote it by $f(z)$. A stochastic process $z_t$ is weakly stationary if its mean and autocovariance functions exist and are time-invariant. Specifically, a process $z_t$ is stationary in the weak sense if (1) $\mu_t = \mu$ for all $t$; (2) $\sigma_t^2 = \sigma^2$ for all $t$; and (3) $\gamma(t, t-j) = \gamma_j$ for all $t$, where $j$ is a given integer. The first two conditions say that the mean and variance are time-invariant. The third condition states that the autocovariance and, hence, the autocorrelation between two variables depend only on their time separation $j$. Unless specifically stated, we