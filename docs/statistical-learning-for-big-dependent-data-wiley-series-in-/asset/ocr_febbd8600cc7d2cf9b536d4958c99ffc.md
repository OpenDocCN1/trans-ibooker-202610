The discussion of this chapter contains certain deviations from the traditional methods available in the literature, because we focus on dependent data and emphasize on the case when the number of predictors k is large, even greater than the sample size T or the forecast origin n. Keep in mind, however, that most of the modern methods discussed in the chapter were developed for independent data so that in some cases we may start with this assumption. Extensions to dependent data will be given later, if available.

Care must be exercised, however, in applying methods developed for independent data to dependent data. It is well known that serial dependence and high multicollinearity may lead to erroneous inference if they are overlooked. See the discussions in Chapter 1 and Section 7.2. Also, some regularizations are needed to build a forecasting model when k > min{n, T). We start our discussion with short-range dependence data, i.e. serial dependence of the variables involved decays to zero exponentially. The case of strong serial dependence will be briefly addressed in a later section.

# 7.1 REGULARIZED LINEAR MODELS

One of the important recent developments in statistics is the regularized estimation under the sparsity assumption, especially the Lasso (Tibshirani, 1996). The linear regression model for the problem of interest of this chapter can be written as

```
$y_{t+h} = \mathbf{x}'_t \boldsymbol{\beta} + e_t = \sum_{i=1}^k \beta_i x_{it} + e_t, \quad t = 1, \dots, n$ (7.1)
```

where the forecast horizon h is given and fixed and $\boldsymbol{\beta}$ is the vector of parameters. For simplicity in notation, it is understood that $\mathbf{x}_t$ may contain the vector of 1. Often in applications, each predictor $x_{it}$ is mean-adjusted and standardized to have unit variance. This normalization works well for independent data, but may encounter difficulties when the data have strong serial correlations. We give some details in Section 72. For regularized linear model one estimates the unknown parameters and uses the estimates along with values of the predictors to obtain prediction.

In matrix- and vector-notation, the linear regression model of Eq. (7.1) becomes

```
$\mathbf{Y} = \mathbf{X}\boldsymbol{\beta} + \mathbf{E},$ (7.2)
```

where $\mathbf{Y}$ is the $n$-dimensional response vector, $\mathbf{X}$ is the design matrix with $i$th row being $\mathbf{x}'_i$, and $\mathbf{E}$ is the error vector.

Under the traditional linear regression analysis, one assumes that (a) the errors $e_1, \dots, e_n$ are independent and identically distributed (i.i.d.) with mean zero and variance $\sigma_e^2$, (b) $e_i$ is independent of $\mathbf{x}_t$, and (c) the predictor $\mathbf{x}_i$ satisfies the condition that $E(\mathbf{x}_i \mathbf{x}'_i)$ is positive definite. We shall relax these conditions later.

If $k$ is sufficiently less than $n$, then the ordinary least squares (LS) method can be used to estimate the model in Eq. (7.1). The estimates are $\hat{\boldsymbol{\beta}} = (\sum_{t=1}^n \mathbf{x}_t \mathbf{x}'_t)^{-1} \sum_{t=1}^n \mathbf{x}_t y_{t+h}$. Furthermore, we have

```
$\sqrt{n}(\hat{\boldsymbol{\beta}} - \boldsymbol{\beta}) \to_d N \left[ \mathbf{0}, \sigma_e^2 \left( \sum_{i=1}^n \mathbf{x}_i \mathbf{x}'_i \right)^{-1} \right],$
```