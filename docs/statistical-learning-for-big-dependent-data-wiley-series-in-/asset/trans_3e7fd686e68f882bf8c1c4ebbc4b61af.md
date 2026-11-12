

接下来，由等式 (6.58) 得

$B'_2 z_t = B'_2 A_1 x_t, \quad (6.63)$

它与等式 (6.59) 中定义的 $B'_1 z_t$ 不相关。因此，

$B'_2 \Gamma_z(0) B_1 B'_1 \Gamma_z(0) B_2 = \mathbf{0}, \quad (6.64)$

这意味着 $B_2$ 由以下矩阵的零特征值对应的 $r$ 个特征向量组成

$S = \Gamma_z(0) B_1 B'_1 \Gamma_z(0). \quad (6.65)$

Gao 和 Tsay (2018) 表明矩阵 $B'_2 A_1$ 是可逆的，因此，由等式 (6.63) 得 $x_t = (B'_2 A_1)^{-1} B_2 z_t$。这提供了公共因子 $x_t$ 及因此的 $f_t$ 的估计。由等式 (6.58)，$x_t$ 不涉及白噪声项 $e_t$，因此估计的公共因子 $\hat{f}_t = Q_1^{-1} x_t$ 不会涉及噪声项 $e_t$。这种估计方法因而提高了公共因子 $f_t$ 估计的效率。作者将等式 (6.65) 中 $S$ 的特征值-特征向量分析称为投影 PCA，因为它在执行 PCA 之前将观测序列 $z_t$ 投影到 $B_1$ 的方向上。

转到实际估计。暂且假设公共因子的数量 $r$ 已知。$r$ 的选择将在下一节给出。首先，我们对样本矩阵进行特征值-特征向量分析

$\widehat{M} = \sum_{h=1}^{h_0} \hat{\Gamma}_z(h) \widehat{\Gamma}_z(h)',$

其中 $h_0$ 是一个小的正整数。令 $\hat{A}_1 = [\hat{a}_1, ..., \hat{a}_r]$ 和 $\hat{B}_1 = [\hat{B}_1, ..., \hat{B}_v]$ 为分别由 $\widehat{M}$ 与非零特征值和零特征值相关联的特征向量组成的半正交矩阵。接下来，我们对 $\hat{S} = \hat{\Gamma}_z(0) \hat{B}_1 \hat{B}'_1 \hat{\Gamma}_z(0)$ 进行特征值-特征向量分析，这是一个秩最大为 $k - r$ 的 $k \times k$ 矩阵。令 $\hat{B}_2 = [\hat{B}_{v+1}, ..., \hat{B}_k]$，其中 $\hat{B}_i$（$i = v + 1, ..., k$）是与 $\hat{S}$ 的 $r$ 个最小特征值相关联的特征向量。对于较大的 $k$，Gao 和 Tsay (2018) 提供了一种计算 $\hat{B}_2$ 的修改方法，如果允许噪声协方差矩阵 $\Gamma_e(0)$ 的某些最大特征值在 $k \to \infty$ 时趋于无穷大。有兴趣的读者可参阅该文档以获取详细信息。然后我们可以通过 $\hat{x}_t = (\hat{B}'_2 \hat{A}_1)^{-1} \hat{B}'_2 z_t$ 估计因子过程。最后，白噪声过程可以通过 $\hat{e}_t = \hat{A}'_2 z_t - \hat{A}'_2 \hat{A}_1 \hat{x}_t$ 估计，其中 $\hat{A}_2 = [\hat{B}_1, ..., \hat{B}_v]$ 由与 $\hat{S}$ 的 $v$ 个最大特征值相关联的特征向量组成。

### 6.10.4 选择公共因子的数量

在 $k \ge T$ 的情况下，典型相关分析无法用于选择公共因子的数量 $r$。然而，根据前一节的讨论，$\hat{B}'_1 z_t$ 是一个 $v$ 维白噪声，其中 $v = k - r$。因此，通过检验矩阵 $\widehat{M}$ 主成分中的白噪声序列数量，我们可以估计 $v$。用 $\hat{v}$ 表示估计的白噪声序列数量。那么，$\hat{r} = k - \hat{v}$。关键问题是如何检验高维白噪声时间序列。

近年来，高维白噪声过程检验已得到广泛研究。这里我们讨论两种方法。其他可能的方法包括 Ling et al. (2020) 及其参考文献。我们讨论的两种方法利用时间序列滞后自相关矩阵的最大绝对交叉相关。为了我们的目的，我们想检测 \(\hat{\boldsymbol{u}}_t = [\hat{\mathbf{A}}_1, \hat{\mathbf{B}}_1]'\boldsymbol{z}_t\) 中的白噪声系列数量，其中 \([\hat{\mathbf{A}}_1, \hat{\mathbf{B}}_1]\) 是等式 (6.62) 中矩阵 \(\hat{\boldsymbol{M}}\) 的特征向量矩阵。为了讨论简便，令 \(\boldsymbol{w}_t\) 为 \(\hat{\boldsymbol{u}}_t\) 的一个 \(d\) 维子序列，其中 \(1 \le d \le k\)。我们的目标是检验原假设 \(H_0 : \Gamma_w(1) = \cdots = \Gamma_w(m) = \mathbf{0}\) 与备择假设 \(H_a : \Gamma_w(h) \neq \mathbf{0}\) 对于某些满足 \(1 \le h \le m\) 的 \(h\)，其中 \(m\) 是正整数，并且为了简便，我们假设 \(\hat{\boldsymbol{u}}_t\) 已标准化，因此 \(\Gamma_w(h)\) 是 \(\boldsymbol{w}_t\) 的滞后-\(h\) 自相关矩阵。第一种方法由 Chang et al. (2017) 提出，检验统计量为

\(T_1(m) = \sqrt{T} \times \max_{1 \le h \le m} \max_{1 \le i \le j \le d} |\hat{\gamma}_{w,ij}(h)|, \quad (6.66)\)

其中 \(\hat{\gamma}_{w,ij}(h)\) 是 \(\hat{\boldsymbol{\Gamma}}_w(h)\) 的 \((i,j)\) 第个元素。作者表明，在 \(\boldsymbol{w}_t\) 具有有限四阶矩和原假设下，\(T_1(m)\) 的极限分布可以近似为正随机向量的 \(L_\infty\)-范数，因此，模拟可用于获取检验统计量 \(T_1(m)\) 的渐近临界值。有限经验表明，如果 \(d\) 适中，该检验统计量表现相当不错。对于较大的 \(d\)，该检验统计量需要密集的计算时间。

白噪声检验的第二种方法由 Tsay (2020) 提出。作者不直接使用 \(\boldsymbol{w}_t\)，而是提议使用 \(\boldsymbol{w}_t\) 的秩序列。对于标量时间序列 \(y_t\) 及其实现 \(\{y_1, \dots, y_t\}\)，其秩序列是通过用样本中的秩统计量替换 \(y_t\) 得到的时间序列。令 \(\hat{\boldsymbol{R}}_w(h)\) 为 \(\boldsymbol{w}_t\) 的秩序列的滞后-\(h\) 样本自相关矩阵。定义

\(T_{\max}(h) = \max_{1 \le i \le j \le d} \hat{r}_{w,ij} \text{ and } T_{\min}(h) = - \min_{1 \le i \le j \le d} \hat{r}_{w,ij},\)

其中 \(\hat{r}_{w,ij}\) 是 \(\hat{\boldsymbol{R}}_w(h)\) 的 \((i,j)\) 第个元素。注意，\(T_{\max}(h)\) 是 \(\boldsymbol{w}_t\) 在滞后 \(h\) 时的最大秩交叉相关，而 \(-T_{\min}(h)\) 是 \(\boldsymbol{w}_t\) 在滞后 \(h\) 时的最小秩交叉相关。在 \(\boldsymbol{w}_t\) 是 \(d\) 维白噪声且 \(\boldsymbol{\Gamma}_w(0)\) 为对角矩阵的原假设下，Tsay (2020) 表明 \(\{\hat{r}_{w,ij}(h)|1 \le i, j \le d\}\) 渐近独立并服从 \(N(0, 1)\) 分布。\(T_{\max}(h)\) 和 \(T_{\min}(h)\) 都是 \(d^2 N(0, 1)\) 随机变量的极值，因此它们渐近服从 Gumbel 分布。此外，\(T_{\max}(h)\) 和 \(T_{\min}(h)\) 渐近独立。现在，定义检验统计量

\(T_2(m) = \sqrt{T} \times \max_{1 \le h \le m} \max \{T_{\max}(h), T_{\min}(h)\} . \quad (6.67)\)

由于在我们的应用中 \(\boldsymbol{w}_t\) 的分量不相关，因此 Tsay (2020) 使用的一个假设自动满足。因此，在 \(\boldsymbol{w}_t\) 具有连续分布且原假设成立的前提下，等式 (6.67) 中的 \(T_2(m)\) 渐近遵循独立 Gumbel 分布的乘积，其临界值可得到解析形式。具体而言，设 \(\alpha\) 为 I 类错误，则若

\(T_2(m) \ge c_{d,m} \times \chi_{1-\alpha/2} + s_{d,m},\)

则在 5% 水平上拒绝原假设 \(H_0\)，其中 \(\chi_{1-\alpha/2} = -\ln(-\ln(1-\alpha/2))\) 是标准 Gumbel 分布的 \((1-\alpha/2)\) 分位数，且

$$c_{d,m} = [2 \ln(d^2 m)]^{-1/2} \quad \text{and} \quad s_{d,m} = \sqrt{2 \ln(d^2 m)} - \frac{\ln(4\pi) + \ln(\ln(d^2 m))}{2(2 \ln(d^2 m))^{1/2}}.$$

除了具有解析形式的临界值外，等式 (6.67) 中的检验统计量 \(T_2(m)\) 还有其他一些优点。它不要求 \(\boldsymbol{w}_t\) 存在任何矩（例如柯西分布），并且对 \(\boldsymbol{w}_t\) 中的异常值具有鲁棒性。

### 6.10.5 载荷估计的渐近性质

对于两个满足 \(\boldsymbol{H}_1'\boldsymbol{H}_1 = \boldsymbol{H}_2'\boldsymbol{H}_2 = \boldsymbol{I}_r\) 的 \(k \times r\) 半正交矩阵 \(\boldsymbol{H}_1\) 和 \(\boldsymbol{H}_2\)，\(\boldsymbol{H}_1\) 和 \(\boldsymbol{H}_2\) 所张列空间之间的差异可通过以下方式度量

$$D(\boldsymbol{H}_1, \boldsymbol{H}_2) = \sqrt{1 - \frac{1}{r} \text{tr}(\boldsymbol{H}_1 \boldsymbol{H}_1' \boldsymbol{H}_2 \boldsymbol{H}_2')}.\tag{6.68}$$

这里 \(D(\boldsymbol{H}_1, \boldsymbol{H}_2) \in [0, 1]\)。当且仅当 \(\boldsymbol{H}_1\) 和 \(\boldsymbol{H}_2\) 的两个列空间等价时，其值为零；当且仅当两个列空间互相正交时，其值为 1。我们使用该度量来量化因子模型（FM）估计的准确性。例如，我们使用 \(D(\widehat{\boldsymbol{A}}_1, \boldsymbol{A}_1)\) 来衡量使用 \(\widehat{\boldsymbol{A}}_1\) 估计 \(\boldsymbol{A}_1\) 的一致性。

对于固定的 \(k\)，在某些温和条件下（例如 \(\boldsymbol{f}_t\) 和 \(\boldsymbol{e}_t\) 存在某些有限阶矩，以及 \(\boldsymbol{z}_t\) 和 \(\boldsymbol{f}_t\) 满足某些 \(\alpha\)-混合条件），我们可以推导 FM 估计量的渐近性质。例如，如预期，\(D(\widehat{\boldsymbol{A}}_1, \boldsymbol{A}_1) = O(T^{-1/2})\)。详情可见 Gao 和 Tsay (2019)。对于高维情况，只要 \(k = o(T^{1/2})\)，即可得到类似结果。另一方面，当 \(k\) 和 \(T\) 都趋于无穷大时，估计量的极限性质会变得更加复杂。考虑等式 (6.57) 中的模型。我们只关注 \(\boldsymbol{f}_t\) 和 \(\boldsymbol{e}_t\) 独立的情况。\(\boldsymbol{f}_t\) 与某些 \(h > 0\) 的 \(\boldsymbol{e}_{t-h}\) 相关的情况可见 Gao 和 Tsay (2018)。

令 \(\boldsymbol{L}_1 = [\boldsymbol{c}_1, \dots, \boldsymbol{c}_r]\) 和 \(\boldsymbol{L}_2 = [\boldsymbol{c}_{r+1}, \dots, \boldsymbol{c}_k]\)。随着 \(k\) 增加，我们需要一些度量来量化载荷和噪声的强度。

- 1. 条件 A：\(\boldsymbol{L}_1\) 的列满足 \(j = 1, \dots, r\) 时 \(\|\boldsymbol{c}_j\|_2^2 \asymp k^{1-\delta_1}\)，且 \(\delta_1 \in [0, 1)\)，其中 \(a \asymp b\) 意味着 \(a = O(b)\) 且 \(b = O(a)\)。此外，对于每个列 \(\boldsymbol{c}_j\)，\(\min_{\theta_i \in \mathcal{K}, i \neq j} \|\boldsymbol{c}_j - \sum_{1 \le i \le r, i \neq j} \theta_i \boldsymbol{c}_i\|_2^2 \asymp k^{1-\delta_1}\)。
- 2. 条件 B：\(\boldsymbol{L}_2\) 允许奇异值分解 \(\boldsymbol{L}_2 = \boldsymbol{A}_2 \boldsymbol{D}_2 \boldsymbol{V}_2'\)，其中 \(\boldsymbol{A}_2 \in \mathcal{R}^{k \times v}\) 在等式 (6.58) 中给出，\(\boldsymbol{D}_2 = \text{diag}(d_1, \dots, d_v)\) 且 \(\boldsymbol{V}_2 \in \mathcal{R}^{v \times v}\) 满足 \(\boldsymbol{V}_2' \boldsymbol{V}_2 = \boldsymbol{I}_v\)。此外，存在一个有限整数 \(0 < g < v\)，使得对于某些 \(\delta_2 \in [0, 1)\)，\(d_1 \asymp \cdots \asymp d_g \asymp k^{(1-\delta_2)/2}\) 且 \(d_{g+1} \asymp \cdots \asymp d_v \asymp 1\)。

条件 A 中的 \(\delta_1\) 用于量化因子的强度，\(\boldsymbol{L}_1 \boldsymbol{L}_1'\) 的特征值均为 \(k^{1-\delta_1}\) 阶。如果 \(\delta_1 = 0\)，相应的因子称为强因子，因为它们包含 \(\boldsymbol{c}_j\) 的每个元素为 \(O(1)\) 的情况。如果 \(\delta_1 > 0\)，相应的因子称为弱因子，且 \(\delta_1\) 越小，因子越强。使用指数 \(\delta_1\) 的一个优点是将收敛

