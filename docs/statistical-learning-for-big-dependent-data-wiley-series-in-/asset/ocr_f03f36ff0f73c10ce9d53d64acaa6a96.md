![](bbox=[0.26, 0.1, 0.73, 0.45])

Figure 3.10 Sample autocorrelation functions of the simple returns of five US market indexes.

**Remark.** Some remarks are in order. Readers who are familiar with seasonal time series models may wish to entertain a multiplicative seasonal model for the five return series. We did not consider any multiplicative model because there exist no significant serial correlations at lag 13. Also, in the R output, we refined the estimation three times using the same threshold 1.5. This is because the model changes after each refinement, so do the *t*-ratios of coefficient estimates.

# R Demonstration: VMA modeling.

```
> VMAorder(rtn)
Q(j,m) Statistics:
j Q(j,m) p-value
[1,] 1.0 780.8 0.00
[2,] 2.0 642.8 0.00
[3,] 3.0 609.0 0.00
[4,] 4.0 580.4 0.00
[5,] 5.0 565.4 0.00
[6,] 6.0 539.3 0.00
[7,] 7.0 510.6 0.00
[8,] 8.0 466.3 0.00
[9,] 9.0 446.6 0.00
[10,] 10.0 420.8 0.00
[11,] 11.0 375.9 0.00
[12,] 12.0 340.0 0.00
[13,] 13.0 213.6 0.24
```