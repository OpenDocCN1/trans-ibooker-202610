

### 轮廓系数最大时为2组
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

### Gap值先取4再取9
# 计算3组的大小
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

### 使用位置数据比较各组
> ### 位置数据已加载并进行分析
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

为了更好地理解台湾AirBox数据的聚类结果，我们仔细检查了Air Boxes的位置。每个Air Box的经度和纬度在文件locations032017.csv中。图5.13的左图显示了位置

![](img/d4c84ae3c2178f4c506af3dd2ed13122_259_0.png)

### 图5.13 台湾AirBox数据中Air Boxes的经度和纬度。左图显示所有516个系列的位置，而右图显示去除六个异常系列后剩余508个系列的位置。

的Air Boxes，由此我们看到位置文件中存在离群位置，因为该岛大约位于东经120–122、北纬22–25之间。因此，在进一步分析中，我们从数据集中删除了八个系列。这些是系列1、29、35、46、70、118、155和157，其中系列29和70的删除之前已解释过。图5.13的右图显示了台湾周围剩余508个PM2.5时间系列的位置。

使用508个差分PM2.5系列，我们应用四种距离度量以及第5.2.3.1节提出的高度图和阶梯图，以识别数据集的聚类数量。四种距离度量分别是基于前5个滞后的ACF和PACF的距离、自动选择阶数的PIC距离以及PER。图5.14显示了高度图的结果。ACF距离的高度图在部分(a)，从中可以清楚地看到三个聚类的迹象。PACF距离的高度图在部分(b)，指示大约五个聚类。PIC距离的高度图在部分(c)，建议两个聚类，PER在部分(d)的结果也建议五个聚类。

图5.15显示了508个差分台湾AirBox系列的阶梯图。同样，分析中使用了相同的四种距离度量。在此特定情况下，PACF和PER距离度量（即部分(b)和(d)）的阶梯图分别建议三个和五个聚类。PIC距离的结果在部分(c)，建议两个聚类。另一方面，ACF距离的阶梯图显示只有一个聚类。然后我们将带有三个聚类的PACF距离度量应用于数据。三个聚类的大小分别为132、346和40。图5.16显示三个聚类成员的位置（经度和纬度）。在此特定情况下，成员位置散布在整个岛上，尽管似乎存在一定的集中。例如，右上方的大部分位置属于聚类1。总体而言，对于一阶差分PM2.5系列，PACF距离并不提供纯粹基于Air Boxes位置的聚类。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_260_0.png)

### 图5.14 508个一阶差分AirBox PM2.5系列的高度图。使用的四种距离度量分别是部分(a)中的前5个滞后的ACF、部分(b)中的PACF、部分(c)中自动选择阶数的PIC，以及部分(d)中的PER。

如第5.1.3节所述，时间序列之间的一种替代差异度量是使用互相关。为演示，我们还将Eq. (5.18)的GCC度量应用于差分AirBox PM2.5数据。使用的命令是`GCCclus`。由轮廓系数统计量选择的聚类数量等于三。三个聚类的成员位置如图5.17所示，其中左上图显示了所有508个系列的位置。从图中可以看出，这三个聚类现在与Air Boxes的地理位置吻合良好。因此，在此特定情况下，基于交叉线性依赖的聚类似乎比基于单个系列PACF的聚类更具信息量。

## 台湾AirBox数据位置的R命令

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

### 图5.17 左上角为所有508个系列的台湾Air Boxes清洗后位置分布，以及基于GCC的序列增量交叉依赖选择的三组分布。

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

| 标签 | 绝对频数 | 相对频数 |
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

## 结果是三个聚类和若干异常系列。
## 用位置绘制三个聚类
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

转向新英格兰地区电力价格的一阶差分系列。图5.18显示第5.2.3.1节的阶梯图。同样，使用了相同的四种距离度量。它们基于5个滞后的ACF和PACF、自动选择AR阶数的PIC距离以及PER。ACF、PACF和PER距离分别选择2、2和3个聚类。另一方面，PIC距离显示单一聚类。我们还应用了GCC相似性度量。使用GCCclus程序，选择的滞后数为10，轮廓系数统计量的聚类数为14。树状图显示于图5.19，来源为Alonso和Peña（2019）。14个聚类中有4个包含192个观测值，分别对应周二、周三、周四和周五的所有系列。周一系列被分为一个包含191个系列的主组和一个孤立系列（周一上午10点，缅因州），后者包含若干离群点。周末系列（周六和周日）也被分为两个聚类，即(i)01至06点（或01至07点）的睡眠小时和(ii)07至24点（08至24点）的清醒小时。

## 用于电力价格数据的R命令

```
> sc2=GCCclus(z,toPlot=TRUE) #z: denotes the differenced data
> k used for GCC: 10
> Number of clusters by using Silhouette statistic 14
```

| 标签 | 绝对频数 | 相对频数 |
|--------|----------|----------|
| 1 | 192 | 0.1428571429 |
| 2 | 192 | 0.1428571429 |
| 3 | 56 | 0.0416666667 |

