statistics, the nugget effect further includes any intrinsic jump $\sigma_0^2$ of the covariance function of the spatial process $Y(s)$ at $h = 0$.

With nugget effect, a simple geostatistical model on the domain $D \subset R^d$ can be written as a HM below:

+   • Data model: Conditional on $\sigma_e^2$, for $i = 1, \dots, m$,
+   $$Z(s_i)|Y(s_i), \sigma_e^2 \sim_{\text{ind}} N[Y(s_i), \sigma_e^2].$$

+   • Process model: Conditional on $\mu$ and $C_y(.)$, $Y(.)$ is a stationary Gaussian process with mean $\mu$ and covariance function $C_y(h)$ with $h \in D$.

Based on the nature of $D$, spatial processes $Y(s)$ are often classified into geostatistical processes, lattice processes, spatial point processes, and random-set processes. For instance, when $D$ consists of two-dimensional grid points, $Y(s)$ is a lattice process, and when $D$ consists of random points in $R^2$, $Y(s)$ is a point process. See Cressie and Wikle (2011, chapter 4) for further details. In what follows, we provide some further details concerning statistical inference of the first three types of spatial process. The random-set processes are less employed in applications.

# 9.3 GEOSTATISTICAL PROCESSES

In applications, some covariates are often available and we postulate that the spatial process $Y(s)$ follows the linear model

```
$$Y(s) = x(s)'\beta + \eta(s), \quad s \in D, \qquad (9.6)$$
```

where $\eta(s)$ is a mean-zero Gaussian process with stationary covariance function $C_y(.)$, $x(.)$ is a $k$-dimensional vector of covariates, which are available at every location in $D$, and $\beta$ is a $k$-dimensional vector of parameters. At the observation locations $s_1, \dots, s_m$, we then have a linear mixed model

```
$$\boldsymbol{Y} = \boldsymbol{X}\boldsymbol{\beta} + \boldsymbol{\eta}, \qquad (9.7)$$
```

where $\boldsymbol{Y} = [Y(s_1), \dots, Y(s_m)]'$, the $j$th row of $\boldsymbol{X}$ is $x(s_j)'$, $\boldsymbol{\eta} = [\eta(s_1), \dots, \eta(s_m)]'$, which denotes a spatial random effect and $\text{Var}(\boldsymbol{\eta}) = [C_y(s_i - s_j)]$ is an $m \times m$ positive-definite matrix. In Eq. (9.7), $\boldsymbol{\beta}$ is the fixed effect of the covariates and we assume that $k < m$. The data model becomes

```
$$Z(s_i) = Y(s_i) + \epsilon(s_i), \quad i = 1, \dots, m, \qquad (9.8)$$
```

where $\{\epsilon(s_i)\}$ are iid $N(0, \sigma_e^2)$. Then, for $s \in D$, we have $\text{Var}[Z(s)] = \sigma_y^2 + \sigma_e^2$, which is referred to as the sill in the geostatistics literature. The nugget effect of the model is

```
$$c_0 = \sigma_0^2 + \sigma_e^2,$$
```

where $\sigma_0^2 = \lim_{h \to 0} [C_y(0) - C_y(h)]$. See Eq. (9.5). The quantity $\sigma_z^2 - c_0 = \sigma_y^2 - \sigma_0^2$ is called the partial sill.