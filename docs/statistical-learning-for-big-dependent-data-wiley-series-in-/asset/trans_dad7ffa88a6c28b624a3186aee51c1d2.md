

对 $\boldsymbol{\phi}_p$ 求导并令其为零，我们得到

$$
\frac{\partial L_C(\boldsymbol{\phi}_p, \sigma^2)}{\partial \boldsymbol{\phi}_p} = 0 = -\boldsymbol{W}'\boldsymbol{\omega}_{p+1}^T + \boldsymbol{W}'\boldsymbol{W}\hat{\boldsymbol{\phi}}_p, \tag{2.92}
$$

因此，

$$
\hat{\boldsymbol{\phi}}_p = (\boldsymbol{W}'\boldsymbol{W})^{-1}\boldsymbol{W}'\boldsymbol{\omega}_{p+1}^T, \tag{2.93}
$$

这是 AR($p$) 模型中 $\boldsymbol{\phi}_p$ 的最小二乘（LS）估计，可以轻松高效地计算。该估计的协方差矩阵由

$$
\text{Cov}(\hat{\boldsymbol{\phi}}_p) = (\boldsymbol{W}'\boldsymbol{W})^{-1}\widehat{\sigma}_a^2, \tag{2.94}
$$

其中

$$
\widehat{\sigma}_a^2 = \frac{1}{T-p} \sum_{t=p+1}^T (\omega_t - \hat{\boldsymbol{\phi}}'_p \boldsymbol{\omega}_{t-1})^2. \tag{2.95}
$$

估计量 $\hat{\boldsymbol{\phi}}_p$ 和 $\widehat{\sigma}_a^2$ 是一致的，且当 $p/T$ 较小时，它们与精确似然估计值接近。

对于 ARMA($p$, $q$) 模型，似然函数关于 MA 参数是非线性的，但 Hannan and Rissanen (1982) 提出了一种快速 LS 估计方法。这是一种近似方法，包含两个步骤。第一步，拟合高阶 AR($p_o$) 模型以获得残差。理想情况下，$p_o > p + q$ 且残差不应具有序列相关性。将残差表示为 $\hat{b}_t$，它用作创新项 $a_t$ 的代理。第二步，考虑模型

$$
w_t = \phi_1 w_{t-1} + \cdots + \phi_p w_{t-p} + \alpha_1 \hat{b}_{t-1} + \cdots + \alpha_q \hat{b}_{t-q} + a_t, \tag{2.96}
$$

并应用 LS 估计。系数 $\hat{\phi}_i$ 和 $-\hat{\alpha}_i$ 分别作为 $\phi_i$ 和 $\theta_i$ 参数的估计量。随着 $p_o$ 随样本量 $T$ 增加，人们期望该方法能产生平稳 ARMA($p$, $q$) 模型的良好估计。在实践中，该方法可用于为 ML 估计提供良好的初始参数估计，尤其是对于向量 ARMA 模型。参见 Tsay (2014)。

在实践中，高阶 $p_o$ 可通过信息准则选择。随后可以使用 AR($p_o$) 残差作为创新项 $a_t$ 的代理，并利用 Eq. (2.96) 估计低阶 ARMA($p$, $q$) 模型。然后可利用估计结果计算 ARMA 模型选择的信息准则。当时间序列数量 $k$ 较大时，这种近似方法可能很有用。

## 2.8.2 在线估计

一旦 ARMA 模型已被估计，当有新观测值可用时，我们可以通过条件似然方法进行快速更新估计。这种在线估计在实践中很有用。假设我们已使用样本 $\omega_t^T$ 估计了参数，并观察到新数据 $\omega_{T+1}$。我们不是重新估计模型，而是可以使用卡尔曼滤波器更新参数。我们先介绍 AR($p$) 模型的情况。使用第 2.4.3 节的方程，观测方程现在为 $\omega_t = \boldsymbol{h}'_t\boldsymbol{\alpha}_t + a_t$，其中 $\boldsymbol{h}'_t = (\omega_{t-1}, \dots, \omega_{t-p})$ 且 $\boldsymbol{\alpha}_t = (\phi_{1,t}, \dots, \phi_{p,t})'$，状态转移方程为 $\boldsymbol{\alpha}_t = \boldsymbol{\alpha}_{t-1}$。在第 2.4.2 节的一般状态表示中，我们有 \(\Omega_t = I\)，\(R_t = 0\)，\(H_t = h'_t\) 和 \(V_t = \sigma^2\)。时刻 \(T\) 的参数（或状态向量）估计由 Eq. (2.93) 给出。换句话说，我们有

$$
\alpha_T = \widehat{\phi}_{p,T} = (W'_T W_T)^{-1} W'_T \omega^T_{p+1},
$$

其中下标 \(T\) 用于表示数据 \(\omega^T_1$，相关的协方差矩阵为

$$
S_{T+1|T} = S_T = \text{Cov}(\widehat{\phi}_{p,T}) = (W'_T W_T)^{-1} \sigma^2_{a,T}.
$$

在预测原点 \(T\) 处，新观测值 \(\omega_{T+1}$ 的预测误差为

$$
e_{T+1} = \widehat{a}_{T+1|T} = \omega_{T+1} - \widehat{\phi}_{p,T} \omega^T_{T-p+1}, \tag{2.97}
$$

方差为（参见 Eq. (2.73)）

$$
\begin{aligned}
p_{T+1|T} &= E(e^2_{T+1}) = [(\omega^T_{T-p+1})'(W'_T W_T)^{-1}(\omega^T_{T-p+1}) + 1]\widehat{\sigma}^2_{a,T} \\
&= (1 + h_{T+1|T})\widehat{\sigma}^2_{a,T},
\end{aligned} \tag{2.98}
$$

其中

$$
h_{T+1|T} = (\omega^T_{T-p+1})'(W'_T W_T)^{-1}(\omega^T_{T-p+1}),
$$

且给定 $\omega_{T+1}$ 时，参数的新估计为

$$
\alpha_{T+1} = \widehat{\phi}_{p,T+1} = \widehat{\phi}_{p,T} + (W'_T W_T)^{-1} \omega^T_{T-p+1} e_{T+1} / (1 + h_{T+1|T}). \tag{2.99}
$$

需要注意的是，参数的变化与预测误差 \(e_{T+1}$ 成正比，如果预测误差为零，则参数不更新。否则，参数更新取决于回归变量向量 \(\omega^T_{T-p+1}$。参数的新协方差矩阵由

$$
S_{T+1} = (W'_{T+1} W_{T+1})^{-1} \widehat{\sigma}^2_{a,T+1}, \tag{2.100}
$$

可以写为

$$
S_{T+1} = (W'_T W_T)^{-1} \widehat{\sigma}^2_{a,T} [I - \omega^T_{T-p+1} (\omega^T_{T-p+1})'(W'_T W_T)^{-1} / (1 + h_{T+1|T})]. \tag{2.101}
$$

由于 \(\widehat{\sigma}^2_a(T) = \sum^T_{t=p+1} (\omega_t - \widehat{\phi}'_p(T) \omega^{t-p}_{t-1})^2 / (T - p)\) 和 \(\widehat{\sigma}^2_a(T+1) = \sum^{T+1}_{t=p+1} (\omega_t - \widehat{\phi}'_p(T+1) \omega^{t-p}_{t-1})^2 / (T + 1 - p)\) 已知，我们可以通过 Eqs. (2.100) 和 (2.101) 计算 \((W'_{T+1} W_{T+1})^{-1}$，而无需直接计算逆矩阵。

对于 ARMA 模型，也可以通过使用线性模型表示 Eq. (2.96) 推导近似更新方案。事实上，Eqs. (2.97) 到 (2.99) 给出的更新算法由 Plackett (1950) 对任何形式为 \(y_t = x'_t \beta_t + u_t$ 的线性模型得到。令 \(e_{T+1} = y_{T+1} - x'_{T+1} \widehat{\beta}_T$，更新为

$$
\widehat{\beta}_{T+1} = \widehat{\beta}_T + (X'_T X_T)^{-1} x_{T+1} e_{T+1} / (1 + h_{T+1|T}), \tag{2.102}
$$

其中 \( h_{T+1|T} = \boldsymbol{x}'_{T+1}(\boldsymbol{X}'_T \boldsymbol{X}_T)^{-1} \boldsymbol{x}_{T+1} $，且

$$
(\boldsymbol{X}'_{T+1} \boldsymbol{X}_{T+1})^{-1} = (\boldsymbol{X}'_T \boldsymbol{X}_T)^{-1} [\boldsymbol{I} - \boldsymbol{x}_{T+1} \boldsymbol{x}'_{T+1} (\boldsymbol{X}'_T \boldsymbol{X}_T)^{-1} / (1 + h_{T+1|T})] \frac{\hat{\sigma}^2_{a,T}}{\hat{\sigma}^2_{a,T+1}}, \tag{2.103}
$$

这些与 Eqs. (2.99) 和 (2.101) 相似。

## 2.8.3 最大似然（ML）估计

对于平稳序列 \(\omega_t$ 的 ARMA 模型，ML 估计通过将数据的联合概率密度函数分解为条件和边缘密度函数的乘积，并对参数进行最大化来进行。数据被视为固定。\(\boldsymbol{\omega}^T_1$ 的联合密度函数为

$$
f(\boldsymbol{\omega}^T_1) = f(\omega_1)f(\omega_2|\omega_1)f(\omega_3|\omega_2,\omega_1) \cdots f(\omega_T|\omega_{T-1}, \dots, \omega_1).
$$

并且，在联合正态性下，所有涉及的条件分布都是正态分布，其期望为 1 步先验预测

$$
E(\omega_t|\omega_{t-1}, \dots, \omega_1) = \widehat{\omega}_{t|t-1}(1) = \omega_{t|t-1}.
$$

令 \( e_{t|t-1} \) 为利用 \(\boldsymbol{\omega}^{t-1}_1$ 的信息并给定过程参数时 \(\omega_t$ 的 1 步先验预测误差。因此，

$$
e_{t|t-1} = \omega_t - \omega_{t|t-1}.
$$

这些预测误差与过程的创新项 \( a_t \) 高度相关，但它们不相同，因为它们取决于初始值。将 \(\sigma^2 v_{t|t-1}$ 称为这些预测误差的方差，ARMA 过程数据的联合密度函数可以写为

$$
f(\boldsymbol{\omega}^T_1) = \prod_{t=1}^T \sigma^{-1} v^{-1/2}_{t|t-1} (2\pi)^{-1/2} \exp \left\{ - \frac{1}{2\sigma^2} \sum_{t=1}^T \frac{(\omega_t - \omega_{t|t-1})^2}{v_{t|t-1}} \right\},
$$

其中 \( v_{1|0} \) 和 \( \omega_{1|0} \) 是初始值。取自然对数并令 \( \boldsymbol{\beta} = (\mu, \sigma^2, \phi_1, \dots, \phi_p, \theta_1, \dots, \theta_q) $ 为待估计参数向量，待最大化的 ARMA 模型对数似然函数为

$$
L(\boldsymbol{\beta}) = -\frac{1}{2} T \ln \sigma^2 - \frac{1}{2} \sum_{t=1}^T \ln v_{t|t-1} - \frac{1}{2} \sum_{t=1}^T \frac{e^2_{t|t-1}}{\sigma^2 v_{t|t-1}}. \tag{2.104}
$$

在实践中，通过将模型置于状态空间形式并使用卡尔曼滤波器，可以对给定参数值高效计算似然函数。读者可参阅第 2.4 节讨论的算法，该算法提供了一种快速递归方法来计算 \(\omega_{t|t-1}$ 和 \(\sigma^2 v_{t|t-1}$。精确似然函数的最大化使用非线性优化算法进行（参见 Box et al., 2015; Shumway and Stoffer, 2017）。

