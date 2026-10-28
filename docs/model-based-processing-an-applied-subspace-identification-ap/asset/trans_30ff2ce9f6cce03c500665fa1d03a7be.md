
且对数似然比为

```
Λ(T_m; Θ) = Λ(T_{m-1}; Θ) + ln Pr[τ_m, Θ(τ_m) | T_{m-1}; H_1] - ln Pr[τ_m | T_{m-1}; Θ; H_0] (8.65)
```

其中，第二项可通过应用贝叶斯法则进一步展开，得到

```
Pr[τ_m, Θ(τ_m) | T_{m-1}; H_1] = Pr[τ_m | T_{m-1}, Θ(τ_m); H_1] × Pr[Θ(τ_m) | T_{m-1}; H_1] (8.66)
```

将该表达式代入式(8.59)的源分布中，得到

```
Pr[T_S(m; Θ_S) | T_{m-1}, \hat{Θ}_S; H_1] → Pr[τ_m | T_{m-1}, Θ(τ_m); H_1] × Pr[Θ(τ_m) | T_{m-1}; H_1] (8.67)
```

考虑到这一点，通过将规定的分布代入式(8.65)，可以直接计算出序贯对数似然，得到

```
Λ(T_m; Θ) = Λ(T_{m-1}; Θ) + ln( R_1 r_0 η_0 + (F_S / R_1) Σ_{n=2}^∞ e_n(ε) ( Σ_{k=1}^{n-1} k e^{-k λ τ_m} ) λ b_0(τ_m) + (2π)^{-N_θ/2} |P_0 + R_{w_θ w_θ}|^{-1/2} × exp{ -1/2 (Θ - Θ_0)^T (P_0 + R_{t_θ w_θ})^{-1} (Θ - Θ_0) } + 1/√(2π σ_{vv}^2) exp{ -1/2 T_V^2(m)/σ_{vv}^2 } ) + 1/2 ln(2π σ_{vv}^2) - 1/2 T_V^2(n)/σ_{vv}^2 (8.68)
```

这完成了裂变过程序贯贝叶斯检测方法的推导。接下来，我们必须更详细地考虑参数估计问题。

## 8.3.6 贝叶斯参数估计

为了实现8.3.5节的广义似然比检验（GLRT），我们必须在每次到达时估计未知参数Θ。我们首先推导批处理方案，然后推导其序贯版本，类似于上一节的序贯贝叶斯检测器。这里我们推导适用于以下问题的贝叶斯参数估计器：

给定一组不确定的多重性计数器（到达间隔时间）测量值T_M；找出未知裂变源参数Θ的“最佳”估计\hat{Θ}。

从统计学角度来看，我们希望估计在给定整个到达间隔数据集T_M条件下源参数Θ的后验分布，即Pr[Θ|T_M]。应用贝叶斯定理，我们有

```
Pr[Θ|T_M] = Pr[T_M|Θ] × Pr[Θ] / Pr[T_M]    (8.69)
```

由于问题的序贯性，即中子多重性计数器逐个中子地测量每个中子到达时间，我们需要一个序贯版本。

## 8.3.7 序贯贝叶斯处理器

可以证明[39]，可以为后验分布推导出序贯贝叶斯解。从式(8.69)的第一项开始，应用贝叶斯法则，我们有

```
Pr[T_M|Θ] = Pr[τ_m, T_{M-1}|Θ] = Pr[τ_m|T_{M-1}, Θ] × Pr[T_{M-1}|Θ]    (8.70)
```

对于分母项，我们有

```
Pr[T_M] = Pr[τ_m, T_{M-1}] = Pr[τ_m|T_{M-1}] × Pr[T_{M-1}]    (8.71)
```

将式(8.70)和(8.71)代入式(8.69)并分组各项，我们得到

```
Pr[Θ|T_M] = ( Pr[τ_m|T_{M-1}, Θ] / Pr[τ_m|T_{M-1}] )_{W(τ_m)} × ( Pr[T_{M-1}|Θ] × Pr[Θ] / Pr[T_{M-1}] )_{Pr[Θ|T_{M-1}]}    (8.72)
```

或

```
Pr[Θ|T_M] = W(τ_m) × Pr[Θ|T_{M-1}]    (8.73)
```

这就是后验分布的贝叶斯序贯形式。如果我们进一步假设到达间隔具有马尔可夫性，即当前到达仅依赖于前一次到达，即 (τ_m, T_{M-1}) → (τ_m, τ_{m-1})，则我们得到所需的序贯传播后验的表达式：

```
Pr[Θ|τ_m] = W(τ_m) × Pr[Θ|τ_{m-1}]    (8.74)
```

其中

```
W(τ_m) = Pr[τ_m|τ_{m-1}, Θ] / Pr[τ_m|τ_{m-1}]    (8.75)
```

这里我们假设参数向量是一个没有关联动态的随机常数Θ，以构建序贯贝叶斯处理器。然而，在实际情况中，显然当测量来自未知源的中子到达间隔时，每个参数都可能很容易出现变化或不确定性。也许对于这些参数变化，更合理的模型是8.3.6节[39]中引入的随机游走/常数模型。

也就是说，我们知道在连续时间下，游走由 $\frac{d}{dt}\Theta(\tau)=w_{\Theta}(\tau)$ 给出，通过取一阶差分来近似导数，我们可以得到采样数据表示[39]：

$$\Theta(\tau_m)=\Theta(\tau_{m-1})+\Delta\tau_m\ w_{\Theta}(\tau_{m-1})\tag{8.76}$$

其中$\Delta\tau_m := \tau_m - \tau_{m-1}$，用于包含在$\Theta(\tau_m) \sim \mathcal{N}(m_{\Theta}(\tau_m), R_{\Theta\Theta}(\tau_m)+R_{w_{\Theta}w_{\Theta}}(\tau_m))$中的参数不确定性，因为$w_{\Theta} \sim \mathcal{N}(0,R_{w_{\Theta}w_{\Theta}}(\tau_m))$。每个$\Theta$参数的变化可以通过其初始猜测$\Theta(\tau_0)$和方差$R_{\Theta\Theta}(\tau_m)+R_{w_{\Theta}w_{\Theta}}(\tau_m)$来控制。

物理上，我们有 $R_{w_{\Theta}w_{\Theta}}=\operatorname{diag}[\sigma_{MM}^2\ \sigma_{k_{eff}k_{eff}}^2\ \sigma_{ee}^2\ \sigma_{\lambda^{-1}\chi^{-1}}^2]$，后续搜索可以为每个初始值 $\Theta_0=\mathcal{U}[a,b]$ 包含一个有界均匀变量，从而能够以更实用的方式对参数集及其各自伴随的不确定性进行建模。随机游走/常数模型的统计量由序贯高斯-马尔可夫结构给出：

$$\begin{aligned}
\Theta(\tau_m)&=\Theta(\tau_{m-1})+\Delta\tau_m\ w_{\Theta}(\tau_{m-1})\ \text{(State)}\\
m_{\Theta}(\tau_m)&=m_{\Theta}(\tau_{m-1})\qquad\text{(Mean)}\\
R_{\Theta\Theta}(\tau_m)&=R_{\Theta\Theta}(\tau_{m-1})+R_{w_{\Theta}w_{\Theta}}(\tau_{m-1})\ \text{(Variance)}
\end{aligned}\tag{8.77}$$

考虑到这个模型，我们像之前一样从批处理方法开始重新推导序贯贝叶斯处理器。我们希望估计$\Theta(\tau_m)$，完整参数集定义为$\Theta_M := \{\Theta(\tau_0),\Theta(\tau_1),\dots,\Theta(\tau_M)\}$。批处理后验由贝叶斯定理给出，如同之前一样：

$$\Pr[\Theta_M|T_M]=\frac{\Pr[T_M|\Theta_M]\times\Pr[\Theta_M]}{\Pr[T_M]}\tag{8.78}$$

第一项可以通过应用贝叶斯法则分解为

$$\begin{aligned}
\Pr[T_M|\Theta_M]&=\Pr[\tau_m,T_{M-1}|\Theta(\tau_m),\Theta_{M-1}]\\&=\Pr[\tau_m|T_{M-1},\Theta(\tau_m),\Theta_{M-1}]\\&\quad\times\Pr[T_{M-1}|\Theta(\tau_m),\Theta_{M-1}]
\end{aligned}\tag{8.79}$$

到达间隔时间$\tau_m$独立于$\Theta_{M-1}$和$T_{M-1}$，因此式(8.79)中的第一项变为$\Pr[\tau_m|\Theta(\tau_m)]$，而第二项简化为$\Pr[\tau_{M-1}|\Theta_{M-1}]$，因为参数向量$\Theta(\tau_m)$被假设与过去的测量数据无关。因此，我们有

$$\Pr[T_M|\Theta_M]=\Pr[\tau_m|\Theta(\tau_m)]\times\Pr[T_{M-1}|\Theta_{M-1}]\tag{8.80}$$

式(8.78)的第二项可以类似地分解为

$$\Pr[\Theta_M]=\Pr[\Theta(\tau_m),\Theta_{M-1}]=\Pr[\Theta(\tau_m)|\Theta_{M-1}]\times\Pr[\Theta_{M-1}]\tag{8.81}$$

分母中$\Pr[T_M]$的分解由式(8.70)给出。

将这些关系式（式(8.70)、(8.80)和(8.81)）代入式(8.78)，并像之前一样假设马尔可夫过程，我们得到

$$\Pr[\Theta_M\mid T_M]=\bigl(\Pr[\tau_m\mid\Theta(\tau_m)]\times\Pr[T_{M-1}\mid\Theta_{M-1}]\bigr)\\\times\frac{\Pr[\Theta(\tau_m)\mid\Theta(\tau_{m-1})]\times\Pr[\Theta_{M-1}]}{\Pr[\tau_m\mid\tau_{m-1}]\times\Pr[T_{M-1}]}$$

(8.82)

现在对各项进行分组，所需的后验分布变为

$$\Pr[\Theta_M\mid T_M]=\left(\frac{\Pr[\tau_m\mid\Theta(\tau_m)]\times\Pr[\Theta(\tau_m)\mid\Theta(\tau_{m-1})]}{\Pr[\tau_m\mid\tau_{m-1}]}\right)\\\times\left(\frac{\Pr[T_{M-1}\mid\Theta_{M-1}]\times\Pr[\Theta_{M-1}]}{\Pr[T_{M-1}]}\right)$$

(8.83)

或简写为（将$\tau_m\to T_M$替换）

$$\Pr[\Theta_m\mid T_m]=W(\tau_m)\times\Pr[\Theta_{m-1}\mid T_{m-1}]$$

(8.84)

其中

$$W(\tau_m):=\frac{\Pr[\tau_m\mid\Theta(\tau_m)]\times\Pr[\Theta(\tau_m)\mid\Theta(\tau_{m-1})]}{\Pr[\tau_m\mid\tau_{m-1}]}$$

(8.85)

这是动态参数模型（随机游走/常数）的序贯贝叶斯解。粒子滤波器是这种递归的一种实现[39, 40]。

## 8.3.8 裂变过程的粒子滤波器

粒子滤波是一种从数据统计采样的“重要性采样”方法发展而来的技术。其关键思想是从最高概率区域，即最高重要性区域中选择粒子（或我们问题中的参数）。一旦确定了由此产生的重要性权重，所需的后验分布由非参数概率质量函数（PMF）近似为：

$$\hat{\Pr}[\Theta_M\mid T_M]=\sum_i w_i(\tau_m)\,\delta(\Theta(\tau_m)-\Theta_i(\tau_m))$$

(8.86)

其中$\mathcal{W}_i$是归一化加权函数，由到达间隔时间$\tau_m$处的后验分布与设计的重要性分布$q$之比给出：

$$W_i(\tau_m):=\frac{\Pr[\Theta(\tau_m)\mid T_m]}{q[\Theta(\tau_m)\mid T_m]}=\frac{\Pr[\tau_m\mid\Theta(\tau_m)]}{q[\tau_m\mid\Theta(\tau_m)]}\\\times\frac{\Pr[\Theta(\tau_m)]}{q[\Theta(\tau_m)\mid\Theta_{m-1},\tau_m]}$$

(8.87)

