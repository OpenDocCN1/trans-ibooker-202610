
证明：在定理16.5.1(e)中令ω = d(f)和ξ = Ω_M，得到

d(f) ∧ i_X(Ω_M) = i_X(d(f)) ∧ Ω_M − i_X(d(f) ∧ Ω_M) = X(f) Ω_M [(16.5.2), 定理7.2.12(b)] = ⟨Grad(f), X⟩ Ω_M, [定理20.3.1]

因此

Div(f X) Ω_M = d(i_{f X}(Ω_M)) [定理20.4.3(a)] = d(f i_X(Ω_M)) [定理16.5.1(b)] = d(f) ∧ i_X(Ω_M) + f d(i_X(Ω_M)) [(16.1.1)] = [⟨Grad(f), X⟩ + f Div(X)]Ω_M. [定理20.4.3(a)]

由于(Ω_M)是Λ^m(M)的一个基，结论成立。

前面的结果与定理10.6.1(b)明显相似。

## 定理20.4.5（半黎曼流形的散度定理）。设(M, g, 𝔒)是紧致带边界的定向半黎曼流形，Ω_M为其体积形式，X为𝔛(M)中的向量场。则 ∫_M Div(X) Ω_M = ∫_{∂M} i_X(Ω_M)。

证明：我们有 ∫_M Div(X) Ω_M = ∫_M d(i_X(Ω_M)) [定理20.4.3(a)] = ∫_{∂M} i_X(Ω_M)。 [定理17.4.1]

## 例20.4.6（ℝ_ν^m）。设(r^1, …, r^m)、(∂/∂r^1, …, ∂/∂r^m)和(ε_1, …, ε_m)分别是ℝ_ν^m的标准坐标、相应的标准坐标标架和符号差。由例15.1.6，(∂/∂r^1, …, ∂/∂r^m) = (e_1, …, e_m)，其中(e_1, …, e_m)是ℝ_ν^m的标准基。设X = (α^1, …, α^m) = Σ_i α^i e_i = Σ_i α^i ∂/∂r^i 是𝔛(ℝ_ν^m)中的向量场。由(18.2.2)、定理19.3.5和定理20.4.1可得Div(X) = Σ_i ∂α^i/∂r^i。

在ℝ_0^3中，使用标准坐标(x, y, z)，这变为Div(X) = ∂α^1/∂x + ∂α^2/∂y + ∂α^3/∂z = div(X)，

而在ℝ_1^4中，使用标准坐标(x, y, z, t)，它变为Div(X) = ∂α^1/∂x + ∂α^2/∂y + ∂α^3/∂z + ∂α^4/∂t。

## 20.5 旋度

设(M,g,𝒟)是指标为ν的定向半黎曼3-流形，Ω_M为其体积形式。旋度是映射

\[\operatorname{Curl}:\mathfrak{X}(M)\longrightarrow\mathfrak{X}(M)\]

定义为

\[\operatorname{Curl}(X)^\mathrm{F}=\star d(X^\mathrm{F}) \tag{20.5.1}\]

对所有𝔛(M)中的向量场X成立。由于X^F是𝔛^*(M)=Λ^1(M)中的余向量场，可知d(X^F)是Λ^2(M)中的形式，因此⋆ d(X^F)是Λ^1(M)中的向量场，所以该定义有意义。我们将Curl(X)称为X的旋度。

## 定理20.5.1. 在上述设定下，

\[i_{\operatorname{Curl}(X)}(\Omega_M)=d(X^\mathrm{F}).\]

证明：我们有

\[i_{\operatorname{Curl}(X)}(\Omega_M)=(-1)^\nu\star(\operatorname{Curl}(X)^\mathrm{F})\qquad [\mathrm{Th\ 20.1.2}]\]
\[\phantom{i_{\operatorname{Curl}(X)}(\Omega_M)}=(-1)^\nu\star^2d(X^\mathrm{F}).\qquad [(20.5.1)]\]

由于d(X^F)是Λ^2(M)中的形式，在定理20.1.1(c)中令m=3和s=2可得

\[\star^2(d(X^\mathrm{F}))=(-1)^\nu d(X^\mathrm{F}).\]

结论成立。

尽管定义(20.5.1)显得陌生，下面的例子表明在ℝ^3_0中Curl与经典旋度算子是一致的。

## 例20.5.2.

设(x,y,z)和(d(x),d(y),d(z))分别是ℝ^3_0上的标准坐标和相应的标准对偶坐标标架，并设X=(α^1,α^2,α^3)是𝔛(ℝ^3_0)中的向量场。由定理3.3.2的流形版本，

\[X^\mathrm{F}=\alpha^1d(x)+\alpha^2d(y)+\alpha^3d(z).\]

则(16.1.2)给出

\[
d(X^\mathrm{F})=\left(\frac{\partial\alpha^3}{\partial y}-\frac{\partial\alpha^2}{\partial z}\right)d(y)\wedge d(z)+\left(\frac{\partial\alpha^1}{\partial z}-\frac{\partial\alpha^3}{\partial x}\right)d(z)\wedge d(x)
\]
\[
\qquad+\left(\frac{\partial\alpha^2}{\partial x}-\frac{\partial\alpha^1}{\partial y}\right)d(x)\wedge d(y).
\]

因此

Curl(X)^F = *d(X^F)
= (∂α³/∂y - ∂α²/∂z) * (d(y) ∧ d(z)) + (∂α¹/∂z - ∂α³/∂x) * (d(z) ∧ d(x))
+ (∂α²/∂x - ∂α¹/∂y) * (d(x) ∧ d(y))
= (∂α³/∂y - ∂α²/∂z) d(x) + (∂α¹/∂z - ∂α³/∂x) d(y)
+ (∂α²/∂x - ∂α¹/∂y) d(z)
= (∂α³/∂y - ∂α²/∂z, ∂α¹/∂z - ∂α³/∂x, ∂α²/∂x - ∂α¹/∂y)^F
= curl(X)^F,

其中第三个等号由例8.5.4得出，第四个等号由定理3.3.2的流形版本得出，最后一个等号由表10.6.1得出。因此，Curl(X) = curl(X)。

## 20.6 Hesse算子

设(M, 𝔤)是半黎曼流形。Hesse算子是映射

Hess : C^∞(M) ⟶ T₂⁰(M)

定义为

Hess(f) = ∇²(f)  (20.6.1)

对所有C^∞(M)中的函数f成立，其中我们记∇ ∘ ∇为∇²。

我们将Hess(f)称为f的海塞（Hessian）。

## 定理20.6.1. 设(M, 𝔤)是半黎曼m-流形，f是C^∞(M)中的函数，X、Y是𝔛(M)中的向量场。则：

- (a)

Hess(f)(X, Y) = X(Y(f)) - ∇_X(Y)(f)
= ∇_X(∇_Y(f)) - ∇_{∇_X(Y)}(f)
= ⟨∇_X(Grad(f)), Y⟩。

- (b) Hess(f)是对称的。

证明：(a)：前两个等号由(18.4.3)得出。我们有

X(Y(f)) = X(⟨Grad(f), Y⟩)  [定理20.3.1]
= ⟨∇_X(Grad(f)), Y⟩ + ⟨Grad(f), ∇_X(Y)⟩  [(19.3.1)]
= ⟨∇_X(Grad(f)), Y⟩ + ∇_X(Y)(f)。  [定理20.3.1]

第三个等号现在由第一个等号得出。

(b)：由于Levi-Civita联络∇是对称的，

∇_X(Y)(f) − ∇_Y(X)(f) = [X,Y](f) = X(Y(f)) − Y(X(f)),

所以

Hess(f)(X, Y) = X(Y(f)) − ∇_X(Y)(f)  [部分(a)]
= Y(X(f)) − ∇_Y(X)(f)
= Hess(f)(Y, X)。  [部分(a)]

## 定理20.6.2（Hesse算子的局部坐标表达式）。如果(M, g)是半黎曼m-流形，且f是C^∞(M)中的函数，则在局部坐标下，Hess(f)的分量为

Hess(f)_{ij} = ∂²f/∂x^i∂x^j − Σ_k Γ^k_{ij} ∂f/∂x^k

对i, j = 1, …, m。

证明：我们有

Hess(f)_{ij} = Hess(f)(∂/∂x^i, ∂/∂x^j)
= ∂/∂x^i (∂f/∂x^j) − ∇_{∂/∂x^i}(∂/∂x^j)(f)  [定理20.6.1(a)]
= ∂²f/∂x^i∂x^j − Σ_k Γ^k_{ij} ∂f/∂x^k。  [(18.2.1)]

## 例20.6.3（ℝ^m_ν）。设(r¹, …, r^m)和(∂/∂r¹, …, ∂/∂r^m)分别是ℝ^m_ν上的标准坐标和相应的标准坐标标架。由定理19.3.5和定理20.6.2可得

Hess(f)_{ij} = ∂²f/∂r^i∂r^j

对i, j = 1, …, m。

## 20.7 拉普拉斯算子

设$(M,\mathfrak{g})$是半黎曼流形。拉普拉斯算子是映射

Lap : $C^{\infty}(M) \longrightarrow C^{\infty}(M)$

定义为

Lap$(f) = \text{Div} \circ \text{Grad}(f)$ (20.7.1)

对所有$C^{\infty}(M)$中的函数$f$成立。我们将Lap$(f)$称为$f$的拉普拉斯量（Laplacian）。

## 定理20.7.1. 如果$(M,\mathfrak{g})$是半黎曼流形，且$f$是$C^{\infty}(M)$中的函数，则

Lap$(f) = C_{12} \circ \text{Hess}(f)$。

证明：我们有

Lap$(f) = \text{Div} \circ \text{Grad}(f)$
$= C_{1}^{1} \circ \nabla \circ S_{1}^{1}(d(f))$ [流形版本的练习6.3.1, (20.3.1), (20.4.1)]
$= C_{1}^{1} \circ S_{1}^{1} \circ \nabla(\nabla(f))$ [定理18.4.3, 定理19.6.3]
$= C_{1}^{1} \circ S_{1}^{1} \circ \nabla^{2}(f)$
$= C_{12} \circ \text{Hess}(f)$。 [(19.6.3), (20.6.1)]

## 定理20.7.2（拉普拉斯算子的标准正交标架表达式）。设$(M,\mathfrak{g})$是带有符号差$(\varepsilon_{1},...,\varepsilon_{m})$的半黎曼流形，$f$是$C^{\infty}(M)$中的函数，$(E_{1},...,E_{m})$是$M$中开集$U$上的标准正交标架。则

Lap$(f) = \sum_{i} \varepsilon_{i} \text{Hess}(f)(E_{i},E_{i})$

在$U$上成立。

证明：这可由定理19.6.2(b)和定理20.7.1得出。

## 例20.7.3（$\mathbb{R}_{\nu}^{m}$）。设$(r^{1},...,r^{m})$、$(\partial/\partial r^{1},...,\partial/\partial r^{m})$和$(\varepsilon_{1},...,\varepsilon_{m})$分别是$\mathbb{R}_{\nu}^{m}$的标准坐标、相应的标准坐标标架和符号差。由例20.6.3，我们有

Hess$(f)\left(\frac{\partial}{\partial r^{i}},\frac{\partial}{\partial r^{j}}\right) = \text{Hess}(f)_{ij} = \frac{\partial^{2} f}{\partial r^{i}\partial r^{j}}$,

并且由例15.1.6，$(\partial/\partial r^{1},...,\partial/\partial r^{m}) = (e_{1},...,e_{m})$，其中$(e_{1},...,e_{m})$是$\mathbb{R}_{\nu}^{m}$的标准基。因此，$(\partial/\partial r^{1},...,\partial/\partial r^{m})$是$\mathbb{R}_{\nu}^{m}$上的一个标准正交标架。那么定理20.7.2给出

Lap$(f) = \sum_{i} \varepsilon_{i} \frac{\partial^{2} f}{\partial (r^{i})^{2}}$。
