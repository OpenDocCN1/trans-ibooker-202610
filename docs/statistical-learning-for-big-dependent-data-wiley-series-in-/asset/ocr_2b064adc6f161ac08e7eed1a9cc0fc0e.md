```
> v <- apply(rtn,1,var)
> plot(ave,v,xlab="mean",ylab="variance")
> text(locator(1),"10/15/2008")
> text(locator(1),"01/03/2001")
> text(locator(1),"01/01/2001")
> text(locator(1),"10/06/2008")
```

# 2.1.2 Static Plots

Static plots are graphical tools to summarize the behavior over the sample period of each of the k time series under study. In these plots, we examine some properties (or summaries) of the marginal distribution of each column and plot those summaries together for all series. We refer to such plots as static plots, because they show a summary of each time series over the observed period and thus the time dynamics is not revealed. The first plot we propose here is to make traditional boxplots of selected quantiles of the distribution of each column (series), including the minimum and maximum of each series. This plot is referred to as a quantile-box plot.

Figure 2.6 shows the quantile-box plot of the AirBox measurements of PM2.5. The selected quantiles are minimum, 25, 50, 75% quantiles, and the maximum of each series. From the plot, it is clear that two series of the PM2.5 measurements have median values close to zero. As a matter of fact, there exist two low points at 25, 50, and 75% quantiles. Since PM2.5 measurements cannot assume any negative value, this plot immediately identifies two potentially outlying series in the data set. Figure 2.7 shows the time plots of three series, which have the smallest sample variances. The series are 29th, 70th, and 348th. From the time plots, it is easily seen that Series 29 and 70 assume lots of zero measurements, indicating that these two series have more than 50% missing values and, hence, should be discarded from further modeling.

Two important summaries of each observed time series are its central location and variability, and we recommend a scatterplot of the standard deviation (or variance) versus mean of each series. This plot is capable of identifying series that have higher variability or unusual sample means. In other words, the plot can spot certain heterogeneity in the data set. Figure 2.8 shows the scatterplot of sample variances versus sample means of the 99 daily log returns of world financial market indexes. From the

![](bbox=[0.27, 0.66, 0.76, 0.84])

Figure 2.6 Quantile-box plot for the AirBox measurements of PM2.5 data set. The quantiles selected are the minimum, 25, 50, 75% quantiles, and the maximum.