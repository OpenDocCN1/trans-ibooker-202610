```
> nn4 <- paste(n2,4,sep="")
> nn5 <- paste(n2,5,sep="")
> nn6 <- paste(n2,6,sep="")
> nn7 <- paste(n2,7,sep="")
> nn8 <- paste(n2,8,sep="")
> colnames(X) <- c(n1,nn1,nn2,nn3,nn4,nn5,nn6,nn7,nn8)
> require(glasso)
> X <- as.matrix(X)
> m1 <- gglasso(X,y,group=grp,loss=c("ls"))
> plot(m1)
> cv.m1 <- cv.gglasso(X,y,group=grp,loss=c("ls"))
> names(cv.m1)
[1] "lambda"       "cvm"          "cvsd"         "cvupper"      "cvlo"
[6] "name"         "gglasso.fit"  "lambda.min"   "lambda.1se"
> plot(cv.m1)
> cv.m1$lambda.min
[1] 0.03526331
> beta <- coef(m1,s=cv.m1$lambda.min)
> length(beta[,1]) ## include the constant term
[1] 49
> plot(1:49,beta[,1],xlab="predictor index",ylab="beta-hat",pch="o",cex=0.6)
> abline(h=c(0))
> idx <- c(1:49)[abs(beta[,1]) > 0.04]
> idx
[1]  1  2 10 14
> pre <- c("cnst",colnames(X))
> pre[idx]
[1] "cnst" "US1"  "UK1"  "CA1"
> grp
[1] 1 1 1 1 1 1 1 1 2 2 2 2 3 3 3 3 4 4 4 4 5 5 5 5
6 6 6 6 7 7 7 7 8 8 8 8 9 9 9 9
```

# 7.2 IMPACTS OF DYNAMIC DEPENDENCE ON LASSO

As seen in the previous section, most properties of penalized regression models were derived under the independence assumption. On the other hand, in most applications, the data are likely to be dynamically dependent either in time or in space or both. In this section, we first summarize the consequences of dynamic dependence in regression analysis and then discuss the situation of Lasso regression. It is well known, see, for instance, Greene (2003) or Griffiths et al. (1985), that when applying regression analysis to time series data, the noises usually have autocorrelations. Consequently, using the LS estimation instead of the generalized LS has the following shortcomings:

+   1. The LS estimator is unbiased, but inefficient;
+   2. The LS variances of the coefficient estimates are biased, and generally under-estimate the true variances of the coefficients;
+   3. The residual variance is underestimated.

These properties of the LS estimation can produce some undesirable effect such as the *spurious regression*, where explanatory variables unrelated to the response may