
\(\rho_A^2 = (1 + \pi_1^2 + \cdots + \pi_{T-h}^2)\), and \(\hat{\omega}_I = \hat{\varepsilon}_h\) for IO. At each time \(h\), the likelihood ratio test (LRT) statistics \(\lambda_h^C = \hat{\omega}_h / s(\hat{\omega}_h)\) for \(C = AO, IO\), where the standard error is computed following the linear regression model. Let \(g\) be the time point such that \(|\lambda_g^A| = \max_h |\lambda_h^A|\) and \(m\) be the time point such that \(|\lambda_m^I| = \max_h |\lambda_h^I|\). Let \(\lambda_{Max} = \max\{|\lambda_g^{AO}|, |\lambda_m^{IO}|\}\). If \(\lambda_{Max} > c_2\), where \(c_2\) is a prespecified critical value, we conclude that an outlier is detected for the series. Otherwise, no outliers exist in the series. The effect of a detected outlier is corrected in the series, for AO \(\hat{\varepsilon}_g = z_g - \hat{\omega}_A\) and for IO the residuals are corrected by \(\hat{\varepsilon}_m = \hat{\varepsilon}_m - \hat{\omega}_I\). The search for AO or IO is continued until no more outliers are detected.

## 4. Final joint estimation of outlier effects and parameters.

When the detection procedure ends, all the detected outlier effects and model parameters are estimated jointly. Assume that level shifts have been detected at times \(t_1, t_2, ..., t_u\), and outliers at times \(t_1^*, t_2^*, ..., t_v^*\). The final model becomes

```
\(z_t = \sum_{j=1}^u \omega_j s_t^{(i)} + \sum_{j=1}^v \omega_j v_{t_j^*}(B) l_t^{(t_j^*)} + \psi(B) a_t,\)
```

where \(v_{t^*}(B) = 1\) if the outlier is an AO and \(v_{t_i^*}(B) = \psi(B)\) if it is an IO. The effects that are not statistically significant are eliminated one by one and the model is re-estimated until all the effects and estimated coefficients are significant.

The critical values \(c_1\) and \(c_2\) are usually taken to be in the interval [3, 3.5] and were obtained by simulation in Chang et al. (1988). These cutoffs must depend on the sample size because the test statistics used are the maximum value over each time index. If the statistics \(\lambda_h\) were independent, then

```
\(Pr(\lambda_1 \le a, ..., \lambda_T \le a) = Pr(\lambda_1 \le a)^T = 1 - \alpha,\)
```

where \(\alpha\) is the type-I error. For instance, if we want the probability of no false outlier detection to be 0.95 or, \(\alpha = 0.05\), we must choose the critical value \(a = z_{0.95^{1/T}}\), where \(z_v\) is the \(100v\) quantile of the standard Gaussian distribution. However, the test statistics \(\lambda_i\) used are dependent, and the previous value \(a\) becomes a conservative choice. The dependence among the test statistics makes it hard, if not impossible, to determine proper critical values for \(c_1\) and \(c_2\). In theory, one can choose them using the concept of false discovery rate (FDR); see, for instance, chapter 15 of Efron and Hastie (2016). However, Monte Carlo simulations have shown that such an approach does not provide better results than using the distribution of the maximum.

When applying this detection procedure, it is important to carry out the corrections of level shifts one by one and to estimate their effects jointly. For instance, suppose a series with two LS of sizes 5 and 10. If the second shift is detected first, its size will not be well estimated, due to the presence of the first shift. However, when the two level shifts are identified and their effects are estimated jointly the bias problem does not occur. A similar bias problem can appear with the remaining outliers, although it is normally not as serious as that of level shifts. To avoid the bias and false detections, it is advisable to correct the outliers one by one and always include the detected outliers in a joint estimation. Note that if the series are nonstationary the IO may be confused with other outlying effects such as with level shifts in an I(1) series and with ramp effects in an I(2) process. Before accepting an IO in an integrated time series, one should always check to ensure that it is not masking with another type of outlier.

## 4.4.1 Other Procedures for Univariate Outlier Detection

There exist some alternative approaches to outlier detection. We briefly mention some of them below. To begin, there is substantial evidence that using the same critical value for all the test statistics can misidentify a level shift as an innovative outlier; see Balke (1993) and Sánchez and Peña (2003). The latter authors showed that the critical values for the test statistic for detecting level shift are different from those for testing additive or innovative outliers. Therefore, it is important to identify level shifts in a series before checking for other types of outlier. Instead of testing for level shift with an intervention model, Bai (1994) proposed a cusum statistic that has several advantages over the LRT intervention model. First, it is not necessary to specify the order of the ARMA model, which can be difficult in the presence of level shifts. Second, the statistic seems to be robust to the presence of other outliers whereas the LRT statistic is not. Galeano et al. (2006) showed in a Monte Carlo study that this approach works better than the LRT. The method is explained in the Appendix.

Next, if a series has several outliers, the parameter estimates can encounter marked biases that, in turn, affect the power of test statistics. Some alternatives to the initial cleaning procedure of the previous section have been studied by Luceño (1998) and Justel et al. (2000). Hendry et al. (2008) proposed a fast way to detect AOs and level shifts using highly saturated intervention models. To simplify the presentation, suppose we are interested in detecting AOs in an AR(p) process. These authors propose to estimate the model via

```
z_t = \sum_{i=1}^p \phi_i z_{t-i} + \sum_{j=h_1}^{h_2} w_j I_t^{(j)} + u_t, \quad t = 1, \dots, T, \quad (4.32)
```

where \(I_t^{(j)}\) is the indicator variable for time index \(j\) and \(h_1\) and \(h_2\) are two positive integers. The procedure works as follows:

- introduce indicator variables in the first half of the observations and estimate Model (4.32) with \(h_1 = 1\) and \(h_2 = [n/2]\), which is the integer part of \(n/2\). Identify observations with significant \(\hat{w}_j\) estimates and treat those data points as a set of potential outliers;
- estimate Model (4.32) with \(h_1 = [n/2] + 1\) and \(h_2 = T\) in the second half of the sample. Augment the set of potential outliers by including data points with significant estimates \(\hat{w}_j\);
- estimate a model with indicator variables for observations in the set of potential outliers and check the new estimates \(\hat{w}_i\) for significance.

In order to control for the large number of tests made in this procedure, the significance level of the \(t\) tests for \(H_0 : w = 0\) is chosen as \(\alpha = 1/T\), so that we expect to find \(T\alpha = 1\) false outlier in the sample. An advantage of the method is that it simultaneously takes care of robust parameter estimation and outlier detection, because the impacts of suspicious observations are taken care of. A drawback, however, is that it may fail to identify correctly the number of outliers. Suppose an AO occurs at \(t = h\). The information about its size is contained in the \(p\) residuals \(\hat{e}_h, \dots, \hat{e}_{h+p}\) and, therefore, we may find several outliers at time points \(h + j\) that are caused by the single outlier at \(t = h\). Nevertheless, the procedure provides a fast way to carry out robust estimation to AOs with many time series. For LS, the efficiency is smaller because all the step variables are highly correlated and it becomes more difficult to separate their effects to identify the level shifts.

## Example 4.3

We use the package tsoutliers to detect outliers in univariate time series. The package is based on the detection procedure of Chen and Liu (1993), which is similar to the one described in Section 4.4. We employ the first two series of hourly AirBox measurements of PM2.5 in March 2017, Taiwan. The model selected by the program is an ARIMA(1,0,1) for the first series and an ARIMA(2,0,0) for the second one.

```
> require(tsoutliers)
>x= read.csv("TaiwanAirBox032017.csv")
>x=as.matrix(x)
>x=x[,-1]   # Remove the row number
>y1=as.ts(x[,1])
>y2=as.ts(x[,2])
>M1=tso(y1)
>M1$outliers
```

| type | ind | time | coefhat   | tstat     |
|------|-----|------|-----------|-----------|
| TC   | 14  | 14   | -16.71296 | -4.207933 |
| LS   | 86  | 86   | 17.30870  | 6.124887  |
| LS   | 464 | 464  | -10.35496 | -4.818682 |
| TC   | 608 | 608  | 21.41543  | 5.225062  |
| AO   | 609 | 609  | 20.87007  | 7.108145  |

```
>ylad=M1$yadj
>ts.plot(y1,main="original and adjusted series", ylab="PM2.5",
      xlab="First series Taiwan AirBox data",pch=0,  lty=1)
>lines (ylad,col="red")
>M2=tso(y2)
>M2$outliers
Eight outliers were detected
```

Thus, five outliers are found in the first series of AirBox data and eight in the second series. The detected outliers of the first series include two level shifts, two temporary level changes, and one AO. Figure 4.5 presents the time plots of the original series and the outlier-adjusted series.

## 4.4.2 New Approaches to Outlier Detection

With advances in high-dimensional statistical analysis, one can use penalized linear regression methods to detect outliers in a more direct way, making the new methods more computationally efficient for handling a collection of many time series. The method used in our study is the Lasso regression, which is briefly mentioned in Chapter 1 and discussed in detail in Chapter 7. Here we use Lasso with step functions to detect level-shifts and, with indicator variables, to detect AOs. The nice feature of Lasso linear regression is that the number of predictors can be larger than the sample size. Consequently, we can consider simultaneously all possible level-shifts or all AOs in a time series.
