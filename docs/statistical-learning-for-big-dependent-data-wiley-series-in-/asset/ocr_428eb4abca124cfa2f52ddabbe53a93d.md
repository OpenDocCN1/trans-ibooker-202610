# 9.3.1 Stationary Variogram

For a spatial process $Y(\mathbf{s})$ in $D \subset R^d$, assume that the mean is constant and differences of variables that are $\mathbf{h}$-apart vary in a way that depends only on $\mathbf{h}$. Then, the function
$$
2\gamma_y(\mathbf{h}) = \text{Var}[Y(\mathbf{s} + \mathbf{h}) - Y(\mathbf{s})], \quad \text{for all } \mathbf{s}, \mathbf{s} + \mathbf{h} \in D, \quad (9.9)
$$

is called the stationary variogram. If $\text{Var}[Y(\mathbf{s})]$ exists, then we have
$$
2\gamma_y(\mathbf{h}) = 2C_y(\mathbf{0})(1 - C_y(\mathbf{h})/C_y(\mathbf{0})) = 2C_y(\mathbf{0})[1 - \rho_y(\mathbf{h})], \quad (9.10)
$$

where $\rho_y(\mathbf{h})$ serves as the autocorrelation of the spatial process if the variogram is isotropic, which is defined below.

The variogram must satisfy the conditional nonpositive-definiteness condition,
$$
\sum_{i=1}^\ell \sum_{j=1}^\ell \alpha_i \bar{\alpha}_j 2\gamma_y(\mathbf{s}_i - \mathbf{s}_j) \le 0, \quad (9.11)
$$

for any positive integer $\ell$, any set of spatial locations $\{\mathbf{s}_1, \dots, \mathbf{s}_\ell\}$, and any set of complex numbers $\{\alpha_1, \dots, \alpha_\ell\}$ satisfying $\sum_{i=1}^\ell \alpha_i = 0$, where $\bar{\alpha}_i$ is the complex conjugate of $\alpha_i$. For a second-order stationary process $Y(\mathbf{s})$, it is easy to show that the condition (9.11) holds. Basically, because $\sum_{i=1}^\ell \alpha_i = 0$, the left side of Eq. (9.11) becomes $-2\text{Var}[\sum_{i=1}^\ell \alpha_i Y(\mathbf{s}_i)]$. This condition is important, because it guarantees that all model-based variances are nonnegative.

If the variogram can be written as a function of $\|\mathbf{h}\| = (h_1^2 + \dots + h_d^2)^{1/2}$, the variogram is said to be *isotropic*; otherwise it is said to be *anisotropic*. Sometimes, we write $\|\mathbf{h}\|$ as $\|\mathbf{h}\|_2$ to emphasize the $\ell_2$ norm.

In the previous chapters, we use the autocovariance function to describe the temporal dependence of a second-order stationary time series. In a similar vein, one often uses variogram to capture the spatial dependence of a second-order stationary spatial process. However, one should note that variogram is more general than the covariance function. For instance, consider the scalar case. The variogram is well defined for a random walk process, for which the covariance function is not defined. In practice, variogram models that depend on only a few parameters $\boldsymbol{\theta}$ are often used to summarize the spatial dependence in the literature. Some examples are given next.

# 9.3.2 Examples of Semivariogram

The function $\gamma_y(\mathbf{h})$ of Eq. (9.9) is referred as the semivariogram of $Y(\mathbf{s})$, which can be written as $\gamma_y(\mathbf{h}) = C_y(\mathbf{0}) - C_y(\mathbf{h})$. If it depends on a finite-dimensional parameter $\boldsymbol{\theta}$, then we have
$$
\gamma_y(\mathbf{h}, \boldsymbol{\theta}) = C_y(\mathbf{0}, \boldsymbol{\theta}) - C_y(\mathbf{h}, \boldsymbol{\theta}). \quad (9.12)
$$

In this section, we consider some specific examples of semivariogram. Parametric models can be useful in spatial prediction such as kriging. The first example of semivariogram is the Matérn semivariogram with

$$
C_y(\mathbf{h}, \boldsymbol{\theta}) = \sigma_0^2(\|\mathbf{h}\| = 0) + \sigma_1^2 \{ 2^{\theta_2-1} \Gamma(\theta_2) \}^{-1} \{ \|\mathbf{h}\|/\theta_1 \}^{\theta_2} K_{\theta_2}(\|\mathbf{h}\|/\theta_1), \quad (9.13)
$$

where $K_{\theta_2}(.)$ is a modified Bessel function of the second kind of order $\theta_2$ (e.g. Abramowitz and Stegun, 1964, pp. 374-379), $\Gamma(.)$ is the gamma function, and $\boldsymbol{\theta} = (\theta_1, \theta_2, \sigma_0^2, \sigma_1^2)'$ with $\theta_1 > 0, \theta_2 > 0, \sigma_0^2 \ge 0, \sigma_1^2 \ge 0$. Figure 9.8(a) shows a plot