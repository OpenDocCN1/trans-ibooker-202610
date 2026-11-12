
For example, for an AR(1) process, since \(\sigma_z^2 = \sigma^2/(1 - \phi^2)\), we have
\(P = 1 - (1 - \phi^2) = \phi^2\).

If the parameter \(\phi\) is near zero, the process is close to being a white noise and its predictability should be close to zero. If \(\phi\) is near one, the process approaches a random walk, and the coefficient \(P\) is close to 1.

The measure \(P\) is not helpful for a nonstationary ARIMA process because then the marginal variance approaches infinity and the value of \(P\) is always 1. To generalize the predictability measure, one can examine carefully the quantities used in Eq. (2.86). Consider the variance ratio \(\sigma^2/\sigma_z^2\). The numerator is the variance of 1-step ahead forecast error, whereas the denominator is the variance of the forecast error with infinite forecast horizon. Thus, \(P\) can also be interpreted as the relative reduction in prediction variability when going from the long run prediction (infinite forecast horizon) to the short run prediction (1-step ahead forecast). Therefore, one possible generalization of predictability measure is to extend the forecast horizon of the numerator yet shorten the forecast horizon of the denominator.

Diebold and Kilian (2001) define the predictability of a time series with forecast horizon \(h\) as follows:
\(P_h(a) = 1 - \frac{\text{Var}[e_t(h)]}{\text{Var}[e_t(h + a)]}\)

where both \(h\) and \(a\) are positive integers. The predictability of Eq. (2.86) is the limiting case of \(h = 1\) and \(a \rightarrow \infty\). The statistic \(P_h(a)\) measures the increase in the variabilities of forecast errors when one increases the forecast horizon from \(h\) to \(h + a\).

The measure \(P_h(a)\) is well-defined for ARIMA model so long as \(a\) and \(h\) are finite. Consider the variance of \(h\)-step ahead forecast error given in Eq. (2.85). It is easy to see that
\(P_h(a) = 1 - \frac{\sum_{i=0}^h \psi_i^2}{\sum_{i=0}^{h+a} \psi_i^2} = \frac{\sum_{i=h+1}^{h+a} \psi_i^2}{\sum_{i=0}^{h+a} \psi_i^2}\),

where \(\psi_0 = 1\).

## 2.7 MODELING A SET OF TIME SERIES

Consider a set of \(k\) scalar time series \(\{z_{it}|i=1, \dots, k; t=1, \dots T\}\). The goal of this section is to explore the basic features of the data, to model individual series, and to summarize the results. In real applications, the \(k\) series may have different sample sizes or were observed at different time spans. In this case, some pre-screening is needed to divide the \(k\) series into subgroups such that each subgroup contains series that were observed at the same time span and have approximately the same sample sizes.

The first step of the proposed analysis is to visualize the set of series using plots discussed in Section 2.1. As illustrated before, those plots can reveal series with gross measurement errors and other sources of heterogeneity. If some series have a small proportion of random missing values or outlying data points, say <10%, then it is recommended to apply interpolation to clean those aberrant data. For a missing value \(z_\nu\), the simplest interpolation is to use the average \((z_{\nu-1} + z_{\nu+1})/2\), provided that \(\nu \neq 1\) or T. It will be seen in Chapter 4 that this solution turns out to be the optimal one if the time series $z_t$ follows a random walk. In some cases, interpolation may require a few iterations to adequately handle a batch of missing values. If the proportion of missing values or outlying observations is high, say > 25%, then special care is needed to handle the aberrant data. See Chapter 4 for statistical methods that can handle missing values and outliers.

If the number of time series \(k\) is not large, one can perform a detailed analysis of each individual series and summarize the resulting models accordingly. On the other hand, when \(k\) is large, it would not be feasible, or be too time-consuming, to model individual series by hand and an automatic modeling procedure is needed. Several procedures have been proposed for automatic ARIMA modeling, such as the TRAMO program, developed by Gómez and Maravall (1994), that was a pioneer approach widely used for analyzing economic time series. The automatic modeling procedure proposed in the next section is based on this and other previous approaches. It focuses on dealing with large data sets with many time series and consists of three steps. The first step, called identification or classification, explores the need of data transformation of individual series. It also investigates the degree of differencing for achieving weak stationarity. The step further includes the specification of a finite set of ARIMA models to be entertained for the series under study. The specified set is typically carried out by choosing the maximum values of \(p\), \(d\), and \(q\) of ARIMA models.

The second step is called estimation and model selection. The parameters of all specified models are estimated and the best model is selected. The estimation is carried out by a quasi-maximum likelihood (ML) method with a Gaussian likelihood function and the best model is selected via an information criterion. The third step is diagnosis checking or validation. The selected model for each series is checked for its adequacy using several test statistics. The goal is to verify that the selected model fulfills the basic assumptions and has no serious inadequacy. If some deficiencies of a selected model are found, the model is refined accordingly. Finally, the best selected model, or a set of models, is used for forecasting. In what follows, we describe the three steps of this procedure in more details.

Keep in mind, however, that there exists no best automatic modeling procedure for all applications. In fact, one is likely to pay a price for using an automatic program, because the complexity of real data can exceed the capability of a given automatic modeling procedure. For instance, most automatic procedures, including the one presented next, only allow for a single seasonal period, yet in reality a seasonal time series can have multiple seasonal periods. Consider the hourly measurements of PM2.5 discussed in Chapter 1. Experience indicates that such a time series may have both diurnal and annual cycles with seasonal periods 24 and 8760 (365 x 24), respectively.

### 2.7.1 Data Transformation

The first decision to make for the proposed automatic procedure is whether to transform the observed series with the objective of working with a series that has a constant variance over time. For many empirical time series, the variability tends to increase with the level of the series in such a way that the standard deviation of the series at a given time, \(\sigma_t\), is a function of the mean \(\mu_t\). That is,

\(\sigma_t \propto \mu_t^{\alpha},\)
(2.87)

where \(\alpha\) is a real number. In this case, it can be shown (Box and Cox, 1964) that the following power transformation

\(y_t = \frac{x_t^{1-\alpha} - 1}{1 - \alpha},\)
(2.88)

can result in \(y_t\) having approximately a constant variability. The family of Box-Cox transformations \(y_t\) defined by Eq. (2.88) includes the powers of the variable and, when \(\alpha \to 1\), \(y_t = \ln(z_t)\), where, for simplicity, we assume \(z_t > 0\). The parameter \(\alpha\) can be estimated by making consecutive homogeneous groups of the observations, calculating the standard deviation, say \(s_i\), and the mean, say \(\bar{x}_i\), of each group, and fitting the linear regression

\(\log s_i = \beta_0 + \widehat{\beta}_1 \log \bar{x}_i, \quad i = 1, \dots, g,\)
(2.89)

where \(g\) is the number of groups, via the ordinary LS method. The \(\alpha\) of Eq. (2.87) can then be estimated by \(\widehat{\beta}_1\) of Eq. (2.89).

Some remarks of the transformation are in order. First, if one is concerned with the impact of outliers or measurement errors on the decision of transformation, he/she can use some robust estimators for the scale and location of each group in lieu of the standard deviation and mean. For instance, one can make use of the group MAD, defined as

\(\text{MAD}(x_1, \dots, x_n) = \text{median}\{|x_i - \text{median}_j(x_j)|\},\)

and choose the scaling parameter as 0.6745xMAD and the group median as the location parameter. Second, if the number of series \(k\) is large, it might be too time-consuming to run the regression (2.89) for all series. To simplify the computation, we only consider four possible values of \(\alpha\) in the proposed procedure. They are \(\alpha = 0\) (no transformation), \(\alpha = 0.5\) (square root), \(\alpha = 1\) (logarithm), and \(\alpha = 2\) (inverse transformation). These four transformations are easier to interpret in practice. Third, with the four possible values of \(\alpha\), we use the following procedure to determine the transformation. Consider the hypotheses \(H_{10} : \beta_1 = 0\); \(H_{20} : \beta_1 = 1\); \(H_{30} : \beta_1 < 1\); and \(H_{40} : \beta_1 > 1\). Starting with \(H_{10}\). If the hypothesis is not rejected, we take no transformation and stop the procedure. If \(H_{10}\) is rejected, we consider \(H_{20}\). If this hypothesis is not rejected, we take the log transformation and terminate the procedure. If \(H_{20}\) is rejected, we consider \(H_{30}\). If the hypothesis is not rejected, we take the square root transformation; otherwise, we take the inverse transformation. Fourth, it is advisable that the grouping of data be carried out so that each group is as homogeneous as possible. For a seasonal series with periodicity \(s\) the size of each group should be a multiple of \(s\) so that at least a complete season is included. Also, the number of groups \(g\) should be at least 7 to have enough data points to estimate the regression in Eq. (2.89). For nonseasonal series we take \(n_g \ge 6\) contiguous observations to form a group and require \(g \ge 7\) so that we assume that the sample size \(T\) is at least 42 for the consideration of checking data transformation.
