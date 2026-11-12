
![](img/d4c84ae3c2178f4c506af3dd2ed13122_164_0.png)

Figure 3.11 Time plots of log series of monthly exports and imports of China and United States from January 1992 to December 2018. The original data are from FRED and in US dollars. The left panel shows the export series and the upper plots are for China. Source: Data from Federal Reserve Economic Data (FRED).

where it is understood that we treat the model as given and \(\psi_t\) is the \(\psi\)-weight coefficient matrix of Eq. (3.60). In practice, the forecasts and the covariance matrices of forecast errors can be calculated recursively.

## Example 3.3

Consider the monthly exports and imports of China and United States from January 1992 to December 2018. The data, downloaded from the FRED, are in US dollars and seasonally adjusted. We take the natural log transformation and the transformed data are in the file m-expimpcnus.csv. Figure 3.11 shows the time plots of the four log time series, where the left panel shows the exports and the upper figures are for China. As expected, these plots confirm the increasing trend in international trade of the two countries. Therefore, we take the first difference. Let \(z_t = (\text{exp-CN}_t, \text{exp-US}_t, \text{imp-CN}_t, \text{imp-US}_t)'\) be the growth series of the monthly international trade. The \(z_t\) series are shown in Figure 3.12. From the plots, there exist some outlying observations, but, for simplicity, we do not consider any treatment for those aberrant values.

Following the modeling approach in Tsay (2014, chapter 3), we use the extended cross-correlation matrices of Tiao and Tsay (1983) to specify the VARMA order. Details of the output are in the attached R output. For this particular instance, a VARMA(1,2) model is specified. The maximum likelihood estimation (MLE) shows that there are many insignificant estimates in the fitted VARMA(1,2) model so we refine the model by removing simultaneously all estimates with t-ratio less than 0.8 in absolute value. The resulting model is

```
z_t = \begin{bmatrix} 0.01 \\ 0 \\ 0.01 \\ 0 \end{bmatrix} + \begin{bmatrix} -0.18 & 0 & 0 & 0.45 \\ 0.28 & -0.27 & 0 & 0.28 \\ 0 & 0.27 & -0.53 & 0 \\ 0.14 & 0.07 & 0.29 & 0 \end{bmatrix} z_{t-1} + a_t \\ - \begin{bmatrix} 0.49 & 0 & -0.02 & 0 \\ 0.27 & 0 & 0 & 0 \\ 0 & 0 & 0 & -0.37 \\ 0.13 & 0 & 0.23 & 0 \end{bmatrix} a_{t-1} + \begin{bmatrix} 0 & 0.22 & -0.06 & 0.63 \\ 0.20 & -0.22 & 0 & 0.32 \\ 0 & 0.36 & -0.28 & 0.57 \\ 0.06 & 0 & 0.18 & 0 \end{bmatrix} a_{t-2}
```

and the covariance matrix of $a_t$ is

```
\hat{\Sigma}_a = 10^{-3} \begin{bmatrix} 2.987 & 0.182 & 1.325 & 0.108 \\ 0.182 & 0.447 & 0.346 & 0.205 \\ 1.325 & 0.346 & 3.448 & 0.253 \\ 0.108 & 0.205 & 0.253 & 0.424 \end{bmatrix}
```

Model checking indicates that the residual series have some cross-correlations at the seasonal lag 12, indicating that some seasonality remains. However, there are no significant serial correlations at the first 10 lags.

From the fitted model, the four series of international trade are inter-related. This is confirmed by the impulse response functions of the model; see Figure 3.13. However, the impulse response functions show that the dependence of China imports on other series is rather weak. The model also shows that the export and import series of China have significant constant terms, supporting the common belief that the strong growth of China economy over the past two decades dramatically increased its international trade. The variabilities of the two Chinese series are also greater than those of the United States.

Finally, the fitted model can be used to produce forecasts. In this particular instance, the 1-step ahead predictions of the four series are 0.063(0.055), −0.001(0.021), 0.091(0.059), and −0.021(0.021), respectively, where the numbers in parentheses denote forecast standard errors. While the approximate 95% prediction intervals contain zero, the predictions suggest that the exports and imports of China would continue to grow despite trade war with the United States. In fact, CNBC reported on 13 February 2019, that the China exports increased by 9.1% from a year ago in January, whereas imports declined by 1.5%. For this particular instance, the fitted model seems to provide reasonable forecasts.

## **R Demonstration**: International trade of China and the United States

```
> da <- read.csv("m-eximpcnus.csv") ## logged data
> da <- da[,-1] ## remove observation sequence
> require(MTS)
> xt <- diffM(da) ## May use diff(da) provided that "da" is a matrix.
> tdx <- c(2:325)/12+1992 ## Create calendar time
> MTSplot(da,tdx)

> Eccm(xt)
```

### p-values table of Extended Cross-correlation Matrices:
Column: MA order
Row: AR order

| | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|---|---|
| 0 | 0.0000 | 0.0000 | 0.0007 | 0.6472 | 0.5537 | 0.8951 | 0.6524 |
| 1 | 0.0000 | 0.0000 | 0.026 | 0.8999 | 0.7330 | 0.9054 | 0.9040 |
| 2 | 0.1933 | 0.9199 | 0.5442 | 0.9776 | 0.8144 | 1.0000 | 0.9995 |
| 3 | 1.0000 | 0.9994 | 1.0000 | 1.0000 | 0.9995 | 0.9999 | 0.9999 |
| 4 | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 0.8856 |
| 5 | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 1.0000 | 0.9998 | 0.9929 |

```
> ml <- VARMA(xt,p=1,q=2) ## Output omitted.
> mla <- refVARMA(ml,thr=0.8)
```

Estimates in matrix form:
Constant term:
Estimates: 0.009814692 0 0.0135986 0

### AR coefficient matrix

AR( 1)-matrix

| | [,1] | [,2] | [,3] | [,4] |
|---|---|---|---|---|
| [1,] | -0.177 | 0.0000 | 0.000 | 0.449 |
| [2,] | 0.277 | -0.2673 | 0.000 | 0.280 |
| [3,] | 0.000 | 0.2675 | -0.532 | 0.000 |
| [4,] | 0.136 | 0.0724 | 0.288 | 0.000 |

### MA coefficient matrix

MA( 1)-matrix

| | [,1] | [,2] | [,3] | [,4] |
|---|---|---|---|---|
| [1,] | 0.486 | 0 | -0.0224 | 0.000 |
| [2,] | 0.272 | 0 | 0.0000 | 0.000 |
| [3,] | 0.000 | 0 | 0.0000 | -0.374 |
| [4,] | 0.130 | 0 | 0.2332 | 0.000 |

### MA( 2)-matrix

| | [,1] | [,2] | [,3] | [,4] |
|---|---|---|---|---|
| [1,] | 0.0000 | -0.224 | 0.0648 | -0.632 |
| [2,] | -0.1951 | 0.219 | 0.0000 | -0.316 |
| [3,] | 0.0000 | -0.360 | 0.2812 | -0.570 |
| [4,] | -0.0621 | 0.000 | -0.1811 | 0.000 |

### Residuals cov-matrix:

| | [,1] | [,2] | [,3] | [,4] |
|---|---|---|---|---|
| [1,] | 0.0029774472 | 0.0001818597 | 0.0013214291 | 0.0001074424 |
| [2,] | 0.0001818597 | 0.0004460318 | 0.0003451413 | 0.0002049293 |
| [3,] | 0.0013214291 | 0.0003451413 | 0.0034373700 | 0.0002521699 |
| [4,] | 0.0001074424 | 0.0002049293 | 0.0002521699 | 0.0004224317 |

--
aic= -27.32588
bic= -26.98671

```
> MTSdiag(mla)
```

### Ljung-Box Statistics:

| | m | Q(m) | df | p-value |
|---|---|---|---|---|
| [1,] | 1.0 | 15.7 | 16.0 | 0.47 |
| [2,] | 2.0 | 20.2 | 32.0 | 0.95 |
| [3,] | 3.0 | 44.1 | 48.0 | 0.63 |
| [4,] | 4.0 | 64.6 | 64.0 | 0.46 |
| [5,] | 5.0 | 83.5 | 80.0 | 0.37 |
| [6,] | 6.0 | 94.6 | 96.0 | 0.52 |
| [7,] | 7.0 | 108.6 | 112.0 | 0.57 |
| [8,] | 8.0 | 121.6 | 128.0 | 0.64 |
| [9,] | 9.0 | 156.7 | 144.0 | 0.22 |
| [10,] | 10.0 | 185.1 | 160.0 | 0.09 |
| [11,] | 11.0 | 219.8 | 176.0 | 0.01 |
| [12,] | 12.0 | 289.1 | 192.0 | 0.00 |

```
> VARMApred(mla,4)
Predictions at origin 323
[,1]   [,2]   [,3]   [,4]
[1,] 0.063025 -0.001014 0.091208 -0.020800
[2,] 0.011863 0.004853 0.021111 -0.004543
[3,] 0.005677 0.000716 0.003671 0.008047
[4,] 0.012422 0.003634 0.011838 0.001880
Standard errors of predictions
[,1]   [,2]   [,3]   [,4]
[1,] 0.05457 0.02112 0.05863 0.02055
[2,] 0.06537 0.02191 0.06590 0.02095
[3,] 0.06731 0.02302 0.06681 0.02159
[4,] 0.06733 0.02342 0.06696 0.02232
```

## 3.5 UNIT ROOTS AND CO-INTEGRATION

As discussed in Chapter 2, a scalar time series \(z_t\) is a unit-root process of order 1 if \(w_t = (1 - B)z_t\) is a stationary and invertible process. In the economic literature, \(z_t\) is called an I(1) process, meaning that it is an integrated process of order 1. In general, \(z_t\) is an I(d) process if \(w_t = (1 - B)^d z_t\) is stationary and invertible. The I(1) processes play an important role in analyzing business and economic time series. For instance, the quarterly GDP of an economy often forms an I(1) process. Similarly, most prices (or log prices) of stocks are also I(1) processes. In this section, we consider multivariate integrated processes with emphasis on the issues of spurious relation and co-integration.

A multivariate time series \(\mathbf{z}_t\) of Eq. (3.1) is an integrated process if \(|\phi(1)| = 0\), but \(|\theta(1)| \neq 0\). In other words, "1" is a solution of the determinant equation \(|\phi(B)| = 0\), but not \(|\theta(B)| = 0\). Integrated processes exist in many real applications, especially the I(1) processes, and their properties have been widely studied in the literature. See, for instance, Phillips (1987), Chan and Wei (1988), and Tsay and Tiao (1990), among many others.
