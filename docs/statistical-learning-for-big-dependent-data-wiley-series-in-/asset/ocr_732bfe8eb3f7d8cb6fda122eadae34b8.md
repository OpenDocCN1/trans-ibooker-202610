![](bbox=[0.245, 0.1, 0.8, 0.305])

Figure 7.11 Coefficient estimates of Group Lasso for the GDP data set. The dependent variable is the US GDP growth rate and the penalty is \(\lambda \approx 0.0353\).

than higher-order lagged predictors. As a matter of fact, for this particular instance, the important predictors have predictor indexes 1, 10, 14, and 2 that correspond to the intercept and the lag-1 GDP growth rates of UK, CA, and US. This is reasonable given the close tie between these three economies.

# R Demonstration: Group Lasso for GDP growth rates.

```
> da <- read.table("gdpsimple6c8018.txt",header=TRUE)
> fix(da)
> dim(da)
[1] 153   6
> da[1:2, ]
      US        UK       FR        AU        GE        CA
1 0.3145153 -0.9719569 1.1197030 0.4478638 0.8012350 0.70678280
2 -0.6064770 -1.9796670 -0.5731772 0.2604569 -0.6438022 -0.03293659
> y <- da[9:153,1] ## Dependent variable
> X <- NULL ## setup predictors
> grp <- NULL ## Grouping
> for (i in 1:8) {
+ X <- cbind(X,da[(9-i):(153-i),1])
+ grp <- c(grp,i)
+ }
> for (i in 1:8) {
+ X <- cbind(X,da[(9-i):(153-i),-1])
+ grp <- c(grp,rep(i+1,5))
+ }
> dim(X)
[1] 145  48
> n1 <- paste("US",1:8,sep="") ## name predictors
> n2 <- colnames(da)[-1]
> n2
[1] "UK" "FR" "AU" "GE" "CA"
> nn1 <- paste(n2,1,sep="")
> nn2 <- paste(n2,2,sep="")
> nn3 <- paste(n2,3,sep="")
```