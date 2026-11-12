
lines for each value of the $\ell_1$ norm is not the same and increases with the $\ell_1$ norm. The numbers shown on top of the plot are how many non-zero coefficient are estimated for a given $\ell_1$ norm. For instance, Figure 7.2 shows that there are 189 non-zero coefficient estimates when $\ell_1$ norm is 5. Note that the maximum number of coefficients is 732 and for $\ell_1 = 15$ only 351 coefficients, less than half of the possible, are shown. The package **glmnet** allows for different types of regression coefficients profile plot. Figure 7.3 shows another coefficient profile plot, where the x-axis is the logarithm of the penalty parameter $\lambda$, and it provides the same information but in a different way. Again each coefficient is represented by a line, but now as heavier penalty reduces the number of non-zero coefficient estimates, the number of lines in

![](img/d4c84ae3c2178f4c506af3dd2ed13122_391_0.png)

Figure 7.2 Coefficients versus $\ell_1$-norm of Lasso linear regression for the monthly growth rates of US IPI from 1960 to 2019.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_391_1.png)

Figure 7.3 Coefficients versus log penalty, ln($\lambda$), of Lasso linear regression for the growth rates of US IPI from 1960 to 2019.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_392_0.png)

Figure 7.4 Results of 10-fold CV of Lasso linear regression for the growth rates of US IPI from 1960 to 2019. The left vertical dashed line locates the penalty for minimum MSE and the right vertical dashed line marks the one standard error limit of \(\lambda\).

the plot decreases as \(\lambda\) increases. The numbers on top of the plot are also counts of non-zero coefficient estimates.

CV is often used to select the penalty parameter in linear regression analysis with independent data. For time series data, out-of-sample forecasts are preferred. However, for illustrative purpose of using Lasso, we use CV in this example. Figure 7.4 shows the results of a 10-fold CV (CV). The out of sample mean squared error (MSE) is plotted versus the logarithm of penalty parameter \(\lambda\) and the two vertical dashed lines locate the penalty corresponding to minimum MSE and a possible range of the penalty parameter. From the plot, a choice of \(\log(\lambda) \approx -2.6\) seems reasonable. As a matter of the fact, the CV selects \(\lambda \approx 0.0743\), which is close to \(\exp(-2.6) = 0.07427\). Figure 7.5 shows estimated coefficients \(\hat{\beta}_j\) when \(\lambda = 0.07429\). Based on the plot, we find that there are nine predictors with \(|\hat{\beta}_j| > 0.04\). These predictors are

- 1. IPDMAT: Industrial production: durable materials (lag-1),
- 2. CLAIMX: Initial claims (lag-1),
- 3. NDMANEMP: All employees: nondurable goods (lag-1),
- 4. T1YFFM: 1-Year treasury constant minus Fed Funds (lag-1),
- 5. CES2000000008: Average hourly earnings: construction (lag-1),
- 6. ISRATIOx: Total business: inventories to sales ratio (lag-2)
- 7. S&P div. yield: S&P composite common stock: dividend yield (lag-2),
- 8. S&P indust: S&P's common stock price index: industrials (lag-3),
- 9. TB3SMFFM: 3-Month treasury constant minus Fed Funds (lag-5)

Therefore, the Lasso regression suggests that the growth rates of US IPI depend on various sectors, including output, labor market, inventories, interest rates, and stock market. This seems reasonable. Note that the choice of threshold 0.04 is arbitrary. Since the predictors are standardized, we can directly compare the magnitude of

![](img/d4c84ae3c2178f4c506af3dd2ed13122_393_0.png)

Figure 7.5 Coefficient estimates of Lasso linear regression for the growth rates of US IPI from 1960 to 2019. The penalty parameter is selected by the 10-fold CV.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_393_1.png)

Figure 7.6 Coefficients versus $\ell_1$-norm of elastic net linear regression for the monthly growth rates of U.S. IPI from 1960 to 2019. The penalty is $0.8\ell_1 + 0.2\ell_2$.

estimated coefficient. If a higher threshold, around 0.05, is chosen, then we would select about seven predictors, instead of nine mentioned earlier.

Next, turn to the elastic net with a linear combination defined by the $\alpha$ parameter of the $\ell_1$- and $\ell_2$-penalties. Figure 7.6 shows the coefficient estimates versus $\ell_1$ norm of elastic net regression with $\alpha = 0.8$. This coefficient profile plot works in the same manner as that of Figure 7.2 and, as expected, it provides similar results. Figure 7.7 shows the results of a 10-fold CV versus the logarithm of penalty parameter $\lambda$. From the plot, a choice of $log(\lambda) \approx -2.5$ seems reasonable. Specifically, the CV selects $\lambda \approx 0.0846$. Figure 7.8 shows the estimated coefficients $\hat{\beta}_j$ when $\lambda$ is selected by the CV. Based on the plot, we find that there are 10 predictors with $|\hat{\beta}_j| > 0.04$. These predictors contain the same selections as those of the Lasso regression shown

![](img/d4c84ae3c2178f4c506af3dd2ed13122_394_0.png)

Figure 7.7 Results of 10-fold CV of elastic net linear regression for the growth rates of US IPI from 1960 to 2019. α = 0.8 is used to define the penalty. The left vertical dashed line locates the minimum MSE and the right vertical dashed line provides one-standard error limit.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_394_1.png)

Figure 7.8 Coefficient estimates of elastic net linear regression for the monthly growth rates of US IPI from 1960 to 2019. The penalty parameter is selected by a 10-fold CV and α = 0.8.

before with an additional predictor, which is the Moody's seasoned Aaa corporate bond yield (lag-1). Therefore, the elastic net suggests that the US monthly growth rates of IP index depend on the same sectors as the Lasso regression.

## R commands used: Lasso and elastic net regressions.

```
da <- read.csv( 'FREMDMApril19.csv',header=TRUE)
x <- as.matrix(da)
require(MTS)
y <- scale(x,scale=TRUE) # standardize the data for plotting
tdx <- c(1:710)/12+1960
TSPlot(y,tdx)
```

```r
set.seed(311)
require(glmnet)
Y <- y[7:710, 6]
X <- cbind(y[6:709, ], y[5:708, ], y[4:707, ], y[3:706, ], y[2:705, ], y[1:704, ])
m2 <- glmnet(X, Y, family=c("gaussian"), alpha=1)
plot(m2) #coefficient profile plot (x-axis of ell-1 norm)
plot(m2, xvar='lambda') #coefficient profile plot (x-axis is log penalty)
cv.m2 <- cv.glmnet(X, Y, family=c("gaussian"), alpha=1)
plot(cv.m2)
names(cv.m2)
cv.m2$lambda.min
beta <- coef(m2, s=cv.m2$lambda.min)
beta <- c(beta[-1, 1]) [-1]
plot(1:732,beta,xlab="variable index",ylab="beta-hat",pch="o",cex=0.6)
idx <- c(1:732) [abs(beta) > 0.04]
idx
colnames(X) [idx]
######### glmnet with alpha = 0.8
m3 <- glmnet(X, Y, family=c("gaussian"), alpha=0.8)
plot(m3)
cv.m3 <- cv.glmnet(X, Y, family="gaussian", alpha=0.8)
plot(cv.m3)
cv.m3$lambda.min
betal <- coef(m3, s=cv.m3$lambda.min)
beta1 <- c(betal[-1, 1]) [-1]
plot(1:732,beta,xlab="variable index",ylab="beta-hat",pch="o",cex=0.6)
idx <- c(1:732) [abs(betal) > 0.04]
idx
colnames(X) [idx]
```

## Example 7.2

For HDTS analysis Group Lasso could be useful. In this section, we demonstrate how to use it. We expect that the serial dependence in stationary time series data decays to zero as lag increases. Group Lasso enables us to implement this domain knowledge by grouping predictors of lagged variables together. Consider the quarterly GDP growth rates of six countries used in Chapter 3. The six countries are US, UK, France (FR), Australia (AU), Germany (GE), and Canada (CA). The sample period is from 1980 to 2018. Suppose that we are interested in predicting the GDP growth rate of US. and employ the lagged values of GDP growth rates of all countries as predictor. Here the dependent variable is yt, the GDP growth rate of US. The predictors used consist of lagged values of GDP growth rates of all countries. Specifically, we have \( x_t = (y_{t-1}, \dots, y_{t-8}, z'_{t-1}, \dots, z'_{t-8})' \), where \( z_t \) is the quarterly GDP growth rates of all countries but US. To make use of properties of Group Lasso, we consider the following groups:

- 1. Group 1: The lagged values of US GDP growth rates, i.e. \( y_{t-1}, \dots, y_{t-8} \).
- 2. Group \( i+1 \): The lag-\( i \) values of GDP growth rates of other countries, i.e. \( z_{t-i} \), where \( i = 1, \dots, 8 \).

Therefore, we have divided the 48 predictors into 9 groups. The basic idea of our predictor grouping is that (a) the lagged values of US GDP growth rate are important
