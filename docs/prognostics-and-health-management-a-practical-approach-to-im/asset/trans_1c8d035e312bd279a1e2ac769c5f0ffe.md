

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_85_0.png)

**图 1.36** 目标函数的形状。

而期望的循环长度表示为 $TR(T)+\int_0^T tf(t)\,dt$，可重写为 $\int_0^T R(t)\,dt$。如果相同的循环无限重复，则基于更新理论，每单位时间的期望成本可表示为

$$\frac{c_pR(T)+c_fF(T)}{\int_0^T R(t)\,dt}$$  (1.123)

其被最小化（Barlow and Proschan 1965; Aven and Jensen 1999）。在许多情况下——例如在快速变化的技术中——最优解必须基于单周期准则获得。每单位时间的成本表示为

$$\begin{cases}\dfrac{c_f}{X}, & \text{if } X\le T\\[4pt]\dfrac{c_p}{T}, & \text{if } X\ge T\end{cases}$$

其期望值为

$$\frac{c_p}{T}R(T)+\int_0^T\frac{c_f}{t}f(t)\,dt$$  (1.124)

然后对其进行最小化。在几种分布类型的情况下，最优解可以很容易地获得。

首先考虑式 (1.123) 中的模型。其导数与以下表达式符号相同

$$(-c_pf(T)+c_ff(T))\int_0^T R(t)\,dt-(c_pR(T)+c_fF(T))R(T)$$

或

$$(c_f-c_p)\left\{\int_0^T R(t)\,dt\,p(T)+R(T)\right\}-c_f$$  (1.125)

括号内表达式的导数为

```
R(T)ρ(T) + ∫₀ᵀ R(t)dt ρ'(T) − f(T) = ρ'(T) ∫₀ᵀ R(t)dt > 0
```

若 ρ(T) 为递增。于是式 (1.125) 为递增函数，且其在 T = 0 时的值为 −c_p < 0。当 T → ∞ 时，若 ρ(T) → ∞，则其收敛于无穷；否则若

```
(c_f − c_p) ∫₀^∞ R(t)dt ρ(∞) − c_f > 0
```

则收敛于正的极限。因此，在这些情况下存在唯一的 T̄ > 0 使式 (1.125) 为 0，且此即为唯一最优解。如果式 (1.125) 在无穷处的极限非正，则不存在有限最优解，设备必须在失效时更换。

式 (1.124) 的导数形式为

```
− c_p/T² R(T) − c_p/T f(T) + c_f/T f(T)
```

其符号与

```
(c_f − c_p)ρ(T) − c_p/T   (1.126)
```

相同。若 ρ(T) 递增，则该函数也递增；在 T = 0 时，其值收敛于 −∞；而当 T → ∞ 时，其极限为正。因此，同样存在唯一的解即唯一优化器。单调方程的解可通过标准计算机方法获得（Yakowitz and Szidarovszky 1989）。

## 示例 1.22  假设威布尔分布参数为 β = 1.5 和 η = 5，且 c_f = 200 和 c_p = 100。我们先前已证明威布尔分布的失效率为

```
ρ(t) = β/η^β t^{β−1}
```

且由式 (1.126) 给出的<|message|> 一阶条件可写为

```
(c_f − c_p) β/η^β T^{β−1} − c_p/T = 0
```

意味着

```
T = { c_p η^β / [β(c_f − c_p)] }^{1/β}
```

在此情况下

```
T = { 100(5)^{1.5} / [1.5(100)] }^{1/1.5} ≈ 3.816
```

### 1.8.5 模型变体与扩展

这些模型存在许多变体和扩展。其中一些将简要介绍。

首先假设预防性更换需要 $T_p$ 时间进行，而 $T_f$ 对失效更换相同。若我们考虑从更换开始运行的周期，则 $T_p$ 或 $T_f$ 必须计入循环长度。式 (1.123) 修改为

$$\frac{c_pR(T)+c_fF(T)}{T_pR(T)+T_fF(T)+\int_0^T R(t)\,dt} \tag{1.127}$$

而式 (1.124) 变为

$$\frac{c_p}{T+T_p}R(T)+\int_0^T\frac{c_f}{t+T_f}f(t)\,dt \tag{1.128}$$

这些简单模型未考虑任何额外因素：可修复失效的维修成本、机器生产的产品产生的收益、更换后的残值、备件到达的最优时间，以及执行维修和更换所需的时间。为开发更先进的模型，我们引入以下记号：

- $c_r$ = 每次维修的成本  
- $M(T)$ = 区间 $[0,T]$ 内可修复失效的期望次数  
- $u(T)$ = 区间 $[0,T]$ 内产生的收益  
- $S(T)$ = 设备使用寿命为 $T$ 后的残值  
- $\alpha$ = 备件单位库存成本  
- $\beta$ = 生产延误单位损失

还必须包含一个新的决策变量：

$W$ = 备件到达计划

为数学简便起见，我们引入以下函数。若设备需在时间 $t$ 安排更换，且备件在时间 $W$ 到达，则额外成本为

$$\gamma(t,W)=\begin{cases}(t-W)\alpha & \text{if } t\ge W \\(W-t)\beta & \text{if } t<W\end{cases}$$

第一种情况下产生库存成本；第二种情况下生产延误导致公司损失。

如果考虑长期期望成本，则式 (1.124) 推广为

$$\frac{(c_rM(T)+c_p-S(T)-u(T)+\gamma(T,W))R(T)+\int_0^T(c_rM(t)+c_p-S(t)-u(t)+\gamma(t,W))f(t)\,dt}{T_pR(T)+T_fF(T)+\int_0^T R(t)\,dt} \tag{1.129}$$

相应的单周期模型扩展为更复杂的目标函数：

$$\frac{c_p+c_rM(T)-S(T)-u(T)+\gamma(T,W)}{T+T_p}R(T)+\int_0^t\frac{c_f+c_rM(t)-S(t)-u(t)+\gamma(t,W)}{t+T_f}f(t)\,dt$$  (1.130)

两个模型均有两个决策变量，因此对其优化，商业软件是最好的选择。一种复杂的分析方法如下。先给定 $T$ 的值，对式 (1.129) 和 (1.130) 关于 $W$ 进行优化。显然，最优 $W$ 值取决于 $T$：$W=W(T)$。然后将 $W(T)$ 代入目标函数，此时它们仅依赖于 $T$，从而成为简单的单变量搜索算法，可用于寻找最优解。

如果同时考虑短期和长期目标，则必须使用多目标优化方法对这两个目标函数进行处理。

我们还可以在式 (1.121) 和 (1.122) 中纳入不可修复失效的可能性。在这种情况下，必须使用时间 $T$ 之后的 TTF 的条件 CDF，可按如下方式获得：

$$P(X<t\mid X>T)=\frac{P(T<X<t)}{P(X>T)}=\frac{F(t)-F(T)}{1-F(T)}$$  (1.131)

请注意，这些目标表示每单位时间的期望成本，而未考虑其不确定性，后者由每单位时间成本的方差表征。期望成本及其方差都应最小化，这再次导致应用多目标优化程序。

最后引入一个简单的模型，用于在设备生命周期中的任意区间内确定所需的维修包数量。类似的模型也可用于规划所需劳动力数量。为简化起见，假设在可修复失效情况下执行最小维修。据第 1.5.1 节可知，任意区间 $[t,T]$ 内的失效次数服从期望为

$$\lambda=M(T)-M(t)=\int_t^T\rho(\tau)\,d\tau$$  (1.132)

的泊松分布，意味着失效时间构成非齐次泊松过程。假设每次维修需要一个维修包单位。确定能保证所有失效维修均能以至少给定概率值 $1-p$ 完成的单位数是一个非常重要的问题。设 $Q$ 表示所需的可用包数；则在记号

$$X_{[t,T]}=\text{区间 }[t,T]\text{ 内实际失效次数}$$

下，我们要求

$$P(X_{[t,T]}\le Q)=\sum_{k=0}^{Q}\frac{\lambda^k}{k!}e^{-\lambda}=e^{-\lambda}\sum_{k=0}^{Q}\frac{\lambda^k}{k!}\ge 1-p$$

意味着

$$\sum_{k=0}^{Q}\frac{\lambda^k}{k!}\ge (1-p)e^{\lambda}$$  (1.133)

Cox, D. (1970). Renewal Theory. London: Methuen and Co.
Elsayed, E.A. (2012). Reliability Engineering. Hoboken, NJ: Wiley.
Eyring, H. (1935). The activated complex in chemical reactions. The Journal of Chemical Physics 3 (2): 107–115.
Finkelstein, M. (2008). Failure Rate Modeling for Reliability and Risk. London: Springer.
Frieden, B.R. (2004). Science from Fisher Information: A Unification. Cambridge, UK: Cambridge University Press.
Hamidi, M., Matsumoto, A., and Szidarovszky, F. (2017). Optimal schedule of repair or replacement after degradation is noticed. 46th Annual Meeting of the Western Decision Science Institute, Vancouver, Canada, 4–8 April.
Hamilton, J. (1994). Time Series Analysis. Princeton, NJ: Princeton University Press.
Hofmeister, J., Goodman, D., and Wagoner, R. (2016). Advanced anomaly detection method for condition monitoring of complex equipment and systems. 2016 Machine Failure Prevention Technology, Dayton, Ohio, US, 24–26 May.
Hofmeister, J., Goodman, D., and Szidarovszky, F. (2017a). PHM/IVHM: checkpoint, restart, and other considerations. 2017 Machine Failure Prevention Technology, Virginia Beach, Virginia, US, 15–18 May.
Hofmeister, J., Szidarovszky, F., and Goodman, D. (2017b). An approach to processing condition-based data for use in prognostic algorithms. 2017 Machine Failure Prevention Technology, Virginia Beach, Virginia, US, 15–18 May.
IEEE. (2017). Draft standard framework for prognosis and health management (PHM) of electronic systems. IEEE 1856/D33.
Jardine, A. (2006). Optimizing maintenance and replacement decisions. Annual Reliability and Maintainability Symposium (RAMS), Newport Beach, California, US, 23–26 January.
Kececioglu, D. and Jacks, J.A. (1984). The Arrhenius, Eyring, inverse power law and combination models in accelerated life testing. Reliability Engineering 8 (1): 1–9.
Kumar, S. and Pecht, M. (2010). Modeling approaches for prognostics and health management of electronics. International Journal of Performability Engineering 6 (5): 467–476.
Medjaher, K. and Zerhouni, N. (2013). Framework for a hybrid prognostics. Chemical Engineering Transactions 33: 91–96. https://doi.org/10.3303/CET1333016.
Milton, J.S. and Arnold, J.C. (2003). Introduction to Probability and Statistics. Boston, MA: McGraw Hill.
Mitov, K.V. and Omey, E. (2014). Renewal Processes. New York: Springer.
Nakagawa, T. (2006). Maintenance Theory and Reliability. Berlin/Tokyo: Springer.
Nakagawa, T. (2008). Advanced Reliability Models and Maintenance Policies. London: Springer.
National Science Foundation Center for Advanced Vehicle and Extreme Environment Electronics at Auburn University (CAVE3). (2015). Prognostics health management for electronics. http://cave.auburn.edu/rsrch-thrusts/prognostic-health-management-for-electronics.html (accessed November 2015).
Nelson, W. (1980). Accelerated life testing – step stress models and data analysis. IEEE Transactions on Reliability R-29 (2): 103–108.
Nelson, W. (2004). Accelerated Testing: Statistical Models, Test Plans, and Data Analysis. New York: Wiley.
O’Connor, P. and Kleyner, A. (2012). Practical Reliability Engineering. Chichester: Wiley.

