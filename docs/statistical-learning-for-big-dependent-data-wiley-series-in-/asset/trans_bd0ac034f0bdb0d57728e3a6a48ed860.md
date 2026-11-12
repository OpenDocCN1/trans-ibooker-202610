

![](img/d4c84ae3c2178f4c506af3dd2ed13122_348_0.png)

![](img/d4c84ae3c2178f4c506af3dd2ed13122_348_1.png)

| | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|
| λ₁ | 0.298 | 0.282 | 0.266 | 0.250 | 0.234 | 0.218 |
| λ₂ | 0.270 | 0.261 | 0.250 | 0.235 | 0.217 | 0.196 |
| λ₃ | 0.070 | 0.067 | 0.065 | 0.062 | 0.061 | 0.060 |
| λ₄ | 0.039 | 0.038 | 0.036 | 0.032 | 0.029 | 0.024 |
| λ₅ | 0.008 | 0.007 | 0.006 | 0.004 | 0.003 | 0.003 |

获得滞后相关矩阵特征值和特征向量的 R 命令如下所示，其中对于 i = 0, ..., 5，矩阵 Si 包含滞后 i 协方差矩阵，Ei 是矩阵 Si 的五个最大特征值，EVfi 是与矩阵 Si 第一个特征值相关联的特征向量，EVsi 是对应于第二个特征向量的特征向量。另外，两个特征值的载荷对于不同滞后是稳定的。因子的估计载荷如图 6.18 所示。第一个因子是所有国家的加权平均值，尽管希腊 (8)、意大利 (10) 和葡萄牙 (16) 的权重比其他国家小。第二个因子载荷根据正负权重区分国家：负载荷最大的国家是希腊 (8)、意大利 (10) 和马耳他 (14)。

# 示例 6.5 使用的 R 命令：

```
> x=as.matrix(UMBdata)
> n=nrow(x)
> G=matrix(0,n,19)
> for (i in 1:19) {G[,i]=x[,1+3*(i-1)]}
> G0=log(G)
> G1=scale(G0)
> ##the data in logs and scale is in G1
> yl <- range(G1)
> tdx <- c(1:76)/4+2000
> plot(tdx,G1[,1],xlab="year",ylab="logGDP", main="GDP Euro countries",
```

![](img/d4c84ae3c2178f4c506af3dd2ed13122_349_0.png)

![](img/d4c84ae3c2178f4c506af3dd2ed13122_349_1.png)

图 6.18 货币同盟 19 个国家 logGDP 系列的两个因子载荷。

```
type="l", ylim=y1)
> for (i in 2:19) {lines (tdx,G1[,i],col=i)}
> out=acf (G1,lag.max=5,type="covariance",plot=FALSE)
>
> S0=eigen(out$acf[1,,])
> B0=S0$values[1:5]
> EVf0=S0$vectors[,1]
> EVs0=S0$vectors[,2]
## Same commands to compute S1-S5 to EVS1-EVS5
> EVf=cbind(EVf0,EVf1,EVf2,EVf3,EVf4,EVf5)
> EVs=cbind(EVs0, EVs1,EVs2, EVs3, EVs4, EVs5)
> out=dfmpc(G1)
> out$r
[1] 2
```

```
> y1 <- range(out$F)
> tdx <- c(1:76) /4+2000
> plot (tdx,-out$F[,1], xlab="year",ylab="Factors", main="1stFactor (red)
and 2ndFactor (blue) GDP Euro countries", type="l",ylim=y1,col="red")
> lines (tdx,-out$F[,2], type="b", col="blue")
> par (mfrow=c(2,1))
> plot(out$L[1,], xlab="Countries",ylab="Loadings",
main="Loadings for the 1stFactor (red) GDP Euro countries",
type="b",col="red")
> plot(out$L[,2], xlab="Countries",ylab="Loadings",
main="Loadings for 2ndFactor (blue) GDP Euro countries",
type="o",col="blue")
```

### 6.5.1 协整与 DFM

正如第 3 章所讨论的，非平稳时间序列向量可以协整，该系列的一些线性组合是平稳的；参见 Banerjee 等人 (1993) 和 Johansen (1995)。具有某些整合因子和平稳噪声的 DFM 意味着系列 \(z_t\) 的协整。令 \(P_1^\perp\) 为 \(P_1\) 的 \(k \times (k - r_1)\) 正交补矩阵，使得 \(P'_1 P_1^\perp = 0_{(r_1 \times k - r_1)}\)，则有

\(P_1^{\perp'} z_t = P_1^{\perp'} P z_{2t} + P_1^{\perp'} n_t.\)

由于前一等式右边由平稳系列的线性组合构成，\(P_1^{\perp'} z_t\) 中的 \(k - r_1\) 个系列是平稳的。事实上，这些系列代表了 \(z_t\) 的协整系列。进一步分析见 Scribano and Peña (1994)。因此，DFM 提供了一种寻找协整关系的直接方法。关于通过 DFM 估计协整秩的一致性建议，参见 Zhang et al. (2018)。当有许多时间序列（即 \(k\) 很大）时，整合因子的数量 \(r_1\) 预计相对较小，因此协整关系的数量 \(k - r_1\) 将很大。有观点认为（参见 Barigozzi et al. (2016, 2017)），具有平稳噪声的 DFM 隐含的大量协整关系与实证证据不一致，这意味着在实际应用中噪声可能遵循整合过程。这些作者还考虑了因子具有降秩表示或因子之间存在协整的可能性，并提出了一种在某些噪声协整时估计它们的程序。噪声可能非平稳这一事实表明，FM 中噪声的协方差矩阵可能有一些主导特征值，这可能导致通过 PCA 或检验特征值比率来识别共同因子数量时出现困难。该问题已在 Gao and Tsay (2018) 中得到解决。

## 6.6 非平稳系列的 GDFM

关于非平稳数据的 GDFM 结果很少，大多数应用都是通过对系列进行差分来构建平稳 GDFM。基于频域中 DPC 的估计程序只能应用于平稳系列，但存在一种时域方法，即广义动态主成分（GDPC），由 Peña and Yohai (2016) 提出，该方法不需要平稳性假设，可以应用于非平稳时间序列。这些广义 DPC 是用于重构数据的最优函数，不限制为观测值的线性组合。请注意，Brillinger 的 DPC 和第 6.5 节中介绍的 ODPC 都是观测系列的线性组合。去除这一限制后，GDPC 可以是观测值的非线性函数，因此可以更好地适应系列的非平稳行为。Smucer (2019) 证明，这些 GDPC 是 GDFM 共同部分的估计的一致估计量。

### 6.6.1 广义 DPC 估计

GDPC 可以按如下计算。有关更详细的解释，参见 Peña and Yohai (2016)。

假设我们观察到满足 $\sum_{t=1}^T z_{j,t} = 0$ 的系列 $z_{j,t}$。考虑两个正整数 $c_1$ 和 $c_2$。我们定义具有 $c_1$ 个滞后和 $c_2$ 个超前的第一个 GDPC 为向量 $\boldsymbol{f} = (f_t)_{-c_1+1 \le t \le T+c_2}$，使得将系列 $z_{j,t}$ ($1 \le j \le k$) 重构为 $\{f_j t - c_1 \le j \le t + c_2\}$ 元素的线性组合，关于 MSE 准则是最优的。更准确地说，原始系列 $z_{j,t}$ 的重构定义为

$\widehat{z}_{j,t} = \sum_{i=-c_1}^{c_2} \beta_{j,i} f_{t+i}.$

我们可以通过令 $L = c_1 + c_2$ 并定义新分量 $f_t^* = f_{t+c_2}$，仅使用分量的滞后来写出此表达式，这样值 $(f_{t+c_2}, \dots, f_{t-c_1})$ 就转换为 $(f_t^*, \dots, f_{t-L}^*)$，并将 $\beta_{j,i}^*$ 称为对应于 $f_{t-i}^*$ 的新系数，它们与之前的系数的关系为 $\beta_{j,i}^* = \beta_{j,c_2-i}$。然后，重构系列也可以表示为

$\widehat{z}_{j,t} = \sum_{i=-c_1}^{c_2} \beta_{j,i} f_{t+i} = \sum_{h=0}^L \beta_{j,h}^* f_{t-h}^* .$

因此，不失一般性，我们可以使用 PC 的 $L$ 个滞后来重构系列。定义 $(L+1) \times 1$ 向量 $\boldsymbol{f}_t = (f_t, \dots, f_{t-L})'$ 和具有行 $\boldsymbol{b}_j' = (\beta_{j,0}, \dots, \beta_{j,L})$ 的 $k \times (L+1)$ 矩阵 $\boldsymbol{B}$，我们希望最小化

$\text{MSE}(\boldsymbol{f}, \boldsymbol{B}) = \frac{1}{Tk} \sum_{j=1}^k \sum_{t=1}^T \left( z_{j,t} - \sum_{i=0}^L \beta_{j,i} f_{t-i} \right)^2 = \frac{1}{Tk} \sum_{t=1}^T (\boldsymbol{z}_t - \boldsymbol{B f}_t)' (\boldsymbol{z}_t - \boldsymbol{B f}_t) . \quad (6.40)$

请注意，即使在非平稳向量时间序列的情况下，该损失函数也定义良好。$T + L$ 维向量 $\boldsymbol{f} = (f_{1-L}, \dots, f_T)'$ 和 $k \times (L + 1)$ 矩阵 $\boldsymbol{B}$ 的最优选择由以下给出

$(\widehat{\boldsymbol{f}}, \widehat{\boldsymbol{B}}) = \arg \min_{\boldsymbol{f} \in \mathbb{R}^{T+L}, \boldsymbol{B} \in \mathbb{R}^{k \times (L+1)}} \text{MSE}(\boldsymbol{f}, \boldsymbol{B}). \quad (6.41)$

显然，如果 $\boldsymbol{f}$ 是最优的，那么 $\gamma \boldsymbol{f} + \boldsymbol{\delta}$ 也是最优的。因此，我们可以选择 $\boldsymbol{f}$ 使得 $\sum_{t=1-L}^T f_t = 0$ 且 $(1/(T + L)) \sum_{t=1-L}^T f_t^2 = 1$。我们称 $\widehat{\boldsymbol{f}}$ 为观测系列 $\boldsymbol{z}_1, \dots, \boldsymbol{z}_T$ 的阶数 $L$ 的第一个（可能非平稳）GDPC。请注意，阶数为 0 的第一个 GDPC 对应于数据的第一个常规 PC。由于

$\text{MSE}(\boldsymbol{f}, \boldsymbol{B}) = \frac{1}{Tk} \left[ \sum_{t=1}^T \boldsymbol{z}_t' \boldsymbol{z}_t - 2 \sum_{t=1}^T \boldsymbol{z}_t' \boldsymbol{B} \boldsymbol{f}_t + \sum_{t=1}^T \boldsymbol{f}_t' \boldsymbol{B}' \boldsymbol{B} \boldsymbol{f}_t \right].$

然后

$\frac{\partial \text{MSE}(\boldsymbol{f}, \boldsymbol{B})}{\partial \boldsymbol{B}} = 0 = - \sum_{t=1}^T \boldsymbol{f} \boldsymbol{z}_t' + \sum_{t=1}^T \boldsymbol{f} \boldsymbol{f}_t' \boldsymbol{B}'. \quad (6.42)$

定义 $\boldsymbol{F}' = [\boldsymbol{f}_1, \dots, \boldsymbol{f}_T]$ 为包含重构数据 $\boldsymbol{Z}$ 所需的分量所有滞后值的 $(L + 1) \times T$ 矩阵。等式 (6.42) 意味着

$\widehat{\boldsymbol{B}}' = (\boldsymbol{F}' \boldsymbol{F}')^{-1} \boldsymbol{F}' \boldsymbol{Z}. \quad (6.43)$

等式 (6.40) 对 $f_t$ 的偏导数考虑到所有项 $z_{1,t}, \dots, z_{j,t+L}$ 都使用 $f_t$ 进行重构，因此方程中会出现 $\beta_{j,i}$ 系数的平方和交叉积。可以证明（参见 Peña and Yohai, 2016），$\widehat{\boldsymbol{f}}$ 的估计可以写成

$\widehat{\boldsymbol{f}} = \boldsymbol{D}(\boldsymbol{B})^{-1} \sum_{j=0}^L \boldsymbol{C}_j(\boldsymbol{Z}) \boldsymbol{\beta}_j, \quad (6.44)$

其中，平方 $(T + L)$ 矩阵 $\boldsymbol{D}(\boldsymbol{B})$ 包含矩阵 $\boldsymbol{B}$ 元素的交叉积，矩阵 $\boldsymbol{C}_j(\boldsymbol{Z})$ 包含 $\boldsymbol{Z}$ 的元素，维度为 $(T + L) \times (L + 1)$。

为了定义计算 $(\widehat{\boldsymbol{f}}, \widehat{\boldsymbol{B}})$ 的迭代算法，只需给出一个初始值 $\boldsymbol{f}^{(0)}$ 和一个规则，描述在已知 $\boldsymbol{f}^{(h)}$ 时如何计算 $\boldsymbol{B}^{(h)}$ 和 $\boldsymbol{f}^{(h+1)}$。对于等式 (6.43)，给定 $\boldsymbol{f}^{(h)}$，系数 $\boldsymbol{B}^{(h)}$ 可以通过线性回归方法获得

$\boldsymbol{B}^{(h)} = [\boldsymbol{F}(\boldsymbol{f}^{(h)})' \boldsymbol{F}(\boldsymbol{f}^{(h)})]^{-1} \boldsymbol{F}(\boldsymbol{f}^{(h)})' \boldsymbol{Z},$

并且根据等式 (6.44)，$\boldsymbol{f}^{*(h+1)}$ 按以下方式计算

$\boldsymbol{f}^{*(h+1)} = \boldsymbol{D}(\boldsymbol{B}^{(h)})^{-1} \sum_{j=0}^L \boldsymbol{C}_j(\boldsymbol{Z}) \boldsymbol{\beta}_j^{(h)},$

并且将分量向量标准化为均值为零、方差为一。

