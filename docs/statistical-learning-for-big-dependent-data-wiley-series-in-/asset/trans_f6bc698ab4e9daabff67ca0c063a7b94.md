

![](img/d4c84ae3c2178f4c506af3dd2ed13122_332_0.png)

图 6.6 EMU 国家 57 个经济增长率序列第一个公因子的时间图。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_332_1.png)

图 6.7 EMU 国家 57 个经济增长率序列第一个公因子的载荷。

由此可见，载荷在第九个序列上给予了较大权重，该序列为塞浦路斯的投资序列。这很不寻常，我们发现第九个序列与公因子之间的样本相关系数为 0.996。图 6.8 显示了 57 个序列的方差，可见第九个序列的方差比其他序列的方差大 10 倍以上。这一现象证实了我们的担忧，即 PCA 严重依赖所涉及序列的量纲。从某种意义上说，第九个序列可视为离群序列。为缓解量纲效应，我们对增长率序列进行了标准化并重复了分析。标准化后的增长率如图 6.9 所示。现在很明显，所有增长率均受 2008 年金融危机影响。对标准化后的增长率进行分析也选择了一个公因子，如图 6.10 所示。相应的载荷在图 6.11 中给出。载荷表明，公因子是标准化后的增长率的加权平均。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_333_0.png)

图 6.8 EMU 国家 57 个经济增长率序列的方差。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_333_1.png)

图 6.9 欧洲货币联盟 19 国 57 个经济系列标准化增长率的时间图。季度数据，2000 年至 2018 年。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_334_0.png)

图 6.10 EMU 国家标准化增长率第一个公因子的时间图。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_334_1.png)

图 6.11 EMU 国家标准化增长率第一个公因子的载荷。

## 示例 6.3 的 R 命令

```
>library(MTS)
>x=as.matrix(EMUdata)
>y=log(x)
>y1=diffM(y)
>tdx= c(2:76)/4+2000 # calendar time
>MTSplot(y1,tdx)
>sall1=dfmpp(y1, stand = 0, mth = 0, r = 1, lagk = 0)
>plot(tdx,sall1$F[,1],xlab='year',ylab='1st factor',type='l')
>plot(1:57,-sall1$L,xlab='country',ylab='1st factor',type='l')
```

```
>syl=scale(y1)
>sal2=dfmpc(syl, stand = 0, mth = 0, r = 1, lagk = 0)
>MTSplot(syl,tdx)
>plot(tdx, -sal2$F[,1], xlab='year', ylab='1st factor', type='l')
>plot(1:57, -sal2$L,xlab='country', ylab='1st factor', type='l')
```

## 6.2.7 DFM 的替代表述

Li and Sheddon (2002) 提出了一种不同的 FM。他们假设向量时间序列 \( \mathbf{y}_t \) 的每个分量 \( y_{it} \) 由以下公式生成

\( y_{it} = \sum_{j=1}^r \lambda_{ij} Z_{ji}(t), \quad i = 1, \dots, k, \quad (6.23) \)

其中 \( \lambda_{ij} \) 是未知实数，分量 \( Z_{ji}(t), i = 1, \dots, k \) 是从共同概率分布中抽取的独立过程。假设序列已标准化，将等式 (6.23) 写为 \( \mathbf{y}_{it} = \boldsymbol{\lambda}'_i \mathbf{Z}_i(t) \)，其中 \( \boldsymbol{\lambda}'_i = (\lambda_{i1}, \dots, \lambda_{ir}) \) 且 \( \mathbf{Z}_i(t) = (Z_{1i}(t), \dots, Z_{ri}(t))' \)，并记 \( \rho_i(h) = E(y_{it} y_{it+h}) / E(y_{it}^2) \)，则有 \( \rho_i(h) = \boldsymbol{\lambda}'_i \Gamma_{\mathbf{Z}_i}(h) \boldsymbol{\lambda}_i / \boldsymbol{\lambda}'_i \Gamma_{\mathbf{Z}_i}(0) \boldsymbol{\lambda}_i \)，其中 \( \Gamma_{\mathbf{Z}_i}(h) \) 是 \( \mathbf{Z}_i(t) \) 的滞后-\( h \) 自协方差矩阵。由于这些分量已标准化且互不相关，矩阵 \( \Gamma_{\mathbf{Z}_i}(0) \) 是单位矩阵，\( \Gamma_{\mathbf{Z}_i}(h) \) 是对角矩阵。记 \( \phi_{ij}(h) = E(Z_{ij}(t) Z_{ij}(t+h)) \) 为 \( \Gamma_{\mathbf{Z}_i}(h) \) 的第 \( j \) 个对角元素，它表示过程 \( Z_{ij}(t) \) 的滞后-\( h \) 自相关系数，则有

\( \rho_i(h) = \sum_{j=1}^r \lambda_{ij}^2 \phi_{ij}(h) / \sum_{j=1}^r \lambda_{ij}^2 = \sum_{j=1}^r p_{ij}^2 \phi_{ij}(h), \)

其中 \( p_{ij}^2 = \lambda_{ij}^2 / \sum_{j=1}^r \lambda_{ij}^2 \) 是满足 \( \sum_{i=1}^r p_{ij}^2 = 1 \) 的权重。这是式 (6.1) 中 EDFM 的相同表达式，其中独立因子遵循相同的分布。就此而言，正如 Peña (2009) 所示，两种表述是等价的。

Gao and Tsay (2019, 2020) 提出了另一种 DFM 模型，避免了噪声协方差矩阵的奇异性。他们假设，观测到的 \( k \) 维时间序列是 \( r \) 个公因子和 \( k-r \) 个个体特异噪声的非奇异线性变换。因此，他们不使用 DFM 模型 \( \mathbf{z}_t = \mathbf{P} \mathbf{f}_t + \mathbf{n}_t \)（其中 \( \mathbf{n}_t \) 的维度为 \( k \)），而是假设噪声的维度为 \( k-r \)。如 Tiao and Tsay (1989) 所示，利用序列及其滞后值之间的典型相关分析，任何有限阶 VARMA 模型都可写为

\( \mathbf{z}_t = \mathbf{L}_1 \mathbf{f}_t + \mathbf{L}_2 \mathbf{e}_t, \quad (6.24) \)

其中 \( \mathbf{L}_1 \) 是 \( k \times r \)，\( \mathbf{L}_2 \) 是 \( k \times (k-r) \)，\( \mathbf{f}_t \) 是具有动态结构的 \( r \) 维因子，满足 \( \Gamma_f(0) = \mathbf{I}_r \)，\( \mathbf{e}_t \) 是均值为零且协方差矩阵 \( \boldsymbol{\Sigma}_e \) 为正定的 \( (k-r) \) 维白噪声过程。此外，\( \mathbf{f}_t \) 的任何线性组合都不是白噪声过程，且 \( \{ \mathbf{f}_t \} \) 和 \( \{ \mathbf{e}_t \} \) 不相关。不失一般性，可假设 \( \mathbf{L}'_1 \mathbf{L}_1 = \mathbf{I}_r \) 且 \( \mathbf{L}'_2 \mathbf{L}_2 = \mathbf{I}_{k-r} \)，但 \( \mathbf{L}_1 \) 一般不与 \( \mathbf{L}_2 \) 正交。然后，记 \( \mathbf{L}^*_1 \) 为 \( \mathbf{L}_1 \) 的正交补，则有

\( (\mathbf{L}^*_1)' \mathbf{z}_t = (\mathbf{L}^*_1)' \mathbf{L}_2 \mathbf{e}_t, \)

因此，得到 \( k-r \) 个 \( z_t \) 的线性组合，它们均为白噪声。因此，因子个数可通过搜索为白噪声的线性组合个数来确定。

同样地，记 \( \mathbf{L}^*_2 \) 为 \( \mathbf{L}_2 \) 的正交补，并对等式 (6.24) 左乘，则有

\( (\mathbf{L}^*_2)' \mathbf{z}_t = (\mathbf{L}^*_2)' \mathbf{L}_1 \mathbf{f}_t, \)

该式不涉及噪声分量 \( \mathbf{e}_t \)。因此，可以推导出不受噪声项影响的公因子 \( \mathbf{f}_t \) 的估计量。这与之前通过 PCA 提到的公因子估计不同，其中估计量 \( \hat{\mathbf{f}}_t \) 始终受噪声分量影响。Gao and Tsay (2019) 将此类估计程序称为投影 PCA。由于该模型在高维情况下（即 \( k > T \））具有一些优势，我们将在 6.10 节进一步讨论。

## 6.3 平稳序列的广义 DFM (GDFM)

DFM 假设序列与公因子之间存在同期关系。本模型可推广，见 Forni et al. (2000)，假设观测时间序列受因子及其所有过去值的影响。所得模型称为 GDFM。假设滞后阶数有限，其表达式为：

\( z_t = \sum_{l=0}^{L-1} P_l f_{t-l} + n_t, \quad (6.25) \)

其中 \( L \) 是正整数。\( f_t \) 是互不相关的 \( q \) 维平稳公因子向量，\( L \) 个矩阵 \( P_l \) 是 \( k \times q \) 因子载荷矩阵，\( n_t \) 是噪声或个体特异部分，为平稳并满足近似 DFM 的条件。当 \( L > 1 \) 时，在等式 (6.25) 中用 \( q \) 表示因子个数，若 \( L = 1 \) 则恢复记号 \( r \)。此记号变更的原因稍后将变得清晰。序列 \( z_t \) 的公共分量为 \( c_t = \sum_{l=0}^{L-1} P_l f_{t-l} \)，由 \( q < k \) 个未观测因子序列生成。尽管 GDFM 的提出允许 \( L \) 趋于无穷，但本节仅讨论 \( L \) 有限的情况，这也是实践中最重要的情形。载荷矩阵和因子的识别条件与 DFM 类似，即 \( P_l' P_l = I_q \)（\( l = 0, 1, \dots, L-1 \)），且 \( E(f_t f_t') \) 为对角阵。

模型 (6.25) 也可写为

\( z_t = P(B)f_t + n_t, \)

其中 \( P(B) = P_0 + P_1 B + \cdots + P_{L-1} B^{L-1} \) 是滞后算子的矩阵多项式。假设因子遵循对角矩阵的向量自回归（VAR）模型

\( A(B)f_t = a_t, \)

则有

\( z_t = \Psi(B)a_t + n_t, \)

其中 \( \Psi(B) = P(B)A(B) \)。请注意，在有限样本中，\( \Psi(B) \) 分解为 \( A(B) \) 和 \( P(B) \) 存在识别问题，因为二者均未观测到。

