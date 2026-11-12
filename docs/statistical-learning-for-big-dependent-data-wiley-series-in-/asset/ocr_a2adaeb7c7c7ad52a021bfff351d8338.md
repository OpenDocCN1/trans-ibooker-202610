![](bbox=[0.15, 0.1, 0.85, 0.46])

**Figure 5.14** The height plots for 508 first-differenced AirBox PM2.5 series. The four distance measures used are the first 5 lags of ACF in part (a), and PACF in part (b), the PIC with order selected automatically in part (c), and the PER in part (d).

As discussed in Section 5.1.3, an alternative dissimilarity measure between time series is to use cross-correlations. To demonstrate, we also apply the GCC measure of Eq. (5.18) to the differed AirBox PM2.5 data. The command used is `GCCclus`. The number of clusters selected by the Silhouette statistics is equal to three. The membership locations of the three clusters are show in Figure 5.17, where the upper-left plot shows the locations of all 508 series. From the plots, we see that the three clusters now coincide well with the geographical locations of the Air Boxes. Therefore, in this particular instance, clustering by cross-linear dependency seems to be much more informative than that based on the PACF of individual series.

# R commands for locations of Taiwan AirBox data

```
##The locations data are cleaned, 508 series remain
> loc <- read.csv("locations032017.csv")
> loc <- loc[,-1]
> loc1 <- loc[-c(1,29,35,46,70,118,155,157), ]
> dim(loc1)
> par(mfcol=c(1,2))
> plot(loc[,2],loc[,1],xlab="longitude",ylab="latitude")
> plot(loc1[,2],loc1[,1],xlab="longitude",ylab="latitude")
> require(Tsclust)
> da <- read.csv("TaiwanAirBox032017.csv'",)
```