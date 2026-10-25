## 第15章

## 光滑流形上的场

在本章中，我们将向量场推广到光滑流形，并定义一系列其他类型的“场”。

## 15.1 向量场

向量场出现在多种情境中。在本节中，我们讨论光滑流形、曲线、参数化曲面和子流形上的向量场。

*光滑流形。* 设 $M$ 是一个光滑流形。**$M$ 上的向量场**是映射 $X$，它为 $M$ 中的每个点 $p$ 指定 $T_p(M)$ 中的一个向量 $X_p$。与正则曲面上的向量场情形一样，我们有时使用“$|_p$”记号作为“下标 $p$”记号的替代，尤其是在涉及其他下标时。根据 (14.3.1)，$X_p$ 满足乘积法则

$X_p(fg) = f(p)\,X_p(g) + g(p)\,X_p(f)$  (15.1.1)

对所有 $f,g\in C^\infty(M)$ 成立。

设 $f\in C^\infty(M)$，并令

$X(f) : M \longrightarrow \mathbb{R}$

为如下定义的函数：

$X(f)(p) = X_p(f)$  (15.1.2)

对所有 $p\in M$ 成立。由 (15.1.1) 可得

$X(fg) = fX(g) + gX(f).$  (15.1.3)

如果 $X(f)\in C^\infty(M)$ 对所有 $f\in C^\infty(M)$ 成立，则称 $X$ 是**（在 $M$ 上）光滑的**。$M$ 上光滑向量场的集合记为 $\mathfrak{X}(M)$。我们通过定义如下运算使 $\mathfrak{X}(M)$ 同时成为 $\mathbb{R}$ 上的向量空间和 $C^\infty(M)$ 上的模：对所有向量场 $X,Y\in\mathfrak{X}(M)$、所有函数 $f\in C^\infty(M)$ 以及所有实数 $c$，令

$(X + Y)_p = X_p + Y_p,$

$(fX)_p = f(p)X_p,$

以及

$(cX)_p = cX_p$

对所有 $p\in M$ 成立。

回顾第 10.3 节中欧氏空间之间光滑向量场 $X : U \longrightarrow \mathbb{R}^m$ 的定义，我们注意到对于 $U$ 中每个点 $p$，向量 $X_p$ 取自 $\mathbb{R}^m$。事后看来，我们当时实际上是隐式地将切空间 $T_p(\mathbb{R}^m)$ 与 $\mathbb{R}^m$ 等同起来。

## 定理 15.1.1. 设 M 是一个光滑流形，设 X 是 𝔛(M) 中的向量场，设 p 是 M 中的一点。如果 λ(t) : (a,b) ⟶ M 是一条光滑曲线，使得对于某个 t0 ∈ (a,b) 有 λ(t0) = p 且 (dλ/dt)(t0) = X_p，则

X(f)(p) = d(f ∘ λ)/dt (t0)

对所有函数 f ∈ C^∞(M) 成立。

证明：我们有

X(f)(p) = X_p(f)      [(15.1.2)]
= dλ/dt (t0)(f)
= d(f ∘ λ)/dt (t0).      [(14.7.5)]

下一个结果保证了对于切空间中的给定向量，总存在一个以该向量为值的光滑向量场。其证明（未给出）依赖于隆起函数。

## 定理 15.1.2（向量的光滑延拓）。设 M 是一个光滑流形，设 p 是 M 中的一点，v 是 T_p(M) 中的一个向量。则存在 𝔛(M) 中的向量场 X 使得 X_p = v。

设 M 是一个光滑 m-流形，U 是 M 中的开集。将 U 视为 m-流形，设 X1,...,Xm 是 𝔛(U) 中的向量场。如果对于所有 p ∈ U，𝒳_p = (X1|p,...,Xm|p) 是 T_p(U) 的一个基，则 m-元组 𝒳 = (X1,...,Xm) 被称为 U 上的一个**标架**。我们将在本节后面看到，对于 M 中的每个点 p，总存在 p 的一个邻域，该邻域上有标架。然而，整个 M 上可能没有标架。

曲线。设 M 是一个光滑流形，λ : (a,b) ⟶ M 是一条光滑曲线。λ 上的向量场是映射 J，它为 (a,b) 中的每个点 t 指定 $T_{\lambda(t)}(M)$ 中的一个向量 $J(t)$。我们注意到不要求 $\lambda$ 是单射，因此 $\lambda$ 的像可能自交。因此，对于 $\lambda((a,b))$ 中的给定点，可能指派两个（或更多）不同的向量。这代表了曲线上的向量场与光滑流形上的向量场之间的显著区别。

设 $f\in C^\infty(M)$，考虑函数

$$J(f):(a,b)\longrightarrow\mathbb{R}$$

定义为

$$J(f)(t)=J(t)(f)$$

对所有 $t\in(a,b)$ 成立。如果 $J(f)\in C^\infty((a,b))$ 对所有 $f\in C^\infty(M)$ 成立，则称 $J$ 是**（在 $\lambda$ 上）光滑的**。$\lambda$ 上光滑向量场的集合记为 $\mathfrak{X}_M(\lambda)$。回忆第 14.7 节，$\lambda$ 在 $t$ 处的速度为 $(d\lambda/dt)(t)$。$\lambda$ 的**速度**是 $\lambda$ 上的向量场，由映射 $t\mapsto(d\lambda/dt)(t)$ 对所有 $t\in(a,b)$ 定义。如果 $\lambda$ 的速度处处非零，即对于任何 $t\in(a,b)$，$(d\lambda/dt)(t)$ 都不是 $T_{\lambda(t)}(M)$ 中的零向量，则称 $\lambda$ 是正则的。

**定理 15.1.3.** *在上述记号下，$d\lambda/dt$ 是 $\mathfrak{X}_M(\lambda)$ 中的向量场。*

*证明.* 在 (15.1.4) 中令 $J=d\lambda/dt$，由 (14.7.5) 可得

$$\frac{d\lambda}{dt}(f)(t)=\frac{d\lambda}{dt}(t)(f)=\frac{d(f\circ\lambda)}{dt}(t)$$

对所有 $t\in(a,b)$ 成立。由于 $f$ 和 $\lambda$ 光滑，根据定理 14.2.4，$f\circ\lambda$ 也光滑。因此，$(d\lambda/dt)(f)\in C^\infty((a,b))$ 对所有 $f\in C^\infty(M)$ 成立。

**定理 15.1.4.** *继续使用上述记号，如果 $X$ 是 $\mathfrak{X}(M)$ 中的向量场，则 $X\circ\lambda$ 是 $\mathfrak{X}_M(\lambda)$ 中的向量场。*

*注.* 显然，$X\circ\lambda$ 是 $\lambda$ 上的向量场，因此断言有意义。

*证明.* 在 (15.1.4) 中令 $J=X\circ\lambda$，由 (15.1.2) 可得

$$(X\circ\lambda)(f)(t)=(X\circ\lambda)(t)(f)=X_{\lambda(t)}(f)=X(f)(\lambda(t))=(X(f)\circ\lambda)(t)$$

对所有 $t\in(a,b)$ 成立。由于 $X$ 和 $f$ 光滑，根据定义 $X(f)$ 也光滑，又因为 $\lambda$ 光滑，根据定理 14.2.4，$X(f)\circ\lambda$ 也光滑。因此，$(X\circ\lambda)(f)\in C^\infty((a,b))$ 对所有 $f\in C^\infty(M)$ 成立。

取决于 $\lambda$，并非 $\mathfrak{X}_M(\lambda)$ 中的每个向量场 $J$ 都能表示为 $\lambda$ 与 $\mathfrak{X}(M)$ 中某个向量场的复合。例如，假设 $\lambda$ 的像在 $(a,b)$ 中的点 $t_1,t_2$ 处自交，且 $J(t_1)\ne J(t_2)$。由于 $\mathfrak{X}(M)$ 中的每个向量场为 $M$ 中每个点 $p$ 指派一个确定的向量，因此不存在 $X\in\mathfrak{X}(M)$ 使得 $J=X\circ\lambda$。




设 $J_1,\dots,J_m$ 为 $\mathfrak{X}_M(\lambda)$ 中的向量场。 $m$-元组 $\mathcal{J}=(J_1,\dots,J_m)$ 在 $\lambda$ 上称为一个 frame，若 $\mathcal{J}(t)=(J_1(t),\dots,J_m(t))$ 对所有 $t$ 在 $(a,b)$ 中都是 $T_{\lambda(t)}(M)$ 的基。

## *参数化曲面。*

设 $M$ 为光滑流形，设 $\sigma(r,s):(a,b)\times(-\varepsilon,\varepsilon)\longrightarrow M$ 为参数化曲面。 $\sigma$ 上的向量场是一个映射 $V$，它将 $(a,b)\times(-\varepsilon,\varepsilon)$ 中的每一点 $(r,s)$ 赋予 $T_{\sigma(r,s)}(M)$ 中的向量 $V(r,s)$。再次强调，$\sigma$ 不要求是单射。设 $f$ 为 $C^\infty(M)$ 中的函数，并考虑函数

$V(f):(a,b)\times(-\varepsilon,\varepsilon)\longrightarrow\mathbb{R}$

定义为

$V(f)(r,s)=V(r,s)(f)\qquad(15.1.5)$

对所有 $(r,s)$ 在 $(a,b)\times(-\varepsilon,\varepsilon)$ 中。我们说 $V$ 是光滑的（在 $\sigma$ 上），如果对所有 $f$ 在 $C^\infty(M)$ 中，$V(f)$ 在 $C^\infty((a,b)\times(-\varepsilon,\varepsilon))$ 中。 $\sigma$ 上的光滑向量场的集合记为 $\mathfrak{X}_M(\sigma)$。

回顾 (14.9.1) 中的记号，我们定义 $\sigma$ 上的向量场 $\partial\sigma/\partial r$ 为赋值 $(r,s)\longmapsto(\partial\sigma/\partial r)(r,s)$，$\partial\sigma/\partial s$ 亦然。

### 定理 15.1.5. 在上述设定下，$\partial\sigma/\partial r$ 和 $\partial\sigma/\partial s$ 是 $\mathfrak{X}_M(\sigma)$ 中的向量场。

证明. 我们有

$\frac{\partial\sigma}{\partial r}(f)(r,s)=\frac{\partial\sigma}{\partial r}(r,s)(f)\quad[(15.1.5)]$ $=\frac{d\sigma_s}{dr}(r)(f)\quad[(14.9.1)]$ $=\frac{d(f\circ\sigma_s)}{dr}(r)\quad[(14.7.5)]$ $=\frac{\partial(f\circ\sigma)}{\partial r}(r,s)$

对所有 $(r,s)$ 在 $(a,b)\times(-\varepsilon,\varepsilon)$ 中。由于 $f$ 和 $\sigma$ 是光滑的，根据定理 14.2.4，$f\circ\sigma$ 也是光滑的，因此 $\partial(f\circ\sigma)/\partial r$ 也是光滑的。于是，$(\partial\sigma/\partial r)(f)$ 对所有 $f$ 在 $C^\infty(M)$ 中都在 $C^\infty((a,b)\times(-\varepsilon,\varepsilon))$ 中，$(\partial\sigma/\partial s)(f)$ 亦然。

## *子流形。*

设 $\overline{M}$ 为光滑流形，$M$ 为子流形。沿 $M$ 的向量场是一个映射 $V$，它将 $M$ 中的每一点 $p$ 赋予 $T_p(\overline{M})$ 中的向量 $V_p$。我们注意到 $V_p$ 要求在 $T_p(\overline{M})$ 中，但不一定在 $T_p(M)$ 中。这解释了术语从“在 $M$ 上”改为“沿 $M$”的原因。

设 $f$ 为 $C^\infty(\overline{M})$ 中的函数，并考虑函数

$V(f):M\longrightarrow\mathbb{R}$

定义为

$V(f)(p)=V_p(f)$

对所有 $p$ 在 $M$ 中。我们说 $V$ 是光滑的（沿 $M$），如果对所有 $f$ 在 $C^\infty(\overline{M})$ 中，$V(f)$ 在 $C^\infty(M)$ 中。沿 $M$ 的光滑向量场的集合记为 $\mathfrak{X}_{\overline{M}}(M)$。特别地，对每个向量场 $X$ 在 $\mathfrak{X}(\overline{M})$ 中，限制 $X|_M$ 在 $\mathfrak{X}_{\overline{M}}(M)$ 中。带有通常的加法和数量乘法定义，$\mathfrak{X}_{\overline{M}}(M)$ 是 $\mathbb{R}$ 上的向量空间且是 $C^\infty(M)$ 上的模。此外，经过适当的等同，$\mathfrak{X}(M)$ 是 $\mathfrak{X}_{\overline{M}}(M)$ 的 $C^\infty(M)$-子模。根据 (14.8.1)，对每个点 $p$ 在 $M$ 中，$T_p(M)$ 是 $T_p(\overline{M})$ 的子空间。我们说 $\mathfrak{X}_{\overline{M}}(M)$ 中的向量场 $V$ 在无处与 $M$ 相切，如果 $V_p$ 对所有 $p$ 在 $M$ 中都不在 $T_p(M)$ 中，或等价地，如果 $V_p$ 对所有 $p$ 在 $M$ 中都在 $T_p(\overline{M}) \setminus T_p(M)$ 中。

设 $M$ 为光滑 $m$-流形，$U$ 为 $M$ 中的开集。回顾定理 14.8.4，$U$ 是 $M$ 的开 $m$-子流形。设 $U$ 是 $M$ 上图表 $(U, (x^i))$ 的坐标域。$(U, (x^i))$ 的第 $i$ 个坐标向量场是向量场

∂/∂x^i   in   𝔛(U)

定义为赋值

p ↦ ∂/∂x^i|_p

对所有 $p$ 在 $U$ 中，$i = 1, …, m$，其中我们记

∂/∂x^i(p)   by   ∂/∂x^i|_p.

则 $(∂/∂x^1, …, ∂/∂x^m)$ 是 $U$ 上的一个 frame，称为对应于 $(U, (x^i))$ 的坐标 frame。

设 $X$ 为 $M$ 上的（不一定光滑的）向量场。则 $X|_U$ 可表示为

X|_U = Σ_i α^i ∂/∂x^i,   (15.1.6)

其中 α^i 是 $U$ 上的唯一确定的函数，称为 $X$ 关于 $(U, (x^i))$ 的分量。为简便起见，我们记

X|_U   by   X.

式 (15.1.6) 的右端称为 $X$ 关于 $(U, (x^i))$ 在局部坐标下的表达式。我们经常在不提及底层图表的情况下给出向量场的局部坐标表达式。这不会引起任何混淆，因为坐标 frame 的记号已嵌入式 (15.1.6) 中使用的记号中，且坐标域的具体细节通常不是立即关心的。

### 例 15.1.6 (ℝ^m)。设 $(r^1, …, r^m)$ 为 ℝ^m 上的标准坐标，设 $(∂/∂r^1, …, ∂/∂r^m)$ 为相应的坐标 frame，称为标准坐标 frame，设 $(e_1, …, e_m)$ 为 ℝ^m 的标准基。

由定理 14.5.7 可知 $(∂/∂r¹, …, ∂/∂rᵐ)$ 可与 $(e₁, …, eₘ)$ 识别，写成

\(\left(\frac{\partial}{\partial r^1},...,\frac{\partial}{\partial r^m}\right) = (e_1,...,e_m).\)

### 定理 15.1.7 (向量场的光滑性准则)。设 $M$ 为光滑流形，$X$ 为 $M$ 上的（不一定光滑的）向量场。则 $X$ 在 𝔛(M) 中当且仅当对 $M$ 上的每个图表 $(U, (xⁱ))$，$X$ 的分量都在 $C^∞(U)$ 中。

证明. (⇒): 设 $X = Σᵢ αⁱ(∂/∂xⁱ)$ 为 $X$ 关于 $(U, (xⁱ))$ 的局部坐标表达式，设 $p$ 为 $U$ 中的点。由于 $xⁱ$ 是 $C^∞(U)$ 中的函数，根据定理 14.2.7，存在 $C^∞(M)$ 中的函数 $\tilde{x}ⁱ$ 和 $p$ 在 $M$ 中的邻域 $\tilde{U} ⊆ U$，使得 $xⁱ$ 与 $\tilde{x}ⁱ$ 在 $\tilde{U}$ 上一致。根据定理 14.3.1(b)，

\[ X(\tilde{x}ⁱ) = \left(\sum_j αʲ \frac{∂}{∂xʲ}\right)(\tilde{x}ⁱ) = \sum_j αʲ \frac{∂\tilde{x}ⁱ}{∂xʲ} = αⁱ \]

在 $\tilde{U}$ 上。由于 $X$ 在 𝔛(M) 中且 $\tilde{x}ⁱ$ 在 $C^∞(M)$ 中，根据定义，$X(\tilde{x}ⁱ)$ 在 $C^∞(M)$ 中。于是 αⁱ 在 $\tilde{U}$ 上光滑。由于 $p$ 是任意的，αⁱ 对 $i = 1, …, m$ 在 $U$ 上光滑。

(⇐): 设 $X = Σᵢ αⁱ(∂/∂xⁱ)$ 为 $X$ 关于 $(U, (xⁱ))$ 的局部坐标表达式。对 $C^∞(M)$ 中的函数 $f$，我们有

\[ X(f) = \sum_i αⁱ \frac{∂f}{∂xⁱ} \]

在 $U$ 上。根据假设，αⁱ 在 $U$ 上光滑，因此 $X(f)$ 也是光滑的。由于 $M$ 被其 atlas 的坐标域覆盖，$X(f)$ 在 $M$ 上光滑。

### 定理 15.1.8 (坐标 frame 的变换)。设 $M$ 为光滑 $m$-流形，设 $(U, (xⁱ))$ 和 $(\tilde{U}, (\tilde{x}ʲ))$ 为 $M$ 上的重叠图表，设 $(∂/∂x¹, …, ∂/∂xᵐ)$ 和 $(∂/∂\tilde{x}¹, …, ∂/∂\tilde{x}ᵐ)$ 为相应的坐标 frame。则

\[ \frac{∂}{∂xⁱ} = \sum_j \frac{∂\tilde{x}ʲ}{∂xⁱ} \frac{∂}{∂\tilde{x}ʲ} \]

在 $U ∩ \tilde{U}$ 上，对 $i = 1, …, m$。

证明. 这由定理 14.3.5(a) 可得。

## 15.2 向量场的表示

设 $M$ 为光滑流形。线性映射

\[ \mathcal{D} : C^∞(M) \longrightarrow C^∞(M) \]

称为 $C^{\infty}(M)$ 上的导子，如果它满足以下乘积法则：

```
$\mathcal{D}(fg) = f\mathcal{D}(g) + g\mathcal{D}(f)$
```

对所有 $f, g$ 在 $C^{\infty}(M)$ 中；即

```
$\mathcal{D}(fg)(p) = f(p)\,\mathcal{D}(g)(p) + g(p)\,\mathcal{D}(f)(p)$
```

对所有 $p$ 在 $M$ 中。$C^{\infty}(M)$ 上的导子的集合记为 $\operatorname{Der}(M)$。$\operatorname{Der}(M)$ 中的零导子，记为 $0$，是把 $C^{\infty}(M)$ 中的所有函数映射到 $C^{\infty}(M)$ 中的零函数的导子。我们把 $\operatorname{Der}(M)$ 设为 $\mathbb{R}$ 上的向量空间和 $C^{\infty}(M)$ 上的模，定义运算如下：对所有导子 $\mathcal{D}, \mathcal{E}$ 在 $\operatorname{Der}(M)$ 中，所有函数 $f, g$ 在 $C^{\infty}(M)$ 中，以及所有实数 $c$，令

```
$(\mathcal{D} + \mathcal{E})(f)(p) = \mathcal{D}(f)(p) + \mathcal{E}(f)(p)$
```

```
$(f\mathcal{D})(g)(p) = f(p)\,\mathcal{D}(g)(p)$
```

且

```
$(c\mathcal{D})(f)(p) = c\,\mathcal{D}(f)(p)$
```

对所有 $p$ 在 $M$ 中。

我们由 (15.1.3) 可见，$M$ 上的向量场可以看作 $C^{\infty}(M)$ 上的导子。沿着这一思路，考虑映射

```
$\mathfrak{F}: \mathfrak{X}(M) \longrightarrow \operatorname{Der}(M)$
```

定义为

```
$\mathfrak{F}(X)(f) = X(f)$
```

对所有向量场 $X$ 在 $\mathfrak{X}(M)$ 中和所有函数 $f$ 在 $C^{\infty}(M)$ 中；即

```
$\mathfrak{F}(X)(f)(p) = X(f)(p)$
```

对所有 $p$ 在 $M$ 中，其中右端由 (15.1.2) 给出。

### **定理 15.2.1 (向量场的表示)。** *若 $M$ 为光滑流形，则 $\mathfrak{F}$ 是 $C^{\infty}(M)$-模同构：*

```
$\mathfrak{X}(M) \approx \operatorname{Der}(M)$.
```

*证明.* 易证 $\mathfrak{F}$ 是 $C^{\infty}(M)$-模同态。剩下要证明 $\mathfrak{F}$ 是双射。

*单射性.* 设 $X$ 为 $\mathfrak{X}(M)$ 中的向量场使得 $\mathfrak{F}(X)=0$；即 $X(f)=0$ 对所有 $f$ 在 $C^{\infty}(M)$ 中。根据定义，$X=0$，所以 $\ker(\mathfrak{F}) = \{0\}$。由定理 B.5.3 可知 $\mathfrak{F}$ 是单射。

*满射性.* 设 $\mathcal{D}$ 为 $C^{\infty}(M)$ 上的导子。对 $M$ 中的每一点 $p$，定义函数

```
$X_{\mathcal{D}|p}: C^{\infty}(M) \longrightarrow \mathbb{R}$
```



by
\[ X_{\mathcal{D}}|_{p}(f) = \mathcal{D}(f)(p) \tag{15.2.2} \]
对所有 $C^{\infty}(M)$ 中的函数 $f$ 成立。由于 $\mathcal{D}$ 是线性的，所以 $X_{\mathcal{D}}|_{p}$ 也是线性的。对 $C^{\infty}(M)$ 中的函数 $f, g$，由 (15.2.1) 可得
\[ X_{\mathcal{D}}|_{p}(fg) = \mathcal{D}(fg)(p) = f(p)\,\mathcal{D}(g)(p) + g(p)\,\mathcal{D}(f)(p) = f(p)\,X_{\mathcal{D}}|_{p}(g) + g(p)\,X_{\mathcal{D}}|_{p}(f), \]

故 $X_{\mathcal{D}}|_{p}$ 满足 (14.3.1)。于是，$X_{\mathcal{D}}|_{p}$ 是 $p$ 处的切向量。令 $X_{\mathcal{D}}$ 为 $M$ 上的向量场，由赋值 $p \longmapsto X_{\mathcal{D}}|_{p}$ 对所有 $p \in M$ 定义。则
\[ X_{\mathcal{D}}(f)(p) = X_{\mathcal{D}}|_{p}(f) \quad [(15.1.2)] = \mathcal{D}(f)(p) \quad [(15.2.2)] \]
对所有 $p \in M$ 成立，故 $X_{\mathcal{D}}(f) = \mathcal{D}(f)$。根据定义，$\mathcal{D}(f)$ 对所有 $C^{\infty}(M)$ 中的函数 $f$ 属于 $C^{\infty}(M)$，因此 $X_{\mathcal{D}}(f)$ 也是如此。于是根据定义，$X_{\mathcal{D}}$ 是 $M$ 上的光滑向量场；即 $X_{\mathcal{D}}$ 属于 $\mathfrak{X}(M)$。由于 $\mathfrak{F}(X_{\mathcal{D}}) = \mathcal{D}$ 且 $\mathcal{D}$ 是任意的，$\mathfrak{F}$ 是满射。

这表明 $\mathfrak{F}$ 是一个 $C^{\infty}(M)$-模同构。

从现在起，我们经常（但不总是）将 $\mathfrak{X}(M)$ 与 $\mathrm{Der}(M)$ 识别起来。然而，我们仍将继续使用之前的术语和记号，例如说“$X$ 是 $\mathfrak{X}(M)$ 中的向量场”而不是“$X$ 是 $\mathrm{Der}(M)$ 中的导子”。通常可以根据上下文判断是否进行了识别，但有时为强调起见，我们会明确指出。

## 15.3 Lie 括号

设 $M$ 为光滑流形，$X$ 和 $Y$ 为 $\mathfrak{X}(M)$ 中的向量场。$X$ 与 $Y$ 的 Lie 括号是映射
\[ [X,Y] : C^{\infty}(M) \longrightarrow C^{\infty}(M) \]
定义为
\[ [X,Y](f) = X(Y(f)) - Y(X(f)) \tag{15.3.1} \]
对所有 $C^{\infty}(M)$ 中的函数 $f$ 成立。注意此定义使用了定理 15.2.1 给出的向量场的表示。暂时回到向量场表述，由 (15.1.2) 可得
\[ X(Y(f))(p) = X_{p}(Y(f)) \quad \text{and} \quad Y(X(f))(p) = Y_{p}(X(f)), \]
从而
\[ [X,Y](f)(p) = X_{p}(Y(f)) - Y_{p}(X(f)) \]
对所有 $p \in M$ 成立。

## 定理 15.3.1. 在上述设定下，[X, Y] 是 $C^{\infty}(M)$ 上的导子。

证明。由于 $Y$ 属于 $\mathfrak{X}(M)$ 且 $f$ 属于 $C^{\infty}(M)$，根据定义，$Y(f)$ 属于 $C^{\infty}(M)$。此外，由于 $X$ 属于 $\mathfrak{X}(M)$ 且 $Y(f)$ 属于 $C^{\infty}(M)$，根据定义，$X(Y(f))$ 属于 $C^{\infty}(M)$。同理，$Y(X(f))$ 属于 $C^{\infty}(M)$。于是 $[X, Y](f)$ 属于 $C^{\infty}(M)$。其余的验证是直接的。

Lie 括号是映射

[·,·] : 𝔛(M) × 𝔛(M) ⟶ 𝔛(M)

定义为赋值

(X, Y) ⟼ [X, Y] = X ∘ Y − Y ∘ X

对所有 $\mathfrak{X}(M)$ 中的向量场 $X, Y$ 成立。

## 定理 15.3.2. 设 $M$ 为光滑流形，$X, Y, Z$ 为 $\mathfrak{X}(M)$ 中的向量场，$f, g$ 为 $C^{\infty}(M)$ 中的函数。则：

- (a) [Y, X] = −[X, Y]。
- (b) [X + Y, Z] = [X, Z] + [Y, Z]。
- (c) [X, Y + Z] = [X, Y] + [X, Z]。
- (d) [fX, gY] = fg[X, Y] + fX(g)Y − gY(f)X。
- (e) [X, [Y, Z]] + [Y, [Z, X]] + [Z, [X, Y]] = 0。 (Jacobi 恒等式)

证明。 (a)–(d)：直接可得。

(e)：我们有

[X, [Y, Z]] = X ∘ [Y, Z] − [Y, Z] ∘ X = X ∘ (Y ∘ Z − Z ∘ Y) − (Y ∘ Z − Z ∘ Y) ∘ X = X ∘ Y ∘ Z − X ∘ Z ∘ Y − Y ∘ Z ∘ X + Z ∘ Y ∘ X.

同理，

[Y, [Z, X]] = Y ∘ Z ∘ X − Y ∘ X ∘ Z − Z ∘ X ∘ Y + X ∘ Z ∘ Y
[Z, [X, Y]] = Z ∘ X ∘ Y − Z ∘ Y ∘ X − X ∘ Y ∘ Z + Y ∘ X ∘ Z.

将恒等式相加即得结果。

在第 15.1 节中已观察到 $\mathfrak{X}(M)$ 是 $C^{\infty}(M)$ 上的模。由定理 15.3.2 可知 $\mathfrak{X}(M)$ 也是 $\mathbb{R}$ 上的 Lie 代数。

## 定理 15.3.3. 设 $M$ 为光滑流形，$X, Y$ 为 $\mathfrak{X}(M)$ 中的向量场，且在局部坐标下，

X = Σ_i α^i ∂/∂x^i    and    Y = Σ_j β^j ∂/∂x^j.

则：

(a)

[X, Y] = Σ_j { Σ_i (α^i ∂β^j/∂x^i - β^i ∂α^j/∂x^i) } ∂/∂x^j .

(b)

[ ∂/∂x^i , ∂/∂x^j ] = 0

对 i, j = 1, …, m。

证明。 (a)：对 $C^{\infty}(M)$ 中的函数 $f$，我们有

(X ∘ Y)(f) = X(Y(f)) = ( Σ_i α^i ∂/∂x^i ) ( Σ_j β^j ∂f/∂x^j ) = Σ_{ij} α^i ∂/∂x^i ( β^j ∂f/∂x^j )

= Σ_{ij} α^i ( ∂β^j/∂x^i ∂f/∂x^j + β^j ∂²f/∂x^i∂x^j )

= ( Σ_{ij} α^i ∂β^j/∂x^i ∂/∂x^j + Σ_{ij} α^i β^j ∂²/∂x^i∂x^j ) (f).

由于 $f$ 是任意的，

X ∘ Y = Σ_{ij} α^i ∂β^j/∂x^i ∂/∂x^j + Σ_{ij} α^i β^j ∂²/∂x^i∂x^j .

同理，

Y ∘ X = Σ_{ij} β^i ∂α^j/∂x^i ∂/∂x^j + Σ_{ij} β^i α^j ∂²/∂x^i∂x^j .

上述两个恒等式与定理 14.3.6 给出结果。

(b)：由部分 (a) 可得。

## 15.4 余向量场

设 $M$ 为光滑 $m$ 流形。$M$ 上的余向量场是映射 $\omega$，它将每个点 $p \in M$ 赋予一个余向量 $\omega_p \in T_p^*(M)$。我们说 $\omega$ 在 $p$ 处消失如果 $\omega_p = 0$，在 $p$ 处非零如果 $\omega_p \neq 0$，并且在 $M$ 上处处非零如果它在每个 $p \in M$ 处都非零。

设 $X$ 为 $\mathfrak{X}(M)$ 中的向量场，并令

ω(X) : M ⟶ ℝ

为函数，定义为

ω(X)(p) = ω_p(X_p)    (15.4.1)

对所有 $p \in M$ 成立。我们说 $\omega$ 是光滑的（在 $M$ 上）如果函数 $\omega(X)$ 属于 $C^{\infty}(M)$ 对所有 $\mathfrak{X}(M)$ 中的向量场 $X$ 成立。光滑余向量场的集合

## 例 15.4.1 ($\mathbb{R}^m$).

设 $(r^1,\dots,r^m)$ 为 $\mathbb{R}^m$ 上的标准坐标，$(d(r^1),\dots,d(r^m))$ 为相应的对偶坐标标架，称为标准对偶坐标标架，$(\xi^1,\dots,\xi^m)$ 为对应于 $\mathbb{R}^m$ 标准基的对偶基。由定理 14.5.7 可知 $(d(r^1),\dots,d(r^m))$ 可与 $(\xi^1,\dots,\xi^m)$ 识别，写为

$(d(r^1),\dots,d(r^m)) = (\xi^1,\dots,\xi^m).$

接下来的三个结果是定理 15.1.2、定理 15.1.7 和定理 15.1.8 的余向量场对应物。

## 定理 15.4.2 (余向量场的光滑性判据).

设 $M$ 为光滑流形，$\omega$ 为 $M$ 上的（不一定光滑的）余向量场。则 $\omega$ 属于 $\mathfrak{X}^*(M)$ 当且仅当对 $M$ 上的每个坐标图 $(U,(x^i))$，$\omega$ 的分量属于 $C^\infty(U)$。

## 定理 15.4.3 (对偶坐标标架的变换).

设 $M$ 为光滑 $m$ 流形，$(U,(x^i))$ 和 $(\widetilde{U},(\widetilde{x}^j))$ 为 $M$ 上重叠的坐标图，$\mathcal{X}^* = (d(x^1),\dots,d(x^m))$ 和 $\widetilde{\mathcal{X}}^* = (d(\widetilde{x}^1),\dots,d(\widetilde{x}^m))$ 为相应的对偶坐标标架。则

$d(x^j) = \sum_i \frac{\partial x^j}{\partial \widetilde{x}^i} d(\widetilde{x}^i)$

在 $U \cap \widetilde{U}$ 上对 $j = 1,\dots,m$ 成立。

证明。这由定理 14.5.4(a) 可得。

## 定理 15.4.4 (余向量的光滑延拓).

设 $M$ 为光滑流形，$p$ 为 $M$ 中的点，$\eta$ 为 $T_p^*(M)$ 中的余向量。则存在余向量场 $\omega$ 属于 $\mathfrak{X}^*(M)$ 使得 $\omega_p = \eta.$

设 $M$ 为光滑流形，并定义映射

$d : C^\infty(M) \longrightarrow \mathfrak{X}^*(M),$

称为外微分，由

$d(f)_p(v) = d_p(f)(v) = v(f)$  (15.4.4)

对所有 $C^\infty(M)$ 中的函数 $f$、$M$ 中的点 $p$ 和 $T_p(M)$ 中的向量 $v$ 成立，其中第二个等式由定理 14.5.5 可得。下一结果的部分 (a) 表明此定义是有意义的。

## 定理 15.4.5.

设 $M$ 为光滑流形，$f$ 为 $C^\infty(M)$ 中的函数，$X$ 为 $\mathfrak{X}(M)$ 中的向量场。则：

- (a) $d(f)$ 是 $\mathfrak{X}^*(M)$ 中的余向量场。
- (b) $d(f)(X) = X(f)$。

证明。 (b)：由定理 14.5.1，$d(f)_p$ 是 $T_p^*(M)$ 中的余向量，故 $d(f)$ 是余向量场。对 $M$ 中的点 $p$，我们有

```
d(f)(X)(p) = d(f)_p(X_p)    [(15.4.1)]
= X_p(f)                [(15.4.4)]
= X(f)(p).              [(15.1.2)]
```

由于 $p$ 是任意的，结果得证。

(a)：由于 $X$ 和 $f$ 都是光滑的，根据定义，$X(f)$ 也是光滑的。因为 $X$ 是任意的，由部分 (b) 可知，根据定义，$d(f)$ 是光滑的。

## 定理 15.4.6. 设 $M$ 为光滑流形，$f, g$ 为 $C^\infty(M)$ 中的函数，$c$ 为实数。则：

- (a) d(cf + g) = c d(f) + d(g)。
- (b) d(fg) = f d(g) + g d(f)。

证明。这由定理 14.5.6 和 (15.4.4) 可得。

## 定理 15.4.7. 若 $M$ 为光滑流形且 $f$ 为 $C^\infty(M)$ 中的函数，则在局部坐标下，

```
d(f) = Σ_i ∂f/∂x^i d(x^i).
```

证明。对 $M$ 中的点 $p$ 和 $T_p(M)$ 中的向量 $v$，我们有

```
d(f)_p(v) = d_p(f)(v)                     [(15.4.4)]
= Σ_i ∂f/∂x^i (p) d_p(x^i)(v)            [Th 14.5.3]
= Σ_i ∂f/∂x^i (p) d(x^i)_p(v)            [(15.4.4)]
= ( Σ_i ∂f/∂x^i d(x^i) |_p )(v).
```

由于 $p$ 和 $v$ 是任意的，结果得证。

## 15.5 余向量场的表示

## 定理 15.5.1. 若 $M$ 为光滑流形且 $\omega$ 为 $\mathfrak{X}^*(M)$ 中的余向量场，则 $\omega$ 是 $C^\infty(M)$-线性的。即对所有 $\mathfrak{X}(M)$ 中的向量场 $X, Y$ 和 $C^\infty(M)$ 中的函数 $f$：

- (a) ω(X + Y) = ω(X) + ω(Y)
- (b) ω(f X) = f ω(X)。




证明。对于M中的一点p，我们有

```
ω(X + Y)(p) = ω_p((X + Y)_p)    [(15.4.1)]
= ω_p(X_p + Y_p)
= ω_p(X_p) + ω_p(Y_p)
= ω(X)(p) + ω(Y)(p)    [(15.4.1)]
= (ω(X) + ω(Y))(p)
```

并且

```
ω(fX)(p) = ω_p((fX)_p)    [(15.4.1)]
= ω_p(f(p)X_p)
= f(p) ω_p(X_p)
= (f ω)_p(X_p)
= (f ω(X))(p).    [(15.4.1)]
```

由于p是任意的，结论成立。

按照第B.5节，我们用

```
Lin_{C^∞(M)}(𝔛(M), C^∞(M))
```

表示从𝔛(M)到C^∞(M)的所有C^∞(M)-线性映射构成的C^∞(M)-模。我们定义一个映射

```
𝔠 : 𝔛*(M) ⟶ Lin_{C^∞(M)}(𝔛(M), C^∞(M)),
```

称为特征映射，其定义如下：

```
𝔠(ω)(X) = ω(X)    (15.5.1)
```

对所有𝔛*(M)中的余向量场ω和𝔛(M)中的向量场X成立，其中(15.5.1)的右边由(15.4.1)给出。由定理15.5.1可知，𝔠(ω)是Lin_{C^∞(M)}(𝔛(M), C^∞(M))中的一个映射，因此该定义是有意义的。此时，𝔠(ω)只不过是从(15.4.1)的角度看待𝔛*(M)中余向量场ω的一种记号简写：它作为一种将向量场变为函数的机制。这种形式化的目的将在后续变得清晰。

我们称一个ℝ-线性映射ℱ : 𝔛(M) ⟶ C^∞(M)是逐点确定的，如果对于M中的所有点p，只要X、Ẋ是𝔛(M)中满足X_p = Ẋ_p的向量场，就有ℱ(X)(p) = ℱ(Ẋ)(p)。由于ℱ(X)(p) = ℱ(Ẋ)(p)等价于ℱ(X − Ẋ)(p) = 0，且X_p = Ẋ_p等价于(X − Ẋ)_p = 0，因此ℱ是逐点确定的当且仅当对于M中的每一点p，只要Y是𝔛(M)中满足Y_p = 0的向量场，就有ℱ(Y)(p) = 0。

设ω是𝔛*(M)中的一个余向量场，p是M中的一点，X是𝔛(M)中满足X_p = 0的向量场。由(15.4.1)和(15.5.1)可得𝔠(ω)(X)(p) = 0。因此，𝔠(ω)是Lin_{C^∞(M)}(𝔛(M), C^∞(M))中逐点确定的映射。值得注意的是，如下一个结果所示，Lin_{C^∞(M)}(𝔛(M), C^∞(M))中的所有映射都具有这一性质。

## 15.5 余向量场的表示

**定理 15.5.2.** 若$M$是光滑流形，且$\mathfrak{Y}$是$\operatorname{Lin}_{C^\infty(M)}(\mathfrak{X}(M),C^\infty(M))$中的一个映射，则$\mathfrak{Y}$是逐点确定的。

*证明.* 设$p$是$M$中的一点，$X$是$\mathfrak{X}(M)$中满足$X_p=0$的向量场。我们需要证明$\mathfrak{Y}(X)(p)=0$。这里只考虑$M$被单个坐标卡$(M,(x^i))$覆盖的情形。在局部坐标下，设$X=\sum_i\alpha^i(\partial/\partial x^i)$。这给出了$X$在整个$M$上的表达式，因此我们可以对$X$应用$\mathfrak{Y}$得到

$$\mathfrak{Y}(X)=\sum_i\alpha^i\mathfrak{Y}\left(\frac{\partial}{\partial x^i}\right),$$

因此

$$\mathfrak{Y}(X)(p)=\sum_i\alpha^i(p)\,\mathfrak{Y}\left(\frac{\partial}{\partial x^i}\right)(p).$$

由于$X_p=0$，可得每个$\alpha^i(p)=0$，因此$\mathfrak{Y}(X)(p)=0$。

**定理 15.5.3（余向量场特征定理）.** 若$M$是光滑流形，则$\mathfrak{C}$是一个$C^\infty(M)$-模同构：

$$\mathfrak{X}^*(M)\cong\operatorname{Lin}_{C^\infty(M)}(\mathfrak{X}(M),C^\infty(M)).$$

*证明.* 容易证明$\mathfrak{C}$是$C^\infty(M)$-模同态。我们需要证明$\mathfrak{C}$是双射。

*单射性.* 假设存在$\mathfrak{X}^*(M)$中的余向量场$\omega$使得$\mathfrak{C}(\omega)=0$。由(15.5.1)可知，对所有$\mathfrak{X}(M)$中的向量场$X$，有$\omega(X)=0$。设$p$是$M$中的一点，$v$是$T_p(M)$中的一个向量。根据定理15.1.2，存在$\mathfrak{X}(M)$中的向量场$Y$使得$Y_p=v$。于是$\omega(Y)=0$，所以(15.4.1)给出

$$\omega_p(v)=\omega_p(Y_p)=\omega(Y)(p)=0.$$

由于$p$和$v$是任意的，所以$\omega=0$。因此，$\ker(\mathfrak{C})=\{0\}$。根据定理B.5.3，$\mathfrak{C}$是单射。

*满射性.* 设$\mathfrak{Y}$是$\operatorname{Lin}_{C^\infty(M)}(\mathfrak{X}(M),C^\infty(M))$中的一个映射。我们需要找到一个$\mathfrak{X}^*(M)$中的余向量场$\widetilde{\mathfrak{Y}}$，使得$\mathfrak{C}(\widetilde{\mathfrak{Y}})=\mathfrak{Y}$。设$p$是$M$中的一点，定义映射$\widetilde{\mathfrak{Y}}_p:T_p(M)\longrightarrow\mathbb{R}$如下：

$$\widetilde{\mathfrak{Y}}_p(v)=\mathfrak{Y}(X)(p) \tag{15.5.2}$$

对所有$T_p(M)$中的向量$v$成立，其中$X$是$\mathfrak{X}(M)$中满足$X_p=v$的任意向量场。根据定理15.1.2，这样的向量场总是存在的。由定理15.5.2可知，$\widetilde{\mathfrak{Y}}_p$不依赖于$X$的选择，因此$\widetilde{\mathfrak{Y}}_p$是良定义的。设$w$是$T_p(M)$中的一个向量，$Y$是$\mathfrak{X}(M)$中满足$Y_p=w$的向量场，$c$是一个实数。则$cX+Y$是$\mathfrak{X}(M)$中的向量场，且

$$(cX+Y)_p=cX_p+Y_p=cv+w,$$

因此
\widetilde{\mathfrak{Y}}_p(cv + w) = \mathfrak{Y}(cX + Y)(p) \quad [(15.5.2)]
= c\mathfrak{Y}(X)(p) + \mathfrak{Y}(Y)(p)
= c\widetilde{\mathfrak{Y}}_p(v) + \widetilde{\mathfrak{Y}}_p(w), \quad [(15.5.2)]

所以\widetilde{\mathfrak{Y}}_p是线性的。因此，\widetilde{\mathfrak{Y}}_p是T_p^*(M)中的一个余向量。我们通过赋值p \longmapsto \widetilde{\mathfrak{Y}}_p在M上定义一个余向量场\widetilde{\mathfrak{Y}}。我们断言\widetilde{\mathfrak{Y}}是光滑的；也就是说，对所有\mathfrak{X}(M)中的向量场Z，函数\widetilde{\mathfrak{Y}}(Z)是光滑的。由(15.4.1)和(15.5.2)，

\widetilde{\mathfrak{Y}}(Z)(p) = \widetilde{\mathfrak{Y}}_p(Z_p) = \mathfrak{Y}(Z)(p)

对所有M中的p成立，因此
\widetilde{\mathfrak{Y}}(Z) = \mathfrak{Y}(Z). \quad (15.5.3)

由于\mathfrak{Y}在Lin_{C^\infty(M)}(\mathfrak{X}(M), C^\infty(M))中且Z在\mathfrak{X}(M)中，根据定义，\mathfrak{Y}(Z)是光滑函数，因此\widetilde{\mathfrak{Y}}(Z)也是光滑的。这证明了上述断言。由(15.5.1)和(15.5.3)可得\mathfrak{C}(\widetilde{\mathfrak{Y}})(Z) = \mathfrak{Y}(Z)。由于Z是任意的，所以\mathfrak{C}(\widetilde{\mathfrak{Y}}) = \mathfrak{Y}。因此，\mathfrak{C}是满射。

将定理15.5.3证明的某个方面单独提出来是有益的。设\mathfrak{Y}是Lin_{C^\infty(M)}(\mathfrak{X}(M), C^\infty(M))中的一个映射。我们证明了\mathfrak{C}^{-1}(\mathfrak{Y})是\mathfrak{X}^*(M)中的余向量场，其定义为
\mathfrak{C}^{-1}(\mathfrak{Y})_p(v) = \mathfrak{Y}(X)(p) \quad (15.5.4)

对所有M中的点p和T_p(M)中的向量v成立，其中X是\mathfrak{X}(M)中满足X_p = v的任意向量场，其存在性由定理15.1.2保证。

现在我们有了定理15.5.3，通常（但并不总是）将\mathfrak{X}^*(M)视为由所有从\mathfrak{X}(M)到C^\infty(M)的C^\infty(M)-线性映射构成的\mathbb{R}上的向量空间和C^\infty(M)上的模。我们将在15.7节看到定理15.5.3的一个重要推广。

## 15.6 张量场

本节中，我们推广15.4节的部分内容。

设M是光滑m-流形，r, s ≥ 0是整数。M上的一个(r,s)-张量场是一个映射\mathcal{A}，它为M中的每一点p指定T_s^r(T_p(M))中的一个(r,s)-张量\mathcal{A}_p。我们也称\mathcal{A}为r-反变-s-协变张量场，或简称为张量场，并定义\mathcal{A}的秩为(r,s)。当s = 0时，\mathcal{A}被称为r-反变张量场，或简称反变张量场；当r = 0时，\mathcal{A}被称为s-协变张量场，或简称为协变张量场。

设\omega^1, ..., \omega^r是\mathfrak{X}^*(M)中的余向量场，X_1, ..., X_s是\mathfrak{X}(M)中的向量场，并考虑函数
\mathcal{A}(\omega^1, ..., \omega^r, X_1, ..., X_s) : M \longrightarrow \mathbb{R}

其定义为

\[
\mathcal{A}(\omega^1,\ldots,\omega^r,X_1,\ldots,X_s)(p)=\mathcal{A}_p(\omega^1|_p,\ldots,\omega^r|_p,X_1|_p,\ldots,X_s|_p)
\tag{15.6.1}
\]

对所有$M$中的$p$成立。我们说$\mathcal{A}$是**光滑的（在$M$上）**，如果函数$\mathcal{A}(\omega^1,\ldots,\omega^r,X_1,\ldots,X_s)$对所有$\mathfrak{X}^*(M)$中的余向量场$\omega^1,\ldots,\omega^r$和所有$\mathfrak{X}(M)$中的向量场$X_1,\ldots,X_s$都属于$C^\infty(M)$。$M$上光滑$(r,s)$-张量场的集合记为$\mathcal{T}^r_s(M)$。特别地，

\[
\mathcal{T}^0_1(M)=\mathfrak{X}^*(M) \qquad\text{and}\qquad \mathcal{T}^1_0(M)=\mathfrak{X}(M).
\tag{15.6.2}
\]

为完整起见，我们定义

\[
\mathcal{T}^0_0(M)=C^\infty(M).
\tag{15.6.3}
\]

将恒等式(15.6.2)和(15.6.3)与恒等式(5.1.1)和(5.1.2)进行比较是有益的。从现在起，我们避免以下平凡情形。

> **除非另有说明，下文始终假设$(r,s)\ne(0,0)$。**

以类似于向量场和余向量场的方式在$\mathcal{T}^r_s(M)$上定义运算，我们使$\mathcal{T}^r_s(M)$成为$\mathbb{R}$上的向量空间和$C^\infty(M)$上的模。

为光滑流形引入的许多定义都以逐点的方式表述（不要与“逐点确定”混淆），并最终依赖于在向量空间背景下给出的早期定义。例如，光滑流形上的张量场本质上是一族张量，每个张量对应光滑流形中的一个点。逐点方法的一个重要结果是，为向量空间给出的早期定理可以直接推广到光滑流形。我们将所得的光滑流形定理称为早期向量空间定理的**流形版本**（缩写为**mv**）。下面是一个例子。

**定理 15.6.1.**

*设$M$是光滑流形，设$\mathcal{A},\mathcal{A}_1,\mathcal{A}_2$、$\mathcal{B},\mathcal{B}_1,\mathcal{B}_2$和$\mathcal{C}$分别是$\mathcal{T}^r_s(M)$、$\mathcal{T}^{r'}_{s'}(M)$和$\mathcal{T}^{r''}_{s''}(M)$中的张量场，并设$f$是$C^\infty(M)$中的函数。则：*

(a) $(\mathcal{A}_1+\mathcal{A}_2)\otimes\mathcal{B}=\mathcal{A}_1\otimes\mathcal{B}+\mathcal{A}_2\otimes\mathcal{B}$.

(b) $\mathcal{A}\otimes(\mathcal{B}_1+\mathcal{B}_2)=\mathcal{A}\otimes\mathcal{B}_1+\mathcal{A}\otimes\mathcal{B}_2$.

(c) $(f\mathcal{A})\otimes\mathcal{B}=f(\mathcal{A}\otimes\mathcal{B})=\mathcal{A}\otimes(f\mathcal{B})$.

(d) $(\mathcal{A}\otimes\mathcal{B})\otimes\mathcal{C}=\mathcal{A}\otimes(\mathcal{B}\otimes\mathcal{C})$.

*证明.* 这是定理5.1.2的流形版本。

设$M$是光滑$m$-流形，$(U,(x^i))$是$M$上的一个坐标卡。设$1\le i_1,\ldots,i_r\le m$和$1\le j_1,\ldots,j_s\le m$是整数，并考虑张量场

\[
\frac{\partial}{\partial x^{i_1}}\otimes\cdots\otimes\frac{\partial}{\partial x^{i_r}}\otimes d(x^{j_1})\otimes\cdots\otimes d(x^{j_s})
\qquad\text{in}\qquad \mathcal{T}^r_s(U)
\]


由如下对应关系定义

$$p\longmapsto \left.\frac{\partial}{\partial x^{i_1}}\right|_p\otimes\cdots\otimes\left.\frac{\partial}{\partial x^{i_r}}\right|_p\otimes d_p(x^{j_1})\otimes\cdots\otimes d_p(x^{j_s})$$

对所有 $p\in U$ 成立。设 $\mathcal A$ 是 $M$ 上的（未必光滑的）$(r,s)$ 型张量场。则 $\mathcal A|_U$ 可表示为

$$\mathcal A|_U=\sum_{\substack{1\le i_1,\ldots,i_r\le m\\1\le j_1,\ldots,j_s\le m}}\mathcal A^{i_1\ldots i_r}_{j_1\ldots j_s}\frac{\partial}{\partial x^{i_1}}\otimes\cdots\otimes\frac{\partial}{\partial x^{i_r}}\otimes d(x^{j_1})\otimes\cdots\otimes d(x^{j_s}),\quad (15.6.4)$$

其中 $\mathcal A^{i_1\ldots i_r}_{j_1\ldots j_s}$ 是 $U$ 上唯一确定的函数，称为 $\mathcal A$ 关于 $(U,(x^i))$ 的**分量**。为简洁起见，我们将

$$\mathcal A|_U\quad\text{记为}\quad\mathcal A.$$

称 (15.6.4) 的右端为 $\mathcal A$ 关于 $(U,(x^i))$ 的**局部坐标**表示。

## 定理 15.6.2（$(r,s)$ 型张量场的光滑性判别法）. 在上述设定下，$\mathcal A\in T^r_s(M)$ 当且仅当对 $M$ 上的每个坐标卡 $(U,(x^i))$，$\mathcal A$ 的分量都属于 $C^\infty(U)$。

## 定理 15.6.3（坐标架的变换）. 设 $M$ 是一个光滑 $m$ 维流形，$\mathcal A$ 是 $T^r_s(M)$ 中的张量场，$(U,(x^i))$ 与 $(\widetilde U,(\widetilde x^i))$ 是 $M$ 上相互重叠的坐标卡，并设 $\mathcal A^{i_1\ldots i_r}_{j_1\ldots j_s}$ 与 $\widetilde{\mathcal A}^{i_1\ldots i_r}_{j_1\ldots j_s}$ 分别是 $\mathcal A$ 关于 $(U,(x^i))$ 和 $(\widetilde U,(\widetilde x^i))$ 的分量。则在 $U\cap\widetilde U$ 上有

$$\widetilde{\mathcal A}^{i_1\ldots i_r}_{j_1\ldots j_s}=\sum_{\substack{1\le k_1,\ldots,k_r\le m\\1\le l_1,\ldots,l_s\le m}}\frac{\partial\widetilde x^{i_1}}{\partial x^{k_1}}\cdots\frac{\partial\widetilde x^{i_r}}{\partial x^{k_r}}\frac{\partial x^{l_1}}{\partial\widetilde x^{j_1}}\cdots\frac{\partial x^{l_s}}{\partial\widetilde x^{j_s}}\mathcal A^{k_1\ldots k_r}_{l_1\ldots l_s}$$

*证明.* 这是定理 5.1.4 的流形版本，但详细推导仍具有启发意义。由定理 15.1.8 和定理 15.4.3，有

\[
\widetilde{\mathcal A}^{\widetilde{i}_1\cdots\widetilde{i}_r}_{\widetilde{j}_1\cdots\widetilde{j}_s}
=\mathcal A\left(d(\widetilde{x}^{i_1}),\ldots,d(\widetilde{x}^{i_r}),\frac{\partial}{\partial\widetilde{x}^{j_1}},\ldots,\frac{\partial}{\partial\widetilde{x}^{j_s}}\right)
\]

\[
=\mathcal A\left(\sum_{k_1}\frac{\partial\widetilde{x}^{i_1}}{\partial x^{k_1}}d(x^{k_1}),\ldots,\sum_{k_r}\frac{\partial\widetilde{x}^{i_r}}{\partial x^{k_r}}d(x^{k_r}),\sum_{l_1}\frac{\partial x^{l_1}}{\partial\widetilde{x}^{j_1}}\frac{\partial}{\partial x^{l_1}},\ldots,\sum_{l_s}\frac{\partial x^{l_s}}{\partial\widetilde{x}^{j_s}}\frac{\partial}{\partial x^{l_s}}\right)
\]

\[
=\sum_{\substack{1\le k_1,\ldots,k_r\le m\\1\le l_1,\ldots,l_s\le m}}
\frac{\partial\widetilde{x}^{i_1}}{\partial x^{k_1}}\cdots\frac{\partial\widetilde{x}^{i_r}}{\partial x^{k_r}}
\frac{\partial x^{l_1}}{\partial\widetilde{x}^{j_1}}\cdots\frac{\partial x^{l_s}}{\partial\widetilde{x}^{j_s}}
\cdot\mathcal A\left(d(x^{k_1}),\ldots,d(x^{k_r}),\frac{\partial}{\partial x^{l_1}},\ldots,\frac{\partial}{\partial x^{l_s}}\right)
\]

\[
=\sum_{\substack{1\le k_1,\ldots,k_r\le m\\1\le l_1,\ldots,l_s\le m}}
\frac{\partial\widetilde{x}^{i_1}}{\partial x^{k_1}}\cdots\frac{\partial\widetilde{x}^{i_r}}{\partial x^{k_r}}
\frac{\partial x^{l_1}}{\partial\widetilde{x}^{j_1}}\cdots\frac{\partial x^{l_s}}{\partial\widetilde{x}^{j_s}}
\mathcal A^{k_1\cdots k_r}_{l_1\cdots l_s}.
\]

## 15.7 张量场的表示

本节我们给出第 15.5 节中定义与结果的推广。

设 $M$ 是光滑流形，$r,s\ge 0$ 是整数。沿用第 15.5 节的记号，用 $\operatorname{Mult}_{C^\infty(M)}(\mathfrak X^*(M)^r\times\mathfrak X(M)^s,C^\infty(M))$ 表示从 $\mathfrak X^*(M)^r\times\mathfrak X(M)^s$ 到 $C^\infty(M)$ 的 $C^\infty(M)$-多重线性映射所构成的 $C^\infty(M)$-模。定义映射

$\mathcal C_s^r:T_s^r(M)\longrightarrow\operatorname{Mult}_{C^\infty(M)}(\mathfrak X^*(M)^r\times\mathfrak X(M)^s,C^\infty(M)),$

称为**特征映射**，如下：

\[
\mathcal C_s^r(\mathcal A)(\omega^1,\ldots,\omega^r,X_1,\ldots,X_s)=\mathcal A(\omega^1,\ldots,\omega^r,X_1,\ldots,X_s) \tag{15.7.1}
\]

对所有张量场 $\mathcal A\in T_s^r(M)$、所有余向量场 $\omega^1,\ldots,\omega^r\in\mathfrak X^*(M)$ 以及所有向量场 $X_1,\ldots,X_s\in\mathfrak X(M)$ 成立，其中 (15.7.1) 的右端由 (15.6.1) 给出。由定理 15.5.1 的推广可知 $\mathcal C_s^r(\mathcal A)$ 属于 $\operatorname{Mult}_{C^\infty(M)}(\mathfrak X^*(M)^r\times\mathfrak X(M)^s,C^\infty(M))$，因此该定义是合理的。

称一个 $\mathbb R$-线性映射 $\mathcal F:\mathfrak X^*(M)^r\times\mathfrak X(M)^s\longrightarrow C^\infty(M)$ 是**逐点确定的**，如果对 $M$ 中所有点 $p$，有

\[
\mathcal F(\omega^1,\ldots,\omega^r,X_1,\ldots,X_s)(p)=\mathcal F(\widetilde\omega^1,\ldots,\widetilde\omega^r,\widetilde X_1,\ldots,\widetilde X_s)(p)
\]

只要 $\omega^i,\widetilde\omega^i$ 是 $\mathfrak X^*(M)$ 中的余向量场，且对 $i=1,\ldots,r$ 有 $\omega^i|_p=\widetilde\omega^i|_p$；$X_j,\widetilde X_j$ 是 $\mathfrak X(M)$ 中的向量场，且对 $j=1,\ldots,s$ 有 $X_j|_p=\widetilde X_j|_p$。

## 定理 15.7.1. 若 $M$ 是光滑流形，且 $\Upsilon$ 是 $\operatorname{Mult}_{C^\infty(M)}(\mathfrak X^*(M)^r\times\mathfrak X(M)^s,C^\infty(M))$ 中的映射，则 $\Upsilon$ 是逐点确定的。

## 定理 15.7.2（张量场特征定理）. 若 $M$ 是光滑流形，则 $\mathfrak c_s^r$ 是一个 $C^\infty(M)$-模同构：

\[
\mathcal T_s^r(M)\approx\operatorname{Mult}_{C^\infty(M)}(\mathfrak X^*(M)^r\times\mathfrak X(M)^s,C^\infty(M)).
\]

设 $\Upsilon$ 是 $\operatorname{Mult}_{C^\infty(M)}(\mathfrak X^*(M)^r\times\mathfrak X(M)^s,C^\infty(M))$ 中的映射。类似于 (15.5.4)，$(\mathfrak c_s^r)^{-1}(\Upsilon)$ 是 $\mathcal T_s^r(M)$ 中的张量场，由下式定义：

\[
(\mathfrak c_s^r)^{-1}(\Upsilon)_p(\eta^1,\ldots,\eta^r,v_1,\ldots,v_s)=\Upsilon(\omega^1,\ldots,\omega^r,X_1,\ldots,X_s)(p) \tag{15.7.2}
\]

对 $M$ 中所有点 $p$、$T_p^*(M)$ 中所有余向量 $\eta^1,\ldots,\eta^r$ 以及 $T_p(M)$ 中所有向量 $v_1,\ldots,v_s$ 成立，其中 $\omega^1,\ldots,\omega^r$ 是 $\mathfrak X^*(M)$ 中满足 $\omega^i|_p=\eta^i$（$i=1,\ldots,r$）的任意余向量场，$X_1,\ldots,X_s$ 是 $\mathfrak X(M)$ 中满足 $X_j|_p=v_j$（$j=1,\ldots,s$）的任意向量场。

在本节的其余部分，我们试图将上述技术性内容置于更大的背景中。

设 $\mathcal A$ 是 $\mathcal T_s^r(M)$ 中的张量场。对 $M$ 中给定的点 $p$，$\mathcal A_p$ 是 $\mathcal T_s^r(T_p(M))$ 中的一个张量；对于给定的余向量 $\eta^1,\ldots,\eta^r\in T_p^*(M)$ 和向量 $v_1,\ldots,v_s\in T_p(M)$，$\mathcal A_p(\eta^1,\ldots,\eta^r,v_1,\ldots,v_s)$ 是它在 $\mathbb R$ 中的值。利用定理 15.7.2 中的同构进行等同后，$\mathcal A$ 现在可以看作 $\operatorname{Mult}_{C^\infty(M)}(\mathfrak X^*(M)^r\times\mathfrak X(M)^s,C^\infty(M))$ 中的一个映射。对于给定的余向量场 $\omega^1,\ldots,\omega^r\in\mathfrak X^*(M)$ 和向量场 $X_1,\ldots,X_s\in\mathfrak X(M)$，$\mathcal A(\omega^1,\ldots,\omega^r,X_1,\ldots,X_s)$ 是 $C^\infty(M)$ 中的一个函数；对于给定的点 $p\in M$，$\mathcal A(\omega^1,\ldots,\omega^r,X_1,\ldots,X_s)(p)$ 是它在 $\mathbb R$ 中的值。定理 15.7.2 引入的创新在于：我们已经从在余向量和向量处求张量 $\mathcal A_p$ 的值，转变为在微分形式和向量场处求函数 $\mathcal A$ 的值。

现在我们已经掌握了定理 15.7.2，因此我们通常（但并非总是）将 $\mathcal T_s^r(M)$ 看作由所有从 $\mathfrak X^*(M)^r\times\mathfrak X(M)^s$ 到 $C^\infty(M)$ 的 $C^\infty(M)$-多重线性映射组成的 $\mathbb R$-向量空间和 $C^\infty(M)$-模。我们不会过于拘泥于记号中是否包含“$\mathfrak c_s^r$”，而是让上下文来表明具体含义，由此获得一种令人欢迎的记号简化。

我们处理张量场的这一新方法的一个优点是，它提供了一种判定机制，可以判断给定映射 $\mathcal F:\mathfrak X^*(M)^r\times\mathfrak X(M)^s\longrightarrow C^\infty(M)$ 是否是（或至少可等同于）$\mathcal T_s^r(M)$ 中的张量场。根据定理 15.7.2，只要 $\mathcal F$ 能被证明是 $C^\infty(M)$-多重线性的，就可以进行这种等同。在实践中，判断 $\mathcal F$ 是否可加通常是直接的。难点通常在于判断 $C^\infty(M)$ 中的函数能否从 $\mathcal F$ 中“提出来”。也就是说，若对所有余向量场 $\omega^1,\ldots,\omega^r\in\mathfrak X^*(M)$、所有向量场 $X_1,\ldots,X_s\in\mathfrak X(M)$ 以及所有函数 $f\in C^\infty(M)$，都有

\[
\mathcal F(\omega^1,\ldots,f\omega^i,\ldots,\omega^r,X_1,\ldots,X_s)=f\,\mathcal F(\omega^1,\ldots,\omega^i,\ldots,\omega^r,X_1,\ldots,X_s).
\]


对于 \(i=1,\ldots,r\)，并且

\[
\mathcal{F}(\omega^1,\ldots,\omega^r,X_1,\ldots,fX_j,\ldots,X_s)=f\mathcal{F}(\omega^1,\ldots,\omega^r,X_1,\ldots,X_j,\ldots,X_s)
\]

对于 \(j=1,\ldots,s\)。在后续章节中，我们会遇到此类计算的若干例子。

在本节结束时，让我们就“表示”作几点说明。在 15.2 节中，我们说明了 \(\mathfrak{X}(M)\) 中的一个向量场等价于从 \(C^\infty(M)\) 到 \(C^\infty(M)\) 的一类映射。在 15.5 节中，我们证明了 \(\mathfrak{X}^*(M)\) 中的一个向量场等价于从 \(\mathfrak{X}(M)\) 到 \(C^\infty(M)\) 的一类映射。在本节中，我们说明了（或至少断言）\(\mathcal{T}^r_s(M)\) 中的一个张量场等价于从 \(\mathfrak{X}^*(M)^r\times\mathfrak{X}(M)^s\) 到 \(C^\infty(M)\) 的一类映射。粗略地说，我们一直在进行一项将“场”表示为生成“函数”的映射的工作。

## 15.8 微分形式

设 \(M\) 是一个光滑 \(m\)-流形，令 \(0\le s\le m\) 是一个整数。**\(M\) 上的微分 \(s\)-形式**是一个映射 \(\omega\)，它给 \(M\) 中的每个点 \(p\) 指派 \(\Lambda^s(T_p(M))\) 中的一个 \(s\)-余向量 \(\omega_p\)。在文献中，微分 \(s\)-形式通常被称为 **\(s\)-形式** 或简称为 **形式**。注意，1-形式与余向量场是同一个概念。设 \(X_1,\ldots,X_s\) 是 \(\mathfrak{X}(M)\) 中的向量场，并定义一个函数

\[
\omega(X_1,\ldots,X_s):M\longrightarrow\mathbb{R}
\]

如下：

\[
\omega(X_1,\ldots,X_s)(p)=\omega_p(X_1|_p,\ldots,X_s|_p)
\]

其中 \(p\) 取遍 \(M\) 中的所有点。我们说 \(\omega\) 是 **在 \(M\) 上光滑** 的，如果对于 \(\mathfrak{X}(M)\) 中的所有向量场 \(X_1,\ldots,X_s\)，函数 \(\omega(X_1,\ldots,X_s)\) 都在 \(C^\infty(M)\) 中。\(M\) 上光滑 \(s\)-形式的集合记为 \(\Lambda^s(M)\)。显然，\(\Lambda^s(M)\) 是 \(\mathcal{T}^0_s(M)\) 的一个 \(\mathbb{R}\)-子空间和 \(C^\infty(M)\)-子模，并且

\[
\Lambda^1(M)=\mathfrak{X}^*(M)=\mathcal{T}^0_1(M). \tag{15.8.1}
\]

为完整起见，并且为了与 (15.6.3) 保持一致，我们定义

\[
\Lambda^0(M)=C^\infty(M). \tag{15.8.2}
\]

根据定理 7.2.12(b)，当 \(s>m\) 时，我们令 \(\Lambda^s(M)=\{0\}\)。

设 \(\omega\) 和 \(\xi\) 分别是 \(\Lambda^s(M)\) 和 \(\Lambda^{s'}(M)\) 中的形式。我们定义一个 \(\Lambda^{s+s'}(M)\) 中的形式 \(\omega\wedge\xi\)，称为 \(\omega\) 和 \(\xi\) 的**楔积**，定义为

\[
(\omega\wedge\xi)_p=\omega_p\wedge\xi_p
\]

其中 \(p\) 取遍 \(M\) 中的所有点。特别地，对于 \(C^\infty(M)=\Lambda^0(M)\) 中的函数 \(f\)，有

\[
f\wedge\omega=f\omega. \tag{15.8.3}
\]

设 \((U,(x^i))\) 是 \(M\) 上的一个坐标卡，令 \(1\le i_1<\cdots<i_s\le m\) 是整数，并令

\[
d(x^{i_1})\wedge\cdots\wedge d(x^{i_s}):U\longrightarrow \Lambda^s(U)
\]

为根据对应关系

\[
p\longmapsto d_p(x^{i_1})\wedge\cdots\wedge d_p(x^{i_s})
\]

定义的映射，其中 \(p\) 取遍 \(U\) 中的所有点。设 \(\omega\) 是 \(\Lambda^s(M)\) 中的一个形式。那么 \(\omega|_U\) 可以表示为

\[
\omega|_U=\sum_{1\le i_1<\cdots<i_s\le m}\alpha_{i_1,\ldots,i_s}\,d(x^{i_1})\wedge\cdots\wedge d(x^{i_s}),\tag{15.8.4}
\]

其中 \(\alpha_{i_1,\ldots,i_s}\) 是 \(C^\infty(U)\) 中唯一确定的函数，称为 \(\omega\) 关于 \((U,(x^i))\) 的**分量**。为简洁起见，我们用 \(\omega\) 表示 \(\omega|_U\)。

(15.8.4) 的右端称为在 **局部坐标** 下关于 \((U,(x^i))\) 表达 \(\omega\)。

**定理 15.8.1.** 设 \(M\) 是一个光滑流形，设 \(\omega,\omega^1,\omega^2\) 和 \(\xi,\xi^1,\xi^2\) 分别是 \(\Lambda^s(M)\) 和 \(\Lambda^{s'}(M)\) 中的形式，并设 \(f\) 是 \(C^\infty(M)\) 中的一个函数。则：

- (a) \((\omega^1+\omega^2)\wedge\xi=\omega^1\wedge\xi+\omega^2\wedge\xi.\)
- (b) \(\omega\wedge(\xi^1+\xi^2)=\omega\wedge\xi^1+\omega\wedge\xi^2.\)
- (c) \((f\omega)\wedge\xi=f(\omega\wedge\xi)=\omega\wedge(f\xi)\)。

*证明.* 这是定理 7.2.2 的流形版本。

**定理 15.8.2.** 设 \(M\) 是一个光滑流形，设 \(\omega\) 和 \(\xi\) 分别是 \(\Lambda^s(M)\) 和 \(\Lambda^{s'}(M)\) 中的形式。则：

- (a) \(\omega\wedge\xi=(-1)^{ss'}\xi\wedge\omega.\)
- (b) 若 \(s=s'=1\)，则 \(\omega\wedge\xi=-\xi\wedge\omega.\)
- (c) 若 \(s=1\)，则 \(\omega\wedge\omega=0.\)

*证明.* 这是定理 7.2.3 的流形版本。

**定理 15.8.3.** 设 \(M\) 是一个光滑流形，设 \(\omega^1,\ldots,\omega^s\) 是 \(\mathfrak{X}^*(M)\) 中的余向量场，并设 \(X_1,\ldots,X_s\) 是 \(\mathfrak{X}(M)\) 中的向量场。则：

- (a)
\[
\omega^1\wedge\cdots\wedge\omega^s(X_1,\ldots,X_s)=
\det\begin{pmatrix}
\omega^1(X_1)&\cdots&\omega^1(X_s)\\
\vdots&\ddots&\vdots\\
\omega^s(X_1)&\cdots&\omega^s(X_s)
\end{pmatrix}.
\]

- (b) 若对于某个 \(1\le i<j\le s\) 有 \(\omega^i=\omega^j\)，则 \(\omega^1\wedge\cdots\wedge\omega^s=0\)。

*证明.* 这是定理 7.2.6 的流形版本与定理 7.2.8 的流形版本的结合。

**定理 15.8.4.** 设 \(M\) 是一个光滑 \(m\)-流形，并设 \(U\) 是 \(M\) 中的一个开集。若 \((X_1,\ldots,X_m)\) 是 \(U\) 上的一个标架，且 \((\omega^1,\ldots,\omega^m)\) 是其对偶标架，则

\[
\omega^{j_1}\wedge\cdots\wedge\omega^{j_s}(X_{i_1},\ldots,X_{i_s})=
\begin{cases}
1 & \text{if } (i_1,\ldots,i_s)=(j_1,\ldots,j_s),\\
0 & \text{if } (i_1,\ldots,i_s)\neq(j_1,\ldots,j_s).
\end{cases}
\]

对一切 \(1\le i_1<\cdots<i_s\le m\) 和 \(1\le j_1<\cdots<j_s\le m\) 成立。特别地，若 \((U,(x^i))\) 是 \(M\) 上的一个坐标卡，且 \((\partial/\partial x^1,\ldots,\partial/\partial x^m)\) 和 \((d(x^1),\ldots,d(x^m))\) 分别是相应的坐标标架和对偶坐标标架，则

\[
d(x^{j_1})\wedge\cdots\wedge d(x^{j_s})(\partial/\partial x^{i_1},\ldots,\partial/\partial x^{i_s})=
\begin{cases}
1 & \text{if } (i_1,\ldots,i_s)=(j_1,\ldots,j_s),\\
0 & \text{if } (i_1,\ldots,i_s)\neq(j_1,\ldots,j_s).
\end{cases}
\]

对一切 \(1\le i_1<\cdots<i_s\le m\) 和 \(1\le j_1<\cdots<j_s\le m\) 成立。

*证明.* 这是定理 7.2.9 的流形版本。

**定理 15.8.5.** 设 \(M\) 是一个光滑 \(m\)-流形，设 \((U,(x^i))\) 是 \(M\) 上的一个坐标卡，并设 \((d(x^1),\ldots,d(x^m))\) 是相应的对偶坐标标架。若 \(\omega\) 是 \(\Lambda^m(U)\) 中的一个形式，则存在唯一确定的函数 \(f\in C^\infty(U)\)，使得 \(\omega = f\, d(x^1)\wedge\cdots\wedge d(x^m)\)。

*证明.* 设 \((\partial/\partial x^1,\ldots,\partial/\partial x^m)\) 是与 \((U,(x^i))\) 对应的坐标标架，为简洁起见，令 \(\Omega = d(x^1)\wedge\cdots\wedge d(x^m)\)。对于 \(U\) 中的每个点 \(p\)，根据定义，\(\omega_p\) 是 \(T_p^*(U)\) 中的一个余向量。由定理 7.2.12(d) 可知，存在某个实数 \(f(p)\) 使得 \(\omega_p = f(p)\Omega_p\)。对应关系 \(p\longmapsto f(p)\) 定义了函数 \(f:U\longrightarrow\mathbb{R}\)，使得 \(\omega = f\Omega\)。由于 \(\omega\) 属于 \(\Lambda^m(U)\)，根据定义，对 \(\mathfrak{X}(U)\) 中的所有向量场 \(X_1,\ldots,X_m\)，\(\omega(X_1,\ldots,X_m)\) 都是 \(C^\infty(U)\) 中的函数。特别地，对 \(\partial/\partial x^1,\ldots,\partial/\partial x^m\) 也如此。由定理 15.8.4，

\[
f = f\,\Omega\left(\frac{\partial}{\partial x^1},\ldots,\frac{\partial}{\partial x^m}\right) = \omega\left(\frac{\partial}{\partial x^1},\ldots,\frac{\partial}{\partial x^m}\right),
\]

因此 \(f\in C^\infty(U)\)。\(f\) 的唯一性由前面的计算得出。

## 15.9 函数的前推与拉回

设 \(M\) 是一个光滑流形，并设 \(F : M \longrightarrow N\) 是一个微分同胚。由 \(F\) 诱导的前推（对于函数）是如下映射

\[
F_* : C^\infty(M) \longrightarrow C^\infty(N)
\]

定义为

\[
F_*(f) = f \circ F^{-1} \tag{15.9.1}
\]

其中 \(f\) 取遍 \(C^\infty(M)\) 中的所有函数。

**定理 15.9.1.** 若 \(M, N\) 和 \(P\) 都是光滑流形，且 \(F : M \longrightarrow N\) 和 \(G : N \longrightarrow P\) 都是微分同胚，则

\[
(G \circ F)_* = G_* \circ F_*.
\]

*证明.* 对于 \(C^\infty(M)\) 中的一个函数 \(f\)，我们有

\[
(G \circ F)_*(f) = f \circ (G \circ F)^{-1} = (f \circ F^{-1}) \circ G^{-1} = F_*(f) \circ G^{-1} = G_*(F_*(f)) = (G_* \circ F_*)(f).
\]

由于 \(f\) 是任意的，结论得证。

设 \(M\) 和 \(N\) 是光滑流形，并设 \(F : M \longrightarrow N\) 是一个光滑映射（但不一定是微分同胚）。由 \(F\) 诱导的拉回（对于函数）是如下映射

\[
F^* : C^\infty(N) \longrightarrow C^\infty(M)
\]

定义为

\[
F^*(g) = g \circ F \tag{15.9.2}
\]

其中 \(g\) 取遍 \(C^\infty(N)\) 中的所有函数。

**定理 15.9.2.** 若 \(M, N\) 和 \(P\) 都是光滑流形，且 \(F : M \longrightarrow N\) 和 \(G : N \longrightarrow P\) 都是光滑映射，则

\[
(G \circ F)^* = F^* \circ G^*.
\]

*证明.* 对于 \(C^\infty(P)\) 中的一个函数 \(g\)，我们有

\[
(G \circ F)^*(g) = (g \circ G) \circ F = G^*(g) \circ F = F^*(G^*(g)) = (F^* \circ G^*)(g).
\]

由于 \(g\) 是任意的，结论得证。

**定理 15.9.3.** 若 \(M\) 是一个光滑流形且 \(F : M \longrightarrow N\) 是一个微分同胚，则：

- (a) \(F^*\) 是双射，其逆为 \((F^*)^{-1} = (F^{-1})^* = F_*\)。
- (b) \(F_*\) 是双射，其逆为 \((F_*)^{-1} = (F^{-1})_* = F^*\)。

*证明.* (a)：由定理 15.9.2，

\[
\mathrm{id}_{C^\infty(M)} = (\mathrm{id}_M)^* = (F^{-1} \circ F)^* = F^* \circ (F^{-1})^*
\]

且

\[
\mathrm{id}_{C^\infty(N)} = (\mathrm{id}_N)^* = (F \circ F^{-1})^* = (F^{-1})^* \circ F^*.
\]

由定理 A.4 可知 \(F^*\) 是双射。前面的恒等式给出第一个等式。对于 \(C^\infty(M)\) 中的一个函数 \(f\)，我们有

\[
(F^{-1})^*(f) = f \circ F^{-1} = F_*(f).
\]

由于 \(f\) 是任意的，第二个等式成立。

(b)：由 (a) 直接得出。




## 15.10 向量场的前推与拉回

设 $M$ 和 $N$ 是光滑流形，$F : M \longrightarrow N$ 是光滑映射，$X$ 是 $\mathfrak{X}(M)$ 中的向量场，$p$ 是 $M$ 中的一个点。根据定义，$d_p(F)(X_p)$ 是 $T_{F(p)}(N)$ 中的一个向量。在没有任何额外假设的情况下，指派 $p \longmapsto d_p(F)(X_p)$ 不一定产生 $\mathfrak{X}(N)$ 中的向量场。例如，若 $F$ 不是满射，则无法在 $F$ 的像之外的点上指定向量。此外，若 $F$ 不是单射，则存在 $M$ 中不同的点 $p_1, p_2$，使得 $F(p_1)=F(p_2)$。当 $d_{p_1}(F)(X_{p_1}) \neq d_{p_2}(F)(X_{p_2})$ 时，为 $F(p_1)$ 指定向量就不会有明确的方式。解决这一困境的办法是像我们现在这样假设 $F$ 是一个微分同胚。在下面的讨论中，我们使用定理 15.2.1 将 $\mathfrak{X}(M)$ 中的向量场与 $\mathrm{Der}(M)$ 中的导子等同起来。

**$X$ 由 $F$ 的前推**是 $\mathfrak{X}(N)$ 中由

$$
F_*(X) = F_\bullet \circ X \circ F^\bullet. \tag{15.10.1}
$$

定义的向量场 $F_*(X)$。这一定义是合理的，因为我们有映射 $F^\bullet : C^\infty(N) \longrightarrow C^\infty(M)$，$X : C^\infty(M) \longrightarrow C^\infty(M)$ 和 $F_\bullet : C^\infty(M) \longrightarrow C^\infty(N)$，因此

$$
F_*(X) : C^\infty(N) \longrightarrow C^\infty(N).
$$

**$F$ 的前推（就向量场而言）**是映射

$$
F_* : \mathfrak{X}(M) \longrightarrow \mathfrak{X}(N)
$$

由指派

$$
X \longmapsto F_*(X)
$$

对所有 $\mathfrak{X}(M)$ 中的向量场 $X$ 定义。**$F$ 的拉回（就向量场而言）**是映射

$$
F^* : \mathfrak{X}(N) \longrightarrow \mathfrak{X}(M)
$$

由

$$
F^* = (F^{-1})_*;
$$

即

$$
F^*(Y) = (F^{-1})_*(Y) \tag{15.10.2}
$$

对所有 $\mathfrak{X}(N)$ 中的向量场 $Y$ 定义。我们称 $F^*(Y)$ 为 **$Y$ 由 $F$ 的拉回**，并注意到它等于 $Y$ 由 $F^{-1}$ 的前推。

**记号 $F_*$ 和 $F^*$ 仅当 $F$ 是微分同胚时才使用。**

下一个结果说明，前推对引言中提出的问题给出了回答。

### 定理 15.10.1。

设 $M$ 和 $N$ 是光滑流形，$F : M \longrightarrow N$ 是微分同胚，$X$ 是 $\mathfrak{X}(M)$ 中的向量场。则

$$
d_p(F)(X_p) = F_*(X)_{F(p)}
$$

对所有 $p \in M$ 成立。

**注。** 仔细查看定义，我们发现 $d_p(F)(X_p)$ 和 $F_*(X)_{F(p)}$ 都是 $T_{F(p)}(N)$ 中的向量，因此这一断言是有意义的。在文献中，记号 $F_{*p}$ 或简写 $F_*$ 常常用来代替 $d_p(F)$。实际上，即使 $F$ 不是微分同胚，这种记号也经常被采用。

**证明。** 对于 $C^\infty(N)$ 中的函数 $g$，我们有

$$
\begin{aligned}
d_p(F)(X_p)(g)
&= X_p(g \circ F) & [(14.4.1)] \\
&= X(g \circ F)(p) & [(15.1.2)] \\
&= (X(F^\bullet(g)) \circ F^{-1})(F(p)) & [(15.9.2)] \\
&= F_\bullet(X(F^\bullet(g)))(F(p)) & [(15.9.1)] \\
&= (F_\bullet \circ X \circ F^\bullet)(g)(F(p)) \\
&= F_*(X)(g)(F(p)) & [(15.10.1)] \\
&= F_*(X)_{F(p)}(g). & [(15.1.2)]
\end{aligned}
$$

由于 $g$ 是任意的，结论成立。

### 定理 15.10.2。

设 $M$、$N$ 和 $P$ 是光滑流形，$F : M \longrightarrow N$ 和 $G : N \longrightarrow P$ 是微分同胚，$Y$ 是 $\mathfrak{X}(N)$ 中的向量场。则：

- (a) $(G \circ F)_* = G_* \circ F_*$
- (b) $(G \circ F)^* = F^* \circ G^*$
- (c) $F^*(Y) = F^\bullet \circ Y \circ F_\bullet$。

**证明。** (a)：对于 $\mathfrak{X}(M)$ 中的向量场 $X$，我们有

$$
\begin{aligned}
(G \circ F)_*(X)
&= (G \circ F)_\bullet \circ X \circ (G \circ F)^\bullet & [(15.10.1)] \\
&= G_\bullet \circ (F_\bullet \circ X \circ F^\bullet) \circ G^\bullet & [\mathrm{Th}\ 15.9.1,\ \mathrm{Th}\ 15.9.2] \\
&= G_\bullet \circ F_*(X) \circ G^\bullet & [(15.10.1)] \\
&= G_*(F_*(X)) & [(15.10.1)] \\
&= (G_* \circ F_*)(X).
\end{aligned}
$$

由于 $X$ 是任意的，结论成立。

(b)：对于 $\mathfrak{X}(P)$ 中的向量场 $Z$，我们有

$$
\begin{aligned}
(G \circ F)^*(Z)
&= ((G \circ F)^{-1})_*(Z) & [(15.10.2)] \\
&= (F^{-1} \circ G^{-1})_*(Z) \\
&= ((F^{-1})_* \circ (G^{-1})_*)(Z) & [\text{part (a)}] \\
&= (F^* \circ G^*)(Z). & [(15.10.2)]
\end{aligned}
$$

由于 $Z$ 是任意的，结论成立。

(c)：我们有

$$
\begin{aligned}
F^*(Y)
&= (F^{-1})_*(Y) & [(15.10.2)] \\
&= (F^{-1})_\bullet \circ Y \circ (F^{-1})^\bullet & [(15.10.1)] \\
&= F^\bullet \circ Y \circ F_\bullet & [\mathrm{Th}\ 15.9.3]
\end{aligned}
$$

### 定理 15.10.3。

若 $M$ 和 $N$ 是光滑流形，且 $F : M \longrightarrow N$ 是微分同胚，则：

- (a) $F^*$ 是双射，其逆为 $(F^*)^{-1} = (F^{-1})^* = F_*$。
- (b) $F_*$ 是双射，其逆为 $(F_*)^{-1} = (F^{-1})_* = F^*$。

**证明。** 其论证与定理 15.9.3 的证明中所用的论证类似。

### 定理 15.10.4。

设 $M$ 和 $N$ 是光滑流形，$F : M \longrightarrow N$ 是微分同胚，$Y$ 是 $\mathfrak{X}(N)$ 中的向量场，$g$ 是 $C^\infty(N)$ 中的函数。则

$$
F^\bullet(Y(g)) = F^*(Y)(F^\bullet(g)).
$$

**证明。** 我们有

$$
\begin{aligned}
F^*(Y)(F^\bullet(g))
&= (F^\bullet \circ Y \circ F_\bullet)(F^\bullet(g)) & [\mathrm{Th}\ 15.10.2(c)] \\
&= F^\bullet(Y(g)). & [\mathrm{Th}\ 15.9.3]
\end{aligned}
$$

## 15.11 余向量场的拉回

设 $M$ 和 $N$ 是光滑流形，$F : M \longrightarrow N$ 是光滑映射，$p$ 是 $M$ 中的一个点。相应的微分映射为 $d_p(F) : T_p(M) \longrightarrow T_{F(p)}(N)$。根据 (7.1.2)，$\Lambda^1(T_p(M)) = T_p^*(M)$ 且 $\Lambda^1(T_{F(p)}(N)) = T_{F(p)}^*(N)$，因此由 (7.3.1) 可知，对余向量而言，由 $d_p(F)$ 诱导的拉回是映射

$$
d_p(F)^* : T_{F(p)}^*(N) \longrightarrow T_p^*(M)
$$

定义为

$$
d_p(F)^*(\eta)(v) = \eta(d_p(F)(v)) \tag{15.11.1}
$$

对所有 $T_{F(p)}^*(N)$ 中的余向量 $\eta$ 和 $T_p(M)$ 中的所有向量 $v$ 成立。$F$ 的拉回（就余向量场而言）是线性映射

$$
F^* : \mathfrak{X}^*(N) \longrightarrow \mathfrak{X}^*(M)
$$

定义为

$$
F^*(\omega)_p(v) = d_p(F)^*(\omega_{F(p)})(v) = \omega_{F(p)}(d_p(F)(v)) \tag{15.11.2}
$$

对所有 $\mathfrak{X}^*(N)$ 中的余向量场 $\omega$、$M$ 中的所有点 $p$ 以及 $T_p(M)$ 中的所有向量 $v$ 成立，其中第二个等式由在 (15.11.1) 中令 $\eta = \omega_{F(p)}$ 得到。我们称

**注。** 对于 (15.11.3) 的左边，我们有映射 $d : C^\infty(N) \to \mathfrak{X}^*(N)$ 和 $F^* : \mathfrak{X}^*(N) \to \mathfrak{X}^*(M)$；对于右边，我们有映射 $F^\bullet : C^\infty(N) \to C^\infty(M)$ 和 $d : C^\infty(M) \to \mathfrak{X}^*(M)$，因此该断言是有意义的。

**证明。** 对于点 $p \in M$ 和向量 $v \in T_p(M)$，我们有

$$
\begin{aligned}
F^*(d(g))_p(v)
&= (d(g))_{F(p)}(d_p(F)(v)) & [(15.11.2)] \\
&= d_{F(p)}(g)(d_p(F)(v)) & [(15.4.4)] \\
&= d_{F(p)}(g) \circ d_p(F)(v) \\
&= d_p(g \circ F)(v) & [\mathrm{Th}\ 14.4.5] \\
&= d(g \circ F)_p(v) & [(15.4.4)] \\
&= d(F^\bullet(g))_p(v). & [(15.9.2)]
\end{aligned}
$$

由于 $p$、$v$ 和 $g$ 都是任意的，结论成立。

### 定理 15.11.3（余向量场的拉回）。

设 $M$ 和 $N$ 是光滑流形，$F : M \longrightarrow N$ 是光滑映射，$(U,(x^i))$ 和 $(V,(y^j))$ 分别是 $M$ 和 $N$ 上的坐标卡，且 $U \cap F^{-1}(V)$ 非空。设 $\omega$ 是 $\mathfrak{X}^*(N)$ 中的余向量场，在局部坐标下记

$$
\omega = \sum_j \alpha_j d(y^j).
$$

则

$$
F^*(\omega) = \sum_i \left(\sum_j (\alpha_j \circ F)\frac{\partial(y^j \circ F)}{\partial x^i}\right)d(x^i).
$$

**证明。** 我们有

$$
\begin{aligned}
F^*(\omega)
&= F^*\left(\sum_j \alpha_j d(y^j)\right) \\
&= \sum_j F^\bullet(\alpha_j)F^*(d(y^j)) & [\mathrm{Th}\ 15.11.1] \\
&= \sum_j F^\bullet(\alpha_j)d(F^\bullet(y^j)) & [\mathrm{Th}\ 15.11.2] \\
&= \sum_j (\alpha_j \circ F)d(y^j \circ F), & [(15.9.2)]
\end{aligned}
$$

并且由定理 15.4.7 可得

$$
d(y^j \circ F) = \sum_i \frac{\partial(y^j \circ F)}{\partial x^i}\,d(x^i).
$$

综合上述等式即得结论。

### 例 15.11.4。

设 $f : \mathbb{R} \to \mathbb{R}$ 是由 $f(t)=t^2$ 给出的函数，并设 $\omega$ 是 $\mathfrak{X}^*(\mathbb{R})$ 中由 $\omega_r = \alpha(r)d(r)$ 定义的余向量场，其中 $\alpha$ 是 $C^\infty(\mathbb{R})$ 中的函数。由定理 15.11.3，

$$
f^*(\omega)_t = \alpha(f(t))\frac{df}{dt}(t)d(t) = 2t\,\alpha(t^2)d(t).
$$

另一种做法是，在 $\alpha(r)d(r)$ 中代入 $r=t^2$ 和 $d(r)=2t\,d(t)$，同样得到 $2t\,\alpha(t^2)d(t)$。这说明“拉回”运算不过是一元实变量微分学中熟悉的“代入”技巧。

### 例 15.11.5。

设 $(r,s)$ 和 $(x,y,z)$ 分别是 $\mathbb{R}^2$ 和 $\mathbb{R}^3$ 上的标准坐标，$F : \mathbb{R}^3 \to \mathbb{R}^2$ 是由

$$
F(x,y,z) = (x^2y,\ y\sin(z)),
$$

给出的光滑映射，并考虑 $\mathfrak{X}^*(\mathbb{R}^2)$ 中的余向量场

$$
\omega_{(r,s)} = s\,d(r) + r\,d(s)
$$

在定理 15.11.3 中令 $(U,(x^i)) = (\mathbb{R}^3,(x,y,z))$、$(V,(y^j)) = (\mathbb{R}^2,(r,s))$，得到

$$
\begin{aligned}
F^*(\omega)_{(x,y,z)}
={}&\left((s \circ F)\frac{\partial(r \circ F)}{\partial x} + (r \circ F)\frac{\partial(s \circ F)}{\partial x}\right)d(x)\\
&+\left((s \circ F)\frac{\partial(r \circ F)}{\partial y} + (r \circ F)\frac{\partial(s \circ F)}{\partial y}\right)d(y)\\
&+\left((s \circ F)\frac{\partial(r \circ F)}{\partial z} + (r \circ F)\frac{\partial(s \circ F)}{\partial z}\right)d(z)\\
={}&\left(y\sin(z)\frac{\partial(x^2y)}{\partial x} + x^2y\frac{\partial(y\sin(z))}{\partial x}\right)d(x)\\
&+\left(y\sin(z)\frac{\partial(x^2y)}{\partial y} + x^2y\frac{\partial(y\sin(z))}{\partial y}\right)d(y)\\
&+\left(y\sin(z)\frac{\partial(x^2y)}{\partial z} + x^2y\frac{\partial(y\sin(z))}{\partial z}\right)d(z)\\
={}&2xy^2\sin(z)\,d(x) + 2x^2y\sin(z)\,d(y) + x^2y^2\cos(z)\,d(z).
\end{aligned}
$$

### 例 15.11.6（极坐标）。

设 $(r,s)$ 是 $\mathbb{R}^2$ 上的标准坐标，$(\rho,\phi)$ 是

$$
U = \{(\rho,\phi) \in \mathbb{R}^2 : \rho > 0,\ 0 \leq \phi < 2\pi\}.
$$

上的极坐标。设 $F : U \to \mathbb{R}^2$ 是由

$$
F(\rho,\phi) = (\rho\cos(\phi),\rho\sin(\phi)),
$$

给出的光滑映射。




and consider the covector field

\[
\omega_{(r,s)}=-s\,d(r)+r\,d(s)
\]

in \(\mathfrak{X}^*(\mathbb{R}^2)\). Setting \((U,(x^i))=(U,(\rho,\phi))\) and \((V,(y^j))=(\mathbb{R}^2,(r,s))\) in Theorem 15.11.3 yields

\[
\begin{aligned}
F^*(\omega)_{(\rho,\phi)}
&=\left((-s\circ F)\frac{\partial(r\circ F)}{\partial \rho}
+(r\circ F)\frac{\partial(s\circ F)}{\partial \rho}\right)d(\rho)\\
&\quad+\left((-s\circ F)\frac{\partial(r\circ F)}{\partial \phi}
+(r\circ F)\frac{\partial(s\circ F)}{\partial \phi}\right)d(\phi)\\
&=\left(-\rho\sin(\phi)\frac{\partial(\rho\cos(\phi))}{\partial \rho}
+\rho\cos(\phi)\frac{\partial(\rho\sin(\phi))}{\partial \rho}\right)d(\rho)\\
&\quad+\left(-\rho\sin(\phi)\frac{\partial(\rho\cos(\phi))}{\partial \phi}
+\rho\cos(\phi)\frac{\partial(\rho\sin(\phi))}{\partial \phi}\right)d(\phi)\\
&=\rho^2 d(\phi).
\end{aligned}
\]

### Example 15.11.7 (Polar Coordinates and Punctured Plane)

Let \((r,s)\) be standard coordinates on the set \(\mathbb{R}^2\setminus\{(0,0)\}\), called the punctured plane, and let \((\rho,\phi)\) be polar coordinates on

\[
U=\{(\rho,\phi)\in\mathbb{R}^2:\rho>0,\ 0\le\phi<2\pi\}.
\]

Let \(F:U\longrightarrow\mathbb{R}^2\setminus\{(0,0)\}\) be the smooth map given by

\[
F(\rho,\phi)=(\rho\cos(\phi),\rho\sin(\phi)),
\]

and consider the covector field

\[
\omega_{(r,s)}=-\frac{s}{r^2+s^2}\,d(r)+\frac{r}{r^2+s^2}\,d(s)
\]

in \(\mathfrak{X}^*(\mathbb{R}^2\setminus\{(0,0)\})\). Setting \((U,(x^i))=(U,(\rho,\phi))\) and \((V,(y^j))=(\mathbb{R}^2\setminus\{(0,0)\},(r,s))\) in Theorem 15.11.3 yields

\[
\begin{aligned}
F^*(\omega)_{(\rho,\phi)}
&=\left[\left(-\frac{s}{r^2+s^2}\circ F\right)
\frac{\partial(r\circ F)}{\partial \rho}
+\left(\frac{r}{r^2+s^2}\circ F\right)
\frac{\partial(s\circ F)}{\partial \rho}\right]d(\rho)\\
&\quad+\left[\left(-\frac{s}{r^2+s^2}\circ F\right)
\frac{\partial(r\circ F)}{\partial \phi}
+\left(\frac{r}{r^2+s^2}\circ F\right)
\frac{\partial(s\circ F)}{\partial \phi}\right]d(\phi)\\
&=\left(-\frac{\rho\sin(\phi)}{[\rho\cos(\phi)]^2+[\rho\sin(\phi)]^2}
\frac{\partial(\rho\cos(\phi))}{\partial \rho}
+\frac{\rho\cos(\phi)}{[\rho\cos(\phi)]^2+[\rho\sin(\phi)]^2}
\frac{\partial(\rho\sin(\phi))}{\partial \rho}\right)d(\rho)\\
&\quad+\left(-\frac{\rho\sin(\phi)}{[\rho\cos(\phi)]^2+[\rho\sin(\phi)]^2}
\frac{\partial(\rho\cos(\phi))}{\partial \phi}
+\frac{\rho\cos(\phi)}{[\rho\cos(\phi)]^2+[\rho\sin(\phi)]^2}
\frac{\partial(\rho\sin(\phi))}{\partial \phi}\right)d(\phi)\\
&=d(\phi).
\end{aligned}
\]

### Theorem 15.11.8

Let \(M\) be a smooth manifold, let \(\lambda:(a,b)\longrightarrow M\) be a smooth curve, let \(\omega\) be a covector field in \(\mathfrak{X}^*(M)\), and view \((a,b)\) as a smooth \(1\)-manifold. Then

\[
\lambda^*(\omega)_t=\omega_{\lambda(t)}\!\left(\frac{d\lambda}{dt}(t)\right)d(t)
\]

for all \(t\in(a,b)\).

**Proof.** Let \(p\) be a point in \(M\), let \((U,(x^i))\) be a chart at \(p\), and, in local coordinates, let

\[
\omega=\sum_i\alpha_i\,d(x^i).
\]

By Theorem 15.11.3,

\[
\lambda^*(\omega)_t=\left(\sum_i\alpha_i(\lambda(t))
\frac{d(x^i\circ\lambda)}{dt}(t)\right)d(t). \tag{15.11.5}
\]

On the other hand, (15.4.4) gives

\[
\omega_{\lambda(t)}=\sum_j\alpha_j(\lambda(t))\,d(x^j)_{\lambda(t)}
=\sum_j\alpha_j(\lambda(t))\,d_{\lambda(t)}(x^j),
\]

hence

\[
\omega_{\lambda(t)}\!\left(\left.\frac{\partial}{\partial x^i}\right|_{\lambda(t)}\right)
=\sum_j\left[\alpha_j(\lambda(t))\,d_{\lambda(t)}(x^j)
\left(\left.\frac{\partial}{\partial x^i}\right|_{\lambda(t)}\right)\right]
=\alpha_i(\lambda(t))
\]

for \(i=1,\dots,m\). By Theorem 14.7.1(b),

\[
\frac{d\lambda}{dt}(t)=\sum_i\frac{d(x^i\circ\lambda)}{dt}(t)
\left.\frac{\partial}{\partial x^i}\right|_{\lambda(t)},
\]

so

\[
\omega_{\lambda(t)}\!\left(\frac{d\lambda}{dt}(t)\right)
=\sum_i\frac{d(x^i\circ\lambda)}{dt}(t)\,
\omega_{\lambda(t)}\!\left(\left.\frac{\partial}{\partial x^i}\right|_{\lambda(t)}\right)
=\sum_i\alpha_i(\lambda(t))\frac{d(x^i\circ\lambda)}{dt}(t). \tag{15.11.6}
\]

Substituting (15.11.6) into (15.11.5) gives the result. \(\square\)

## 15.12 Pullback of Covariant Tensor Fields

Let \(M\) and \(N\) be smooth manifolds, let \(F:M\longrightarrow N\) be a smooth map, and let \(p\) be a point in \(M\). The corresponding differential map is \(d_p(F):T_p(M)\longrightarrow T_{F(p)}(N)\). According to (5.2.1), the pullback by \(d_p(F)\) for covariant tensors is the family of linear maps

\[
d_p(F)^*:\mathcal{T}_s^0(T_{F(p)}(N))\longrightarrow\mathcal{T}_s^0(T_p(M))
\]

defined for \(s\ge 1\) by

\[
d_p(F)^*(\mathcal{B})(v_1,\dots,v_s)
=\mathcal{B}(d_p(F)(v_1),\dots,d_p(F)(v_s)) \tag{15.12.1}
\]

for all tensors \(\mathcal{B}\) in \(\mathcal{T}_s^0(T_{F(p)}(N))\) and all vectors \(v_1,\dots,v_s\in T_p(M)\). Pullback by \(F\) (for covariant tensor fields) is the family of linear maps

\[
F^*:\mathcal{T}_s^0(N)\longrightarrow\mathcal{T}_s^0(M)
\]

defined for \(s\ge 1\) by

\[
F^*(\mathcal{A})_p(v_1,\dots,v_s)
=d_p(F)^*(\mathcal{A}_{F(p)})(v_1,\dots,v_s)
=\mathcal{A}_{F(p)}(d_p(F)(v_1),\dots,d_p(F)(v_s)) \tag{15.12.2}
\]

for all tensor fields \(\mathcal{A}\) in \(\mathcal{T}_s^0(N)\), all points \(p\in M\), and all vectors \(v_1,\dots,v_s\in T_p(M)\), where the second equality follows from setting \(\mathcal{B}=\mathcal{A}_{F(p)}\) in (15.12.1). We refer to \(F^*(\mathcal{A})\) as the pullback of \(\mathcal{A}\) by \(F\).

To give meaning to \(F^*\) when \(s=0\), recall from (15.6.3) that \(\mathcal{T}_0^0(N)=C^\infty(N)\). We therefore define

\[
F^*(g)(p)=g(F(p))=F^\bullet(g)(p)
\]

for all functions \(g\in C^\infty(N)\) and all \(p\in M\); that is, we define

\[
F^*=F^\bullet. \tag{15.12.3}
\]

### Theorem 15.12.1

Let \(M\), \(N\), and \(P\) be smooth manifolds, let \(F:M\longrightarrow N\) and \(G:N\longrightarrow P\) be smooth maps, let \(\mathcal{A}\), \(\mathcal{B}\), and \(\mathcal{C}\) be tensor fields in \(\mathcal{T}_s^0(N)\) and \(\mathcal{T}_{s'}^0(N)\), respectively, and let \(g\) be a function in \(C^\infty(N)\). Then:

- (a) \(F^*(\mathcal{A}+\mathcal{B})=F^*(\mathcal{A})+F^*(\mathcal{B})\).
- (b) \(F^*(g\mathcal{A})=F^*(g)F^*(\mathcal{A})\).
- (c) \(F^*(\mathcal{A}\otimes\mathcal{C})=F^*(\mathcal{A})\otimes F^*(\mathcal{C})\).
- (d) \((G\circ F)^*=F^*\circ G^*\).

Since \(g\otimes\mathcal{A}=g\mathcal{A}\), part (b) of Theorem 15.12.1 follows from part (c). Identity (15.12.3) has several implications: Theorem 15.9.2 follows from Theorem 15.12.1(d); the identity in Theorem 15.10.4 can be expressed as

\[
F^*(Y(g))=F^*(Y)(F^*(g))
\]

for all vector fields \(Y\in\mathfrak{X}(N)\) and all functions \(g\in C^\infty(N)\); Theorem 15.11.1(b) follows from Theorem 15.12.1(b); and (15.11.4) can be expressed as

\[
F^*(d(g))=d(F^*(g))
\]

for all functions \(g\in C^\infty(N)\).

### Theorem 15.12.2 (Pullback of Covariant Tensor Field)

Let \(M\) be a smooth \(m\)-manifold, let \(N\) be a smooth \(n\)-manifold, let \(F:M\to N\) be a smooth map, and let \((U,(x^j))\) and \((V,(y^i))\) be charts on \(M\) and \(N\), respectively, such that \(U\cap F^{-1}(V)\) is nonempty. Let \(\mathcal{A}\) be a tensor field in \(\mathcal{T}_s^0(N)\), and, in local coordinates, let

\[
\mathcal{A}=\sum_{1\le i_1,\ldots,i_s\le n}A_{i_1\ldots i_s}\,d(y^{i_1})\otimes\cdots\otimes d(y^{i_s}).
\]

Then

\[
F^*(\mathcal{A})=\sum_{1\le j_1,\ldots,j_s\le m}\left(\sum_{1\le i_1,\ldots,i_s\le n}(A_{i_1\ldots i_s}\circ F)
\frac{\partial(y^{i_1}\circ F)}{\partial x^{j_1}}\cdots
\frac{\partial(y^{i_s}\circ F)}{\partial x^{j_s}}\right)
d(x^{j_1})\otimes\cdots\otimes d(x^{j_s}).
\]

**Proof.** We have from Theorem 15.12.1 that

\[
F^*(\mathcal{A})=
F^*\left(\sum_{1\le i_1,\ldots,i_s\le n}A_{i_1\ldots i_s}\,d(y^{i_1})\otimes\cdots\otimes d(y^{i_s})\right)
=\sum_{1\le i_1,\ldots,i_s\le n}F^*(A_{i_1\ldots i_s})
F^*\left(d(y^{i_1})\otimes\cdots\otimes d(y^{i_s})\right), \tag{15.12.4}
\]

\[
F^*(A_{i_1\ldots i_s})=A_{i_1\ldots i_s}\circ F.
\]

We also have

\[
F^*\left(d(y^{i_1})\otimes\cdots\otimes d(y^{i_s})\right)
=F^*(d(y^{i_1}))\otimes\cdots\otimes F^*(d(y^{i_s})) \qquad [\mathrm{Th\ 15.12.1(c)}]
=d(F^*(y^{i_1}))\otimes\cdots\otimes d(F^*(y^{i_s})) \qquad [\mathrm{Th\ 15.11.2,\ (15.12.3)}]
=d(y^{i_1}\circ F)\otimes\cdots\otimes d(y^{i_s}\circ F). \qquad [(15.9.2),\ (15.12.3)] \tag{15.12.5}
\]

and, from Theorem 15.4.7,

\[
d(y^{i_k}\circ F)=\sum_{j_k}\frac{\partial(y^{i_k}\circ F)}{\partial x^{j_k}}\,d(x^{j_k})
\]

for \(k=1,\ldots,s\). Thus,

\[
F^*\left(d(y^{i_1})\otimes\cdots\otimes d(y^{i_s})\right)
=\left(\sum_{j_1}\frac{\partial(y^{i_1}\circ F)}{\partial x^{j_1}}d(x^{j_1})\right)\otimes\cdots\otimes
\left(\sum_{j_s}\frac{\partial(y^{i_s}\circ F)}{\partial x^{j_s}}d(x^{j_s})\right)
=\sum_{1\le j_1,\ldots,j_s\le m}
\frac{\partial(y^{i_1}\circ F)}{\partial x^{j_1}}\cdots
\frac{\partial(y^{i_s}\circ F)}{\partial x^{j_s}}
d(x^{j_1})\otimes\cdots\otimes d(x^{j_s}). \tag{15.12.6}
\]

Substituting (15.12.5) and (15.12.6) into (15.12.4) and reversing summations gives the result. \(\square\)

### Example 15.12.3 (Polar Coordinates)

Let \((r,s)\) be standard coordinates on \(\mathbb{R}^2\), and let \((\rho,\phi)\) be polar coordinates.

\[
U=\{(\rho,\phi)\in\mathbb{R}^2:\rho>0,\ 0\le\phi<2\pi\}.
\]

Let \(F:U\to\mathbb{R}^2\) be the smooth map given by

\[
F(\rho,\phi)=(\rho\cos(\phi),\rho\sin(\phi)),
\]

and consider the tensor field

\[
\mathbf{e}_{(r,s)}=d(r)\otimes d(r)+d(s)\otimes d(s)
\]

in \(\mathcal{T}_2^0(\mathbb{R}^2)\). Setting \((U,(x^i))=(U,(\rho,\phi))\) and \((V,(y^j))=(\mathbb{R}^2,(r,s))\) in Theorem 15.12.2, and observing that \(\mathbf{e}_{11}\circ F=\mathbf{e}_{22}\circ F=1\) and \(\mathbf{e}_{12}\circ F=\mathbf{e}_{21}\circ F=0\), yields

\[
\begin{aligned}
F^*(\mathbf{e})_{(\rho,\phi)}={}
&\left(\frac{\partial(r\circ F)}{\partial\rho}\frac{\partial(r\circ F)}{\partial\rho}
+\frac{\partial(s\circ F)}{\partial\rho}\frac{\partial(s\circ F)}{\partial\rho}\right)d(\rho)\otimes d(\rho)\\
&+\left(\frac{\partial(r\circ F)}{\partial\rho}\frac{\partial(r\circ F)}{\partial\phi}
+\frac{\partial(s\circ F)}{\partial\rho}\frac{\partial(s\circ F)}{\partial\phi}\right)d(\rho)\otimes d(\phi)\\
&+\left(\frac{\partial(r\circ F)}{\partial\phi}\frac{\partial(r\circ F)}{\partial\rho}
+\frac{\partial(s\circ F)}{\partial\phi}\frac{\partial(s\circ F)}{\partial\rho}\right)d(\phi)\otimes d(\rho)\\
&+\left(\frac{\partial(r\circ F)}{\partial\phi}\frac{\partial(r\circ F)}{\partial\phi}
+\frac{\partial(s\circ F)}{\partial\phi}\frac{\partial(s\circ F)}{\partial\phi}\right)d(\phi)\otimes d(\phi)\\
={}
&\left(\frac{\partial(\rho\cos(\phi))}{\partial\rho}\frac{\partial(\rho\cos(\phi))}{\partial\rho}
+\frac{\partial(\rho\sin(\phi))}{\partial\rho}\frac{\partial(\rho\sin(\phi))}{\partial\rho}\right)d(\rho)\otimes d(\rho)\\
&+\left(\frac{\partial(\rho\cos(\phi))}{\partial\rho}\frac{\partial(\rho\cos(\phi))}{\partial\phi}
+\frac{\partial(\rho\sin(\phi))}{\partial\rho}\frac{\partial(\rho\sin(\phi))}{\partial\phi}\right)d(\rho)\otimes d(\phi)\\
&+\left(\frac{\partial(\rho\cos(\phi))}{\partial\phi}\frac{\partial(\rho\cos(\phi))}{\partial\rho}
+\frac{\partial(\rho\sin(\phi))}{\partial\phi}\frac{\partial(\rho\sin(\phi))}{\partial\rho}\right)d(\phi)\otimes d(\rho)\\
&+\left(\frac{\partial(\rho\cos(\phi))}{\partial\phi}\frac{\partial(\rho\cos(\phi))}{\partial\phi}
+\frac{\partial(\rho\sin(\phi))}{\partial\phi}\frac{\partial(\rho\sin(\phi))}{\partial\phi}\right)d(\phi)\otimes d(\phi)\\
={}
&[\cos^2(\phi)+\sin^2(\phi)]d(\rho)\otimes d(\rho)\\
&+[-\rho\cos(\phi)\sin(\phi)+\rho\sin(\phi)\cos(\phi)]d(\rho)\otimes d(\phi)\\
&+[-\rho\sin(\phi)\cos(\phi)+\rho\cos(\phi)\sin(\phi)]d(\phi)\otimes d(\rho)\\
&+[\rho^2\sin^2(\phi)+\rho^2\cos^2(\phi)]d(\phi)\otimes d(\phi)\\
={}
&d(\rho)\otimes d(\rho)+\rho^2d(\phi)\otimes d(\phi).
\end{aligned}
\]

## 15.13 Pullback of Differential Forms

Let \(M\) and \(N\) be smooth manifolds, and let \(F:M\to N\) be a smooth map. In Section 15.12, we defined \(F^*:\mathcal{T}_s^0(N)\to\mathcal{T}_s^0(M)\), the pullback by \(F\) for



协变张量场。我们寻求微分形式对应的拉回。注意到 Λ^s(M) 是 T_s^0(M) 的子空间，Λ^s(N) 是 T_s^0(N) 的子空间，且 F*(Λ^s(N)) 是 Λ^s(M) 的子空间，因此我们可以通过限制 (15.12.1)–(15.12.3) 中定义的映射来继续进行。由 F 拉回（对于微分形式）是线性映射族

F* : Λ^s(N) ⟶ Λ^s(M)

定义为 s ≥ 1 时

F*(ω)_p(v1,…,vs) = d_p(F)*(ω_{F(p)})(v1,…,vs) = ω_{F(p)}(d_p(F)(v1),…,d_p(F)(vs))

对所有微分形式 ω ∈ Λ^s(N)、所有点 p ∈ M 以及所有向量 v1,…,vs ∈ T_p(M) 成立。我们将 F*(ω) 称为 ω 由 F 拉回。如前所述，当 s = 0 时，我们定义 F* = F•。

## 定理 15.13.1. 设 M、N 和 P 为光滑流形，设 F : M ⟶ N 和 G : N ⟶ P 为光滑映射，设 ω、ξ 和 ζ 为 Λ^s(N) 和 Λ^{s'}(N) 中的形式，分别设 g 为 C^∞(N) 中的函数。则：
- (a) F*(ω + ξ) = F*(ω) + F*(ξ)。
- (b) F*(gω) = F*(g)F*(ω)。
- (c) F*(ω ∧ ζ) = F*(ω) ∧ F*(ζ)。
- (d) (G ∘ F)* = F* ∘ G*。

## 定理 15.13.2. 设 M 为光滑 m-流形，设 f^1,…,f^s 为 C^∞(M) 中的函数，其中 s ≤ m。设 (U,(x^j)) 为 M 上的一个坐标图，设 (∂/∂x^1,…,∂/∂x^m) 和 (d(x^1),…,d(x^m)) 为相应的坐标和对偶坐标标架。则在局部坐标下，

d(f^1) ∧ ⋯ ∧ d(f^s) = Σ_{1≤j1<⋯<js≤m} det[ ∂f^1/∂x^1 … ∂f^1/∂x^m ; ⋮ ⋱ ⋮ ; ∂f^s/∂x^1 … ∂f^s/∂x^m ]_{(j1,…,js)} d(x^{j1}) ∧ ⋯ ∧ d(x^{js}).

证明. 在局部坐标下，设

在 (14.5.4) 和定理 15.8.3(a) 的帮助下，前述恒等式的左边可以表示为

d(f¹) ∧ ⋯ ∧ d(fˢ) ( ∂/∂x^{i₁}, …, ∂/∂x^{iₛ} ) = det ⎛⎜⎝ d(f¹)(∂/∂x^{i₁}) … d(f¹)(∂/∂x^{iₛ}) ⋮ ⋱ ⋮ d(fˢ)(∂/∂x^{i₁}) … d(fˢ)(∂/∂x^{iₛ}) ⎞⎟⎠ = det ⎛⎜⎝ ∂f¹/∂x^{i₁} … ∂f¹/∂x^{iₛ} ⋮ ⋱ ⋮ ∂fˢ/∂x^{i₁} … ∂fˢ/∂x^{iₛ} ⎞⎟⎠ = det ⎛⎜⎝ ∂f¹/∂x¹ … ∂f¹/∂xᵐ ⋮ ⋱ ⋮ ∂fˢ/∂x¹ … ∂fˢ/∂xᵐ ⎞⎟⎠_{(i₁,…,iₛ)}.

于是，

α_{j₁,…,jₛ} = det ⎛⎜⎝ ∂f¹/∂x¹ … ∂f¹/∂xᵐ ⋮ ⋱ ⋮ ∂fˢ/∂x¹ … ∂fˢ/∂xᵐ ⎞⎟⎠_{(j₁,…,jₛ)}. (15.13.2)

将 (15.13.2) 代入 (15.13.1) 得到结果。

## 定理 15.13.3 (微分形式的拉回). 设 M 为光滑 m-流形，设 N 为光滑 n-流形，设 F : M ⟶ N 为光滑映射，设 (U, (xⁱ)) 和 (V, (yʲ)) 为 M 和 N 上的坐标图，分别使得 U ∩ F⁻¹(V) 非空。设 ω 为 Λˢ(N) 中的形式，且在局部坐标下，设

ω = Σ_{1 ≤ j₁ < ⋯ < jₛ ≤ n} α_{j₁…jₛ} d(y^{j₁}) ∧ ⋯ ∧ d(y^{jₛ}), 其中 s ≤ min(m, n)。则：

(a) F*(ω) = Σ_{1 ≤ i₁ < ⋯ < iₛ ≤ m} [ Σ_{1 ≤ j₁ < ⋯ < jₛ ≤ n} (α_{j₁…jₛ} ∘ F) det( (J_F)_{(i₁,…,iₛ)}^{(j₁,…,jₛ)} ) ] · d(x^{i₁}) ∧ ⋯ ∧ d(x^{iₛ}), 其中

J_F = ⎡ ∂(y¹ ∘ F)/∂x¹ … ∂(y¹ ∘ F)/∂xᵐ ⎤ ⋮ ⋱ ⋮ ⎣ ∂(yⁿ ∘ F)/∂x¹ … ∂(yⁿ ∘ F)/∂xᵐ ⎦ (15.13.3)

为相应的雅可比矩阵。

(b) 若 $s = m = n$ 且 $\omega = \alpha\, d(y^1) \wedge \cdots \wedge d(y^m)$，则

$$F^*(\omega) = (\alpha \circ F)\,\det(J_F)\, d(x^1) \wedge \cdots \wedge d(x^m).$$

证明. (a): 我们由定理 15.13.1 有

$$F^*(\omega) = F^*\left(\sum_{1 \le j_1 < \cdots < j_s \le n} \alpha_{j_1 \dots j_s}\, d(y^{j_1}) \wedge \cdots \wedge d(y^{j_s})\right)$$ 

$$= \sum_{1 \le j_1 < \cdots < j_s \le n} F^*(\alpha_{j_1 \dots j_s})\, F^*\left(d(y^{j_1}) \wedge \cdots \wedge d(y^{j_s})\right), \qquad (15.13.4)$$ 

且由 (15.9.2) 和 (15.12.3) 有

$$F^*(\alpha_{j_1 \dots j_s}) = \alpha_{j_1 \dots j_s} \circ F^*. \qquad (15.13.5)$$ 

我们还有

$$F^*\left(d(y^{j_1}) \wedge \cdots \wedge d(y^{j_s})\right)$$ 

$$= F^*\left(d(y^{j_1})\right) \wedge \cdots \wedge F^*\left(d(y^{j_s})\right) \qquad [\text{Th 15.13.1(c)}]$$

$$= d\left(F^*(y^{j_1})\right) \wedge \cdots \wedge d\left(F^*(y^{j_s})\right) \qquad [\text{Th 15.11.2, (15.12.3)}]$$

$$= d(y^{j_1} \circ F) \wedge \cdots \wedge d(y^{j_s} \circ F), \qquad [(15.9.2),\ (15.12.3)]$$

且由定理 15.13.2 有

$$d(y^{j_1} \circ F) \wedge \cdots \wedge d(y^{j_s} \circ F)$$ 

$$= \sum_{1 \le i_1 < \cdots < i_s \le m} \det\left(\begin{bmatrix} \dfrac{\partial(y^{j_1} \circ F)}{\partial x^1} & \cdots & \dfrac{\partial(y^{j_s} \circ F)}{\partial x^m} \\ \vdots & \ddots & \vdots \\ \dfrac{\partial(y^{j_s} \circ F)}{\partial x^1} & \cdots & \dfrac{\partial(y^{j_s} \circ F)}{\partial x^m} \end{bmatrix}_{(i_1,\dots,i_s)}\right) \cdot d(x^{i_1}) \wedge \cdots \wedge d(x^{i_s})$$

$$= \sum_{1 \le i_1 < \cdots < i_s \le m} \det\left(\begin{bmatrix} \dfrac{\partial(y^1 \circ F)}{\partial x^1} & \cdots & \dfrac{\partial(y^1 \circ F)}{\partial x^m} \\ \vdots & \ddots & \vdots \\ \dfrac{\partial(y^n \circ F)}{\partial x^1} & \cdots & \dfrac{\partial(y^n \circ F)}{\partial x^m} \end{bmatrix}_{(i_1,\dots,i_s)}^{(j_1,\dots,j_s)}\right) \cdot d(x^{i_1}) \wedge \cdots \wedge d(x^{i_s})$$

$$= \sum_{1 \le i_1 < \cdots < i_s \le m} \det\left((J_F)_{(i_1,\dots,i_s)}^{(j_1,\dots,j_s)}\right) d(x^{i_1}) \wedge \cdots \wedge d(x^{i_s}).$$

于是，

$$F^*\left(d(y^{j_1}) \wedge \cdots \wedge d(y^{j_s})\right)$$ 

$$= \sum_{1 \le i_1 < \cdots < i_s \le m} \det\left((J_F)_{(i_1,\dots,i_s)}^{(j_1,\dots,j_s)}\right) d(x^{i_1}) \wedge \cdots \wedge d(x^{i_s}). \qquad (15.13.6)$$

将 (15.13.5) 和 (15.13.6) 代入 (15.13.4) 并交换求和次序即得结果。
(b): 这由 (a) 部分可得。

## 例 15.13.4 (极坐标).

设 $(r,s)$ 为 $\mathbb{R}^2$ 上的标准坐标，设 $(\rho,\phi)$ 为

$$U = \{(\rho,\phi) \in \mathbb{R}^2 : \rho > 0, 0 \le \phi < 2\pi\}$$

上的极坐标。

设 $F : U \longrightarrow \mathbb{R}^2$ 为由

$$F(\rho,\phi) = (\rho \cos(\phi), \rho \sin(\phi)),$$

给出的光滑映射，并考虑形式

$$\omega_{(r,s)} = d(r) \wedge d(s)$$

在 $\Lambda^2(\mathbb{R}^2)$ 中。设 $(U,(x^i)) = (U,(\rho,\phi))$ 且 $(V,(y^j)) = (\mathbb{R}^2,(r,s))$ 如定理 15.13.3(b) 中，则

$F^*(d(r) \wedge d(s)) = \det\begin{pmatrix} \dfrac{\partial(r \circ F)}{\partial \rho} & \dfrac{\partial(r \circ F)}{\partial \phi} \\ \dfrac{\partial(s \circ F)}{\partial \rho} & \dfrac{\partial(s \circ F)}{\partial \phi} \end{pmatrix} d(\rho) \wedge d(\phi)$

$= \det\begin{pmatrix} \dfrac{\partial(\rho \cos(\phi))}{\partial \rho} & \dfrac{\partial(\rho \cos(\phi))}{\partial \phi} \\ \dfrac{\partial(\rho \sin(\phi))}{\partial \rho} & \dfrac{\partial(\rho \sin(\phi))}{\partial \phi} \end{pmatrix} d(\rho) \wedge d(\phi)$

$= \det\begin{pmatrix} \cos(\phi) & -\rho \sin(\phi) \\ \sin(\phi) & \rho \cos(\phi) \end{pmatrix} d(\rho) \wedge d(\phi)$

$= \rho \, d(\rho) \wedge d(\phi).$

## 15.14 张量场的收缩

本节简要给出第 5.4 节中若干结果的流形版本。

## 定理 15.14.1.

若 $M$ 为光滑流形，则存在唯一的 $C^\infty(M)$-线性映射

$$\mathrm{C}_1^1 : \mathcal{T}_1^1(M) \longrightarrow C^\infty(M),$$

称为 $(1,1)$-收缩，使得

$$\mathrm{C}_1^1(X \otimes \omega) = \omega(X)$$

对所有向量场 $X$ 在 $\mathfrak{X}(M)$ 中以及所有余向量场 $\omega$ 在 $\mathfrak{X}^*(M)$ 中成立。

## 定理 15.14.2. 设 \(M\) 为光滑流形，设 \(\mathcal A\) 为 \(\mathcal T^1_1(M)\) 中的张量场，且在局部坐标下，设

$$\mathcal A=\sum_{ij} A^i_j\frac{\partial}{\partial x^i}\otimes d(x^j).$$

则

$$C^1_1(\mathcal A)=\sum_i A^i_i.$$

## 定理 15.14.3. 若 \(M\) 为光滑流形，且 \(1\leq k\leq r\) 和 \(1\leq l\leq s\) 为整数，则存在唯一的 \(C^\infty(M)\)-线性映射

$$C^k_l:\mathcal T^r_s(M)\longrightarrow\mathcal T^{r-1}_{s-1}(M),$$

称为 **\((k,l)\)-收缩**，使得

$$C^k_l(X_1\otimes\cdots\otimes X_r\otimes\omega^1\otimes\cdots\otimes\omega^s)$$
$$=X_k(\omega^l)X_1\otimes\cdots\otimes\widehat{X_k}\otimes\cdots\otimes X_r\otimes\omega^1\otimes\cdots\otimes\widehat{\omega^l}\otimes\cdots\otimes\omega^s$$

对所有向量场 \(X_1,\ldots,X_r\) 在 \(\mathfrak X(M)\) 中以及所有余向量场 \(\omega^1,\ldots,\omega^s\) 在 \(\mathfrak X^*(M)\) 中成立，其中 \(\widehat{\phantom X}\) 表示省略一项。

## 定理 15.14.4. 设 \(M\) 为光滑流形，设 \(\mathcal A\) 为 \(\mathcal T^r_s(M)\) 中的张量场，设 \(\omega^1,\ldots,\omega^r\) 为 \(\mathfrak X^*(M)\) 中的余向量场，设 \(X_1,\ldots,X_s\) 为 \(\mathfrak X(M)\) 中的向量场。则存在一个在 \(M\) 上的收缩 \(C\) 使得

$$\mathcal A(\omega^1,\ldots,\omega^r,X_1,\ldots,X_s)=C(X_1\otimes\cdots\otimes X_s\otimes\mathcal A\otimes\omega^1\otimes\cdots\otimes\omega^r).$$



