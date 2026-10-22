![](bbox=[0.18, 0.14, 0.77, 0.6])

The 4 points at the top of this plot are irksome, as they suggest a touch of heteroscedasticity, but most of the residuals are certainly within 2 RSEs of 0. Moreover, the residuals appear randomly distributed and do not exhibit any pronounced nonlinear trends. All together, then, the assumptions about ε seem reasonable, so let’s explore the model further. To complement our inspection of the residuals, let’s check a histogram of Strength to see if the response variable seems roughly normally distributed :

```
>>> sns.histplot( data=concrete, x="Strength")
```

Figure 3.5. Strength values are roughly normally distributed