

M 的体积是通过取 C^∞(M) 中常值为 1 的函数 f 获得的：

vol(M) = ∫_M Ω_M.

## Example 19.10.3 (Area of Regular Surface in ℝ₀³).

设 (M, 𝔤, 𝔒) 是 ℝ₀³ 中的紧致定向正则曲面，设 (U, φ) 是 M 上的正定向坐标图，设 ℋ = (H₁, H₂) 为相应的坐标标架。 这里“图”的用法与第 11.2 节中的意义相同：U 是 ℝ² 中的开集且 φ : U ⟶ ℝ₀³ 是一个光滑映射。 在本设定中，我们将“体积”称为“面积”。 经过第 16.7 节中定义的推导并使用定理 19.10.2，我们发现

area(φ(U)) = ∬_U √det(𝔤_ℋ) dr¹ dr².

我们由定理 4.6.3 知 det(𝔤_ℋ) > 0 在 U 上成立，因此上述积分是良定义的。 由定理 8.4.6(b)，‖H₁ × H₂‖ = √det(𝔤_ℋ)，故

area(φ(U)) = ∬_U ‖H₁ × H₂‖ dr¹ dr².

这就是来自两个实变量积分学的熟悉的面积定义。

## Example 19.10.4 (𝓢²).

设我们将 (𝓢², 𝔢|𝓢²) 视为黎曼 3 流形 ℝ₀³ = (ℝ³, 𝔢) 的黎曼 2 子流形。 回顾第 13.4 节的结果，并考虑 𝓢² 上的图 (U, φ = (θ, φ))，其中 U = (0, π) × (−π, π) 且 φ : U ⟶ 𝓢² 由

φ(θ, φ) = (sin(θ) cos(φ), sin(θ) sin(φ), cos(θ)).

给出。 由

𝔤_ℋ(θ, φ) = [1 0; 0 sin²(θ)]

和定理 19.10.2，体积形式由

Ω_𝓢²|_(θ,φ) = √det(𝔤_ℋ(θ, φ)) d(θ) ∧ d(φ) = sin(θ) d(θ) ∧ d(φ).

给出。 由于 φ(U) 等于 𝓢² 除去一个大圆的一部分（其面积为 0），𝓢² 的面积为

area(𝓢²) = ∬_U Ω_𝓢²|_(θ,φ) dθ dφ = ∫_{-π}^{π} ∫_0^{π} sin(θ) dθ dφ = 4π.

## 19.11 Orientation of Hypersurfaces

在本节中，我们总结并扩展了关于超曲面定向的先前结果。

𝔛_M̄(M ∩ U ∩ V) 中的向量场。 见图 19.11.1(b)，其中 G 是 “∂/∂x¹ 的向量积”，因此与 ∂/∂x¹ 正交。

设 (M ∩ Ũ, (x̃¹, …, x̃m−1)) 是 𝔐 中的另一个（正定向）图，设 X̃ = (∂/∂x̃¹, …, ∂/∂x̃m−1) 为 M ∩ Ũ 上的相应（正定向）坐标标架，设 Ẽ 是与 M ∩ Ũ 相交的 M̄ 中邻域 Ṽ 上的正定向标准正交标架，并设 G̃|_{M∩Ũ∩Ṽ} 为 𝔛_M̄(M ∩ Ũ ∩ Ṽ) 中的相应单位法向量场。 设 M ∩ U ∩ V 与 M ∩ Ũ ∩ Ṽ 相交。 由于 𝒳 和 X̃ 是正定向的，它们是一致的，同样原因，Ẽ 和 Ẽ 也是如此。 由定理 8.4.11(b) 和定理 8.4.13(b) 可知 G_p = G̃_p 对所有 p ∈ M ∩ (U ∩ V) ∩ (Ũ ∩ Ṽ) 成立。 因此我们可以为 M 中的每一点 p 分配一个在 p 处为单位法向量的向量 𝒩_p，并且在坐标域和标准正交标架的域与 M 重叠时做到一致。 这定义了沿 M 的单位法向量场 𝒩。 根据构造，𝒩 在 M 中每一点的邻域上光滑，因此在 M 上光滑。 于是，𝒩 是 𝔛_M̄(M) 中的单位法向量场。

(⇐): 设 V 是 𝔛_M̄(M) 中的单位法向量场。 由于 V 是 𝔛_M̄(M) 中的无处切向向量场，由定理 16.6.9 可知 V 诱导了 M 的一个定向。 根据定义，存在一个一致的光滑图册用于 M 来诱导该定向，因此根据定义，M 是可定向的。

## Theorem 19.11.2.

设 (M̄, ḡ, 𝔇) 是定向的半黎曼 m̄ 流形或带边界的定向半黎曼 m̄ 流形，其中 m̄ ≥ 2，并设 Ω_M̄ 为其体积形式。 设 (M, ḡ|_M) 是 M̄ 的可定向半黎曼超曲面或 M̄ 的带边界的可定向半黎曼超曲面。 设 V 是 𝔛_M̄(M) 中的单位法向量场（如定理 19.11.1 所给），并设 𝔇_M 是由 i_V(Ω_M̄)|_M 诱导的 M 的定向（如定理 16.6.9 所给），使得 (M, ḡ|_M, 𝔇_M) 是 M̄ 的定向半黎曼超曲面或 M̄ 的带边界的定向半黎曼超曲面。 设 Ω_M 为其体积形式。 则：

(a)

 i_V(Ω_M̄)|_M = Ω_M.

(b) 更一般地，若 X 是 𝔛_M̄(M) 中的向量场，则

 i_X(Ω_M̄)|_M = ε_M⟨X, V⟩ Ω_M.

证明。 这是定理 8.3.6 的流形版本。

我们上面指出，对于带边界的光滑流形，定理 17.2.5 保证了边界沿着存在外指向的光滑向量场。 在半黎曼流形的更专门的设定中，外指向的光滑向量场可以升级为单位法向光滑向量场，如我们现在所示。

## Theorem 19.11.3 (Existence of Outward-Pointing Vector Field).

设 (M, g) 是带边界的半黎曼 m 流形，其中 m ≥ 2，且

或 $\mathcal{N}|_{\partial M\cap M}= -G|_{\partial M\cap M}$。 这表明 $\mathcal{N}$ 在 $\partial M$ 中每一点的邻域上在 $\partial M$ 上光滑。 于是，$\mathcal{N}$ 在 $\partial M$ 上光滑。

## **Theorem 19.11.4.**

设 $(M,\epsilon|_M)$ 是 $\mathbb{R}_0^3=(\mathbb{R}^3,\epsilon)$ 的可定向黎曼带边界超曲面，设 $V=(V^1,V^2,V^3)$ 是 $\mathfrak{X}_{\mathbb{R}_0^3}(M)$ 中的单位法向量场（如定理 19.11.1 所给），并设 $\mathcal{D}_M$ 是由 $V$ 诱导的 $M$ 的定向，使得 $(M,\epsilon|_M,\mathcal{D}_M)$ 是 $\mathbb{R}_0^3$ 的定向黎曼带边界超曲面。 设 $\Omega_M$ 为其体积形式。 设 $W$ 是 $\mathfrak{X}(\partial M)$ 中的唯一外指向单位法向量场（如定理 19.11.3 所给），并设 $\mathcal{D}_{\partial M}$ 是由 $W$ 诱导的（Stokes）定向，使得 $(\partial M,\epsilon|_{\partial M},\mathcal{D}_{\partial M})$ 是 $(M,\epsilon|_M,\mathcal{D}_M)$ 的定向黎曼超曲面。 设 $\Omega_{\partial M}$ 为其体积形式，并设 $U=V\times W$。 则：

+   (a) 对所有点 $p$ 在 $M$ 中和所有向量 $v,w$ 在 $T_p(M)$ 中，
+   
+   $$
+   \Omega_M|_p(v,w)=\langle V_p,v\times w\rangle
+   $$
+   
+   $$
+   =\bigl(V^1(p)\,d(y)\wedge d(z)+V^2(p)\,d(z)\wedge d(x)
+   +V^3(p)\,d(x)\wedge d(y)\bigr)(v,w).
+   $$

+   (b) 对所有点 $q$ 在 $\partial M$ 中和所有向量 $u$ 在 $T_q(\partial M)$ 中，
+   
+   $$
+   \Omega_{\partial M}|_q(u)=\langle U_q,u\rangle.
+   $$

+   (c) 对所有点 $q$ 在 $\partial M$，$(U_q,V_q,W_q)$ 和 $(V_q,W_q,U_q)$ 是 $\mathbb{R}_0^3$ 的具有标准定向的正交基。

+   (d) $U$ 是 $\mathfrak{X}(\partial M)$ 中关于 $\mathcal{D}_{\partial M}$ 正定向的唯一单位向量场。

+   (e) 函数 $\Omega_{\partial M}(U)$ 在 $C^\infty(\partial M)$ 中具有常值 $1$。

**Proof.** (a): 设 $v=(a^1,a^2,a^3)$ 和 $w=(b^1,b^2,b^3)$，并设 $\Omega_{\mathbb{R}_0^3}=d(x)\wedge d(y)\wedge d(z)$ 为 $\mathbb{R}_0^3$ 上的体积形式，其中，如通常假设，$\mathbb{R}_0^3$ 具有标准定向。 则

```
$$
\begin{aligned}
\Omega_M|_p(v,w)
&=\bigl(i_V(\Omega_{\mathbb{R}_0^3}|_M)\bigr)|_p(v,w)
=\Omega_{\mathbb{R}_0^3}|_p(V_p,v,w)\\
&=d(x)\wedge d(y)\wedge d(z)(V_p,v,w)\\
&=\langle V_p,v\times w\rangle
=\det\begin{pmatrix}
V^1(p)&a^1&b^1\\
V^2(p)&a^2&b^2\\
V^3(p)&a^3&b^3
\end{pmatrix}\\
&=V^1(p)\det\begin{pmatrix}a^2&b^2\\a^3&b^3\end{pmatrix}
-V^2(p)\det\begin{pmatrix}a^1&b^1\\a^3&b^3\end{pmatrix}\\
&\quad+V^3(p)\det\begin{pmatrix}a^1&b^1\\a^2&b^2\end{pmatrix}\\
&=\bigl(V^1(p)d(y)\wedge d(z)+V^2(p)d(z)\wedge d(x)\\
&\quad+V^3(p)d(x)\wedge d(y)\bigr)(v,w).
\end{aligned}
$$
```

其中第一个等式由定理 19.11.2(a) 推出，第四和第五个等式由定理 8.4.5(a) 推出，最后一个等式由定理 7.2.8 推出。

(b): 我们有

```
Ω_{∂M}|_q(u) = (i_W(Ω_M)|_{∂M})|_q(u) = Ω_M|_q(W_q, u)
= ⟨V_q, W_q × u⟩ = ⟨V_q × W_q, u⟩ = ⟨U_q, u⟩,
```

其中第一个等式由定理 19.11.2(a) 推出，第三个等式由部分 (a) 推出，第四个等式由定理 8.4.5(a) 推出。

(c): 根据定义，V_q 是 T_q(M)⊥ 中的单位向量，W_q 是 T_q(M) 中的单位向量，且 U_q = V_q × W_q。 由定理 8.4.6(b) 可知 U_q 是单位向量。 于是，(U_q, V_q, W_q)，从而 (V_q, W_q, U_q)，是 ℝ₀³ 的正交基。 由定理 8.4.10(b)，(U_q, V_q, W_q) 具有标准定向，所以 (V_q, U_q, W_q) 具有相反定向，故 (V_q, W_q, U_q) 具有标准定向。 见图 19.11.2。

![](img/d712710a8eb78c0959204afd5dde4c91_572_0.png)

# Figure 19.11.2. Diagram for Theorem 19.11.4

(d): 设 u 是 T_q(∂M) 中的非零向量。 由本节引言中的第 (v) 条可知，(u) 是关于 𝔇_{∂M}(q) 正定向的 T_q(∂M) 的基当且仅当 (V_q, W_q, u) 是具有标准定向的 ℝ₀³ 的基。 因此我们由部分 (c) 可知 (U_q) 是关于 𝔇_{∂M}(q) 正定向的 T_q(∂M) 的基。 由于 q 是任意的，结果得证。

(e): 由部分 (b)，

```
Ω_{∂M}|_q(U_q) = ⟨U_q, U_q⟩ = 1
```

对所有 q ∈ ∂M 成立。 结果得证。

我们注意到，前述定理的各部分 (d) 和 (e) 合在一起与定理 19.10.1 一致。 作为部分 (a) 的一个例子，单位球面 S² 的外指向单位法向量场由 V_{(x,y,z)} = (x, y, z) 给出，所以

```
Ω_{S²}|_{(x,y,z)} = x d(y) ∧ d(z) + y d(z) ∧ d(x) + z d(x) ∧ d(y).
```

