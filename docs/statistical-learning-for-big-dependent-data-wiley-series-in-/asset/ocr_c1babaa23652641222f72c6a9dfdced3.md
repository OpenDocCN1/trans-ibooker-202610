![](bbox=[0.22, 0.09, 0.77, 0.27])

Figure 4.6 AirBox data set: outlier detection via Lasso. The upper plot is the number of level shifts and the lower plot is the number of AOs.

We apply the proposed level shift procedure first to the first two series, as in Example 4.3, and then to all the series. The AR order is p = 2 for the first two series in order to use a similar model to compare the results. The number of outliers detected remains at 5. In the first series all the outliers are identified as LS whereas in Example 4.3 two are LS, two TC and an AO. Since TC has not been considered here, the two TC are identified as LS. In the second series, only three AOs are found.

To detect outliers for all time series, we set p = 5 to cover different dynamic behaviors of the data. If a series contains detected level shifts, we remove the impact of level shifts based on the proposed procedure. We then use the level-shift-adjusted series to detect AOs. The results are summarized in Figure 4.6. The upper plot shows the number of level shifts of each series, whereas the lower plot shows the number of additive outliers. For the 514 time series considered in our study, there are 5014 detected level shifts and 1524 AOs. The percentages for level shifts and AOs are 1.31% and 0.40%, respectively. From the plot, the maximum number of level shifts detected occurs in Series 495 with 62 level shifts. This phenomenon might be affected by the fact that some aberrant observations are classified as level shifts as we focus on level shifts and AOs only. Overall, the percentage of level shifts or AOs is about 1.71%, which seems reasonable. Finally, the R commands used are given below. The command outlierLasso is available from the package SLBDD associated with the book.

# Lasso approach to outlier detection:

```
>air <- read.csv("TaiwanAirBox032017.csv")
>air <- as.matrix(air)
>air <- air[,-1]
>air <- air[, -c(29,70)] # remove series 29 and 70.
>set.seed(11)
>m1 <- outlierLasso(air[,1:2],p=2,crit=3.5)
>m1$nLS
[1] 5 0
>m1$nAO
[1] 0 3
```