

因此

$d(\omega) = \sum_{i=1}^{m} d(\alpha_i) \wedge d(r^1) \wedge \cdots \widehat{d(r^i)} \wedge \cdots \wedge d(r^m)$

$= \sum_{i=1}^{m} \left[\left(\sum_{j} \frac{\partial \alpha_i}{\partial r^j} d(r^j)\right) \wedge d(r^1) \wedge \cdots \widehat{d(r^i)} \wedge \cdots \wedge d(r^m)\right]$

$= \sum_{i=1}^{m} \left(\sum_{j} \frac{\partial \alpha_i}{\partial r^j} d(r^j) \wedge d(r^1) \wedge \cdots \widehat{d(r^i)} \wedge \cdots \wedge d(r^m)\right)$

$= \sum_{i=1}^{m} \left(\frac{\partial \alpha_i}{\partial r^i} d(r^i) \wedge d(r^1) \wedge \cdots \widehat{d(r^i)} \wedge \cdots \wedge d(r^m)\right)$

$= \left(\sum_{i=1}^{m} (-1)^{i-1} \frac{\partial \alpha_i}{\partial r^i}\right) d(r^1) \wedge \cdots \wedge d(r^m),$

其中第一个等号来自定理 16.1.2，第二个等号来自定理 15.4.7，第四个等号来自定理 15.8.3(b)，最后一个等号来自定理 15.8.2(b)，其中 ^ 表示省略一项。于是

$\int_{\mathbb{H}^m} d(\omega)$

$= \int_C \left(\sum_{i=1}^{m} (-1)^{i-1} \frac{\partial \alpha_i}{\partial r^i}\right) d(r^1) \wedge \cdots \wedge d(r^m)$

$= \sum_{i=1}^{m} (-1)^{i-1} \int_C \frac{\partial \alpha_i}{\partial r^i} d(r^1) \wedge \cdots \wedge d(r^m)$

$= \sum_{i=1}^{m} (-1)^{i-1} \int_0^{c_m} \int_{-c_{m-1}}^{c_{m-1}} \cdots \int_{-c_1}^{c_1} \frac{\partial \alpha_i}{\partial r^i}(r^1,\dots,r^m) dr^1 \cdots dr^m$

$= \sum_{i=1}^{m-1} (-1)^{i-1} \int_0^{c_m} \int_{-c_{m-1}}^{c_{m-1}} \cdots \widehat{\int_{-c_i}^{c_i}} \cdots \int_{-c_1}^{c_1} \left(\int_{-c_i}^{c_i} \frac{\partial \alpha_i}{\partial r^i}(r^1,\dots,r^m) dr^i\right)$

$dr^1 \cdots \widehat{dr^i} \cdots dr^m$

$+ (-1)^{m-1} \int_{-c_{m-1}}^{c_{m-1}} \cdots \int_{-c_1}^{c_1} \left(\int_0^{c_m} \frac{\partial \alpha_m}{\partial r^m}(r^1,\dots,r^m) dr^m\right) dr^1 \cdots dr^{m-1}$

$= \sum_{i=1}^{m-1} (-1)^{i-1} \int_0^{c_m} \int_{-c_{m-1}}^{c_{m-1}} \cdots \widehat{\int_{-c_i}^{c_i}} \cdots \int_{-c_1}^{c_1} \alpha_i(r^1,\dots,r^m)\Big|_{r^i=c_i}^{r^i=-c_i}$

$dr^1 \cdots \widehat{dr^i} \cdots dr^m$

$+ (-1)^{m-1} \int_{-c_{m-1}}^{c_{m-1}} \cdots \int_{-c_1}^{c_1} \alpha_m(r^1,\dots,r^m)\Big|_{r^m=c_m}^{r^m=0} dr^1 \cdots dr^{m-1},$

其中第三个等号来自 (16.7.2)，第四个等号来自定理 10.5.4，最后一个等号来自微积分基本定理。选择

# 第 18 章

# 带联络的光滑流形

在第三部分的引言中，我们提出了在先前对曲线和曲面的研究基础上发展微分几何理论的任务，但不需要涉及环境空间。这项工作的隐含目标是尽可能恢复为曲线和曲面所呈现的结果。第 14–17 章已经完成了这一议程的重要部分。然而，明显缺少的是关于“协变导数”和“度量”的讨论。本章通过在光滑流形的讨论中加入“联络”来弥补前一个缺失。

## 18.1 协变导数

设 $M$ 是一个光滑流形。$M$ 上的一个**联络**是一个映射

$$\nabla : \mathfrak{X}(M) \times \mathfrak{X}(M) \longrightarrow \mathfrak{X}(M)$$

+   使得对于 $\mathfrak{X}(M)$ 中的所有向量场 $X,Y,Z$ 以及 $C^\infty(M)$ 中的所有函数 $f$：
+   
+   [V1] $\nabla(X+Y,Z)=\nabla(X,Z)+\nabla(Y,Z)$.
+   
+   [V2] $\nabla(fX,Y)=f\nabla(X,Y)$.
+   
+   [V3] $\nabla(X,Y+Z)=\nabla(X,Y)+\nabla(X,Z)$.
+   
+   [V4] $\nabla(X,fY)=X(f)Y+f\nabla(X,Y)$.

我们将二元组 $(M,\nabla)$ 称为一个**带联络的光滑流形**。一个给定的光滑流形可以有多个联络。因此，联络不是流形的基本组成部分，不像例如边界构成带边界光滑流形的一部分那样。

### 例 18.1.1。

联络有两个明显的例子。设 U 是 ℝ^m 中的开集。将 U 视为光滑 m-流形，令

D : 𝔛(U) × 𝔛(U) ⟶ 𝔛(U)

是由

D(X, Y) = D_X(Y)

对 𝔛(U) 中的所有向量场 X, Y 定义的映射，其中 D_X 是关于 X 的欧几里得导数。由定理 10.3.1 可知，D 是 U 上的联络。

类似地，设 (M, 𝔤) 是 ℝ³ 中的正则曲面。根据例 14.1.5，M 是光滑 2-流形。令

∇ : 𝔛(M) × 𝔛(M) ⟶ 𝔛(M)

是由

∇(X, Y) = ∇_X(Y)

对 𝔛(M) 中的所有向量场 X, Y 定义的映射，其中 ∇_X 是关于 X 的协变导数。我们从定理 12.4.1 得到 ∇ 是 M 上的联络。◇

在前面的每个例子中，我们都从一个“导数”出发，并用它来定义联络。给定一个带联络的光滑流形，我们可以反过来进行这个过程。

设 (M, ∇) 是一个带联络的光滑流形，设 X 是 𝔛(M) 中的一个向量场。我们定义映射

∇_X : 𝔛(M) ⟶ 𝔛(M)

为

∇_X(Y) = ∇(X, Y)

对 𝔛(M) 中的所有向量场 Y。从 [∇3] 和 [∇4] 可知 ∇_X 是 ℝ^m-线性的，并且从 [∇4] 可知

∇_X(fY) = X(f)Y + f∇_X(Y)

对所有 C^∞(M) 中的函数 f 和 𝔛(M) 中的所有向量场 Y 成立。根据定理 16.2.7，存在一个唯一的张量导数

∇_X : 𝒯_s^r(M) ⟶ 𝒯_s^r(M)

在 M 上，称为关于 X 的协变导数，其中 (∇_X)_s^r 已简写为 ∇_X。特别地，我们有

∇_X(f) = X(f) (18.1.1)

对所有 C^∞(M) 中的函数 f 成立。

### 定理 18.1.2（协变导数的向量场性质）。

设 (M, ∇) 是一个带联络的光滑流形，设 X, Y, Z 是 𝔛(M) 中的向量场，设 f 是 C^∞(M) 中的函数。则：

+   (a) ∇_{X+Y}(Z) = ∇_X(Z) + ∇_Y(Z).
+   (b) ∇_{fX}(Y) = f∇_X(Y).
+   (c) ∇_X(Y + Z) = ∇_X(Y) + ∇_X(Z).
+   (d) ∇_X(fY) = X(f)Y + f∇_X(Y).

证明。这直接从性质 [∇1]–[∇4] 得出。

### 定理 18.1.3（协变导数的张量场性质）。

设 (M,∇) 是一个带联络的光滑流形，设 X 是 𝔛(M) 中的一个向量场，设 𝒜,ℬ 和 𝒞 分别是 𝒯_s^r(M) 和 𝒯_{s'}^r'(M) 中的张量场。则：

+   (a) ∇_X(𝒜 + ℬ) = ∇_X(𝒜) + ∇_X(ℬ).
+   (b) ∇_X(𝒜 ⊗ 𝒞) = ∇_X(𝒜) ⊗ 𝒞 + 𝒜 ⊗ ∇_X(𝒞).

证明。这由第 16.2 节给出的张量导数的定义性质得出。

### 定理 18.1.4（协变导数的整体表达式）。

设 (M,∇) 是一个带联络的光滑流形，设 𝒜 是 𝒯_s^r(M) 中的一个张量场，设 X, Y₁,...,Y_s 是 𝔛(M) 中的向量场，设 ω¹,...,ω^r 是 𝔛*(M) 中的余向量场。则

∇_X(𝒜)(ω¹,...,ω^r, Y₁,...,Y_s)
= X(𝒜(ω¹,...,ω^r, Y₁,...,Y_s))
− ∑_{i=1}^r 𝒜(ω¹,...,ω^{i−1}, ∇_X(ω^i), ω^{i+1},...,ω^r, Y₁,...,Y_s)
− ∑_{j=1}^s 𝒜(ω¹,...,ω^r, Y₁,...,Y_{j−1}, ∇_X(Y_j), Y_{j+1},...,Y_s).

证明。这由定理 16.2.4 得出。

### 例 18.1.5。

设 (M,∇) 是一个带联络的光滑流形，设 𝒜 是 𝒯₂⁰(M) 中的一个张量场，设 X,Y,Z 是 𝔛(M) 中的向量场，设 ω 是 𝔛*(M)=𝒯₁⁰(M) 中的一个余向量场。我们从 (18.1.1) 和定理 18.1.4 得到

∇_X(ω)(Y) = X(ω(Y)) − ω(∇_X(Y)) = ∇_X(ω(Y)) − ω(∇_X(Y))    (18.1.2)

以及

∇_X(𝒜)(Y,Z) = X(𝒜(Y,Z)) − 𝒜(∇_X(Y),Z) − 𝒜(Y,∇_X(Z))
= ∇_X(𝒜(Y,Z)) − 𝒜(∇_X(Y),Z) − 𝒜(Y,∇_X(Z)).    (18.1.3)

容易证明，如果 𝒜 在其两个参数上是对称的，那么 ∇_X(𝒜) 也是对称的。

设 $(M,\nabla)$ 是一个带联络的光滑流形，设 $X$ 是 $\mathfrak{X}(M)$ 中的一个向量场。可以证明，$\nabla_X$ 在以下意义上是逐点确定的：对于 $M$ 中的所有点 $p$ 和 $\mathfrak{X}(M)$ 中的所有向量场 $Y$，

$$\nabla_X(Y)_p = \nabla_{\tilde{X}}(Y)_p$$

对于 $\mathfrak{X}(M)$ 中所有满足 $X_p = \tilde{X}_p$ 的向量场 $X,\tilde{X}$。对于 $M$ 中的每个点 $p$ 和 $T_p(M)$ 中的向量 $v$，我们通过

$$\nabla_v(Y) = \nabla_X(Y)_p \tag{18.1.4}$$

定义 $T_p(M)$ 中的向量 $\nabla_v(Y)$，其中 $X$ 是 $\mathfrak{X}(M)$ 中满足 $X_p = v$ 的任意向量场。由定理 15.1.2，这样的 $X$ 总是存在。由于 $\nabla_X$ 是逐点确定的，$\nabla_v(Y)$ 与向量场的选择无关，因此是良定义的。由 (18.1.4) 可知，对于 $\mathfrak{X}(M)$ 中的所有向量场 $X,Y$，

$$\nabla_{X_p}(Y) = \nabla_X(Y)_p \tag{18.1.5}$$

根据定义，$\operatorname{Lin}(\mathfrak{X}(M),\mathfrak{X}(M))$ 是从向量空间 $\mathfrak{X}(M)$ 到自身的所有 $\mathbb{R}$-线性映射构成的向量空间。我们通过将乘法定义为映射的复合，使 $\operatorname{Lin}(\mathfrak{X}(M),\mathfrak{X}(M))$ 成为 $\mathbb{R}$ 上的环。设 $\Theta,\Upsilon,\Psi$ 是三个这样的映射，并定义 $\Theta$ 和 $\Upsilon$ 的括号为

$$[\Theta,\Upsilon] = \Theta \circ \Upsilon - \Upsilon \circ \Theta.$$

容易证明，在这个定义下，$\operatorname{Lin}(\mathfrak{X}(M),\mathfrak{X}(M))$ 是 $\mathbb{R}$ 上的李代数。特别地，雅可比恒等式成立：

$$[[\Theta,\Upsilon],\Psi] + [[\Upsilon,\Psi],\Theta] + [[\Psi,\Theta],\Upsilon] = 0.$$

对于 $\mathfrak{X}(M)$ 中的向量场 $X,Y,Z$，从定理 18.1.2 的 (c) 和 (d) 部分可知，$\nabla_X,\nabla_Y,\nabla_Z$ 是 $\operatorname{Lin}(\mathfrak{X}(M),\mathfrak{X}(M))$ 中的映射，因此

$$[[\nabla_X,\nabla_Y],\nabla_Z] + [[\nabla_Y,\nabla_Z],\nabla_X] + [[\nabla_Z,\nabla_X],\nabla_Y] = 0. \tag{18.1.6}$$

## 18.2 Christoffel 符号

在我们对 $\mathbb{R}_p^3$ 中正则曲面的研究中，我们定义了 Christoffel 符号，并用它们来构造协变导数，现在我们将其视为与联络等价（参见例 18.1.1）。我们对带联络的光滑流形的处理方法则是相反的：我们从一个联络开始，然后用它来定义 Christoffel 符号。

设 $(M,\nabla)$ 是一个带联络的光滑 $m$-流形，设 $X$ 是 $\mathfrak{X}(M)$ 中的一个向量场，设 $(U,(x^i))$ 是 $M$ 上的一个坐标卡，设 $\mathcal{X} = (\partial/\partial x^1,\dots,\partial/\partial x^m)$ 是对应的坐标标架。可以证明，$\nabla$ 无歧义地限制到光滑 $m$-流形 $U$ 上。为简化记号，我们将限制

$$\nabla|_U \quad \text{记为} \quad \nabla.$$

