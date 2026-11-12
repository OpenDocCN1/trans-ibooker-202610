![](bbox=[0.155, 0.095, 0.845, 0.495])

Figure 3.7 Impulse response functions of the model in Eq. (3.54). Orthogonal innovations are used.

to produce forecasts. Details of 1-step to 4-step ahead forecasts and their associated errors are shown in the R output. The forecast variance decomposition is also given.

# R output for GDP Data: Output edited to save space.

```
> gdp <- read.table("gdpsimple6c8018.txt",header=T)
> dim(gdp)
[1] 153  6
> tdx <- c(1:153)/4 + 1980 ## Define calendar time (quarters)
> MTSplot(gdp,tdx)   ### Time series plots
> VARorder(gdp) ## Order selection
selected order: aic =  13
selected order: bic =  1
selected order: hq =  1
Summary table:
         p     AIC     BIC     HQ     M(p) p-value
[1,]     0 -6.8887 -6.8887 -6.8887   0.0000 0.0000
[2,]     1 -7.7959 -7.0829 -7.5063 182.5630 0.0000
[3,]     2 -7.7156 -6.2895 -7.1362  49.3614 0.0681
[4,]     3 -7.5512 -5.4121 -6.6823  36.9038 0.4269
[5,]     4 -7.5679 -4.7157 -6.4093  55.7927 0.0187
[6,]     5 -7.4977 -3.9324 -6.0494  43.4377 0.1841
[7,]     6 -7.3380 -3.0598 -5.6001  31.8750 0.6652
[8,]     7 -7.1339 -2.1425 -5.1063  25.7069 0.8983
[9,]     8 -7.3742 -1.6699 -5.0570  64.3401 0.0025
```