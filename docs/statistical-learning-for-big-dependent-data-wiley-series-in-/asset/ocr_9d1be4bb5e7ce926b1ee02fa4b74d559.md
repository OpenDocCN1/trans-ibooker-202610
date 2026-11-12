![](bbox=[0.22, 0.095, 0.775, 0.335])

Figure 9.8 Scatterplots of $C_y(\boldsymbol{h}, \boldsymbol{\theta})$ for selected semivariograms. (a) Matérn semivariogram with $\boldsymbol{\theta} = (1, 1.5, 0.1, 0.9)'$. (b) Powered-exponential semivariogram with $\boldsymbol{\theta} = (1, 1, 0.1, 0.9)'$. Both plots show a 10% nugget effect.

of $C_y(\boldsymbol{h}, \boldsymbol{\theta})$ of Eq. (9.13) with $\boldsymbol{\theta} = (1, 1.5, 0.1, 0.9)'$ versus $\|\boldsymbol{h}\|$, indicating that the covariance function decays smoothly to zero as $\|\boldsymbol{h}\|$ increases. The plot also shows a 10% nugget effect. The values of $\boldsymbol{\theta}$ used are from Cressie and Wikle (2011). For the model in Eq. (9.13), $\sigma_0^2$ is the nugget, $\sigma_1^2$ is the variance, $\theta_1$ is called the *scale* or *range* parameter, and $\theta_2$ is the *shape* or *smoothness* parameter. This is an isotropic semivariogram.

Another example of commonly used isotropic semivariogram is the powered exponential given by

```
$$C_y(\boldsymbol{h}, \boldsymbol{\theta}) = \sigma_0^2 I(\|\boldsymbol{h}\| = 0) + \sigma_1^2 \exp[-(\|\boldsymbol{h}\|/\theta_1)^{\theta_2}], \quad (9.14)$$
```

where $\boldsymbol{\theta} = (\theta_1, \theta_2, \sigma_0^2, \sigma_1^2)'$ with $\theta_1 > 0, 0 < \theta_2 \le 2, \sigma_0 \ge 0$, and $\sigma_1^2 \ge 0$. Figure 9.8(b) shows a plot of $C_y(\boldsymbol{h}, \boldsymbol{\theta})$ of Eq. (9.14) versus $\|\boldsymbol{h}\|$ with $\boldsymbol{\theta} = (1, 1, 0.1, 0.9)'$. From the plot, the covariance function decays quickly and smoothly to zero as $\|\boldsymbol{h}\|$ increases. A 10% nugget effect is also shown in the plot. The powered-exponential semivariogram is discussed in Diggle and Ribeiro (2007). In practice, $\theta_2 = 1$ is often used.

The following isotropic semivariogram models without nugget effect are also available in some R packages:

+   1. Linear model:
+   $$r_y(\boldsymbol{h}, \boldsymbol{\theta}) = \begin{cases} 0, & \text{if } \boldsymbol{h} = \boldsymbol{0}, \\ b_0 + b_1 \|\boldsymbol{h}\|, & \text{if } \boldsymbol{h} \neq \boldsymbol{0}, \end{cases}$$

where $\boldsymbol{\theta} = (b_0, b_1)'$ with $b_0 \ge 0$ and $b_1 \ge 0$.

+   2. Spherical model: valid for $R^d$ with $1 \le d \le 3$$
+   $$r_y(\boldsymbol{h}, \boldsymbol{\theta}) = \begin{cases} 0, & \boldsymbol{h} = \boldsymbol{0}, \\ b_0 + b_1 [1.5(\|\boldsymbol{h}\|/b_2) - 0.5(\|\boldsymbol{h}\|/b_2)^3], & 0 < \|\boldsymbol{h}\| \le b_2, \\ b_0 + b_1, & \|\boldsymbol{h}\| \ge b_2, \end{cases}$$

where $\boldsymbol{\theta} = (b_0, b_1, b_2)'$ with $b_i \ge 0$ for $i=1, 2, 3$.