all information among the series in estimating the missing values jointly. Similar to the scalar case, one can replace all missing values with zeros to obtain a complete data set, which is denoted by $z^*$. Equipped with this complete data, one can apply intervention analysis to estimate the model parameters and the missing values. Here the intervention model would involve some indicator variables constructed based on the locations of missing values.

Consider the case that $z_h$ is completely missing. Then $z_h^*$ is a zero vector and the intervention model is

```
$z_t^* = w'l_t^{(h)} + z_t,$
```

where $z_t$ follows a vector ARMA model. If the model of $z_t$ is known, then we can easily estimate $w$. The estimate of the missing value is similar to that of Eq. (4.12) and is given by

```
$\hat{z}_h = z_h^* - \hat{w} = -\hat{w}.$
```

Next, consider the case that $z_h$ is only partially missing. For simplicity, we assume that $z_t$ follows a VAR(1) model and the first $k_0$ components of $z_h$ are missing. Partition the vector $z_t = (z'_{1t}, z'_{2t})'$, where $z_{1t}$ is $k_0 	imes 1$ and $z_{2t}$ is $(k - k_0) 	imes 1$, and apply the same to the innovation $a_t = (a'_{1t}, a'_{2t})'$. Write the AR(1) coefficient matrix as $\phi_1 = [\phi_{ij}]$. Then, we have

```
$z_{1h} = \phi_{11}z_{1,h-1} + \phi_{12}z_{2,h-1} + a_{1h},$\n$z_{1,h+1} = \phi_{11}z_{1h} + \phi_{12}z_{2h} + a_{1,h+1}.$
```

Therefore, the missing values are related to the observed data via

```
\(\begin{bmatrix} \phi_{11}z_{1,h-1} + \phi_{12}z_{2,h-1} \\ z_{1,h+1} - \phi_{12}z_{2h} \end{bmatrix} = \begin{bmatrix} z_{1h} \\ -\phi_{11}z_{1h} \end{bmatrix} + \begin{bmatrix} -a_{1h} \\ a_{1,h+1} \end{bmatrix}.\) (4.15)
```

Equation (4.15) assumes the form of a multivariate multiple linear regression in which the response variable is known, the explanatory variable is the matrix $[I, -\phi'_{11}]'$, and the unknown parameters are in $z_{1h}$. Note that $-a_{1h}$ has the same distribution as $a_{1h}$. Consequently, $z_{1h}$ can be estimated by the least squares method provided that $\phi_1$ is known. Specifically, if we rewrite Eq. (4.15) as

```
$Y = Xz_{1h} + e,$
```

then we have

```
$\hat{z}_{1h} = (X'X)^{-1}(X'Y).$
```

In practice, $\phi_1$ is unknown so that one can use an iterative algorithm to obtain estimates of $z_{1h}$ and $\phi_1$ jointly. Thus, the interpolation approach to missing values takes into account all the relationships among the series.

In the literature, the problem of missing values has been widely studied. Time series analysis is no exception. In general, there are two approaches to handle missing values. One approach is the expectation-maximization (EM) algorithm of Dempster et al. (1977) and the other is Markov chain Monte Carlo (MCMC) methods. The interpolation methods discussed in the section belong to EM algorithm. Readers are referred to Tsay (2014, chapter 6) for additional discussion of missing values in vector time series.