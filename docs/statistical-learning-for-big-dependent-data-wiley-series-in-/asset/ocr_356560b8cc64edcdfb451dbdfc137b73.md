and (b) for a given lag, we treat the growth rates of other countries equally. There are other ways to group the predictors, our grouping is mainly for illustrative purpose. There are several R packages available for Group Lasso, e.g. gglasso and grplassp. We use gglasso in our analysis. The R commands used are given below, including grouping. Figure 79 shows the coefficient profile of Group Lasso for the GDP example. This plot can be interpreted in a similar way as that of Lasso. As expected, the profile shows that lagged predictors are useful in predicting US GDP growth rate. Figure 710 shows the result of 10-fold CV in selecting the penalty parameter \(\lambda\). The plot shows that the minimum of LS loss is achieved approximately at \(ln(\lambda) = -3.4\). Figure 711 shows the plot of \(\hat{\beta}_i\) (including the constant term) when \(\lambda\) is at the minimum of CV. From the plot, we see that lag-1 predictors are more important

![](bbox=[0.23, 0.31, 0.8, 0.53])

# **Figure 7.9** Coefficients versus ln(lambda) of Group Lasso for the quarterly GDP growth rates of six countries from 1980 to 2018. The dependent variable is US GDP growth rate.

![](bbox=[0.23, 0.605, 0.8, 0.83])

# **Figure 7.10** Results of 10-fold CV of Group Lasso for the GDP growth series. The dependent variable is the US GDP growth rate.