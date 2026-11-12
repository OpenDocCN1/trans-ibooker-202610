where \(i = AO, IO, LS\) depending on the type of outlier. This statistic treats all the components jointly. For a fixed \(h\) and assuming that the model is known, \(J_{i,h}\) is distributed as a chi-square random variable with \(k\) degrees of freedom under the null hypothesis. The second test statistic is the maximum in absolute value of components of \(\widehat{\boldsymbol{\omega}}_{i,h}\) when \(\Sigma_{i,h}\) is known. That is,

```
C_{i,h} = \max_{1 \le j \le k} |\hat{\omega}_{j,i,h}| / \sqrt{\sigma_{j,i,h}},\)
```

where \(\hat{\omega}_{j,i,h}\) and \(\sigma_{j,i,h}\) are the \(j\)th element of \(\widehat{\boldsymbol{\omega}}_{i,h}\) and the \((j, j)\)th element of \(\Sigma_{i,h}\) respectively.

We define the overall test statistics as

```
\(J_{\max}(i, h_i) = \max_h J_{i,h}, \quad C_{\max}(i, h_i^*) = \max_h C_{i,h}, \quad i = AO, IO, LS, \quad (4.35)\)
```

where it is understood that \(h_i\) denotes the time index when the maximum of test statistic \(J_{i,h}\) occurs and \(h_i^*\) denotes the time index when the maximum of \(C_{i,h}\) occurs. Under the null hypothesis of no outlier in the sample and assuming that the model of \(\mathbf{z}_t\) is known, \(J_{\max}(I, h_i)\) is the maximum of a random sample of size \(T\) from a chi-square distribution with \(k\) degrees of freedom and its asymptotic distribution can be obtained using the extreme value distribution. Each of the other three joint test statistics in (4.35) is the maximum of a dependent sample from a chi-square distribution with \(k\) degrees of freedom and their asymptotic distributions are, therefore, more complicated. In practice, simulation is required to generate finite-sample critical values of the two test statistics.

As in the univariate case, if a single joint statistic \(J_{\max}(i, h_o)\) is significant at time index \(h_o\), we identify a multivariate outlier of type \(i\) at \(h_o\), where \(i = AO, IO, LS\). In the case of multiple significant joint test statistics, we identify the outlier type based on the test statistic that has the smallest empirical \(p\)-value. For example, if \(J_{\max}(A, h_A)\) has the smallest \(p\)-value at time index \(h_o\) and the \(p\)-value is smaller than 0.05, then we identify an AO at time index \(h_o\) at the 5% significance level. When all of the four joint statistics are insignificant at a given level, we use the component statistics \(C_{\max}(i, h_i^*)\) to check for additional outliers. This step ensures that no component outliers are overlooked.

Once an outlier is identified, its impact on the underlying time series is removed. The adjusted series is treated as a new data set and the detecting procedure is iterated. We terminate the detection procedure when no significant outliers are detected. Finally, we recommend a joint estimation of the model parameters and all detected outliers. If some outlier parameters are found to be insignificant in this joint estimation, they are deleted. The joint estimation is repeated until all detected outliers are significant at the given significance level.

# 4.5.2 Outlier Detection by Projections

For a large set of time series, we need a fast way to clean the data before any modeling exercise. To this end, we classify any series with 30% or more observations missing as an outlying series and remove it from the data set for further consideration. Similarly, we also remove any series from the data set if it contains 30% or more observations with the same value. Series with high proportion of missing values or of data assuming the same value can occur when the series were collected