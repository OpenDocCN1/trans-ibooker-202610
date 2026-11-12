
In summary, the univariate outliers are again detected, but some detected multivariate ones, such as $h = 109$, were not identified in the univariate search.

## R commands for multivariate outlier detection:

```
>da<- read.csv("m-eximpcnus.csv")
>da=da[,1]
>da=as.ts(da)
>xt=diff(da)
# The inputs of outliers.hdts are (Yt,r.maxn,ind).
# Yt is the T-by-k data matrix,
# r.max is the number of factors and ind is an indicator.
# Here r.max = 1. We set ind=1 for taking no difference.
# If ind=2, the first difference is taken.
>out=outliers.hdts(xt,1,1)
[1] "First step - Running the first substep:
projections of maximum kurtosis"
Maximum kurtosis: Direction number = 1 Number of outliers = 4
[1] "First step - Running the second substep: random projections"
Random projections: Number = 1 Number of outliers = 3
[1] "First step - Running the third substep: univariate cleaning"
Univariate cleaning: Number = 1 Number of outliers = 0
Univariate cleaning: Number = 2 Number of outliers = 3
Univariate cleaning: Number = 3 Number of outliers = 1
Univariate cleaning: Number = 4 Number of outliers = 2
>out$times.kurt
[1] 277 278 313 314
out$times.rand
[1] 12 13 109
out$times.uni
[1] 26 200 203 203 14 202
#Note that this number may be different in another run because
# the random directions used are different.
```

## Example 4.7

As a second illustration, we use the consumer price index (CPI) data of European countries. See Chapter 1. We search for multivariate and univariate outliers using the program outliers.hdts that search for MAOs using projections, followed by search for AO in the individual series. We concentrate on the initial cleaning of outliers without using any multivariate model. The series analyzed are the differenced data $VV_{12} \log \zeta_t$. The commands used are given below:

```
>x=read.csv2("CPIEurope2000-15.csv")
>x=as.ts(x)
>lx=log(x)
>x1=diff(lx,lag=12)
>xl2=diff(x1)
>out=outliers.hdts(xl2,1,1)
```

The output obtained first is

```
[1] "First step - Running the first substep: projections of maximum kurtosis"
Maximum kurtosis: Direction number = 1 Number of outliers = 3
-------------------------------------------------------------
[1] "First step - Running the second substep: random projections"
Random projections: Number = 1 Number of outliers = 0
-------------------------------------------------------------
Random projections: Number = 3 Number of outliers = 0
-------------------------------------------------------------
#The number of MAO identified by kurtosis and random projection
is obtained
> out$times.kurt
[1] 24 48 120 1 3 14 15 108
> out$times.rand
[1] 114
# and for the univariate outliers
out$times.uni
[1] 96 110 144 6 126 173 13 69 72 94
```

The output of the univariate search indicates 18 outliers. The largest number appear in Series 23 (Romania), then Series 25 (Slovakia), and Series 3 (Czech. Rep.).

## 4.6 ROBUST ESTIMATION

An alternative approach to handling outliers is to use estimation methods that are insensitive to aberrant data points. The approach is called *robust estimation* and is a complementary way to outlier detection for handling data heterogeneity. Outlier detection emphasizes on searching and estimating effects for possible outliers whereas robust estimation investigates methods that mitigate their impacts in statistical inference. Thus, the two approaches complement each other.

It is well known that the least squares (LS) estimation is optimal under normality but it may become inefficient when the observations are from a heavy-tailed distribution, which tends to produce outlying data points. The *breakdown point* of an estimator is defined as the minimum fraction of outliers in the sample that makes the estimate to assume any arbitrary value. For instance, the LS estimate of an AR(1) process has a breakdown point of 1/T because a single outlier may change \(\phi\) to any arbitrary value. If instead of the LS method we use a robust estimate with high breakdown point, \(b/T\) and we have a fraction of outlier smaller than this breakdown point, \(b/T\), the outliers can be identified as large residuals of the robust model. Usually robust estimates minimize a function of the residuals that grows slower than the square or, even better, that is bounded, so that the maximum effect of an outlier is limited. Consider a scalar AR(\(p\)) model and let \(\phi_p\) be the parameter vector. A general class of robust estimates, introduced by Huber (1981), are M-estimators that minimize

\(M = \sum_{t=1}^{T} \gamma \left[ \frac{e_t(\phi_p)}{\hat{\sigma}} \right] ,\) (4.36)

where \(\gamma\) is a nondecreasing bounded function with \(\gamma(0) = 0\) and \(\gamma(\infty) = 1\), so that the effect of each residual, \(e_t(\phi_p) = z_t - \phi'_p z_{t-1,p}\), where \(z_{t-1,p} = (z_{t-1}, ..., z_{t-p})'\), is bounded, and \(\hat{\sigma}\) is an initial estimate of \(\sigma\) that makes the M-estimate independent of

the scale of the observations. Note that in Eq. (4.36) the LS estimate is obtained for \(\gamma(x) = x^2\), and the \(L_1\) estimate for \(\gamma(x) = |x|\), and these two estimates do not require an initial value of \(\hat{\sigma}\) in contrast to M-estimates. This initial value of the variability can be obtained by, for instance, interpolating in a simple way all suspicious observations and computing an initial estimating \(\hat{\sigma}\) by a robust measure of scale as the mad (median of absolute deviation) of these residuals:

\(\hat{\sigma} = \frac{1}{0.675} \text{Med}(|e_i|, e_i \neq 0).\)

The function (4.36) is minimized by an iterative algorithm as follows. Taking the derivative of \(M\) in (4.36) with respect \(\boldsymbol{\phi}_p\), and dropping constant terms, the estimates must verify

\(\sum_{t=p+1}^{T} \gamma' \left( \frac{z_t - \boldsymbol{\phi}'_p \boldsymbol{z}_{t-1,p}}{\hat{\sigma}} \right) \boldsymbol{z}_{t-1,p} = 0, \quad (4.37)\)

where \(\gamma'(x) = d\gamma(x)/dx\). Next, define the weight function

\(\omega(x) = \gamma'(x)/x \quad \text{if } x \neq 0; \quad \omega(0) = 0,\)

and the solution of Eq. (4.37) can be written as

\(\sum_{i=1}^{n} \omega_i \boldsymbol{z}_{t-1,p}(z_t - \boldsymbol{z}'_{t-1,p} \boldsymbol{\phi}_p) = 0,\)

where \(\omega_i = \omega(e_i(\boldsymbol{\phi}_p)/\widehat{\sigma})\). The estimator is

\(\hat{\boldsymbol{\phi}}_p = \left[ \sum_{i=1}^{T} \omega_i \boldsymbol{z}_{t-1,p} \boldsymbol{z}'_{t-1,p} \right]^{-1} \left( \sum_{i=1}^{T} \omega_i \boldsymbol{z}_{t-1,p} z_t \right), \quad (4.38)\)

which is a weighted LS estimate with weights depending on the standardized residuals \(e_i(\boldsymbol{\phi}_p)/\widehat{\sigma}\) so that observations with large residuals will have smaller weight in the estimation. The function (4.36) is minimized by using some initial estimate \(\boldsymbol{\phi}_p^{(0)}\) to obtain the residuals and then computing a new value of the estimate by (4.38). This estimate will produce new residuals and new weights, and the procedure is iterated until convergence.

The M-estimates are useful when the sample only have low leverage outliers. By low leverage, we mean that the outlying observation does not markedly affect the parameter estimate. A better alternative in the general situation is to use MM-estimates, in which the estimation is carried out in two steps: (1) an initial consistent estimate \(\hat{\boldsymbol{\phi}}_p^{(0)}\) is computed that has a high breakdown point; although it may not be efficient, and a robust scale is obtained using this estimate; (2) Eq. (4.36) is minimized by the previous iterative procedure starting with \(\hat{\boldsymbol{\phi}}_p^{(0)}\).

A key problem in robust estimation is to avoid the propagation of outliers when computing the residuals. For instance, as seen before, an AO in an AR(p) at \(t=h\) contaminates all p consecutive residuals starting at \(t=h\), because \(e_t(\boldsymbol{\phi}_p) = z_t - \boldsymbol{\phi}'_p \boldsymbol{z}_{t-1,p}\). Thus, robust filters to avoid this effect have been proposed, see, for instance, Muler et al. (2009) in the univariate case and Muler (2013) for the
