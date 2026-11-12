
###The maximum value of the Silhouette is for 2 groups
> memb=cutree(scl,1:10)
> gap.clus (Macf,memb, 100)
$optim.k
[1] 9
$gap.values
gap
gap
1 -0.037521773
2 -0.097965198
3 -0.087936795
4 -0.005051419
5 -0.018106280
6 -0.006000857
7 -0.004245895
8 -0.034548137
9 0.001659488

###The maximum Gap is first 4 then 9.
#compute the size of the 3 groups
> memb=cutree(scl,1:3)
> g1u=which(memb==1)
> g2u=which(memb==2)
> g3u=which(memb==3)
> ng1=length(which(memb==1))
> ng1
[1] 390
> ng2=length(which(memb==2))
> ng2
[1] 107
> ng3=length(which(memb==3))
> ng3
[1] 17

###compares the groups with location data
> ###The location are loaded and analyzed
> locations032017 <- read.csv("XXX/data/locations032017.csv")
> lo=as.matrix(locations032017)
> lo=lo[,-1]
> par(mfrow=c(2,2))
> g1lo=lo[g1u,]
> g2lo=lo[g2u,]
> g3lo=lo[g3u,]
> plot(lo, main="All locations")
> plot(g1lo, main="Group 1 locations")
> plot(g2lo, main="Group 2 locations")
> plot(g3lo, main="Group 3 locations")

To understand better the clustering results of the Taiwan AirBox data, we examine carefully the locations of Air Boxes. The longitude and latitude of each Air Box are in the file locations032017.csv. The left plot of Figure 5.13 shows the locations

![](img/d4c84ae3c2178f4c506af3dd2ed13122_259_0.png)

### Figure 5.13 Longitude and latitude of Air Boxes for the Taiwan AirBox data. The left plot shows the locations of all 516 series whereas the right plot shows the locations of 508 series with six outlier series removed.

of the Air Boxes, from which we see that there exist outlying locations in the location file because the island is located roughly between 120–122 in longitude and 22–25 in latitude. Therefore, we remove eight series from the data set in our further analysis. These are Series 1, 29, 35, 46, 70, 118, 155, and 157, where the removal of Series 29 and 70 were explained before. The right plot of Figure 5.13 shows the locations of the remaining 508 PM2.5 time series around Taiwan.

Using the 508 differenced series of PM2.5 series, we apply the four distance measures and the proposed height and step plots of Section 5.2.3.1 to identify the number of clusters for the dataset. The four distance measures are the distances based on the first 5 lags of ACF and PACF, the PIC distance with order selected automatically, and the PER. Figure 5.14 shows the results of height plots. The height plot of the ACF distance is in part (a), from which we see a clear indication of three clusters. The height plot of PACF distance is in part (b), which indicates about five clusters. The height plot of PIC distance is in part (c), which suggests two clusters and the result for PER in part (d) also suggests five clusters.

Figure 5.15 shows the step plots of the 508 differenced Taiwan AirBox series. Again, the same four distance measures are used in the analysis. In this particular instance, the step plots of PACF and PER distance measures, i.e. parts (b) and (d), suggest three and five clusters, respectively. The result of PIC distance is in part (c) and it suggests two clusters. The step plot of ACF distance, on the other hand, indicates there is only a single cluster. We then apply PACF distance measure with three clusters to the data. The sizes of the three clusters are 132, 346, and 40, respectively. Figure 5.16 shows the locations (longitude and latitude) of members of the three clusters. In this particular instance, the membership locations scatter across the Island, even though there appear to have certain concentration. For example, most of the right-upper locations belong to Cluster 1. Overall, for the first-differenced PM2.5 series, the PACF distance does not provide clusters based purely on the locations of Air Boxes.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_260_0.png)

### Figure 5.14 The height plots for 508 first-differenced AirBox PM2.5 series. The four distance measures used are the first 5 lags of ACF in part (a), and PACF in part (b), the PIC with order selected automatically in part (c), and the PER in part (d).

As discussed in Section 5.1.3, an alternative dissimilarity measure between time series is to use cross-correlations. To demonstrate, we also apply the GCC measure of Eq. (5.18) to the differenced AirBox PM2.5 data. The command used is `GCCclus`. The number of clusters selected by the Silhouette statistics is equal to three. The membership locations of the three clusters are shown in Figure 5.17, where the upper-left plot shows the locations of all 508 series. From the plots, we see that the three clusters now coincide well with the geographical locations of the Air Boxes. Therefore, in this particular instance, clustering by cross-linear dependency seems to be much more informative than that based on the PACF of individual series.

## R commands for locations of Taiwan AirBox data

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
> da <- read.csv("TaiwanAirBox032017.csv")
```

![](img/d4c84ae3c2178f4c506af3dd2ed13122_261_0.png)
![](img/d4c84ae3c2178f4c506af3dd2ed13122_261_1.png)

![](img/d4c84ae3c2178f4c506af3dd2ed13122_262_0.png)

### Figure 5.17 Distribution of the cleaned locations of the Taiwan Air Boxes for all 508 series in upper-left, and for three groups selected by cross dependency with the GCC for the increments of the series

```
> da <- as.matrix(da[,-1])
> da <- da[,-c(1,29,35,46,70,118,155,157)]
> dda <- diffM(da)
> mPACF <- diss(t(dda),METHOD="PACF" ,lag.max=5)
> cPACF <- hclust(as.dist(mPACF),method="complete")
> names(cPACF)
[1] "merge"     "height"    "order"     "labels"    "method"    "call"
[7] "dist.method"
> memb <- cutree(cPACF,3)
> locg1 <- which(memb==1)
> locg2 <- which(memb==2)
> locg3 <- which(memb==3)
> length(locg1)
[1] 132
> length(locg2)
[1] 346
> length(locg3)
[1] 30
> par(mfcol=c(1,3))
> plot(loc1[locg1,2],loc1[locg1,1],xlab="Longitude",ylab="Latitude",
main="Cluster-1")
> plot(loc1[locg2,2],loc1[locg2,1],xlab="Longitude",ylab="Latitude",
main="Cluster-2")
> plot(loc1[locg3,2],loc1[locg3,1],xlab="Longitude",ylab="Latitude",
main="Cluster-3")
>
##A similar analysis with GCC is applied.
##(Not all commands are shown)
##GCC dissimilarity cluster is applied to the cleaned 508 series
> scl=GCCclus(dda,lag=2,silh=1)
> Number of clusters by using Silhouette statistic 14
```

| labels | abs.freq | rel.freq |
|--------|----------|----------|
| 1 | 34 | 0.066929134 |
| 2 | 184 | 0.362204724 |
| 3 | 1 | 0.001968504 |
| 4 | 279 | 0.549212598 |
| 5 | 1 | 0.001968504 |
| 6 | 1 | 0.001968504 |
| 7 | 1 | 0.001968504 |
| 8 | 1 | 0.001968504 |
| 9 | 1 | 0.001968504 |
| 10 | 1 | 0.001968504 |
| 11 | 1 | 0.001968504 |
| 12 | 1 | 0.001968504 |
| 13 | 1 | 0.001968504 |
| 14 | 1 | 0.001968504 |

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
>plot (g1lo)

Turn to the first-differenced series of electricity price of New England regions. Figure 5.18 shows the step plots of Section 5.2.3.1. Again, the same four distance measures are used. They are based on 5 lags of ACF and PACF, the PIC distance with AR order selected automatically, and the PER. The ACF, PACF, and PER distances selects 2, 2, and 3 clusters, respectively. On the other hand, the PIC distance shows a single cluster. We also apply the GCC similarity measure. Using the program GCCclus, the number of lags selected is 10 and the number of clusters with the Silhouette statistic is 14. The Dendrogram is shown in Figure 5.19 from Alonso and Peña (2019). Four out of the 14 clusters have 192 observations and correspond to all the series of Tuesday, Wednesday, Thursday, and Friday. Monday series are split into a main group of 191 series and an isolated series (Monday, 10 a.m., Maine), that contains several outlier points. The weekends series, of Saturday and Sunday, are also split into two clusters with (i) sleeping hours from 01 to 06th (or 01 to 07th) hours and (ii) the awake hours from 07 to 24th (08 to 24th) hours.

## R commands used for Electricity price data

```
> sc2=GCCclus(z,toPlot=TRUE) #z: denotes the differenced data
> k used for GCC: 10
> Number of clusters by using Silhouette statistic 14
```

| labels | abs.freq | rel.freq |
|--------|----------|----------|
| 1 | 192 | 0.1428571429 |
| 2 | 192 | 0.1428571429 |
| 3 | 56 | 0.0416666667 |
