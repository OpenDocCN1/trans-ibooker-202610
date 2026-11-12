The OLS estimate of $\Sigma_a$ is

$$\hat{\Sigma}_a = \frac{1}{T-(k+1)p-1} \sum_{t=p+1}^T \hat{a}_t \hat{a}'_t$$ (3.31)

where $\hat{a}_t = z_t - \hat{\phi}_0 - \sum_{i=1}^p \hat{\Phi}_i z_{t-i}$ is the residual series. If ML estimation is used, then the ML estimate of $\Sigma_a$ is

$$\hat{\Sigma}_a = \frac{1}{T-p} \sum_{t=p+1}^T \hat{a}_t \hat{a}'_t.$$ (3.32)

**Theorem 3.1** Assume that $a_t$ of the VAR($p$) model is a sequence of independent and identically distributed random vectors with mean zero and positive-definite covariance matrix $\Sigma_a$. Then, the OLS estimation gives (a) $E(\hat{\beta}) = \beta$, (b) $E(\hat{\Sigma}_a) = \Sigma_a$, (c) the residual $\hat{A}$ is uncorrelated with $\hat{\beta}$, and (d) the covariance matrix of the parameter estimates is

$$Cov[vec(\hat{\beta})] = \tilde{\Sigma}_a \otimes (X'X)^{-1}.$$

To establish the limiting normal distribution of the estimate $\hat{\beta}$, we further assume that

$$E|a_{it}a_{ju}a_{ru}a_{vt}| < \infty, \quad \text{for all } 1 \le i, j, u, v \le k \text{ and } t, \quad (3.33)$$

which implies that all fourth moments of $a_t$ are finite.

**Theorem 3.2** Assume that the condition of Theorem 3.1 and Eq. (3.33) hold. Then, as $T \to \infty$,

+   (i) $\hat{\beta} \to_p \beta,$

+   (ii) $\sqrt{T-p}[vec(\hat{\beta}) - vec(\beta)] \to_d N(0, \Sigma_a \otimes G^{-1}),$

where $\to_p$ and $\to_d$ denote convergence in probability and in distribution, respectively, and

$$G = \begin{bmatrix} 1 & 0' \\ 0 & \Gamma_0^* \end{bmatrix} + \begin{bmatrix} 0 \\ u \end{bmatrix} [0, u']$$

where 0 is a $kp$-dimensional zero vector, $u = 1_p \otimes \mu$ with $1_p$ being the $p$-dimensional vector of ones, and

$$\Gamma_0^* = \begin{bmatrix} \Gamma_z(0) & \Gamma_z(1) & \cdots & \Gamma_z(p-1) \\ \Gamma_z(-1) & \Gamma_z(0) & \cdots & \Gamma_z(p-2) \\ \vdots & \vdots & \ddots & \vdots \\ \Gamma_z(1-p) & \Gamma_z(2-p) & \cdots & \Gamma_z(0) \end{bmatrix}.$$

# ## 3.2.4 Model Building

**Order selection:** The first step in building a VAR model for a time series $z_t$ is to select the order $p$. There are several methods available in the literature. See, for instance, Tsay (2014, chapter 2). The most commonly used method is to employ one of the following criterion functions: