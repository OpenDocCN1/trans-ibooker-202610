

CH准则通过最大化下式来选择组数：

CH(g) = \frac{SSB_g / (g - 1)}{SSW_g / (n - g)}.

一个有用的图形显示是将此比率作为组数的函数进行绘制。

## 示例 5.4

我们将k-means应用于TaiwanAirBox数据。在剔除八个时间序列后，我们使用508个时间序列的集合，其中六个与偏远位置相关，另外两个只有少数非零数据点。参见示例5.3。使用的R命令是kmeans，位于stats包中。使用了三种维度不同的变量。第一种类型由每个序列的ACF前5个滞后组成，因此p = 5；第二种类型是样本中31天每一天序列的平均值，因此p = 31；第三种类型将时间序列的每个观测值作为一个变量，因此p = T = 743。使用Calinski和Harabasz准则（CHC）来选择聚类数，如下计算。最多允许20个聚类，对于每个组数，k-means使用10个随机起始值运行，准则值保存在向量CHC中。

### k-means的R命令：

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

图5.20显示了CHC准则的图，从中我们可以看出三个聚类似乎是合理的。聚类的大小分别为148、191和169。这些组根据前五个ACF的强度来划分序列。基于每个组的中心（即平均ACF），我们发现第3组似乎包含白噪声序列，即观测序列的ARIMA(0,1,0)模型。然而，这些组与Air Boxes的位置无关，如图5.21所示。

接下来，我们通过取PM₂.₅测量值的日均值来构建每个序列的变量。因此，每个序列有31个变量，每个变量是24小时PM₂.₅测量值的平均值。这些变量在显示Air Boxes位置方面比ACF表现更好。组的大小分别为196、171和141。图5.22显示了三个组的位置。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_269_0.png)

**图5.20** 台湾AirBox数据的CHC准则。该序列已进行差分。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_270_0.png)

图5.21 使用k-means和ACF前5个滞后得到的三组Air-Boxes的位置（经度和纬度）。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_270_1.png)

图5.22 使用PM2.5测量值的日均值得到的三组Air Boxes的位置（经度和纬度）。

### 聚类的R命令：

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

最后，我们将每个观测值作为一个变量，并直接对观测时间序列应用k-means。在这种情况下，变量数多于观测数，p = T = 743 > k = 508。同样，CHC准则选择三组，大小分别为152、103和253。图5.23显示了三组Air Boxes的位置。有趣的是，如预期，第1组和第2组中Air Boxes的位置更加集中。此结果与示例5.3一致，在示例5.3中，我们表明考虑序列之间的互相关关系能够根据Air Boxes的位置得到更有意义的分类。根据公式(5.1)，如果两个序列具有强正互相关，则它们之间的欧氏距离较小；如果互相关较弱或为负，则距离会增加。因此，SSW准则将具有强正互相关的序列归为同一组。图5.24显示了这一点，它展示了每组内时间序列之间相关性的直方图：两组由强相关序列组成，第三组包含低相关或负相关的序列。正如我们之前发现的，地理上的邻居预期具有较高的互相关性。

## 5.3.2 k-Medoids

k-means算法可能受异常值影响，因此提出了多种替代方法来解决这个问题。Kaufman和Rousseeuw（1987）提出了一种与k-means相关但存在几个差异的分区算法。首先，组的中心是其medoid，而不是样本均值。组的medoid是使其与组内所有成员的平均离差（或平均距离）最小化的成员。对于标量变量，组的medoid是L1距离的中位数。然而，对于多元变量，组的中位数可以有多种定义，且medoid不唯一。其次，组间对象交换的方式与k-means不同。第三，它可以应用于任意点间的离差或距离度量D(i, j)，而k-means是基于欧氏距离构建的。一旦选择了度量D(i, j)，准则就是最小化

SM = \sum_{j=1}^G \sum_{i \in G_j} D(i, M(j)), \quad (5.28)

其中M(j)是第i个对象所属的第j组的medoid，G是组的数量，G_j表示第j组。k-medoids算法，或称为围绕medoid分区（PAM）算法，工作方式如下（更多细节请参见Kaufman和Rousseeuw，2009）：

- 1. Build：从k个对象（在我们的情况中为时间序列）中选择G个作为初始组的领导者（medoids），并将每个对象与其最近的medoid关联。选择方法如下：首先选择使目标函数（5.28）下降最大的对象作为第一个领导者。对于每个非领导者对象，计算其导致的目标函数下降量，并选择（5.28）值最小的对象作为新的领导者。继续同样的方式，直到找到G个领导者。

- 2. Swap：检查将领导者与其组内某个成员交换是否会减小（5.28）。对所有对象和组进行此检查。

- 3. 如果通过交换两个对象可以减小Eq. (5.28)中的目标函数SM，则执行交换。当无法再减小目标函数时，停止过程。

对于大量对象，该算法较慢，因此开发了一些近似算法，例如R中的Clustering Large Applications（CLARA）等。**CLARA**在多个子样本上应用PAM并给出最佳结果，而其他算法则通过抽样仅探索对象与领导者之间可能交换的一个子集。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_272_0.png)

图5.23 直接使用每小时PM2.5测量值得到的三组Air Boxes的位置（经度和纬度）。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_272_1.png)

图5.24 按序列间欧氏距离聚类的Air Boxes，各组内序列之间互相关的直方图。

## 5.3.3 基于变量的模型聚类

对于基于模型的方法，我们假设k个时间序列由G个不同的模型生成，目标是将序列分类为这G个组，使得每个组的所有成员都遵循同一模型。通常，模型由一个p参数向量表征。它可以是模型的AR表示系数，也可以是通常假设服从多元正态分布的一般特征向量。将序列划分为组的自然方法是假设参数向量θ_i由G个多元正态分布的混合生成。然后，目标是共同估计混合中各分布的参数以及每个序列属于混合中且仅属于一个分量的后验概率。这种方法称为基于模型的聚类，由Banfield和Raftery（1993）等人提出。这些作者还基于多元数据的正态分布混合开发了算法MCLUST。

