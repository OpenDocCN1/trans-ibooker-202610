

# 表 7.5 随机实现算法：过去输出-MOESP（PO-MOESP）方法。

正交投影（LQ-分解）
\mathcal{D} = \begin{bmatrix} \mathcal{U}_f \\ \mathcal{D}_p \\ \mathcal{Y}_f \end{bmatrix} = \begin{bmatrix} L_{11} & 0 & 0 \\ L_{21} & L_{22} & 0 \\ L_{31} & L_{32} & L_{33} \end{bmatrix} \begin{bmatrix} \tilde{Q}_1' \\ \tilde{Q}_2' \\ \tilde{Q}_3' \end{bmatrix} \quad (Data Matrix)

奇异值分解
L_{32} = [U_S U_N] \begin{bmatrix} \Sigma_S & 0 \\ 0 & \Sigma_N \end{bmatrix} \begin{bmatrix} V_S' \\ V_N' \end{bmatrix} \approx \underbrace{U_S \times \Sigma_S \times V_S'}_{\mathcal{O}_s} \approx (U_S \Sigma_S^{1/2}) \times (\Sigma_S^{1/2} V_S') \quad (Observability Matrix)

KSP 系统矩阵
A = (\mathcal{O}_N_s' \mathcal{O}_N_s)^{-1} \mathcal{O}_N_s' \mathcal{O}_N_s = (\Sigma_S^{-1/2} U_S') \mathcal{O}_N_s' \quad (System/Process Matrix)

KSP 测量（输出）矩阵
C = \mathcal{O}(1 : N_y, 1 : N_x) \quad (Output Matrix)

KSP 输入/输入/输出传输矩阵
\begin{bmatrix} \hat{D} \\ -- \\ \hat{B} \end{bmatrix} = \mathcal{U}^\# \times \tilde{Z} \quad (Input Matrices)

状态协方差方程
\bar{\Pi} = A \bar{\Pi} A' + (B - A \bar{\Pi} C')(D + D' - C \bar{\Pi} C')^{-1} (B - A \bar{\Pi} C')' \quad (Riccati Equation)

KSP 方程
R_{ee} = D + D' - C \bar{\Pi} C'
K_p = (B - A \bar{\Pi} C') \times R_{ee}^{-1} \quad (KSP Equations)

噪声协方差方程
R_{ww}^e = K_p R_{ee} K_p'
R_{vv}^e = R_{ee}
R_{wv}^e = K_p R_{ee} \quad (Noise-Covariances)

我们将 PO-MOESP 算法总结在表 7.5 中，并考虑以下噪声中正弦波的示例。

## 示例 7.2

考虑以下正弦系统，其代表了可能发生在航行于海洋的船只上振动机械、飞行于天空的飞机中结构如桥梁或建筑物甚至地震事件中从 0 到 25 Hz 的频率范围。这些事件的信号模型可以很容易地用噪声中的正弦波表示：

$y(t_k) = \sum_{i=1}^{N_i} A_i \sin 2\pi f_i t_k + v(t_k); \quad k = 1, \dots, K$

其中 $v \sim \mathcal{N}(0, \sigma_{vv}^2)$。

数据由六个正弦波在频率 $f_i = \{2, 5, 10, 14, 20, 22\ \text{Hz}\}$ 处合成，所有振幅均为单位振幅，信噪比（SNR）为 0 dB，采样间隔为 $\Delta t = 0.02$ 秒，采样点数为 $K = 10\ 000$。模拟数据如图 7.3 所示，显示了 (a) 中的噪声序列及其使用相关方法得到的对应功率谱 (b)。我们注意到在 0 dB SNR 下正弦频率清晰可辨。

![](img/4bb66282061cdabd9d492d7740bb0233_360_0.png)

![](img/4bb66282061cdabd9d492d7740bb0233_360_1.png)

图 7.3 噪声中模拟正弦波（0 dB）。(a) 噪声测量。(b) 谱估计（相关方法）。

![](img/4bb66282061cdabd9d492d7740bb0233_361_0.png)

**图 7.4** 噪声中正弦波的子空间辨识（0 dB）。(a) 谱估计：真实、数据和辨识谱。(b) 使用 SVD 的阶数估计：阶数 = 12。(c) 极点-零点图，带辨识频率（连续）。

PO-MOESP 和 N4SID（随后介绍）SID 方法均应用于该数据集，结果非常相似。所得谱估计如图 7.4 所示，估计谱在 (a) 中显示，以及在 (c) 中辨识的频率。通过改变阶数从 5 到 15 应用 SVD 方法进行阶数估计，从奇异值中估计出正确的 12 个状态阶数或等价于 6 个正弦波，如 (b) 所示。估计信号如图 7.5 所示，显示了真实和估计脉冲响应的一个小快照以及对噪声数据的“拟合”。每个都表明对信号和测量的合理估计。验证统计也相当良好，均值零/白度检验结果为 Z-M: 0.003 < 0.022；W-T: 3.3% 超出，带权重的平方和残差（WSSR）统计低于阈值，再次表明对数据的良好拟合。最终估计的正弦频率如表 7.6 所示。在此 SNR 下，我们再次看到两种方法均获得极佳结果，N4SID 的相对误差约为 ~0.5%，MOESP 技术约为 ~1.1%——性能基本相同。

![](img/4bb66282061cdabd9d492d7740bb0233_362_0.png)

![](img/4bb66282061cdabd9d492d7740bb0233_362_1.png)

图 7.5 噪声中辨识正弦波的比较（0 dB）。(a) 脉冲响应拟合。(b) 估计数据拟合。

## 表 7.6 随机子空间辨识：六（6）个正弦系统。

| Frequency | N4SID | Relative | MOESP | Relative | Z-Mean | WSSR |
|---|---|---|---|---|---|---|
| Hz | Estimate | Error | Estimate | Error | 3.1 × 10⁻² | Below τ |
| 2 | 1.997 | 1.50 × 10⁻³ | 2.006 | 3.0 × 10⁻³ | 1.6 × 10⁻⁸ | Yes |
| 5 | 5.001 | 0.20 × 10⁻³ | 4.984 | 3.2 × 10⁻³ | 1.6 × 10⁻⁸ | Yes |
| 10 | 10.007 | 0.70 × 10⁻³ | 10.004 | 0.40 × 10⁻³ | 1.6 × 10⁻⁸ | Yes |
| 14 | 14.002 | 0.14 × 10⁻³ | 14.009 | 0.06 × 10⁻³ | 1.6 × 10⁻⁸ | Yes |
| 20 | 20.001 | 0.05 × 10⁻³ | 20.001 | 0.05 × 10⁻³ | 1.6 × 10⁻⁸ | Yes |
| 22 | 21.997 | 0.14 × 10⁻³ | 22.004 | 0.02 × 10⁻³ | 1.6 × 10⁻⁸ | Yes |
| Average | — | 0.46 × 10⁻³ | — | 1.09 × 10⁻³ | — | Yes |

## 7.6 随机子空间实现：斜投影

分解或分离数据空间的替代方法是应用斜投影算子，从而在消除过去输入/输出/噪声数据的同时提取随机状态空间模型。该技术首先通过对输入/输出块汉克尔数据矩阵特定行空间的斜投影，直接从噪声输入/输出估计状态序列，然后通过状态/残差方程的最小二乘解提取底层模型 Σ_INV。在此方法中，状态序列的行空间可使用 SVD 获得，其中斜 SID 的关键要素基于提取：

```
$\mathcal{X}_k = [\,x(k)\ x(k+1)\ \cdots\ x(K+k-1)\,]$ 
```

这是当应用于本章的随机实现问题时，前一章 N4SID 方法的底层概念。

我们从式 (7.70) 的过去和未来输入/输出关系出发，并包含通过展开 INV 状态空间模型而得到的相应状态关系，即

```
$\hat{x}(k)=A^k\hat{x}(j)+\sum_{n=0}^{k-1}A^{k-n-1}Bu(n+j)+\sum_{n=0}^{k-1}A^{k-n-1}K_pe(n+j)$
```

以确定未来状态关于过去状态 $\mathcal{X}_p$、输入 $\mathcal{U}_p$ 和 INV $\mathcal{E}_p$ 的表达式

```
$\mathcal{X}_f=A^k\mathcal{X}_p+C_{AB}^{\leftarrow}(k)\mathcal{U}_p+C_{AK}^{\leftarrow}(k)\mathcal{E}_p \qquad (7.99)$
```

![](img/4bb66282061cdabd9d492d7740bb0233_365_0.png)

**图 7.6** 投影：$\mathcal{Y}_f$ 在联合子空间 $\mathcal{D}_p \vee \mathcal{U}_f$ 上的正交投影，然后沿未来输入 $\mathcal{U}_f$ 对过去数据 $\mathcal{D}_p$ 的斜投影。

合并项得

$$\mathcal{X}_f = \left[\mathcal{C}_{AB}^{\leftarrow}(k)-A^k\mathcal{O}_k^{\#}\mathcal{T}_k\;\middle|\;A^k\mathcal{O}_k^{\#}\;\middle|\;\mathcal{C}_{AK}^{\leftarrow}(k)-A^k\mathcal{O}_k^{\#}\Xi_k\right]\begin{bmatrix}\mathcal{U}_p\\\mathcal{Y}_p\\\mathcal{E}_p\end{bmatrix}$$

或简写为

$$\mathcal{X}_f=\Theta_p\times\mathcal{D}_p+\underbrace{\left(\mathcal{C}_{AK}^{\leftarrow}(k)-A^k\mathcal{O}_k^{\#}\Xi_k\right)}_{0}\mathcal{E}_p$$

表明未来状态 *仅* 依赖于过去输入、输出和 INV，而不依赖于未来输入 $\mathcal{U}_f$。然而，由于 $\mathcal{X}_f$ 位于 $\mathcal{D}_p \vee \mathcal{U}_f$ 的联合（行）子空间中，且创新与联合空间 $(\mathcal{E}_p,\mathcal{D}_p \vee \mathcal{Y}_f)=0$ 正交，则式 (7.107) 的最后一项为零，从而得到未来状态的最终表达式如下：

$$\mathcal{X}_f=\mathcal{P}^{\perp}_{\mathcal{X}_f\mid\mathcal{D}_p\circ\mathcal{U}_f}=\Theta_p\times\mathcal{D}_p \tag{7.108}$$

如前式 (6.106)⁵ 所示，从而得到未来输出在过去数据上沿未来输入的斜投影的最终表达式：

$$\mathcal{P}^{\parallel}_{\mathcal{Y}_f\mid\mathcal{D}_p\circ\mathcal{U}_f}=\mathcal{G}_k\mathcal{P}^{\parallel}_{\mathcal{X}_f\mid\mathcal{D}_p\circ\mathcal{U}_f}=\mathcal{G}_k\times\mathcal{X}_f=\mathcal{G}_k\times(\Theta_p\times\mathcal{D}_p) \tag{7.109}$$

⁵ 回顾式 (6.109) 中正交到斜投影关系的表达式在该证明中被应用。

