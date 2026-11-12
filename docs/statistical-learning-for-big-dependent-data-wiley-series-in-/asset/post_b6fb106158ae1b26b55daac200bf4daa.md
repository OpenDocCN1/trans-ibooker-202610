
The CH criterion selects the number of groups by maximizing

CH(g) = \frac{SSB_g / (g - 1)}{SSW_g / (n - g)}.

A useful graphical display is to plot this ratio as a function of the number of groups.

## Example 5.4

We apply k-means to the TaiwanAirBox data. We use the set of 508 time series after removing eight series with six of them associated with outlying locations and two series with only a few non-zero data points. See Example 5.3. The R command kmeans is used, which is in the stats package. Three types of variables of different dimensions are used. The first type consists of the first 5 lags of ACF of each series so that p = 5; the second type is the average value of the series in each of the 31 days in the sample so that p = 31; and the third type uses every observation of the time series as a variable so that we have p = T = 743. The number of clusters is selected using the Calinski and Harabasz criterion (CHC), that is computed below. A maximum of 20 clusters are allowed and, for each number of groups, k-means is applied with 10 random starting values and the value of the criterion is saved in the vector CHC.

### R commands for k-means:

```
##- means with the first five autocorrelation coefficients.
## The outlier-cleaned series of Example 5.3 is used.
> x=as.matrix(TaiwanAirBox032017)[,-1]
> lo=as.matrix(locations032017)[, -1]
> remv = c(1,29,35,46,70,118,155,157) # outlying locations
> x = x[,-remv]
> lo = lo[-remv,]
> z=diff(x)
##the acf of the stationary series z are computed
> h=6
> aa=acf(z,lag.max=h,plot=FALSE)
> H=c(2:6)
> nl=length(H)
> k = dim(z)[2]
##a matrix with columns the acf of each series is formed
> ACM=matrix(0,nl,k)
> for (i in 1:k) {
> ACM[,i] = aa$acf[H,i,i] }
##the k-means is applied
> Kmax=20
> bbb=rep(0,Kmax)
> Wk=as.vector(bbb)
> Bk=Wk
> HC=Bk[-1]
> CHC=HC
> n=dim(ACM)[2]
> for (i in 1:Kmax) {
+ center=i
+ AA=kmeans(t(ACM),centers=i,nstart=10)
+ Wk[i]=AA$tot.withinss
+ Bk[i]=AA$betweenss
> for (j in 1:(Kmax-1)) {
> HC[j] = (Wk[j] - Wk[j+1]) / (Wk[j+1] / (n-j))
> CHC[j] = (Bk[j] / (j)) / (Wk[j] / (n-j+1)) }
> par(mfrow=c(1,1))
> plot(CHC)
# Three groups are identified
> AA=kmeans(t(ACM),centers=3,nstart=10)
> AA
k-means clustering with 3 clusters of sizes 148, 191, 169
Cluster means:
[,1] [,2] [,3] [,4] [,5]
1 0.13979848 -0.02746675 -0.04727573 -0.06181010 -0.061825575
2 0.08036432 -0.12370731 -0.09944578 -0.03522905 -0.004116231
3 -0.03754526 -0.05904016 -0.03590138 -0.02665427 -0.024233302
(output truncated)
```

Figure 5.20 shows the plot of the CHC criterion, from which we see that three clusters seems reasonable. The sizes of the clusters are 148, 191, and 169, respectively. These groups split the series based on the strength of the first five ACFs. Based on the center of each group, which is the average ACFs, we see that Group 3 seems to contain white noise series, that is, an ARIMA(0,1,0) model for the observed series. However, the groups are not related to the locations of Air Boxes, as shown in Figure 5.21.

Next, we construct variables for each series by taking the daily average of PM₂.₅ measurements. Thus, we have 31 variables for each series and each variable is an average of 24 hourly PM₂.₅ measurements. These variables perform better in showing the locations of the Air Boxes than does the ACF. The group sizes are 196, 171, and 141, respectively. Figure 5.22 shows the locations of the three groups.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_269_0.png)

**Figure 5.20** The CHC criterion for the Taiwan AirBox Data. The series is differenced.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_270_0.png)

Figure 5.21 Locations (longitude and latitude) of Air-Boxes for the three groups obtained by k-means with the first 5 lags of ACFs.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_270_1.png)

Figure 5.22 Locations (longitude and latitude) of Air Boxes for the three groups obtained by daily average of PM2.5 measurements.

### R commands for clustering:

```
##A set of variables is built as the average value of series each day
> ke=dim(x)[2]
> Md=matrix(0,31,ke)
> for (i in 1:31){
+ for (j in 1:ke) {
+ sl=1+12*(i-1)
+ s2=sl+11
+ Md[i,j]=sum(x[sl:s2,j])/12
+ }
+ }
##For comparison purpose, we also use 3 groups.
> Data=t(Md)
> AD=kmeans(Data,centers=3,nstart=10)
> AD$size
[1] 196 171 141
> lc1 <- which(AD$cluster==1)
> lc2 <- which(AD$cluster==2)
> lc3 <- which(AD$cluster==3)
> par(mfcol=c(1,3))
> plot(lo[lc1,1],lo[lc1,1],xlab="longitude",ylab="latitude",
main="Group-1")
# Similar commands apply to Groups 2 and 3.
```

Finally, we take every observation as a variable and apply k-means directly to the observed time series. In this case we have more variables than observations, p = T = 743 > k = 508. Again, the CHC criterion selects three groups with sizes 152, 103, and 253. Figure 5.23 shows the locations of the three groups of Air Boxes. It is interesting to see that the locations of Air Boxes in Group 1 and Group 2 are more concentrated, as one would expect. This result is in agreement with Example 5.3, where we showed that taking into account the cross-correlations between the series led to a more meaningful classification in terms of locations of the Air Boxes. The Euclidean distance between two series, by (5.1), would be small if they have a strong positive cross-correlation and would increase if the cross-correlation is weak or negative. Thus, the SSW criterion puts in the same group series with strong positive cross-correlations. This is shown in Figure 5.24, that presents the histograms of the correlation between the time series in each group: two of the groups are formed by series strongly correlated, and the third one incorporates series with low or negative correlation. As we found before, geographical neighbors are expected to have high cross-correlations.

## 5.3.2 k-Medoids

The k-means algorithm may be affected by outliers and several alternatives have been proposed to address the issue. Kaufman and Rousseeuw (1987) proposed a partition algorithm that is related to k-means but with several differences. First, the center of a group is its medoid, instead of the sample mean. The medoid of a group is its member that minimizes the average dissimilarity (or average distance) to all members of the group. For a scalar variable, the medoid of a group is the median of the L1 distance. However, for multivariate variables, the median of a group can be defined in several ways and the medoid is not unique. Second, the way to swap objects among the groups is different than that of the k-means. Third, it can be applied with any dissimilarity or distance measure between points, D(i, j), whereas the k-means is built with the Euclidean distance. Once the measure D(i, j) is selected, the criterion is to minimize

SM = \sum_{j=1}^G \sum_{i \in G_j} D(i, M(j)), \quad (5.28)

where M(j) is the medoid of the jth group to which ith object belongs, G is the number of groups and G_j denotes the jth group. The k-medoids algorithm, or partitioning around medoids (PAM) algorithm, works as follows (see Kaufman and Rousseeuw, 2009, for more details):

- 1. Build: Select G of the k objects (or time series in our case) as leaders (medoids) of the initial groups and associate each object to its closest medoid. This selection is done as follows: choose as the first leader the object that produces the largest decrease in the objective function (5.28). Each object that is not a leader is used to compute the decrease in the objective function and the object with the smallest value of (5.28) is selected as a new leader. Continue in the same way until G leaders are found.

- 2. Swap: check whether swapping the leader and a member of its group reduces (5.28). Check this for all objects and groups.

- 3. If it is possible to reduce the objective function SM in Eq. (5.28) by swapping two objects then do so. When it is not possible to reduce the objective function, stop the process.

For a large number of objects, the algorithm is slow, and some approximate algorithms such as the Clustering Large Applications (CLARA) in R and others have been developed. **CLARA** applies PAM to multiple subsamples and give the best result, others only explore a subset of the possible swaps between objects and leaders by sampling.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_272_0.png)

Figure 5.23 Locations (longitude and latitude) of Air Boxes for the three groups obtained by using directly the hourly PM2.5 measurements.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_272_1.png)

Figure 5.24 Histograms of the cross-correlation between series in each of the groups for the Air Boxes clustered by Euclidean distance between the series.

## 5.3.3 Model-Based Clustering by Variables

For model-based methods we assume that the k time series were generated by G different models and the objective is to classify the series into these G groups such that every member of a group follows the same model. Typically, a model is characterized by a vector of p parameters. They can be the coefficients of the AR representation of a model or by a general vector of features, that usually are assumed to follow a multivariate normal distribution. A natural approach to carry out the subdivision of the series into groups is to assume that the parameter vectors θ_i have been generated by a mixture of G multivariate normal distributions. Then, the objective is to estimate jointly the parameters of the distributions in the mixture and the posterior probabilities that each series belongs to one and only one component of the mixture. This approach, called model-based clustering, was proposed, among others, by Banfield and Raftery (1993). These authors also developed the algorithm MCLUST, based on mixtures of normal distributions for multivariate data.
