

因此，\(\boldsymbol{w}_t\) 遵循平稳 VARMA(1,2) 模型。然而，由式 (3.67) 可知，若将 \(\boldsymbol{w}_t\) 的 MA 矩阵多项式记为 \(\theta^*(B)\)，则可见

\(|\theta^*(1)| = \begin{vmatrix} 1 - 0.5 - 0.005 & -0.5 + 0.245 \\ 0.495 & 1 - 1 - 0.255 \end{vmatrix} = \begin{vmatrix} 0.495 & -0.255 \\ 0.495 & -0.255 \end{vmatrix} = 0.\)

这意味着 1 是行列式方程 \(|\theta^*(B)| = 0\) 的一个解。因此，\(\boldsymbol{w}_t\) 的 VARMA(1,2) 模型是不可逆的。

对协整向量过程的各个分量进行差分，若其协整秩小于维度，会导致不可逆的 VARMA 模型，在时间序列文献中这被称为 *过度差分*。*过度差分* 会导致对协整过程建模时出现复杂情况。例如，由于 \(\boldsymbol{w}_t\) 不可逆，它没有有限阶 VAR 近似。因此，在实践中对 \(\boldsymbol{w}_t\) 进行建模以获得良好近似，通常需要高阶 VAR 模型。另一个复杂之处在于，不可逆模型的似然函数解位于参数空间的边界上，因此常规的 MLE 渐进理论不适用。有兴趣的读者可参阅 Tanaka (2017, 第 7 章) 及其中的参考文献。

为避免 *过度差分*，Engle and Granger (1987) 提出了误差修正的思想。详情见下一节。

## 3.6 误差修正模型

考虑式 (3.1) 中的 VARMA 模型，但假设 1 可能是行列式方程 \(|\phi(B)| = 0\) 的一个解。换句话说，我们假设 \(\boldsymbol{z}_t\) 可以是 1 阶整合过程。记 \(\nabla \boldsymbol{z}_t = \boldsymbol{z}_t - \boldsymbol{z}_{t-1} = (I - B)\boldsymbol{z}_t\) 为 \(\boldsymbol{z}_t\) 的一阶差分系列。通过直接的代数运算，例如 Tsay (2014, 第 5 章)，可得

\(\nabla \boldsymbol{z}_t = \Pi \boldsymbol{z}_{t-1} + \sum_{i=1}^{p-1} \Phi_i^* \nabla \boldsymbol{z}_{t-i} + \boldsymbol{c}_0 + \theta(B)\boldsymbol{a}_t, \quad (3.68)\)

其中 \(\Pi = \sum_{i=1}^p \Phi_i - I = -\phi(1)\)，且对 \(i = 1, \dots, p-1\)，\(\Phi_i^* = -(\phi_{i+1} + \cdots + \phi_p)\)。式 (3.68) 称为整合过程 \(\boldsymbol{z}_t\) 的 *误差修正模型* (ECM)。注意，该模型是可逆的，因为我们没有改变 MA 矩阵多项式。然而，若 \(|\phi(1)| = 0\)，则系数矩阵 \(\Pi\) 为奇异矩阵。一般来说，有以下几种情况：

- 若 rank(\(\Pi\)) = 0，则 \(\Pi = \mathbf{0}\) 且 \(\boldsymbol{z}_t\) 有 \(k\) 个单位根。它不是协整的。
- 若 rank(\(\Pi\)) = \(k\)，则 \(\Pi\) 非奇异且 \(\boldsymbol{z}_t\) 是平稳过程。
- 若 rank(\(\Pi\)) = \(m\) 且 \(1 \le m < k\)，则 \(\boldsymbol{z}_t\) 有 \(\nu = k - m\) 个单位根且存在 \(m\) 个协整向量。在这种情况下，可以写 \(\Pi = \boldsymbol{\alpha}\boldsymbol{\beta}'\)，其中 \(\boldsymbol{\alpha}\) 和 \(\boldsymbol{\beta}\) 均为秩为 \(m\) 的 \(k \times m\) 实值矩阵。令 \(\boldsymbol{w}_t = \boldsymbol{\beta}'\boldsymbol{z}_t\)，它是一个 \(m\) 维平稳过程。换句话说，\(\boldsymbol{\beta}\) 的列是 \(\boldsymbol{z}_t\) 的协整向量。注意，对任意 \(m \times m\) 正交矩阵 \(\boldsymbol{P}\)，均有 \(\boldsymbol{\alpha}\boldsymbol{\beta}' = (\boldsymbol{\alpha}\boldsymbol{P})(\boldsymbol{P}\boldsymbol{\beta})'\)。因此，若 \(m > 1\)，协整向量不是唯一确定的。不过，\(\boldsymbol{\beta}\) 的列空间是唯一的。

对于协整过程 \(\boldsymbol{z}_t\)，\(\boldsymbol{w}_t\) 的平稳性通过将模型改写为以下形式即可容易看出
\(\nabla z_t = \alpha w_{t-1} + \sum_{i=1}^{p-1} \phi_i^* \nabla z_{t-i} + c_0 + \theta(B) a_t. \tag{3.69}\)

这里 \(\alpha\) 是满秩且 \(\nabla z_t\) 是平稳的，因此 \(\boldsymbol{w}_t\) 必须是平稳的，因为平稳过程不能依赖于任何单位根过程。

式 (3.68) 的误差修正模型被称为 *过渡形式* 的 ECM，因为系数矩阵 \(\phi_i^*\) 表示过渡效应。ECM 的另一种形式如下
\(\nabla z_t = \Pi z_{t-p} + \sum_{i=1}^{p-1} \phi_i^* \nabla z_{t-i} + c_0 + \theta(B) a_t, \tag{3.70}\)

其中 \(\Pi\) 保持不变，即 \(\Pi = -\phi(1)\)，但对 \(i=1, \dots, p-1\)，\(\phi_i^* = -(\mathbf{I} - \phi_1 - \cdots - \phi_i)\)。这些新的 \(\phi_i^*\) 矩阵包含长期累积效应，式 (3.70) 中的 ECM 被称为 *长期形式* 的 ECM。

### 3.6.1 协整检验

检验多元过程中协整向量的存在性（及其数量）在计量经济学文献中被称为 *协整* 检验。文献中有多种可用于协整检验的方法。对于 VAR 模型，最常用的协整检验是 Johansen 检验 (Johansen, 1991; Johansen and Juselius, 1990)。该检验可以使用 (3.68) 或 (3.70) 的 ECM 进行。在我们的讨论中采用过渡形式，并将式 (3.68) 的 ECM 扩展以包含可能的时间趋势。即，

\(\nabla z_t = \Pi z_{t-1} + \sum_{i=1}^{p-1} \phi_i^* \nabla z_{t-i} + d_t + a_t, \tag{3.71}\)

其中 \(d_t = c_0 + c_1 t\)，\(c_0\) 和 \(c_1\) 为 \(k\) 维实向量，其余系数与式 (3.68) 相同。协整检验随后关注矩阵 \(\Pi\) 的秩，该秩与 \(z_{t-1}\) 和 \(\nabla z_t\) 之间的相关矩阵有关。假设多元正态性，Johansen (1991, 1995) 使用 ML 检验来检测 \(\Pi\) 的秩。这本质上基于典型相关分析。具体地，为减轻平稳部分对协整检验的影响，考虑两个初步的多元线性回归

\(\nabla z_t = \sum_{i=1}^{p-1} \gamma_i \nabla z_{t-i} + d_t + u_t \tag{3.72}\)
\(z_{t-1} = \sum_{i=1}^{p-1} \gamma_i^* \nabla z_{t-i} + d_t^* + v_t \tag{3.73}\)

其中上标“*”用于 Eq. (3.73) 的系数矩阵，确定性趋势是预先指定的，\(u_t\) 和 \(v_t\) 分别表示误差项。令 \(\hat{u}_t\) 和 \(\hat{v}_t\) 分别为 Eqs. (3.72) 和 (3.73) 的 OLS 估计残差。然后，考虑多元线性回归

\(\hat{u}_t = \Pi \hat{v}_t + \epsilon_t, \quad (3.74)\)

其中 \(\epsilon_t\) 是误差项。对于 Eqs. (3.71) 和 (3.74)，\(\Pi\) 的最小二乘估计是等价的。因此，在多元正态性假设下，可以通过检验 \(\hat{u}_t\) 和 \(\hat{v}_t\) 之间的典型相关系数来检查 \(\Pi\) 的秩。令 \(\lambda_1^2 \ge \lambda_2 \ge \cdots \ge \lambda_k^2\) 为 \(\hat{u}_t\) 和 \(\hat{v}_t\) 之间按顺序排列的平方典型相关系数。Johansen (1991) 考虑了用于识别 rank(\(\Pi\)) 的两种检验统计量。

对 \(m = 0, \dots, k-1\)，考虑原假设 \(H(m): \text{Rank}(\Pi) = m\) 与备择假设 \(H_a : \text{rank}(\Pi) > m\)。检验统计量为

\(L_{tr}(m) = -(T - kp) \sum_{i=m+1}^k \ln(1 - \lambda_i^2), \quad (3.75)\)

其中 \(T\) 为样本量，\(p\) 为 VAR 阶数，\(k\) 为维度。该检验的思想是，在原假设 \(H(m)\) 下，\(\text{rank}(\Pi) = m\)，因此 \(\hat{u}_t\) 和 \(\hat{v}_t\) 之间最大的 \(m\) 个典型相关系数为正，而其余 \(k - m\) 个典型相关系数为零。该检验统计量被称为迹检验。与平稳情况不同，\(L_{tr}(m)\) 在 \(H(m)\) 下的极限分布不是卡方分布。它是标准布朗运动的函数。详情见 Johansen (1991) 和 Tsay (2014, 第 5 章) 及其中的参考文献。在实践中，\(L_{tr}(m)\) 的渐进临界值通过模拟获得。

Johansen (1991) 考虑的第二个检验统计量涉及以下检验

\(H_0 : \text{Rank}(\Pi) = m \quad \text{vs} \quad \text{Rank}(\Pi) = m + 1.\)

在这种情况下，检验统计量为

\(L_{\max}(m) = -(T - kp) \ln(1 - \lambda_{m+1}^2). \quad (3.76)\)

该检验统计量称为最大检验。类似地，\(L_{\max}(m)\) 的极限分布也不是卡方分布，而是标准布朗运动的函数。

一旦协整向量的数量 \(m\) 确定，\(\hat{u}_t\) 和 \(\hat{v}_t\) 之间典型相关分析中与最大的 \(m\) 个特征值 \(\{\lambda_i^2 | i = 1, \dots, m\}\) 相关的特征向量可用于估计协整向量。

### 示例 3.4

考虑 1952 年 5 月至 2019 年 6 月的月度 Moody's 老牌 Aaa 和 Baa 债券收益率。数据可从圣路易斯联邦储备银行的 FRED 获取，如图 3.14 所示。从图中看，两种债券收益率大致同步波动，偶尔有偏离。

为演示协整检验，我们从标量单位根检验开始。对于 Aaa 债券收益率，选用 AR(3) 模型，增广 Dickey-Fuller 检验值为 \(-0.45\)。与 1% 或 5% 的临界值相比，不能拒绝单位根原假设。对于 Baa 债券收益率，选用 AR(2) 模型，增广 Dickey-Fuller 检验统计量为 \(-0.40\)，同样，这未能拒绝单位根

