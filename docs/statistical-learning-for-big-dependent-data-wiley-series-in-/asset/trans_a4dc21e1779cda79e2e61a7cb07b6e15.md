

```R
> max(Macf)
[1] 0.9031959
> arrayInd(which.max(Mpacf),dim(Mpacf))
     [,1] [,2]
[1,] 378 29
> hist(Mpic[Mpic>0])
> max(Mpic)
[1] 1.440145
> arrayInd(which.max(Mpic),dim(Mpic))
     [,1] [,2]
[1,] 232 29
> hist(MPer[MPer>0])
> max(MPer)
[1] 0.3543029
> arrayInd(which.max(MPer),dim(MPer))
     [,1] [,2]
[1,] 140 70
```

```R
> # The average distances of each method are computed
> md1=apply(Macf,1,mean)
> md2=apply(Mpacf,1,mean)
> md3=apply(Mpic,1,mean)
> md4=apply(MPer,1,mean)
> plot(md1, main="Average acf distances")
> plot(md2, main="Average pacf distances")
> plot(md3, main="Average pic distances")
> plot(md4, main="Average per distances")
> # identification of extreme series
> which(md1>.5)
[1] 29
> which(md2>.5)
[1] 29
> which(md3>.8)
[1] 29
> which(md4>.12)
[1] 29 70
```

图 5.2 显示了四个距离的直方图。ACF、PACF 和 PIC 距离给出了相似的结果，大多数距离遵循似乎向右偏斜（长距离）的非对称分布，表明存在多个组。另一方面，PER 的直方图显示距离集中，只有少量大距离，表明存在一个大组和一个小组。ACF、PACF 和 PIC 产生的最大距离出现在序列 29。请注意，序列 29 和 70 在第 2 章中被发现为离群时间序列，因为它们的大多数值为零。我们还计算了

![](img/d4c84ae3c2178f4c506af3dd2ed13122_241_0.png)

图 5.2 台湾 AirBox 数据四个距离的直方图。自相关系数（Macf）和偏自相关系数（Mpacf）、Piccolo 距离（Mpic）及周期图距离（MPer）。

每个时间序列与其他所有序列的平均距离，其值如图 5.3 所示。在所有四种方法中，最大值均出现在序列 29。序列 70 的距离也较大，但程度稍小。但是，PER 是唯一能清楚地将这两个离群序列与其他序列分开的指标。

我们还使用 h = 5 的公式 (5.12) 计算了相关系数行列式之间的不相异性。请参见图 5.4 和 5.5。结论与通过自相关向量之间的距离所得结论相似。使用的 R 命令如下：

# 不相异性的 R 命令：

```R
## Continue with Example 5.1.
# We compute the autocorrelation matrices for each series
# to compute the difference between the determinants
>nR=h+1
>R=array(0,dim=c(k,nR,nR))
>for (i in 1:k){
 > for (j in 1:nR){
  > for (s in 1:nR){
   > d=abs(j-s)
```

![](img/d4c84ae3c2178f4c506af3dd2ed13122_242_0.png)

**图 5.3** 台湾 AirBox 数据的平均距离。

```R
> if(d==0) R[i,j,s]=1 else R[i,j,s] <-ACM[d,i]       }       }
>DetR=matrix(0,k,1)
>for (i in 1:k){
>     DetR[i]=det( R[i,] ))
>Mdis=matrix(0,k,k)
>for (i in 1:k){
>   for (j in 1:k){
>    Mdis[i,j]=abs (DetR[i] ^ (1/nR) -DetR[j] ^ (1/nR)) }}
>Md=Mdis [Mdis>0]
>hist(Md)
>max(Md)
>arrayInd (which.max(Mdis) ,dim(Mdis))
>mD=apply (Mdis,1,mean)
>plot (mD, main="Average distances determinant")
```

作为第二个示例，我们使用文件 Pelectricity1344.csv 中的电价数据。数据涵盖美国新英格兰（NE）八个地区在 678 周内每天每小时的每周电价系列，共有 1344 个系列。请注意，此处 k > T。系列对应于一周中的每一天、每天中的每个小时以及 NE 中的八个地区之一。即，7 × 24 × 8 = 1344。使用相同的 R 命令，得到图 5.6 和 5.7。两组数据的图表相似，但其尺度表明电价似乎比 PM2.5 测量值更具同质性。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_243_0.png)

图 5.4 台湾 AirBox 数据相关系数行列式之间距离的直方图。

## 相关系数行列式的平均距离

![](img/d4c84ae3c2178f4c506af3dd2ed13122_243_1.png)

图 5.5 台湾 AirBox 数据相关系数行列式之间的平均距离。

## 5.1.3 基于交叉线性依赖的不相异性

前面的措施比较了一些刻画单个时间序列序列依赖的特征，但它们不考虑任何两个系列之间的依赖。我们已经看到，两个时间序列值之间的欧氏距离仅取决于滞后为零的自相关。比较两个系列线性依赖的替代方法是使用不相异性 D(xr, yi)，它们是

![](img/d4c84ae3c2178f4c506af3dd2ed13122_244_0.png)

![](img/d4c84ae3c2178f4c506af3dd2ed13122_244_1.png)

![](img/d4c84ae3c2178f4c506af3dd2ed13122_244_2.png)

![](img/d4c84ae3c2178f4c506af3dd2ed13122_244_3.png)

图 5.6 电价数据单变量系列之间四个距离措施的直方图。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_244_4.png)

![](img/d4c84ae3c2178f4c506af3dd2ed13122_244_5.png)

图 5.7 电价数据行列式之间的直方图和距离。

为正且对称。此外，我们希望 0 ≤ D(xt, yt) ≤ 1，当两个系列相同时值为 0，且当且仅当两个时间序列之间所有交叉相关系数均为零时值为 1。因此，D(xt, yt) 的极端值意味着完全线性依赖或完全缺乏线性依赖。最后，我们通常关心两个系列之间依赖的强度而非符号。这些考虑导致了以下措施。

考虑交叉相关向量

$\rho_{xy} = [\rho_{xy}(-h), \dots, \rho_{xy}(-1), \rho_{xy}(0), \rho_{xy}(1), \dots, \rho_{xy}(h)]'$,
(5.13)

其中 \(h\) 是正整数且 \(\rho_{xy}(-h) = \rho_{yx}(h)\)。作为两个系列之间线性依赖的措施，我们可以使用欧几里得范数

\(\|\rho_{xy}\| = \sqrt{\rho'_{xy}\rho_{xy}} = \sqrt{\sum_{j=-h}^{h} \rho_{xy}^2(j)}\)。

但是 \(\|\rho_{xy}\|\) 不是 \(x_t\) 和 \(y_t\) 之间的距离，因为它不满足三角不等式 \(\|\rho_{xy}\| \le \|\rho_{xz}\| + \|\rho_{yz}\|\)。例如，假设 \(x_t\) 和 \(y_t\) 相关，使得 \(\|\rho_{xy}\| > 0\)，但 \(z_t\) 与这两个系列都不相关，使得 \(\|\rho_{xz}\| = \|\rho_{yz}\| = 0\)。另一方面，如果 \(x_t\) 和 \(y_t\) 独立，则 \(\|\rho_{xy}\| = 0\)。另外，如果 \(x_t = ay_t + b\)，其中 \(a\) 和 \(b\) 是实数且 \(a \neq 0\)，则 \(\|\rho_{xy}\| = \|\rho_{xx}\| = \|\rho_{yy}\|\)，其中根据 (5.13)

\(\rho_{yy} = [\rho_y(-h), \dots, \rho_y(-1), 1, \rho_y(1), \dots, \rho_y(h)]'\)。

因此，我们可以基于线性依赖构建两个系列之间的不相异性

\(D_{CC2}(x_t, y_t) = 1 - \frac{\|\rho_{xy}\|^2}{\|\rho_{xx}\|\|\rho_{yy}\|}.\)
(5.14)

显然，如果两个系列独立，则 \(D_{CC2}(x_t, y_t) = 1\)，如果两个系列相同，则 \(D_{CC2}(x_t, y_t) = 0\)。然而，用交叉相关系数向量的范数来总结依赖关系的一个缺点是，该措施对滞后的排列具有不变性。因此，具有相同全局依赖关系的两个系列可能在滞后之间具有非常不同的系数分布。另外，时间上移位的两个相同时间序列可能表现为不同。例如，\(y_t = x_{t-1}\)。

总结全局线性依赖的更好方法是使用矩阵 \(R_{yx,h}\)，它对应于由平稳向量过程 \(Z_t = (y_t, y_{t-1}, \dots, y_{t-h}, x_t, x_{t-1}, \dots, x_{t-h})'\) 给出的相关系数矩阵，即

\(R_{yx,h} = \begin{pmatrix} R_{y,h} & C'_{xy,h} \\ C_{xy,h} & R_{x,h} \end{pmatrix},\)
(5.15)

其中 \(R_{x,h}\) 是前面定义的 \(x_t\) 的 \((h+1) \times (h+1)\) 半正定相关系数矩阵，\(R_{y,h}\) 对应于 \(Y_{t,h} = (y_t, y_{t-1}, \dots, y_{t-h})'\) 的相关系数矩阵，而 \(C_{xy,h}\) 包含两个向量系列之间的交叉相关。请注意，\(|R_{yx,h}| = |R_{xy,h}|\)，且行列式验证了（1）\(0 \le |R_{yx,h}| \le 1\)，当 \(R_{yx,h}\) 为对角矩阵且两个系列都序列不相关且线性无关时等号成立；（2）当存在非平凡线性组合 \(a'Z_t = 0\) 使得系列恰好线性相关时，\(|R_{yx,h}| = 0\)。一种可能的相似性措施是两个时间序列之间的 *总相关*，如 (5.11) 所示，给出为

\(TC_h = 1 - |R_{yx,h}|^{1/[2(h+1)]},\)
(5.16)

