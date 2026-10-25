

定理 10.1.17 和定理 11.2.8 表明映射

\[ \mu = (\mu^1, \mu^2) = \varphi^{-1} \circ \lambda : (a,b) \longrightarrow U \]

是光滑的。由定理 10.1.10，

\[ v = \frac{d\lambda}{dt}(t_0) = \frac{d(\varphi \circ \mu)}{dt}(t_0) = \sum_i \frac{d\mu^i}{dt}(t_0) H_i|_q, \]

所以 $v$ 在 $H_1|_q$ 和 $H_2|_q$ 的张成空间中。因此，$T_p(M) \subseteq \operatorname{span}\{H_1|_q, H_2|_q\}$。

$(\supseteq)$：设 $(e_1,e_2)$ 为 $\mathbb{R}^2$ 的标准基。对给定的 $1 \le i \le 2$，我们定义光滑映射

\[ \zeta = (\zeta^1,\zeta^2) : (-\varepsilon,\varepsilon) \longrightarrow U \]

为 $\zeta(t)=q+t e_i$，其中 $\varepsilon>0$ 选得足够小使得 $(q-\varepsilon e_i,q+\varepsilon e_i)\subset U$。考虑光滑曲线 $\lambda(t):(-\varepsilon,\varepsilon)\longrightarrow M$ 定义为 $\lambda=\varphi\circ\zeta$，并注意到 $\lambda(0)=\varphi(q)$。由定理 10.1.10，

\[ \frac{d\lambda}{dt}(0)=\frac{d(\varphi\circ\zeta)}{dt}(0)=\sum_j \frac{d\zeta^j}{dt}(0)H_j|_q = H_i|_q, \qquad (11.3.1) \]

所以 $H_i|_q$ 在 $T_p(M)$ 中，$i=1,2$。因此，$\operatorname{span}\{H_1|_q, H_2|_q\}\subseteq T_p(M)$。

这证明了断言。我们由第 11.2 节的 [C1] 和定理 11.2.2 得到 $H_1|_q$ 和 $H_2|_q$ 线性无关。结果得证。

(c)：这由定理 11.2.1(b) 和部分 (b) 得出。

(d)：我们由部分 (c) 得到 $d_q(\varphi):\mathbb{R}^2\longrightarrow T_p(M)$ 是良定义的，且由第 11.2 节的 [C1] 得到它是单射。结果现在由部分 (a) 和定理 1.1.14 得出。

在定理 11.3.1 的记号中，令我们记

\[ (H_1, H_2) \quad \text{by} \quad \mathcal{H}, \]

和

\[ (H_1|_q, H_2|_q) \quad \text{by} \quad \mathcal{H}_q. \]

我们称 $\mathcal{H}$ 为对应于 $(U,\varphi)$ 的坐标标架。尽管有把 $T_p(M)$ 视为在点 $p$ 处“切”于 $M$ 的倾向，由定理 11.3.1(a)，$T_p(M)$ 是 $\mathbb{R}^3$ 的一个子空间。作为子空间，$T_p(M)$ 经过 $\mathbb{R}^3$ 的原点 $(0,0,0)$。在几何术语中，是 $T_p(M)+p$，即 $T_p(M)$ 由 $p$ 平移后，才是切于 $p$ 处的 $M$。话虽如此，在图中把切平面标记为 $T_p(M)$ 而非 $T_p(M)+p$ 是方便的。

## 例 11.3.2 ($\mathcal{S}^2$)。

继续例 11.2.6，考虑图表 $(D,\varphi_1)$，其中 $\varphi_1(x,y)=(x,y,\sqrt{1-x^2-y^2})$。相应的坐标标架由

\[ \left(\frac{\partial\varphi_1}{\partial x}(x,y),\frac{\partial\varphi_1}{\partial y}(x,y)\right)=\left(\left(1,0,-\frac{x}{\sqrt{1-x^2-y^2}}\right),\left(0,1,-\frac{y}{\sqrt{1-x^2-y^2}}\right)\right). \]

例如，在 $\varphi_1(0,0)=(0,0,1)$ 处，坐标基是 $((1,0,0),(0,1,0))$，它张成 $\mathbb{R}^3$ 中的 $xy$-平面。

### 定理 11.3.3 (坐标基变换)。

设 $M$ 为正则曲面，$p$ 为 $M$ 中的点，$(U,\varphi)$ 和 $(\widetilde U,\widetilde\varphi)$ 为 $p$ 处的图表，且 $\mathcal H$ 和 $\widetilde{\mathcal H}$ 为相应的坐标标架。则

$$[\operatorname{id}_{T_p(M)}]_{\mathcal H_q}^{\widetilde{\mathcal H}_{\widetilde q}}=J_{\widetilde\varphi^{-1}\circ\varphi}(q),$$

其中 $q=\varphi^{-1}(p)$ 且 $\widetilde q=\widetilde\varphi^{-1}(p)$。

**证明。** 设 $\mathcal H=(H_1,H_2)$ 且 $\widetilde{\mathcal H}=(\widetilde H_1,\widetilde H_2)$，且 $(r^1,r^2)$ 和 $(\widetilde r^1,\widetilde r^2)$ 分别为 $U$ 和 $\widetilde U$ 上的坐标。由定理 11.2.9， transition map $F=(F^1,F^2)=\widetilde\varphi^{-1}\circ\varphi$ 是微分同胚，因而光滑。由定理 10.1.10 可得

$$H_{\widetilde j\mid q}=\frac{\partial\varphi}{\partial r^j}(q)=\frac{\partial(\widetilde\varphi\circ F)}{\partial r^j}(q)=\sum_i\frac{\partial F^i}{\partial r^j}(q)\widetilde H_{i\mid\widetilde q},$$

然后由 (2.2.6)、(2.2.7) 和 (10.1.2) 可得

$$[\operatorname{id}_{T_p(M)}]_{\mathcal H_q}^{\widetilde{\mathcal H}_{\widetilde q}}=\left[\frac{\partial F^i}{\partial r^j}(q)\right]=J_{\widetilde\varphi^{-1}\circ\varphi}(q).$$

### 例 11.3.4 (极坐标)。

$\mathbb R^3$ 中的 $xy$-平面，这里记为 $\operatorname{Pln}$，显然是一个正则曲面。考虑 $\mathbb R^2$ 中的开集

$$U=\{(\rho,\phi)\in\mathbb R^2:\rho>0,\ 0<\phi<2\pi\}$$

并定义映射 $\varphi:U\to\mathbb R^3$ 为

$$\varphi(\rho,\phi)=(\rho\cos(\phi),\rho\sin(\phi),0)$$

对所有 $(\rho,\phi)$ 在 $U$ 中。$\varphi$ 的像为去掉非负 $x$-轴的 $\operatorname{Pln}$。易证 $(U,\varphi)$ 是 $\operatorname{Pln}$ 上的一个图表，我们称之为 **极坐标**。

$\operatorname{Pln}$ 上的一个覆盖图表由 $(\widetilde U=\mathbb R^2,\widetilde\varphi)$ 给出，其中 $\widetilde\varphi:\mathbb R^2\to\operatorname{Pln}$ 定义为 $\widetilde\varphi(x,y)=(x,y,0)$。对应于 $(U,\varphi)$ 和 $(\widetilde U,\widetilde\varphi)$ 的坐标标架由

$$\mathcal H_{(\rho,\phi)}=\left(\frac{\partial\varphi}{\partial\rho}(\rho,\phi),\frac{\partial\varphi}{\partial\phi}(\rho,\phi)\right)=\big((\cos(\phi),\sin(\phi),0),(-\rho\sin(\phi),\rho\cos(\phi),0)\big)$$

和

$$\widetilde{\mathcal H}_{(x,y)}=\left(\frac{\partial\widetilde\varphi}{\partial x}(x,y),\frac{\partial\widetilde\varphi}{\partial y}(x,y)\right)=((1,0,0),(0,1,0)),$$

分别给出。 transition map $\widetilde\varphi^{-1}\circ\varphi:U\to\widetilde U$ 定义为

$$\widetilde\varphi^{-1}\circ\varphi(\rho,\phi)=(\rho\cos(\phi),\rho\sin(\phi)).$$

经典地，$x$ 和 $y$ 被视为 $\rho$ 和 $\phi$ 的函数，并表示为

$$x(\rho,\phi)=\rho\cos(\phi)\qquad\text{and}\qquad y(\rho,\phi)=\rho\sin(\phi).$$

## 正则曲面中的开集。

正如我们现在所示，正则曲面中的开集本身就是一个正则曲面。

## 定理 11.4.1 (开集)。设 $M$ 为正则曲面，$V$ 为 $M$ 中的开集，并把 $V$ 视为 $\mathbb{R}^3$ 的拓扑子空间。则：

- (a) $V$ 是正则曲面。
- (b) 对所有点 $p$ 在 $V$ 中，$T_p(V) = T_p(M)$。

证明。 (a)：设 $p$ 为 $V$ 中的点，且 $(U, \varphi)$ 为 $M$ 上在 $p$ 处的图表。我们有第 11.2 节的性质 [C1]–[C3] 可用。根据 [C2]，$\varphi(U)$ 在 $M$ 中是开的，因此 $\varphi(U) \cap V$ 也是开的。由 [C3] 和定理 9.1.14 可得 $U′ = \varphi^{-1}(V \cap \varphi(U))$ 在 $U$ 中是开的。由定理 11.2.10，$(U′, \varphi|U′)$ 是在 $p$ 处的图表使得 $\varphi|U′(U′) \subseteq V$。由于 $p$ 是任意的，结果得证。
(b)：设 $v$ 为 $T_p(V)$ 中的向量，且 $\lambda(t) : (a, b) \longrightarrow V$ 为光滑曲线使得 $\lambda(t₀) = p$ 且 $(d\lambda/dt)(t₀) = v$ 对某个 $t₀$ 在 $(a, b)$ 中。 由于 $V \subseteq M$，同一光滑曲线表明 $v$ 也是 $T_p(M)$ 中的向量。因此，$T_p(V) \subseteq T_p(M)$。由定理 11.3.1(a)，$T_p(V)$ 和 $T_p(M)$ 都是 2 维的，然后定理 1.1.7(b) 给出 $T_p(V) = T_p(M)$。

## 在此之后，正则曲面中的任何开集都被视为正则曲面。

## 函数的图像。

根据定理 11.2.11，正则曲面被函数的图像所覆盖。我们现在孤立地考虑函数的图像。设 $U$ 为 $\mathbb{R}^2$ 中的开集，且 $f$ 为 $C^\infty(U)$ 中的函数。回顾定理 11.2.11，

graph(f) = { (x, y, f(x, y)) ∈ ℝ³ : (x, y) ∈ U },

其中我们把 $\mathbb{R}^2$ 识别为 $\mathbb{R}^3$ 中的 $xy$-平面。定义映射 $\varphi : U ⟶ ℝ³$ 为

φ(x, y) = (x, y, f(x, y))

对所有 $(x, y)$ 在 $U$ 中，我们看到 graph(f) 是 $\varphi$ 的像；即

graph(f) = φ(U)。

## 定理 11.4.2 (函数图像)。在上述设定下，graph(f)，视为 $\mathbb{R}^3$ 的拓扑子空间，是正则曲面，且 $(U, φ)$ 是 graph(f) 上的覆盖图表。

证明。显然，φ : U ⟶ ℝ³ 是参数化曲面。我们需要证明：

- [C1] φ : U ⟶ ℝ³ 是浸入。
- [C2] φ(U) 在 graph(f) 中是开集。
- [C3] φ : U ⟶ φ(U) 是同胚。

证明如下：

[C1]：雅可比矩阵为

| 1 | 0 |
| 0 | 1 |
| ∂f/∂x (x,y) | ∂f/∂y (x,y) |

相应的向量积为

```
(1,0,∂f/∂x(x,y)) × (1,0,∂f/∂y(x,y)) = (-∂f/∂x(x,y), -∂f/∂y(x,y),1)
```

它永不等于 $(0,0,0)$。由定理 11.2.2，φ 是浸入。

[C2]：这由 φ(U) = graph(f) 得出。

[C3]：显然，φ 是双射的。由于 φ 是光滑的，由定理 10.1.7 它是连续的。设 P: ℝ³ ⟶ ℝ² 为投影映射定义为 P(x,y,z) = (x,y)，其中我们把 ℝ² 识别为 ℝ³ 中的 $xy$-平面。由于 P 是连续的且 φ⁻¹|graph(f) = P|graph(f)，结果得证。

因此，$(U,φ)$ 是 graph(f) 上的图表，且由于 φ(U) = graph(f)，它是覆盖图表。

## 旋转曲面。

设 ρ(t), h(t) : (a,b) ⟶ ℝ 为光滑函数使得：

- [R1] ρ 在 $(a,b)$ 上严格为正。
- [R2] h 在 $(a,b)$ 上严格递增或严格递减。

我们称 ρ 和 h 分别为半径函数和高度函数。在此之后，方便地用上点表示 ρ 和 h 关于 $t$ 的导数。考虑光滑曲线 σ(t) : (a,b) ⟶ ℝ³ 定义为

```
σ(t) = (ρ(t),0,h(t))
```

对所有 $t$ 在 $(a,b)$ 中。我们观察到 [R2] 等价于 ḣ 在 $(a,b)$ 上严格为正或严格为负，由此可得 σ 是正则曲线。设

```
U = (a,b) × (-π,π)
```

并考虑光滑映射 φ : U ⟶ ℝ³ 定义为

```
φ(t,φ) = (ρ(t)cos(φ), ρ(t)sin(φ), h(t))
```

对应于 σ 的旋转曲面记为 rev(σ) 并定义为 φ 的像：

```
rev(σ) = φ(U)
```

因此，rev(σ) 是通过绕 $z$-轴旋转 σ 的像得到的。注意在定义 $U$ 时选择了 $(-π,π)$ 而非例如 $[-π,π)$ 或 $[0,2π)$，以确保 $U$ 是 $\mathbb{R}^2$ 中的开集。因此，旋转曲面并不完全绕 $z$-轴转一圈。

