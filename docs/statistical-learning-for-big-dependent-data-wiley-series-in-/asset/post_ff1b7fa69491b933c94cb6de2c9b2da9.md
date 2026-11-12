
$AP = P\Lambda$, where $\Lambda = \text{diag}\{\lambda_1, \dots, \lambda_k\}$; (2) $P'P = PP' = I$, the \(k\)-dimensional identity matrix; (3)

$$A = P\Lambda P' = \sum_{i=1}^k \lambda_i e_i e_i'.$$

Property (3) is referred to as the spectral decomposition of the positive-definite matrix \(\boldsymbol{A}\). Using the spectral decomposition and properties of eigenvalues and eigenvectors of \(\boldsymbol{A}\), we have the following properties.

### Property I

Assume that \(\boldsymbol{A}\) is a positive-definite \(k \times k\) real-valued matrix with spectral decomposition given by eigenvalue-eigenvector pairs \((\lambda_i, \boldsymbol{e}_i)\). Then,

$$\max_{\boldsymbol{x} \neq \boldsymbol{0}} \frac{\boldsymbol{x}' \boldsymbol{A} \boldsymbol{x}}{\boldsymbol{x}' \boldsymbol{x}} = \lambda_1$$

$$\min_{\boldsymbol{x} \neq \boldsymbol{0}} \frac{\boldsymbol{x}' \boldsymbol{A} \boldsymbol{x}}{\boldsymbol{x}' \boldsymbol{x}} = \lambda_k,$$

where the maximum and minimum are attained with \(\boldsymbol{x} = \boldsymbol{e}_1\) and \(\boldsymbol{x} = \boldsymbol{e}_k\), respectively. Furthermore,

$$\max_{\boldsymbol{x} \neq \boldsymbol{0}, \boldsymbol{x}' \boldsymbol{e}_j = 0, j=1, \dots, i} \frac{\boldsymbol{x}' \boldsymbol{A} \boldsymbol{x}}{\boldsymbol{x}' \boldsymbol{x}} = \lambda_{i+1}$$

where the maximum is attained when \(\boldsymbol{x} = \boldsymbol{e}_{i+1}\), where \(i = 2, \dots, k\).

## EXERCISES

- 1. Consider the 99 world financial market indexes. Compute the log returns of the indexes. Obtain a time plot of all series and perform a PCA of the log returns. Summarize the results of PCA, including scree plot and PCA plots of the first six PCs. [An R command for PCA is `princomp`].

- 2. Consider the clothing data set of Figure 1.8. Perform PCA on the sales data and summarize the results. Obtain time plots of the first 12 PCs with 6 series on one page.

- 3. Consider again, the clothing data set. Obtain the three summary plots of the sample cross-correlations for lags 1 to 21.

- 4. Consider the temperature data of Figure 1.1. (a) Obtain the sample mean and sample covariance matrix of the data. (b) Obtain the lag-1 to lag-10 sample CCMs of the data.

- 5. Consider the hourly PM\(_{2.5}\) measurements at 15 monitoring stations in the southern Taiwan; columns 4 to 18 of the file `TaiwanPM25.csv`. (a) Compute the sample mean and sample covariance matrix of the 15 time series. (b) Obtain a time plot of the 15 time series.

- 6. Prove Eq. (1.16) and discuss the increase in the variance of sample mean with respect to the independent case when the series has a non-zero first-order autocorrelation coefficient and zero autocorrelations for all higher lags. That is, the series follows an MA(1) model.

## REFERENCES

Bühlmann, P. and van de Geer, S. (2011). Statistics for High-Dimensional Data. Springer, New York, NY.

Chang, J., Guo, B., and Yao, Q. (2014). Segmenting multiple time series by contemporaneous linear transformation: PCA for time series. Working paper, London School of Economics.

Dahlhaus, R. (2012). Locally stationary processes. In Handbook of statistics, 30: 351–413.

Peña, D. and Box, G. (1987). Identifying a simplifying structure in time series. Journal of the American Statistical Association, 82: 836–843.

Peña, D. and Rodriguez, J. (2003). Descriptive measures of multivariate scatter and linear dependence. Journal of Multivariate Analysis, 85: 361–374.

Reinsel, G. (1993). Elements of Multivariate Time Series Analysis. Springer-Verlag, New York, NY.

Taniguchi, M. and Krishnaiah, P. (1987). Asymptotic distributions of functions of the eigenvalues of sample covariance matrix and canonical correlation matrix in multivariate time series. Journal of Multivariate Analysis, 22: 156–176.

Tiao, G. C. and Box, G. E. P. (1981). Modeling multivariate time series with applications. Journal of American Statistical Association, 72: 802–816.

Tibshirani, R. (1996). Regression shrinkage and selection via Lasso. Journal of the Royal Statistical Society, Series B, 58: 267–288.

Tsay, R. S. (2014). Multivariate Time Series Analysis with R and Financial Applications. John Wiley & Sons, Hoboken, NJ.

Zhang, X. and Tong, H. (2020). Some cautionary comments on PCA for time series data. Working paper, London School of Economics and Political Science, UK.

## LINEAR UNIVARIATE TIME SERIES

In this chapter, we study methods for modeling and forecasting univariate time series. Our study is brief and focuses on the automatic analysis of large sets of time series. Interested readers can consult any of the many time series textbooks for further details. See, for instance, Peña et al. (2001), Box et al. (2015), Brockwell and Davis (2013), Cryer and Chan (2008), Shumway and Stoffer (2017), and Tsay (2010, 2014), among others.

Given a set of time series, the first step of the analysis is to visualize the data by plotting the series in order to understand their basic properties. One would also try to detect gross measurement errors that are quite common with data recorded in an automatic way. These errors may happen at some specific times in many series, or be concentrated in a few series making them different from the others. The graphical representations we present are designed to identify both cases of measurement errors. Once the set of time series has gone through some cleaning, we present statistical methods to model and forecast the series. The models most commonly used for stationary time series are the autoregressive moving-average (ARMA) models, that have proved to be useful in many scientific fields. Particular members of this family are the autoregressive (AR) models, where the series depends only on its first p lagged values with p being a positive integer. AR models are characterized by having infinitely many non-zero autocorrelation coefficients which decay exponentially to zero as the lag increases. Thus, AR processes have a relatively long memory, since the current value of a series is correlated with many previous ones, although with vanishing coefficients. Scalar processes with finite-memory property are the moving-average (MA) models, that are formed by weighted averages of a finite number of past innovations (or noises). Combining the AR and MA structures together, we obtain the ARMA models, that provide a broad and flexible family of stationary stochastic processes useful in representing many empirical time series.

Some stationary time series can be thought of as a mixture of several cyclical processes. This is the motivation for the spectral analysis in which the variability of the series is explained by harmonic processes with different frequencies and variabilities. The spectrum of a stationary time series is a linear combination of its autocovariances of all lags, and each ARMA process implies a given spectrum and vice versa. This approach, referred to as the frequency domain analysis, provides complementary information to the time domain analysis using ARMA models.

A time series can be nonstationary in the mean, the variance, the autocorrelations, or in other characteristics of its distribution. As mentioned in Chapter 1, the most important nonstationary processes are the integrated processes, especially the I(1) processes. The basic feature of an I(1) process is that its mean changes over time, but its increments form a stationary process. Therefore, the series can become a stationary one by differencing. The family of autoregressive integrated moving-average (ARIMA) models applies to time series that become stationary ARMA process by differencing. An important property, that distinguishes integrated processes from stationary ones, is the way by which the serial dependence behaves. For stationary processes the autocorrelations decay to zero exponentially with the increase in lag, whereas those of an integrated time series do not converge to zero as the lag increases.

Another important class of nonstationary processes consists of seasonal processes, for which the mean of the series is not constant, but exhibits a cyclical pattern. For example, the series of monthly temperature at a geographical location often shows strong seasonality with higher mean values in the summer. If this seasonal pattern is persistent, then one would need a seasonal difference to transform the series into stationarity. ARIMA models with seasonal differencing are called seasonal ARIMA (SARIMA) models.

An alternative way to model nonstationary time series is to treat it as an additive model of trend, seasonality, and irregular components. This approach is called the structural time series modeling. It is a special case of a general procedure to describe the evolution of a dynamic system, called the state space representation. One advantage of the state-space model (SSM) is that it provides an effective recursive way for estimating the model parameters and generating forecasts. Also, this model generalizes straightforwardly to multivariate time series, which we discuss in Chapter 3.

The second part of this chapter studies how to build ARIMA models for a large set of time series, with emphasis in automatic procedures. The fitting of a model to each time series is carried out in three steps. In the first step, we examine whether a transformation (power family) is required for the series and then classify the resulting series into one of the following categories: stationary, unit-root nonstationary, or seasonal series. This step can be regarded as a classification step. The second step is model selection in which multiple models are estimated and an information criterion is used to select the best one among the entertained models. In the third step, diagnostic checking is performed and the fitted model is refined, if necessary. The selected model for each series can then be used for forecasting. Keep in mind that in some applications several models may fit a given time series well and one can then use model average to produce combined forecasts.
