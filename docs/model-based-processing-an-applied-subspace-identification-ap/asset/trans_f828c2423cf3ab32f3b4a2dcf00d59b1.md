

## MATLAB 注释

MATLAB 提供了丰富的线性代数算法，可应用于子空间实现问题，包括创建 Hankel 矩阵和 Toeplitz 矩阵及其分解的命令，使用奇异值分解（svd）、Gram Schmidt 或 QR 分解，该分解可轻松转换为贯穿始终使用的下三角/正交 LQ 变换。其他技术，如协方差矩阵的 Cholesky 分解以及 LDU 分解方法，可进一步扩展这些方法。系统辨识工具箱（ident）提供了大量用于状态空间模型的子空间（n4sid）和预测误差方法，包括黑箱和灰箱表示。分析命令可实现对已辨识模型的性能分析以及完善的验证方法。使用嵌入式 GUI，用户可以轻松导入输入/输出数据，进行初步预处理（例如均值和趋势去除），将数据划分为辨识和验证两部分，并可即时分析结果，进行信号估计绘图、残差分析、频域绘图和瞬态响应分析。这对于该问题集来说是一个非常强大的工具箱。子空间算法的其他实现可从 [6] 和 [9] 的文本中提取。

## 参考文献

- 1 Kalman, R. (1963). Mathematical description of linear dynamical systems. SIAM J. Control 82: 152–192.
- 2 Kalman, R., Falb, P., and Arbib, M. (1969). Topics in Mathematical System Theory. New York: McGraw-Hill.
- 3 Ho, B. and Kalman, R. (1966). Effective reconstruction of linear state variable models from input/output data. Regelungstechnik 14: 545–548.
- 4 Candy, J. (2006). Model-Based Signal Processing. Hoboken, NJ: Wiley/IEEE Press.
- 5 Ljung, L. (1999). System Identification: Theory for the User, 2e. Englewood Cliffs, NJ: Prentice-Hall.
- 6 van Overschee, P. and De Moor, B. (1996). Subspace Identification for Linear Systems: Theory, Implementation, Applications. Boston, MA: Kluwer Academic Publishers.
- 7 Verhaegen, M. and Verdult, V. (2007). Filtering and System Identification: A Least-Squares Approach. Cambridge: Cambridge University Press.
- 8 Tangirala, A. (2015). Principles of System Identification: Theory and Practice. Boca Raton, FL: CRC Press.
- 29 Golub, G. and van Loan, C. (1996). *Matrix Computations*, 3e. Baltimore, MD: Johns Hopkins University Press.
- 30 Bretscher, O. (2015). *Linear Algebra with Applications*, 5e. Upper Saddle River, NJ: Pearson Prentice-Hall.
- 31 Sharf, L. (1991). *Statistical Signal Processing: Detection, Estimation and Time Series Analysis*. Reading, MA: Addison-Wesley.
- 32 Swevers, J., Adams, M., and De Moor, B. (1987). A new direct time domain identification method for linear systems using singular value decomposition. In: *Proceedings of the 12th International Seminar on Modal Analysis, 12-3*, 1–20.
- 33 De Moor, B., Vandewalle, J., Vandenberghe, L., and Mieghem, P. (1988). A geometrical strategy for the identification of state space models of linear multivariable systems with singular value decomposition. *Proc. IFAC 88*: 700–704.
- 34 Van Der Veen, A., Deprettere, E.F., and Swindlehurst, A.L. (1993). Subspace-based methods for the identification of linear time-invariant systems. *Proc. IEEE* 81 (9): 1277–1308.
- 35 Katayama, T. and Picci, G. (1994). Realization of stochastic systems with exogenous inputs and subspace identification methods. *Automatica* 35 (10): 75–93.
- 36 Astrom, K. and Eykhoff, P. (1971). System identification—a survey. *Automatica* 7 (2): 123–162.
- 37 Ljung, L. and Soderstrom, T. (1983). *Theory and Practice of Recursive Identification*. Boston, MA: MIT Press.
- 38 Akaike, H. (1974). A new look at the statistical model identification. *IEEE Trans. Autom. Control* 19 (6): 716–723.
- 39 Sakamoto, Y., Ishiguro, M., and Kitagawa, G. (1986). *Akaike Information Criterion Statistics*. Boston, MA: D. Reidel/Kluwer Academic.
- 40 Bauer, D. (2001). Order estimation for subspace methods. *Automatica* 37: 1561–1573.
- 41 Pham Gia, T. and Hung, T. (2001). The mean and median absolute deviations. *Math. Comput. Model.* 34: 921–936.
- 42 Zoubi, A. and Iskander, D. (2004). *Bootstrap Techniques for Signal Processing*. Cambridge: Cambridge University Press.
- 43 Reynders, E. (2012). System identification methods for (operational) modal analysis: review and comparison. *Arch. Comput. Methods Eng.* 19 (1): 51–124.
- 44 Cara, F., Juan, J., Alarcon, E. et al. (2013). Modal contribution and state space order selection in operational modal analysis. *Mech. Syst. Sig. Process.* 38: 276–298.

## 问题

6.1 经典实现理论基于 MIMO 脉冲响应矩阵的 Hankel 矩阵的分解为满秩、可观测性和可控性矩阵，$\mathcal{H}_{K,K'} = \mathcal{O}_K \times \mathcal{C}_{K'}$，证明 $\mathcal{H}_{K,K'}$ 的每一行（列）是相关的当且仅当它是 $\mathcal{O}_K$（$\mathcal{C}_{K'}$）的相关行（列）。（该结果的证明构成了规范实现理论的基础。）

6.2 假设我们给出了具有 [10] 的连续时间系统的如下状态空间描述

$\dot{x}(t) = \begin{bmatrix} -1 & -4/\theta \\ 4\theta & -2 \end{bmatrix} x(t) + \begin{bmatrix} 1 \\ 2\theta \end{bmatrix} u(t)$
$y(t) = \begin{bmatrix} -1 & 2/\theta \end{bmatrix} x(t)$

连续时间（下标“c”）可观测性和可控性 Gramians 满足以下 Lyapunov 方程：

$A_c' \mathcal{O}_c + \mathcal{O}_c A_c = -C_c' C_c \qquad \text{(Continuous Gramians)}$
$A_c C_c + C_c A_c' = -B_c B_c'$

- (a) 计算可观测性和可控性 Gramians。
- (b) 这是一个平衡实现吗？展示结果。
- (c) 如果是，需要 $\theta$ 的哪些值？

6.3 假设我们给出了传递函数

$H(s) = \frac{4s^2 - 2s - 6}{2s^4 + 2s^3 + 2s^2 + 3s + 1}$

- (a) 这个系统的阶数是多少？验证你的结果。
- (b) 使用 Ho–Kalman 方法求一个平衡实现？

6.4 我们给出了如下连续系统：

$\dot{x}(t) = A x(t)$

其中

$A = \begin{bmatrix} -4 & -4 \\ 2 & -6 \end{bmatrix}$

(a) 建立对应 *Lyapunov 方程* 的解：

\[A'P + PA = -Q\qquad \text{(Lyapunov Equation)}\]

其中 \(Q=I\)。

(b) 这个连续系统稳定吗？展示结果。

(c) 使用计算得到的特征值验证稳定性。

### 6.5

考虑如下标量例子，其脉冲响应为，\(H(k)=\{1,\ 1/2,\ 1/4,\ 1/8,\ 1/16\}\) 且 \(N=5\)。使用 SVD 方法，我们希望提取实现 \(\Sigma=(A,b,c)\)。建立 Hankel 矩阵并应用 SVD–Ho 算法来提取系统。

### 6.6

我们选择 \(K>N_x\)，然后对给定的 \(\mathcal{H}_{K,K}\) 进行 SVD 并选择 *平衡实现* 使得

\[\mathcal{H}_{K,K}=U\Sigma V'=\bigl(U(\Sigma_{N_x}^{1/2})\bigr)\times\bigl((\Sigma_{N_x}^{1/2})V'\bigr)=\mathcal{O}_K\times\mathcal{C}_K\]

其中 \(N_x\) 是 Hankel 矩阵的秩。利用 \(\mathcal{H}_{K,K}\to\mathcal{H}^{\dagger}_{K,K}\) 的平移不变性，证明如果我们选择 \(N_x\) 个奇异值中的前 \(N_1\) 个（\(N_1<N_x\)），则我们可以从 \(N_x\) 次实现中提取 \(N_1\) 维实现 \(\Sigma_{A_1B_1C_1D_1}\)。这是否意味着我们可以从单个 \(N_x\) 实现中作为子矩阵提取 *所有* 低阶系统 \((N<N_x)\)？证明之？这些实现是“嵌套”的吗（\(A_1\in A_{N_x}\)）？

### 6.7

假设我们有如下离散传递函数：

\[H(z)=\frac{z+3}{z^2+3z+2}\]

- (a) 这个系统的脉冲响应是什么？（*提示：* 长除法）
- (b) 由脉冲响应创建 Hankel 矩阵。
- (c) 使用原始 Ho–Kalman 算法，提取系统 \(\Sigma_{ABCD}\)。
- (d) 使用 SVD–Ho–Kalman 算法，也提取系统 \(\Sigma_{ABCD}\)。
- (e) 这些实现等价吗？（*提示：* 特征值？脉冲响应？）

### 6.8

我们给出了如下由传递函数表示的离散系统

\[H(z)=\begin{bmatrix}\dfrac{z-0.5}{z^2-1} & \dfrac{1}{z-1}\\[4pt] \dfrac{z^2}{z^2-1} & \dfrac{2z}{z-1}\end{bmatrix}\]

- (a) 计算这个系统的脉冲响应。
- (b) 创建 Hankel 矩阵。

