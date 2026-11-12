an initial forecast. With the availability of high-frequency data $\{x_{t,j}\}$, one can make use of the newly available information to update the forecast of $y_t$. Such an updating approach to forecasting is referred to as *nowcasting*, because some information in the current time period $t$ is used in the prediction.

Again, we use a single dependent variable and a single high-frequency predictor $\{x_{t,j}|j = 1, ..., g\}$ to introduce nowcasting. Consider the situation of $j_o$ into the quarter $t$, where $1 \le j_o < g$. The nowcasting regression may assume the form

```
$y_i = \phi_0 + \sum_{j=1}^p \phi_j y_{i-j} + \sum_{v=1}^q \beta_v \left[ \sum_{j=1}^g w_j x_{(i-v)g-(g-j)} \right] + \sum_{j=1}^{j_o} \gamma_j x_{ig-(g-j)} + e_i, \quad (7.57)$
```

where all parameters, but those of the newly added predictors $x_{i,j}$, are defined in Eq. (7.56) and $\gamma_j$ denotes the conditional contribution of $x_{i,j}$ on $y_i$ in the presence of all other predictors. If $j_o$ is large, then $\gamma_j$ may follow some weight functions similar to those used in Eq. (7.56). In other words, an alternative model for nowcasting is

```
$y_i = \phi_0 + \sum_{j=1}^p \phi_j y_{i-j} + \sum_{v=1}^q \beta_v \left[ \sum_{j=1}^g w_j x_{(i-v)g-(g-j)} \right] + \gamma \sum_{j=1}^{j_o} v_j x_{ig-(g-j)} + e_i, \quad (7.58)$
```

where $v_j$ are weights for aggregating the predictors $\{x_{t,j}|j = 1, ..., j_o\}$ in time period $t$.

Obviously, the idea of nowcasting applies to most statistical forecasting models when high-frequency predictors are available. For instance, instead of using the weight functions of the MIDAS regression, one can cast the model as a high-dimensional AR model with exogenous variables and estimate the model by some regularized estimation method discussed before, e.g. the Lasso method.

MIDAS regression is available in R via the package *midasr*. The package can also be used to compute the weights of Almon lag polynomials. Details of the package can be found in Ghysels et al. (2016).

# Example 7.9

To illustrate the MIDAS regression, we consider the growth rate of US quarterly GDP from the first quarter of 1986 to the second quarter of 2019. The two high-frequency predictors used are (a) changes in the monthly unemployment rates, ($x_{t,i}|i=1, 2, 3$), and (b) changes in the weekly crude oil price, Western Texas Intermediate, Cushing, Oklahoma, ($z_{t,i}|i=1, ..., 12$). Thus, we have one monthly and one weekly exogenous predictor. For the forecasting model of Eq. (7.56), we use $p=2$ for the AR part, $q=2$ for unemployment rate and $q=4$ for crude oil price. In addition, we use exponential Almon lag polynomial (*nealmon* command in *midasr*) as the weight function. For the monthly unemployment rate, the lags used are 3-5 and for the weekly crude oil price, the lags used are 12-23. These lags cover the high-frequency predictors in a quarter. The R commands used for such a MIDAS regression and the associated output are given below in the R demonstration. In this particular case, the output shows that the conditional contributions of the high-frequency predictor are not strong.

Turn to nowcasting. Two cases are considered. In the first case, we use high-frequency data one month into the quarter. Here we use $q=2$ for the unemployment