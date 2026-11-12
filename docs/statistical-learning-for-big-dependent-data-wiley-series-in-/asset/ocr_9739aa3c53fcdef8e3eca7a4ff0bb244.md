assumptions about \( Z \); the data locations are permuted and \( Q \) is then computed for all possible permutations. Empirical quantiles of the permuted \( Q \) values are then used to make inference, e.g. if the observed \( Q \) of Eq. (9.23) is greater than the 97.5 percentile or lower than the 2.5 percentile of the permuted \( Q \) values, then \( H_0 \) is rejected.

# ### 9.3.6 Kriging

Kriging is a spatial prediction method; see Matheron (1963). Following Cressie and Wikle (2011), we introduce kriging using a HM framework. The spatial process of interest is \( \{Y(s)|s \in D\} \) with \( 0 < \text{Var}[Y(s)] < \infty \) for all \( s \in D \). Note that \( Y(s) \) may not be second-order stationary. The observed data \( Z(s) \) are noisy and follow the model in Eq. (9.8). Assume, for now, that the first two moment equations of \( Y(s) \) are known and given by

\(\mu_y(s) = E[Y(s)], \quad s \in D,\) (9.24)

\(C_y(s_1, s_2) = \text{Cov}[Y(s_1), Y(s_2)], \quad s_1, s_2 \in D.\) (9.25)

In addition, the variance of the measurement error \(\sigma^2_e\) is also known. In Eq. (9.25), we do not assume \( C_y(s_1, s_2) \) is a function of \( s_1 - s_2 \). The variogram of \( Y(s) \) is then given by

\(2\gamma_y(s_1, s_2) = \text{Var}[Y(s_1) - Y(s_2)] = C_y(s_1, s_1) + C_y(s_2, s_2) - 2C_y(s_1, s_2).\)

Kriging is to obtain an optimal spatial prediction of \( Y(s_0) \) conditional on \( \boldsymbol{Z} = \{Z(s_1), \dots, Z(s_m)\}\), where \( s_0 \in D \) is the location of interest. Three types of kriging are considered in the literature, depending on the assumption imposed on the mean equation of \( Y(s) \). The first kriging is simple kriging, in which \( E[Y(s)] \) is known. The second type of kriging is ordinary kriging, in which \( E[Y(s)] = \mu \) is an unknown constant, and the final type of kriging is universal kriging, in which \( E[Y(s)] \) depends on some covariates with unknown parameters.

# ### 9.3.6.1 Simple Kriging

The method known as *simple kriging* is to search over all heterogeneously linear predictors, \( \{\boldsymbol{a}'\boldsymbol{Z} + u | \boldsymbol{a} \in R^m, u \in R\} \), to obtain an optimal spatial predictor of \( Y(s_0) \). Like minimizing the mean squared prediction error (MSPE) in time series analysis, one seeks \( \boldsymbol{a} \) and \( u \) by taking partial derivatives of

\(\text{MSPE}(\boldsymbol{a}, u) = E[Y(s_0) - \boldsymbol{a}'\boldsymbol{Z} - u]^2,\) (9.26)

with respect to \( \boldsymbol{a} \) and \( u \). Equation (9.26) can be written as

\(\text{MSPE}(\boldsymbol{a}, u) = \text{Var}[Y(s_0) - \boldsymbol{a}'\boldsymbol{Z} - u] + \{E[Y(s_0) - \boldsymbol{a}'\boldsymbol{Z} - u]\}^2.\)

The second term is \([\mu_y(s_0) - \boldsymbol{a}'\mu_y - u]^2\), where \( \mu_y = [\mu_y(s_1), \dots, \mu_y(s_m)]'\), which is zero if \( u \) satisfies

\(u = \mu_y(s_0) - \boldsymbol{a}'\mu_y.\) (9.27)

With \( u \) given in Eq. (9.27), the first term of MSPE is

\(C_y(s_0, s_0) - 2\boldsymbol{a}' \text{Cov}[Y(s_0), \boldsymbol{Z}] + \boldsymbol{a}'\Sigma_z \boldsymbol{a},\) (9.28)