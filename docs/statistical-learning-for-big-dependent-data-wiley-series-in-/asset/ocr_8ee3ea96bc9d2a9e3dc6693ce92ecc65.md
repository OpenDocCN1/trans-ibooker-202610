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