

显然，当两个序列不相关时，EV($z_t$) 取得最大值，即 \(\rho_{12}(0) = 0\)。对于一个 \(k\)-维过程 $z_t$，由于方阵的行列式是其特征值的乘积，有效方差是 \(\Gamma(0)\) 特征值的几何平均值。

我们可以通过其分量的线性组合来总结平稳向量过程 $z_t$ 的性质。例如，考虑线性过程

$$y_t = c'z_t = \sum_{i=1}^k c_i z_{it}$$

其中 $c = (c_1, ..., c_k)' \neq 0$。该过程是平稳的，因为它是平稳分量 $z_{it}$ ($i = 1, ..., k$) 的有限线性组合。更具体地说，期望为 $E(y_t) = c_1 E(z_{1t}) + ... + c_k E(z_{kt}) = c'E(z_t) = c'\mu$，它是时间不变的。同样，$y_t$ 的方差由 $\text{Var}(y_t) = \text{Var}(c'z_t) = c'\text{Var}(z_t)c = c'\Gamma(0)c$ 给出，这也是时间不变的。一般来说，我们有 $\text{Cov}(y_t, y_{t-\ell}) = c'\Gamma(\ell)c$，它只依赖于 $\ell$。因此，$y_t$ 是平稳的。此外，由于 $\text{Var}(y_t)$ 是非负的且 $c$ 是任意非零向量，我们得出 $\Gamma(0)$ 是半正定的。

类似于标量情况，平稳向量过程 $z_t$ 在 Eq. (1.5) 中的自协方差矩阵必须满足某些条件。为了说明这一点，我们考虑 $z_t$ 及其 $m$ 个滞后值的非零线性组合，例如，

$$y_t = \sum_{i=1}^m b'_i z_{t-i+1} = b'_1 z_t + b'_2 z_{t-1} + \cdots + b'_m z_{t-m+1}$$

其中 $b_j = (b_{j1}, ..., b_{jk})'$ 为 $j = 1, ..., m$ 的 $k$-维实向量。定义 $b = (b'_1, b'_2, ..., b'_m)'$，即一个非零的 $km$-维常数向量，以及 $z_{t,m} = (z'_t, z'_{t-1}, ..., z'_{t-m+1})'$，则有 $\text{Var}(y_t) = b'\text{Var}(z_{t,m})b = b'M_z(m)b$，其中

$$M_z(m) = \begin{bmatrix} \Gamma(0) & \Gamma(1) & \cdots & \Gamma(m-1) \\ \Gamma(-1) & \Gamma(0) & \cdots & \Gamma(m-2) \\ \vdots & \vdots & \ddots & \vdots \\ \Gamma(-m+1) & \Gamma(-m+2) & \cdots & \Gamma(0) \end{bmatrix} \quad (1.8)$$

是一个非负分块 Toeplitz 矩阵。这个 $M_z(m)$ 矩阵是标量序列 $z_t$ 在 Eq. (1.3) 中的 $M_z(m)$ 的推广。

$z_t$ 的互相关矩阵（CCMs）定义为标准化过程 $x_t = D^{-1/2}(z_t - \mu)$ 的自协方差，其中矩阵 $D$ 定义为 $\text{diag}(\gamma_{11}(0), ..., \gamma_{kk}(0))$，由 $z_t$ 的分量方差组成。具体来说，$z_t$ 的滞后-\(\ell\) CCM 为

$$R_z(\ell) = \text{Cov}(x_t, x_{t-\ell}) = D^{-1/2} \text{Cov}(z_t, z_{t-\ell}) D^{-1/2}$$

$$= \begin{bmatrix} \rho_{11}(\ell) & \rho_{12}(\ell) & \cdots & \rho_{1k}(\ell) \\ \rho_{21}(\ell) & \rho_{22}(\ell) & \cdots & \rho_{2k}(\ell) \\ \vdots & \vdots & \ddots & \vdots \\ \rho_{k1}(\ell) & \rho_{k2}(\ell) & \cdots & \rho_{kk}(\ell) \end{bmatrix} \quad (1.9)$$

根据定义，我们有 \(\rho_{ii}(0) = 1\) 且

\(\rho_{ij}(\ell) = \frac{\gamma_{ij}(\ell)}{\sqrt{\gamma_{ii}(0)\gamma_{jj}(0)}}.\)

类似于自协方差矩阵，我们有 \(M_z(\ell) = [M_z(-\ell)]'\).

$z_t$ 的有效相关性由

\(\text{ER}_z = 1 - |\text{R}_z(0)|^{1/k}\) (1.10)

给出，其值在零和一之间。例如，当 \(k=2\) 时，\(\text{ER}_z = 1 - [1 - \rho_{12}(0)]^{1/2}\)。更多性质请参见 Peña and Rodriguez (2003)。

### 1.2.2.1 向量白噪声

若 \(k\)-维向量过程 \(z_t\) 满足 (a) \(E(z_t) = 0\)，(b) \(\text{Var}(z_t) = \Gamma(0)\) 为正定，且 (c) 对所有 \(\ell \neq 0\) 有 \(\Gamma(\ell) = 0\)，则称其为白噪声过程。这是标量白噪声的直接推广。因此，向量白噪声过程由零均值、正定协方差且无滞后自相关或互相关的随机向量组成。

### 1.2.2.2 可逆性

在许多应用中，我们使用线性回归类型的模型来描述向量过程的动态依赖性。该模型可写为

\(z_t = \pi_0 + \sum_{i=1}^{\infty} \pi_i z_{t-i} + a_t\) (1.11)

其中 \(\{a_t\}\) 是平稳向量白噪声过程，\(\pi_0\) 是常数 \(k\)-维向量，\(\pi_i\) 为 \(k \times k\) 实值矩阵。为了使 Eq. (1.11) 有意义，系数矩阵必须满足条件

\(\sum_{i=1}^{\infty} \|\pi_i\| < \infty\) (1.12)

其中 \(\|\mathbf{A}\|\) 表示矩阵 \(\mathbf{A}\) 的一个范数，例如 \(\|\mathbf{A}\| = \left( \sum_{i=1}^k \sum_{j=1}^k a_{ij}^2 \right)^{1/2}\)，即 \(Frobenius\) 范数。若向量过程 \(z_t\) 可以采用满足 Eq. (1.12) 条件的 (1.11) 模型，则称其为 \(invertible\)（可逆）。若 Eq. (1.11) 中的求和在有限整数 \(p\) 处截断，则 \(z_t\) 可逆，且该过程遵循向量自回归（VAR）模型。

可逆性是时间序列分析中的另一个重要概念。可逆模型的一个必要条件是当 \(i \to \infty\) 时 \(\|\pi_i\| \to 0\)。这意味着，在已知信息 \(\{z_{t-1}, z_{t-2}, \dots, z_{t-\ell+1}\}\) 的条件下，\(z_{t-\ell}\) 对 \(z_t\) 的附加贡献随着 \(\ell\) 增加而减小。考虑标量过程 \(z_t\)，任何有限 \(p\) 的 AR(\(p\)) 过程都是可逆的。另一方面，过程 \(z_t = a_t - a_{t-1}\)，其中 \(\{a_t\}\) 是白噪声，是平稳的，但不可逆。

## 1.3 平稳向量过程的样本矩

在实际应用中，我们通常观察到平稳向量过程 \(z_t\) 的一系列实现，例如 \(\{z_1, \dots, z_T\}\)。我们的目标是根据观察数据研究 \(z_t\) 的性能。为此，某些探索性数据分析很有用。特别是，通过研究其样本矩，可以了解许多关于 \(z_t\) 的信息。

### 1.3.1 样本均值

标量过程 \(z_{it}\) 总体均值的无偏估计量是样本均值

\(\bar{z}_{\cdot i} = \frac{1}{T} \sum_{t=1}^T z_{it}.\) (1.13)

将它们合在一起，向量过程 \(\boldsymbol{z}_t\) 的样本均值为

\(\bar{\boldsymbol{z}} = (\bar{z}_1, \dots, \bar{z}_k)' = \frac{1}{T} \sum_{t=1}^T \boldsymbol{z}_t.\) (1.14)

对于独立同分布的随机样本，容易看出 \(E(\bar{z}_i) = E(z_{it}) = \mu_i\)；且 \(\bar{z}_i\) 的方差为 \(\sigma_i^2 / T\)，其中 \(\sigma_i^2 = \text{Var}(z_{it})\)。因此，在这种情况下，当 \(T \to \infty\) 时，样本均值是总体均值 \(\mu_i\) 的一致估计量。

需要注意的是，对于平稳过程，样本均值的一致性不一定成立。例如，令 \(z_{11}\) 为来自均值为零、方差为 \(\sigma^2\) 的正态分布的随机样本。令 \(t = 2, \dots, T\) 时 \(z_{1t} = z_{11}\)。这个特殊过程 \(\{z_{1t}\}\) 是平稳的，因为 (1) \(E(z_{1t}) = 0\)，是时间不变的，(2) \(\text{Var}(z_{1t}) = \sigma^2\)，也是时间不变的，且 \(\text{Cov}(z_{1t}, z_{1t-j}) = \sigma^2\)，对所有 \(j\) 都是时间不变的。然而，在这个特定情况下，无论样本量 \(T\) 为多少，都有 \(\bar{z}_{\cdot 1} = z_{11}\) 和 \(\text{Var}(\bar{z}_{\cdot 1}) = \sigma_1^2\)。因此，当 \(T \to \infty\) 时，样本均值不是这个特殊过程的一致估计量。

样本均值 \(\bar{z}_{\cdot i}\) 作为总体均值 \(\mu_i\) 的一致估计量的条件，被称为随机过程的 *遍历* 条件。遍历理论关注的是长实现的时间平均（样本均值）收敛到动态系统空间平均（在给定时刻下许多随机样本的平均）的条件。对于标量弱平稳 \(z_t\)，\(\bar{z}\) 收敛到 \(\mu = E(z_t)\) 的遍历条件是 \(\sum_{i=0}^{\infty} |\gamma_i| < \infty\)。从本书起，我们假设此条件成立。

对于弱平稳向量过程 \(\boldsymbol{z}_t\)，样本均值 \(\bar{\boldsymbol{z}}\) 的协方差矩阵由

\(\begin{aligned} \text{Var}(\bar{\boldsymbol{z}}) &= E \left[ (\bar{\boldsymbol{z}} - \boldsymbol{\mu})(\bar{\boldsymbol{z}} - \boldsymbol{\mu})' \right] = E \left[ \left\{ \frac{1}{T} \sum_{t=1}^T (\boldsymbol{z}_t - \boldsymbol{\mu}) \right\} \left\{ \frac{1}{T} \sum_{j=1}^T (\boldsymbol{z}_j - \boldsymbol{\mu})' \right\} \right] \\ &= \frac{1}{T^2} \sum_{t=1}^T E \left[ (\boldsymbol{z}_t - \boldsymbol{\mu}) \left\{ \sum_{j=1}^T (\boldsymbol{z}_j - \boldsymbol{\mu})' \right\} \right] = \frac{1}{T^2} \left[ \sum_{|j-t| \le T-1} (T - |j|)\boldsymbol{\Gamma}(j) \right] . \end{aligned}\) (1.15)

这意味着标量样本均值 \(\bar{z}_{\cdot i}\) 的渐近方差为

\(T \times \text{Var}(\bar{z}_{\cdot i}) \to \gamma_{ii}(0) + 2 \sum_{\ell=1}^{\infty} \gamma_{ii}(\ell), \quad \text{as } T \to \infty.\) (1.16)

再次说明，\(\gamma_{ii}(\ell)\) 是 \(z_{it}\) 的滞后-\(\ell\) 自协方差。根据 Equation (1.2)，如果 \(\{z_{it}\}\) 是白噪声，则样本均值 \(\bar{z}_{\cdot i}\) 的方差为 \(\sigma_i^2 / T\)。一般来说，我们有

\(\text{Var}(\bar{z}_{\cdot i}) \approx \frac{1}{T} \left[ \gamma_{ii}(0) + 2 \sum_{l=1}^{T} \gamma_{ii}(l) \right] , \quad \text{as } T \to \infty.\)

