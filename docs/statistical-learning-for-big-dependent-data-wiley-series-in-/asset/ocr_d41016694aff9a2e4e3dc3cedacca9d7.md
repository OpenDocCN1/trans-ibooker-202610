rate and q = 3 for the crude oil price. Details of the commands and the associated output are also given in the R demonstration below. It is interesting to see that for both high-frequency predictors the concurrent values (i.e. unrate11 and coilchg11) are significant at the conventional 5% level. In the second case, we use high-frequency data two months into the quarter. The results are similar to those of the one-month case as the concurrent high-frequency data are helpful in explaining the quarterly growth rate of US GDP.

# **R Demonstration: MIDAS regression.** The R scripts are provided by Dr. Yuefeng Han of the Rutgers University.

+   (A) MIDAS regression for forecasting:

+   (1) R scripts

```
gdp8619<-read.csv("q-gdp8619.csv", header = TRUE)
gdp<-gdp8619$A191RP1Q027SBEA
unrate8619d.csv<-read.csv("m-unrate8619d.csv", header = TRUE)
unrate<-unrate8619d.csv$ratechg
coilchg.csv<-read.csv("w-coilchg.csv", header = TRUE)
coilchg<-coilchg.csv$coilchg
library(midasr)
t=2 # time lag for gdp
t1=3 # time lag for unrate
t2=12 # time lag for crude oil price
fit<-midas_r(gdp~mls(gdp,1:t,1)+mls(unrate,3:(t1+2),3,nealmon)+
mls(coilchg,12:(t2+11),12,nealmon),
start=list(unrate=c(-1,-1),coilchg=rep(0,4)),
#aggregation within a quarter 2 and 4
method="Nelder-Mead")
#print(fit); print(summary(fit))
```

+   (2) Output

```
> names(fit)
[1] "coefficients"   "midas_coefficients" "model"
[4] "unrestricted"   "term_info"      "fn0"
[7] "rhs"            "gen_midas_coef" "opt"
[10] "argmap_opt"     "start_opt"      "start_list"
[13] "call"           "terms"          "gradient"
[16] "hessian"        "gradD"          "Zenv"
[19] "use_gradient"   "nobs"           "tau"
[22] "lhs"            "lhs_start"      "lhs_end"
[25] "convergence"    "fitted.values"  "residuals"
> summary(fit)
```

MIDAS regression model with "numeric" data:
Start = 3, End = 133

```
Formula gdp ~ mls(gdp,1:t,1)+mls(unrate,3:(t1+2),3,nealmon) +
mls(coilchg, 12:(t2 + 11), 12, nealmon)
```

Parameters:

| Estimate | Std. Error | t value | Pr(>|t|) |
|---|---|---|---|
| (Intercept) | 2.03136 | 0.51532 | 3.942 | 0.000135 *** |
| gdp1 | 0.34660 | 0.10876 | 3.187 | 0.001826 ** |
| gdp2 | 0.23341 | 0.10295 | 2.267 | 0.025131 * |
| unrate1 | -0.96387 | 3.29969 | -0.292 | 0.770700 |