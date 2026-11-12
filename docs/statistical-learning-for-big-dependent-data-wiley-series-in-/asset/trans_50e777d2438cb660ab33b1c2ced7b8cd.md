

关于 \( Z \) 的假设；数据位置被置换，然后对所有可能的置换计算 \( Q \)。然后使用置换后的 \( Q \) 值的经验分位数进行推断，例如，如果观察到的 Eq. (9.23) 中的 \( Q \) 大于置换后的 \( Q \) 值的第 97.5 百分位数或小于第 2.5 百分位数，则拒绝 \( H_0 \)。

## 9.3.6 克里金

克里金是一种空间预测方法；参见 Matheron (1963)。遵循 Cressie and Wikle (2011)，我们使用 HM 框架介绍克里金法。感兴趣的空间过程是 \( \{Y(s)|s \in D\} \)，对所有 \( s \in D \) 都有 \( 0 < \text{Var}[Y(s)] < \infty \)。注意 \( Y(s) \) 可能不是二阶平稳的。观测数据 \( Z(s) \) 带有噪声，并遵循 Eq. (9.8) 中的模型。暂时假设 \( Y(s) \) 的前两个矩方程已知并由

\(\mu_y(s) = E[Y(s)], \quad s \in D,\) (9.24)

\(C_y(s_1, s_2) = \text{Cov}[Y(s_1), Y(s_2)], \quad s_1, s_2 \in D.\) (9.25)

此外，测量误差的方差 \(\sigma^2_e\) 也是已知的。在 Eq. (9.25) 中，我们不假设 \( C_y(s_1, s_2) \) 是 \( s_1 - s_2 \) 的函数。\( Y(s) \) 的变差图如下给出

\(2\gamma_y(s_1, s_2) = \text{Var}[Y(s_1) - Y(s_2)] = C_y(s_1, s_1) + C_y(s_2, s_2) - 2C_y(s_1, s_2).\)

克里金是为了在条件为 \( \boldsymbol{Z} = \{Z(s_1), \dots, Z(s_m)\}\) 时获得 \( Y(s_0) \) 的最优空间预测，其中 \( s_0 \in D \) 是感兴趣的位置。文献中考虑了三种类型的克里金，取决于对 \( Y(s) \) 均值方程的假设。第一种是简单克里金，其中 \( E[Y(s)] \) 是已知的。第二种是普通克里金，其中 \( E[Y(s)] = \mu \) 是未知常数，最后一种是通用克里金，其中 \( E[Y(s)] \) 依赖于带有未知参数的一些协变量。

### 9.3.6.1 简单克里金

称为 *简单克里金* 的方法是搜索所有异质线性预测器，\( \{\boldsymbol{a}'\boldsymbol{Z} + u | \boldsymbol{a} \in R^m, u \in R\} \)，以获得 \( Y(s_0) \) 的最优空间预测器。与时间序列分析中最小化均方预测误差（MSPE）类似，人们通过对

\(\text{MSPE}(\boldsymbol{a}, u) = E[Y(s_0) - \boldsymbol{a}'\boldsymbol{Z} - u]^2,\) (9.26)

对 \( \boldsymbol{a} \) 和 \( u \) 求偏导数来寻找 \( \boldsymbol{a} \) 和 \( u \)。方程 (9.26) 可以写成

\(\text{MSPE}(\boldsymbol{a}, u) = \text{Var}[Y(s_0) - \boldsymbol{a}'\boldsymbol{Z} - u] + \{E[Y(s_0) - \boldsymbol{a}'\boldsymbol{Z} - u]\}^2.\)

第二项是 \([\mu_y(s_0) - \boldsymbol{a}'\mu_y - u]^2\)，其中 \( \mu_y = [\mu_y(s_1), \dots, \mu_y(s_m)]'\)，如果 \( u \) 满足

\(u = \mu_y(s_0) - \boldsymbol{a}'\mu_y.\) (9.27)

则该项为零。

给定 Eq. (9.27) 中的 \( u \)，MSPE 的第一项为

\(C_y(s_0, s_0) - 2\boldsymbol{a}' \text{Cov}[Y(s_0), \boldsymbol{Z}] + \boldsymbol{a}'\Sigma_z \boldsymbol{a},\) (9.28)

其中 \( m \times m \) 协方差矩阵 \( \Sigma_z = [C_z(s_i, s_j)] \) 由

\(\begin{array}{ c }
C_z(u, v) = \left\{ \begin{array}{ c c }
C_y(v, v) + \sigma_\epsilon^2, & \text{if } u = v, v \in D, \\
C_y(u, v), & \text{if } u \neq v, 
\end{array} \right. \\
\end{array}\)

给出，并且

\(\text{Cov}[Y(s_0), Z] = [C_y(s_0, s_1), \dots, C_y(s_0, s_m)]' \equiv c_y(s_0). \quad (9.29)\)

对 Eq. (9.28) 关于 \( a \) 求偏导数并令结果等于 \( \mathbf{0} \)，我们得到

\( -2c_y(s_0) + 2\Sigma_z a = \mathbf{0}. \)

因此，最优系数向量是 \( a_* = \Sigma_z^{-1}c_y(s_0) \)，因此 \( u_* = \mu_y(s_0) - c_y(s_0)'\Sigma_z^{-1}\mu_y \)。基于先前的推导，简单克里金预测器是

\( Y^*(s_0) = \mu_y(s_0) + c_y(s_0)'\Sigma_z^{-1}(Z - \mu_y), \quad (9.30) \)

其中 \( c_y(s_0) \) 由 Eq. (9.29) 给出，\( \mu_y \) 在 Eq. (9.27) 中定义。将 \( u_* \) 和 \( a_* \) 代入 Eq. (9.26)，最小化的 MSPE 是

\( \sigma_{y,sk}^2(s_0) = \text{MSPE}(a_*, u_*) = C_y(s_0, s_0) - c_y(s_0)'\Sigma_z^{-1}c_y(s_0), \quad (9.31) \)

其中下标 \( sk \) 用于表示简单克里金。Eq. (9.31) 中的量 \( \sigma_{y,sk}^2(s_0) \) 经常被称为简单克里金方差。

当协变量可用且模型遵循 Eqs. (9.6) 到 (9.8) 时，我们有 \( \mu_y(s) = x(s)'\beta \)，简单克里金预测器变为

\( Y^*(s_0) = x(s_0)'\beta + c_y(s_0)'\Sigma_z^{-1}(Z - X\beta). \quad (9.32) \)

Eq. (9.31) 中的简单克里金方差保持不变。

请注意，简单克里金解和方差也可以在以下 HM 下得到：

- Data model: Conditional on \( \sigma_\epsilon^2 \),

\( Z(s_i)|Y(s_i), \sigma_\epsilon^2 \sim_{ind} N[Y(s_i), \sigma_\epsilon^2], \quad s_i \in D, \quad i = 1, \dots, m. \)

- Process model: Conditional on \( \beta \) and \( C_y(\cdot, \cdot) \), \( Y(\cdot) \) is a Gaussian process satisfying

\( E[Y(s)] = x(s)'\beta, \quad \text{Cov}[Y(s_i, s_j)] = C_y(s_i, s_j), \quad s_1, s_2 \in D. \)

来自数据模型，我们有 \( Z|Y \sim N(Y, \sigma_\epsilon^2 I) \)，其中 \( Y \) 在 Eq. (9.7) 中给出，\( I \) 是 \( m \times m \) 单位矩阵。来自过程模型，我们有

\(\begin{bmatrix} Y(s_0) \\ Y \end{bmatrix} \sim N \left( \begin{bmatrix} x(s_0)' \\ X \end{bmatrix} \beta, \begin{bmatrix} C_y(s_0, s_0) & c_y(s_0)' \\ c_y(s_0) & \Sigma_y \end{bmatrix} \right), \quad (9.33)\)

其中 \(\Sigma_y = \text{Var}(Y)\)。因此，根据多元正态分布的性质，

\(Y(s_0)|Y \sim N[x(s_0)'\beta + c_y(s_0)'\Sigma_y^{-1}(Y - X\beta), C_y(s_0, s_0) - c_y(s_0)'\Sigma_y^{-1}c_y(s_0)].\) (9.34)

接下来，根据贝叶斯定理，\([Y|Z] \propto [Z|Y][Y]\)，其中 \([A|C]\) 表示给定 \(C\) 时 \(A\) 的条件分布，\([Y]\) 表示 \(Y\) 的边缘分布。根据高斯分布的共轭性质，我们可以得到后验分布 \([Y|Z]\) 为

\(Y|Z \sim N[D^{-1}(\sigma_\epsilon^{-2}Z + \Sigma_y^{-1}X\beta), D^{-1}],\) (9.35)

其中 \(D = \sigma_\epsilon^{-2}I + \Sigma_y^{-1}\) 是两个精度矩阵之和。使用众所周知的 Sherman-Morrison-Woodbury 矩阵恒等式，我们可以将条件分布重写为

\(Y|Z \sim N(X\beta + K(Z - X\beta), (I - K)\Sigma_y),\) (9.36)

其中 \(K = \Sigma_y\Sigma_z^{-1}\) 与 \(\Sigma_z = \Sigma_y + \sigma_\epsilon^2 I\)。现在，使用迭代期望和事实 \([Y(s_0)|Y, Z] = [Y(s_0)|Y]\)，我们有

\(E[Y(s_0)|Z] = E[\{E[Y(s_0)|Y, Z]\}|Z]\)
\(= E[x(s_0)'\beta + c_y(s_0)'\Sigma_y^{-1}(Y - X\beta)|Z]\)
\(= x(s_0)'\beta + c_y(s_0)'\Sigma_y^{-1}K(Z - X\beta) \quad \text{[Equation (9.36)]}\)
\(= x(s_0)'\beta + c_y(s_0)'\Sigma_z^{-1}(Z - X\beta)\)
\(= Y^*(s_0).\) (9.37)

对于后验方差，我们有

\(\text{Var}[Y(s_0)|Z] = E\{\text{Var}[Y(s_0)|Y]|Z\} + \text{Var}\{E[Y(s_0)|Y]|Z\}\)
\(= C_y(s_0, s_0) - c_y(s_0)'\Sigma_y^{-1}c_y(s_0) + \text{Var}[c_y(s_0)'\Sigma_y^{-1}Y|Z]\)
\(= C_y(s_0, s_0) - c_y(s_0)'\Sigma_y^{-1}Kc_y(s_0) \quad \text{[Equation (9.36)]}\)
\(= C_y(s_0, s_0) - c_y(s_0)'\Sigma_z^{-1}c_y(s_0)\)
\(= \sigma^2_{y.sk}(s_0).\) (9.38)

这完成了在 HM 框架下的推导。

### 9.3.6.2 普通克里金

简单克里金的推导假设均值和协方差函数已知。这里我们放松假设，允许均值函数未知，但强加某些无偏性，导致在空间预测中使用数据的齐次线性组合。普通克里金是指 \(Y(s)\) 的均值是常数但未知的情况。

普通克里金预测器是数据 \(Z\) 的线性组合，记为 \(b'Z\)，它最小化 MSPE

\( MSPE(b) = E[Y(s_0) - b'Z]^2, \quad (9.39) \)

受无偏性条件 \( E(b'Z) = E[Y(s_0)] = \mu \) 对所有 \( \mu \in R \) 的约束。由于 \( E[Z(s)] = E[Y(s)] = \mu \)，我们有 \( E(Z) = \mu 1 \)，其中 \( 1 \) 是 1 组成的 \( m \)-维向量。无偏性条件则意味着

\( b'1 = 1, \quad (9.40) \)

即普通克里金系数的和为 1。然后使用拉格朗日乘数法来最小化目标函数

\( \ell(b) = MSPE(b) - 2\lambda(b'1 - 1). \)

更具体地说，目标函数是

\( \ell(b, \lambda) = C_y(s_0, s_0) - 2b'c_y(s_0) + b'\Sigma_z b - 2\lambda(b'1 - 1), \quad (9.41) \)

其中，再次，\( \Sigma_z = Var(Z) \)。对 \( b \) 和 \( \lambda \) 求偏导数并令它们等于零，我们有

\( -c_y(s_0) + \Sigma_z b = \lambda 1, \\ b'1 = 1. \quad (9.42) \)

求解上述方程，我们得到

\( b_* = \Sigma_z^{-1}[c_y(s_0) + k_* 1], \)

和 \( k_* = [1 - 1'\Sigma_z^{-1}c_y(s_0)] / (1'\Sigma_z^{-1}1) \)。因此，普通克里金预测器是

\( \widehat{Y}(s_0) = b_*'Z = \{c_y(s_0) + 1(1 - 1'\Sigma_z^{-1}1) / (1'\Sigma_z^{-1}1)\}'\Sigma_z^{-1}Z, \quad (9.43) \)

和最小化的 MSPE 是

\( \sigma_{y,ok}^2 = C_y(s_0, s_0) - c_y(s_0)'\Sigma_z^{-1}c_y(s_0) + [1 - 1'\Sigma_z^{-1}c_y(s_0)]^2 / (1'\Sigma_z^{-1}1), \quad (9.44) \)

其中下标 \( ok \) 表示普通克里金。\( \sigma_{y,ok}^2 \) 称为普通克里金方差。

### 9.3.6.3 通用克里金

通用克里金是指 \( E[Y(s)] = x(s)'\beta \) 且 \( \beta \) 未知的情况。这里 \( \beta \) 由广义最小二乘法（gls）估计

