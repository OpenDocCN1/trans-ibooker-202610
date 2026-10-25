

## 18.10 正则坐标

设 $(M,\nabla)$ 为带有联络的光滑 $m$-流形，$p$ 为 $M$ 中的一点，$(\widetilde{e}_1,\dots,\widetilde{e}_m)$ 为 $T_p(M)$ 的一个基。映射

$\ell_p : \mathbb{R}^m \longrightarrow T_p(M)$

定义为

$\ell_p(r^1,\dots,r^m)=\sum_i r^i\widetilde{e}_i \qquad (18.10.1)$

是线性同构。设 $V$ 为 $p$ 在 $M$ 中的一个正规邻域，$\exp_p : \widetilde{V} \longrightarrow V$ 为相应的微分同胚，并考虑光滑映射

$\ell_p^{-1}\circ\exp_p^{-1} : V \longrightarrow \ell_p^{-1}(\widetilde{V})$

则

$(V,\ell_p^{-1}\circ\exp_p^{-1}=(x^1,\dots,x^m)) \qquad (18.10.2)$

是在 $p$ 处的坐标系，称为 $p$ 处的**正规坐标**。设 $\mathcal{X}_p=((\partial/\partial x^1)|_p,\dots,(\partial/\partial x^m)|_p)$ 和 $(d_p(x^1),\dots,d_p(x^m))$ 为 $p$ 处相应的坐标基和对偶坐标基。

### 定理 18.10.1. 在上述设定下：

(a)

$\widetilde{e}_i = \left.\frac{\partial}{\partial x^i}\right|_p$

对 $i=1,\dots,m$。

(b)

$x^i\circ\exp_p = d_p(x^i)$

对 $i=1,\dots,m$。

(c)

$\exp_p^{-1}(q)=\sum_i x^i(q)\left.\frac{\partial}{\partial x^i}\right|_p$

对所有 $q$ 在 $V$ 中。

证明. (a): 设 $\mathcal{E}=(e_1,\dots,e_m)$ 为 $\mathbb{R}^m$ 的标准基，并设 $\varphi=\ell_p^{-1}\circ\exp_p^{-1}$。我们有

$d_{\varphi(p)}(\varphi^{-1})=d_{\varphi(p)}(\exp_p\circ\ell_p)=d_{\ell_p(\varphi(p))}(\exp_p)\circ d_{\varphi(p)}(\ell_p)\quad[\text{Th 14.4.5}] =d_0(\exp_p\circ\ell_p)\quad[\text{Th 14.4.4}]=\ell_p.\quad[\text{Th 18.9.4}]$

则定理 14.8.5(d) 和 (18.10.1) 给出

$\left.\frac{\partial}{\partial x^i}\right|_p = \ell_p(e_i)=\widetilde{e}_i.$

对 i, j = 1, ..., m。注意 $\Gamma^k_{ij}(p)$ 与 $(r^1, ..., r^m)$ 的选取无关。令

```
(r^1, ..., r^i, ..., r^m) = (0, ..., 1, ..., 0)
```

得到

```
Γ^k_ii(p) = 0    (18.10.4)
```

对 $i = 1, ..., m$；并令

```
(r^1, ..., r^i, ..., r^j, ..., r^m) = (0, ..., 1, ..., 1, ..., 0)
```

得到

```
0 = Γ^k_ii(p) + Γ^k_ij(p) + Γ^k_ji(p) + Γ^k_jj(p) = 2Γ^k_ij(p)
```

对 $i \neq j = 1, ..., m$，其中最后的等式由定理 18.6.3(b) 和 (18.10.4) 推出。

## 18.11 Jacobi 场

从直观上看，似乎测地线从光滑流形上给定点辐射的方式，以及它们扩散的快慢，应该与该点处光滑流形的“曲率”有关。这一推理线索导致了对 Jacobi 场的考虑。

设 $(M, \nabla)$ 为带有联络的光滑流形，并设 $\gamma(r) : (a, b) \longrightarrow M$ 为一条测地线。我们说向量场 $J$ 在 $\mathfrak{X}_M(\gamma)$ 中是 Jacobi 场，如果它满足 Jacobi 方程：

```
R(J, dγ/dr) dγ/dr + ∇²J/dr² = 0,    (18.11.1)
```

其中我们记

```
∇/dr ∘ ∇/dr  by  ∇²/dr².
```

由定理 18.7.1(a)，Jacobi 方程等价于

```
R(dγ/dr, J) dγ/dr = ∇²J/dr².
```

### 定理 18.11.1 (Jacobi 场的存在性)。设 $(M, \nabla)$ 为带有联络的光滑流形，$p$ 为 $M$ 中的一点。设 $\gamma(r) : (a, b) \longrightarrow M$ 为一条测地线，且 $\gamma(0) = p$，并设 $v, w$ 为 $T_p(M)$ 中的向量。则存在唯一的 Jacobi 场 $J$ 在 $\mathfrak{X}_M(\gamma)$ 中使得 $J(0) = v$ 且 $(\nabla J/\partial r)(0) = w$。

证明. 可以证明 Jacobi 方程等价于常微分方程组。结果现在由微分方程理论得出。

设 $(M, \nabla)$ 为带有联络的光滑流形，并设

$\sigma(r,s): (a,b) \times (-\varepsilon,\varepsilon) \longrightarrow M$

为一个参数化曲面。回顾第 14.9 节，对于 $(-\varepsilon,\varepsilon)$ 中的给定点 $s$，相应的经线曲线 $\sigma_s(r): (a,b) \longrightarrow M$ 定义为 $\sigma_s(r) = \sigma(r,s)$。设 $\gamma(r): (a,b) \longrightarrow M$ 为一条测地线。我们说 $\sigma$ 是 $\gamma$ 的**测地线变分**，如果 $\sigma_0 = \gamma$ 且每个 $\sigma_s$ 都是测地线。

假设 $\sigma$ 确实是 $\gamma$ 的测地线变分。则

$\frac{d\gamma}{dr}(r) = \frac{\partial \sigma}{\partial r}(r,0)$

(18.11.2)

对所有 $r$ 在 $(a,b)$ 中。$\sigma$ 的**变分场**是向量场 $J$ 在 $\mathfrak{X}_M(\gamma)$ 中定义为

$J(r) = \frac{\partial \sigma}{\partial s}(r,0)$

(18.11.3)

对所有 $r$ 在 $(a,b)$ 中。由于 $\sigma_s$ 是测地线且 $\partial \sigma / \partial r$ 是其速度，我们有

$\frac{\nabla}{\partial r}\left(\frac{\partial \sigma}{\partial r}\right)(r,s) = 0$

(18.11.4)

对所有 $(r,s)$ 在 $(a,b) \times (-\varepsilon,\varepsilon)$ 中。见图 18.11.1。

![](img/d712710a8eb78c0959204afd5dde4c91_525_0.png)

**图 18.11.1. 测地线变分**

### 例 18.11.2 ($\mathcal{S}^2$)。在第 13.4 节的记号下，考虑参数化曲面

$\varphi(\theta,\phi): (0,\pi) \times (-\pi,\pi) \longrightarrow \mathcal{S}^2$

给定为

$\varphi(\theta,\phi)=(\sin(\theta)\cos(\phi),\sin(\theta)\sin(\phi),\cos(\theta)).$

光滑曲线

$\gamma(\theta)=\varphi(\theta,0)=(\sin(\theta),0,\cos(\theta)):(0,\pi)\longrightarrow S^2$

的像是原点为中心的 $xz$-平面内的单位圆的一部分，这是 $S^2$ 的一个大圆。由例 18.8.10 可知 $\gamma$ 是测地线。因此，$\varphi$ 是 $\gamma$ 的测地线变分。

### 定理 18.11.3.

如果 $(M,\nabla)$ 是带有对称联络的光滑流形且 $\gamma$ 是测地线，则任意测地线变分的变分场是 $\mathcal{X}_M(\gamma)$ 中的 Jacobi 场。

*证明.* 设 $\sigma(r,s):(a,b)\times(-\varepsilon,\varepsilon)\longrightarrow M$ 为 $\gamma$ 的测地线变分，并设 $J$ 为其变分场。则

```
$$\begin{aligned}
0&=\frac{\nabla}{\partial s}\left(\frac{\nabla}{\partial r}\left(\frac{\partial\sigma}{\partial r}\right)\right)\\
&=\frac{\nabla}{\partial r}\left(\frac{\nabla}{\partial s}\left(\frac{\partial\sigma}{\partial r}\right)\right)+R\left(\frac{\partial\sigma}{\partial s},\frac{\partial\sigma}{\partial r}\right)\frac{\partial\sigma}{\partial r}\\
&=\frac{\nabla}{\partial r}\left(\frac{\nabla}{\partial r}\left(\frac{\partial\sigma}{\partial s}\right)\right)+R\left(\frac{\partial\sigma}{\partial s},\frac{\partial\sigma}{\partial r}\right)\frac{\partial\sigma}{\partial r}\\
&=\frac{\nabla^2}{\partial r^2}\left(\frac{\partial\sigma}{\partial s}\right)+R\left(\frac{\partial\sigma}{\partial s},\frac{\partial\sigma}{\partial r}\right)\frac{\partial\sigma}{\partial r}.
\end{aligned}$$
```

令 $s=0$ 并利用 (18.11.2) 和 (18.11.3) 得到 (18.11.1)。

### 定理 18.11.4.

设 $(M,\nabla)$ 为带有联络的光滑流形，$p$ 为 $M$ 中的一点，并设 $\psi(s):(-\varepsilon,\varepsilon)\longrightarrow T_p(M)$ 为光滑曲线。考虑参数化曲面 $\sigma(r,s):[0,1]\times(-\varepsilon,\varepsilon)\longrightarrow M$ 给定为

$$\sigma(r,s)=\exp_p(r\psi(s)),$$

其中假设 $r\psi(s)$ 对所有 $(r,s)$ 在 $[0,1]\times(-\varepsilon,\varepsilon)$ 中属于 $\mathcal{E}_p$。则：

- (a) 对 $(-\varepsilon,\varepsilon)$ 中的每个点 $s$，$\sigma_s$ 是以 $p$ 为起点、初速度为 $\psi(s)$ 的径向测地线 $\rho_{p,\psi(s)}$。
- (b) $\sigma$ 是 $\sigma_0$ 的测地线变分。
- (c) $$\frac{\partial\sigma}{\partial r}(r,s)=d_{r\psi(s)}(\exp_p)(\psi(s)).$$
- (d) $$\frac{\partial\sigma}{\partial s}(r,s)=r\,d_{r\psi(s)}(\exp_p)\left(\frac{d\psi}{ds}(s)\right).$$

**证明.** (a): 由定理 18.9.3，

\[
\sigma_s(r)=\sigma(r,s)=\exp_p(r\psi(s))=\rho_{p,\psi(s)}(r)
\]

对所有 \(r\) 在 \([0,1]\) 中。

(b): 这由 (a) 部分得出。

(c), (d): 我们定义光滑映射

\[
\widetilde{\sigma}(r,s):[0,1]\times(-\varepsilon,\varepsilon)\longrightarrow T_p(M)
\]

为

\[
\widetilde{\sigma}(r,s)=r\psi(s),
\]

使得 $\sigma=\exp_p\circ\widetilde{\sigma}$。则

\[
\frac{\partial\widetilde{\sigma}}{\partial r}(r,s)=\psi(s)
\qquad\text{and}\qquad
\frac{\partial\widetilde{\sigma}}{\partial s}(r,s)=r\frac{d\psi}{ds}(s).
\]

故

\[
\frac{\partial\sigma}{\partial r}(r,s)
=\frac{\partial(\exp_p\circ\widetilde{\sigma})}{\partial r}(r,s)
=d_{\sigma(r,s)}(\exp_p)\left(\frac{\partial\widetilde{\sigma}}{\partial r}(r,s)\right)
=d_{r\psi(s)}(\exp_p)(\psi(s)).
\]

类似地，

\[
\frac{\partial\sigma}{\partial s}(r,s)
=\frac{\partial(\exp_p\circ\widetilde{\sigma})}{\partial s}(r,s)
=d_{\sigma(r,s)}(\exp_p)\left(\frac{\partial\widetilde{\sigma}}{\partial s}(r,s)\right)
=d_{r\psi(s)}(\exp_p)\left(r\frac{d\psi}{ds}(s)\right)
=r\,d_{r\psi(s)}(\exp_p)\left(\frac{d\psi}{ds}(s)\right).
\]

### 定理 18.11.5.

*设 $(M,\nabla)$ 为带有联络的光滑流形，$p$ 为 $M$ 中的一点，并设 $v,w$ 为 $T_p(M)$ 中的向量。设 $\rho_{p,v}(r)=\exp_p(rv):[0,1]\to M$ 为以 $p$ 为起点、初速度为 $v$ 的径向测地线。设 $\sigma(r,s):[0,1]\times(-\varepsilon,\varepsilon)\to M$ 为 $\rho_{p,v}(r)$ 的测地线变分，定义为*

\[
\sigma(r,s)=\exp_p(r(v+sw)),
\]

*其中假设 $r(v+sw)$ 对所有 $(r,s)\in[0,1]\times(-\varepsilon,\varepsilon)$ 属于 $E_p$。则：*

(a) *变分场 $J$ 的 $\sigma$ 给出为*

\[
J(r)=r\,d_{rv}(\exp_p)(w)
\]

*对所有 $r$ 在 $[0,1]$ 中。*

