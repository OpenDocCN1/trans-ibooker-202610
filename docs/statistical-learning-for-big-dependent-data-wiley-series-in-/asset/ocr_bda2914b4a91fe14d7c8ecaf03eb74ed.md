```
labels abs.freq rel.freq
1 1 34 0.066929134
2 2 184 0.362204724
3 3 1 0.001968504
4 4 279 0.549212598
5 5 1 0.001968504
6 6 1 0.001968504
7 7 1 0.001968504
8 8 1 0.001968504
9 9 1 0.001968504
10 10 1 0.001968504
11 11 1 0.001968504
12 12 1 0.001968504
13 13 1 0.001968504
14 14 1 0.001968504
##The result is three clusters and several outlier series.
##The three clusters are plotted with their location
>g1=which(scl$labels==1)
>g2=which(scl$labels==2)
>g3=which(scl$labels==4)
>g1lo=loc1[g1,]
>g2lo=loc1[g2,]
>g3lo=loc1[g3,]
>par(mfrow=c(2,2))
>plot (loc1)
>plot (g2lo)
>plot (g3lo)
>plot (gilo)
```

Turn to the first-differenced series of electricity price of New England regions. Figure 5.18 shows the step plots of Section 5.2.3.1. Again, the same four distance measures are used. They are based on 5 lags of ACF and PACF, the PIC distance with AR order elected automatically, and the PER. The ACF, PACF, and PER distances selects 2, 2, and 3 clusters, respectively. On the other hand, the PIC distance shows a single cluster. We also apply the GCC similarity measure. Using the program GCCclus, the number of lags selected is 10 and the number of clusters with the Silhouette statistic is 14. The Dendrogram is shown in Figure 5.19 from Alonso and Peña (2019). Four out of the 14 clusters have 192 observations and correspond to all the series of Tuesday, Wednesday, Thursday, and Friday. Monday series are split into a main group of 191 series and an isolated series (Monday, 10 a.m., Maine), that contains several outlier points. The weekends series, of Saturday and Sunday, are also split into two clusters with (i) sleeping hours from 01 to 06th (or 01 to 07th) hours and (ii) the awake hours from 07 to 24th (08 to 24th) hours.

# R commands used for Electricity price data

```
> sc2=GCCclus(z,toPlot=TRUE) #z: denotes the differenced data
> k used for GCC: 10
> Number of clusters by using Silhouette statistic 14
labels abs.freq rel.freq
1 1 192 0.1428571429
2 2 192 0.1428571429
3 3 56 0.0416666667
```