## 附录 C

### C.1 奇异值分解

奇异值分解（SVD）是子空间辨识方法 [1–4] 中不可或缺的组成部分。它通过矩形矩阵 $Z \in \mathbb{R}^{n\times m}$ 的正交分解来刻画：

$$Z = U \times \Sigma \times V' \quad (C.1)$$

其中矩阵 $U \in \mathbb{R}^{n\times n}$ 和 $V \in \mathbb{R}^{m\times m}$ 为正交矩阵，满足 $U'U = I_n$、$V'V = I_m$，另有对角矩阵 $\Sigma = \operatorname{diag}[\sigma_1 \cdots \sigma_N]$，其中 $N < \min(n,m)$。有序奇异值集合为 $\{\sigma_i\}$，$i = 1, \ldots, N$，并满足 $\sigma_1 > \sigma_2 > \cdots > \sigma_N$。$Z$ 的奇异值以及 $U$、$V$ 的相应列向量 $u_i$、$v_i$ 共同构成第 $i$ 个左/右奇异向量，使得 [1]

$$Zv_i = \sigma_i u_i$$

$$Z'u_i = \sigma_i v_i$$

这直接由 SVD 得出，因为 $ZV = U\Sigma$ 且 $Z'U = V\Sigma'$。此外，将式 (C.1) 左乘 $U'$、右乘 $V$，并利用正交性质，可得如下关系：

$$U' \times Z \times V = \Sigma = \operatorname{diag}[\sigma_1 \cdots \sigma_N] \in \mathbb{R}^{n\times m} \quad \text{for } N = \min(n,m) \quad (C.2)$$

同样可以得出，对于秩为 $r$ 的矩阵，SVD 可表示为：

$$Z = U\Sigma V' = [U_r \mid U_{n-r}]\begin{bmatrix}\Sigma_r & 0 \\ 0 & 0\end{bmatrix}\begin{bmatrix}V_r' \\ V_{m-r}'\end{bmatrix} = U_r\Sigma_rV_r' \quad (C.3)$$

该近似中出现的数值误差可用奇异值量化如下：

$$
E_{\mathrm{abs}} = \sqrt{ \sum_{i=r+1}^{N} \sigma_i^2 } \quad \text{(Absolute Error)}
$$

$$
E_{\mathrm{rel}} = \sqrt{ \frac{ \sum_{i=r+1}^{N} \sigma_i^2 }{ \sum_{i=1}^{N} \sigma_i^2 } } \quad \text{(Relative Error)} \quad (C.4)
$$

矩阵的 SVD 具有以下性质：

- $Z$ 的奇异值集合等于 $Z'Z$ 的特征值的正平方根：$\sigma_i = +\sqrt{\lambda_i}$。

- 对于秩为 $r$ 的 $Z$，其奇异值满足

  $$\sigma_1 \ge \cdots \ge \sigma_r > \sigma_{r+1} = \cdots = \sigma_N = 0$$

- 秩条件给出

  $$\rho(Z) = r$$
  $$\mathrm{null}(Z) = \mathrm{span}\{v_{r+1}, \ldots, v_m\}$$
  $$\mathrm{range}(Z) = \mathrm{span}\{u_1, \ldots, u_r\} \quad (C.5)$$

- 矩阵分解可以直接用奇异值表示为

  $$Z = U_r \Sigma_r V_r' = \sum_{i=1}^{r} \sigma_i u_i v_i' \quad (C.6)$$

- 范数也可以用奇异值表示为

  $$\| Z \|_F = \sqrt{\sum_{i=1}^{N} \sigma_i^2} \quad N < \min(n, m) \quad \text{(Frobenius Norm)}$$
  $$\| Z \|_2 = \sigma_1 \quad \text{(2-Norm)} \quad (C.7)$$

- 矩阵的伪逆（$Z^{\#} = (Z'Z)^{-1} Z'$（行空间）或 $Z^{\#} = Z'(ZZ')^{-1}$（列空间））可以很容易地由下式确定

  $$Z^{\#} = (U \Sigma V')^{\#} = V \Sigma^{-1} U' \quad (C.8)$$

与式 (C.3) 的 SVD 相关联的是投影的概念，即将一个向量投影到一条直线或一个平面上 [4]。利用 SVD，我们有以下正交投影矩阵 [5]：

$$
\begin{aligned}
P_{Z \mid U} &= U_1 \times U_1' = Z \times Z^{\#} \\
P_{Z \mid U}^{\perp} &= U_2 \times U_2' = I - Z \times Z^{\#} \\
P_{Z \mid V} &= V_1 \times V_1' = Z^{\#} \times Z \\
P_{Z \mid V}^{\perp} &= V_2 \times V_2' = I - Z^{\#} \times Z
\end{aligned}
\tag{C.9}
$$

最后，SVD 同时对角化投影、格拉姆矩阵和伪逆 [2]

$$
\begin{aligned}
P_{Z \mid U} &= U \times \begin{bmatrix} I & 0 \\ 0 & 0 \end{bmatrix} \times U' \quad \text{(Projection)} \\
G_Z &:= Z' \times Z = V \times \Sigma^2 \times V' \quad \text{(Gramian)} \\
Z^{\#} &= V \times \Sigma^{-1} \times U' \quad \text{(Pseudo-Inverse)}
\end{aligned}
\tag{C.10}
$$

### C.2 QR 分解

矩阵的 QR 分解是著名的 Gram–Schmidt 向量正交化过程的矩阵实现 [4]。它由一个正交矩阵 $Q \in \mathbb{R}^{n\times m}$（其中 $m \ge n$）与一个上三角矩阵 $R_q \in \mathbb{R}^{m\times m}$ 的乘积定义，即

$$R = Q \times R_q \quad (C.11)$$

其中 $Q' \times Q = I_n$，使得 $Q' R = R_q$，$Q'$ 是将 $R$ 变换为 $R_q$ 的正交矩阵。数值上使用 Householder 变换 $T = I - 2 u u'$ 实现，其中向量 $u$ 定义为

$$u = \pm \frac{v_1 - v_2}{\| v_1 - v_2 \|} \quad \| u \| = 1 \quad (C.12)$$

### C.3 LQ 分解

LQ 分解是 QR 分解的对偶形式：$L = R'$，$Q = Q'$，其中

$$Z = L \times Q \quad (C.13)$$

$L$ 为下块三角矩阵，$Q$ 为正交矩阵。

或者等价地，描述 $Z$ 的基向量与 $Q$ 的基向量之间的关系。实际上，从这个角度看，我们可以将 $Q = [Q_Z \mid Q_Z^{\perp}]$ 进行分块，使得一个秩亏的数据矩阵 $Z$ 可以（理论上）分解为

$$Z = LQ = [L_Z \mid 0]\begin{bmatrix}Q_Z\\Q_Z^{\perp}\end{bmatrix} = L_Z \times Q_Z$$

与该分解相关的*投影矩阵*由 [4] 给出

$$P = Q \times Q' \quad \text{(Projection Matrix)}$$

### 参考文献

1. Golub, G. 和 Van Loan, C. (1989). *矩阵计算*，第 2 版。巴尔的摩，马里兰州：约翰斯·霍普金斯大学出版社。
2. Scharf, L. (1990). *统计信号处理：检测、估计与时间序列分析*。雷丁，马萨诸塞州：艾迪生-韦斯利出版公司。
3. Katayama, T. (2005). *系统辨识的子空间方法*。伦敦：施普林格出版社。
4. Bretscher, O. (2009). *线性代数及其应用*。上萨德尔里弗，新泽西州：普伦蒂斯-霍尔国际出版公司。
5. Klema, V. 和 Laub, A. (1980). 奇异值分解：其计算与若干应用。*IEEE 自动控制汇刊* AC-25 (2): 164–176.
