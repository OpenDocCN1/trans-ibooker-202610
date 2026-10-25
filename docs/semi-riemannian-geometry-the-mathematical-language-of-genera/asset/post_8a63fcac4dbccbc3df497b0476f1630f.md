
(b): Let p be a point in M. Since ℝV_p = T_p(M)⊥ = ℝṼ_p, we have Ṽ_p = c V_p for some real number c. Then ±1 = ⟨Ṽ_p, Ṽ_p⟩ = c²⟨V_p, V_p⟩ = ±c², hence c = ±1. Thus, ⟨Ṽ_p, Ṽ_p⟩ = ⟨±V_p, ±V_p⟩ = ⟨V_p, V_p⟩. Since p was arbitrary, the result follows.

## Theorem 12.2.4. Let M be a regular surface, let V be a unit normal vector field along M, and suppose 𝔤, the first fundamental form on M, satisfies [G1]. Then 𝔤 satisfies [G2] if and only if V satisfies [V2].

Proof. Since V is a unit vector field along M, [V2] is equivalent to: ⟨V_p, V_p⟩ = 1 for all p in M, or ⟨V_p, V_p⟩ = −1 for all p in M. By Theorem 12.2.2(b), (−1)^{ν−ind(𝔤_p)} = ⟨V_p, V_p⟩. The result follows.

Let M be a regular surface, and let 𝔤 be the first fundamental form on M. When 𝔤 is a metric, the pair (M, 𝔤) is called a regular surface in ℝ³_ν. In that case, we ascribe to 𝔤 those properties of 𝔤_p that are independent of p. Accordingly, 𝔤 is said to be bilinear, symmetric, nondegenerate, and so on. The common value of the ind(𝔤_p) is denoted by ind(𝔤) and called the index of 𝔤 or the index of M.

The next result shows that we could have defined a regular surface in ℝ³_ν using properties [V1] and [V2] instead of [G1] and [G2].

## Theorem 12.2.5. Let M be a regular surface. Then (M, 𝔤) is a regular surface in ℝ³_ν if and only if there is a unit normal vector field along M satisfying [V1] and [V2].

Proof. This follows from Theorem 12.2.3(a) and Theorem 12.2.4.

## Theorem 12.2.6. Let M be a regular surface, and let 𝔤 be the first fundamental form on M. Let (U, φ) be a chart on M, let (H₁, H₂) be the corresponding coordinate frame, and, using Example 11.2.5, view φ(U) as a regular surface. Then (φ(U), 𝔤|_{φ(U)}) is a regular surface in ℝ³_ν if and only if ⟨H₁|_q × H₂|_q, H₁|_q × H₂|_q⟩ is positive for all q in U, or negative for all q in U, where the vector product is computed with respect to the standard basis for ℝ³_ν.

Theorem 8.4.8, and Theorem 11.4.1(b) that $H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)} = c_p W_p$ for some nonzero real number $c_p$, hence

$$\langle H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)}, H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)} \rangle = c_p^2 \langle W_p, W_p \rangle$$

for all $p$ in $\varphi(U)$. Since $W$ satisfies [V2], property (*) is satisfied.

(⇐): We have from property (*) that $||H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)}|| \neq 0$ for all $p$ in $\varphi(U)$. Setting $(V,\mathfrak{g}) = \mathbb{R}^3_{\nu}$, $U = T_p(M)$, and $(u_1,u_2) = (H_1|_{\varphi^{-1}(p)}, H_2|_{\varphi^{-1}(p)})$ in Theorem 8.4.9, and using Theorem 11.4.1(b), shows that for all $p$ in $\varphi(U)$, $(\mathfrak{g}|_{\varphi(U)})|_p$ is nondegenerate on $T_p(\varphi(U))$ if and only if $||H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)}|| \neq 0$. Thus, $\mathfrak{g}|_{\varphi(U)}$ satisfies [G1]. For a given point $p$ in $\varphi(U)$, consider the vector

$$V_p = \frac{H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)}}{||H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)}||}.$$

We have from Theorem 8.4.8 that $V_p \neq (0,0,0)$, and from Theorem 8.4.4 and Theorem 11.4.1(b) that $V_p$ is in $T_p(\varphi(U))^{\perp}$. Thus, the assignment $p \longmapsto V_p$ defines a unit normal vector field $V$ along $\varphi(U)$. It follows from property (*) that $V$ satisfies [V2]. By Theorem 12.2.4, $\mathfrak{g}|_{\varphi(U)}$ satisfies [G2].

## Theorem 12.2.7 (Open Set). If $(M,\mathfrak{g})$ is a regular surface in $\mathbb{R}^3_{\nu}$ and $W$ is an open set in $M$, then $(W,\mathfrak{g}|_W)$ is a regular surface in $\mathbb{R}^3_{\nu}$. In particular, if $(U,\varphi)$ is a chart on $M$, then $(\varphi(U),\mathfrak{g}|_{\varphi(U)})$ is a regular surface in $\mathbb{R}^3_{\nu}$.

Proof. By Theorem 11.4.1, $W$ is a regular surface and $T_p(W) = T_p(M)$ for all $p$ in $W$. It follows that $(\mathfrak{g}|_W)|_p$ is nondegenerate on $T_p(W)$ and $\operatorname{ind}(\mathfrak{g}_p) = \operatorname{ind}((\mathfrak{g}|_W)_p)$ for all $p$ in $W$. Thus, $\mathfrak{g}|_W$ satisfies [G1] and [G2], which proves the first assertion. According to [C2] of Section 11.2, $\varphi(U)$ is an open set in $M$, so the second assertion follows from the first.

## Theorem 12.2.8 (Graph of Function). In the notation of Theorem 11.4.2, graph$(f)$ is a regular surface in $\mathbb{R}^3_{\nu}$ if and only if

$$\left(\frac{\partial f}{\partial x}(x,y)\right)^2 + \left(\frac{\partial f}{\partial y}(x,y)\right)^2 - 1$$

is positive for all $(x,y)$ in $U$, or negative for all $(x,y)$ in $U$.

Proof. As remarked in connection with Theorem 12.2.6, the case $\nu = 0$ is straightforward, so assume $\nu = 1$. We have from Example 11.2.5 that graph$(f)$ is a regular surface, from Theorem 11.4.2 that $(U,\varphi)$ is a covering chart, and from the proof of part (b) of Theorem 12.10.2 that

$$\langle H_1|_{(x,y)} \times H_2|_{(x,y)}, H_1|_{(x,y)} \times H_2|_{(x,y)} \rangle = \left(\frac{\partial f}{\partial x}(x,y)\right)^2 + \left(\frac{\partial f}{\partial y}(x,y)\right)^2 - 1.$$

The result now follows from Theorem 12.2.6.

## Theorem 12.2.9 (Surface of Revolution). In the notation of Theorem 11.4.3, rev(σ) is a regular surface in ℝ³_ν if and only if ḣ(t)² − ρ̇(t)² is positive for all t in (a,b), or negative for all t in (a,b).

Proof. As remarked in connection with Theorem 12.2.6, the case ν = 0 is straightforward, so assume ν = 1. We have from Example 11.2.5 that rev(σ) is a regular surface, from Theorem 11.4.3 that (U, φ) is a covering chart, and from the proof of part (b) of Theorem 12.10.4 that

```
⟨H₁|_(t,φ) × H₂|_(t,φ), H₁|_(t,φ) × H₂|_(t,φ)⟩ = ρ(t)²[ḣ(t)² − ρ̇(t)²].
```

By definition, ρ(t) is positive, hence nonzero, for all t in (a,b). The result now follows from Theorem 12.2.6.

Let 𝒰 be an open set in ℝ³_ν, and let f be a function in C^∞(𝒰). The gradient of f (in ℝ³_ν) is the map

Grad(f) : 𝒰 ⟶ ℝ³

defined by

Grad(f)_p = (∂f/∂x (p), ∂f/∂y (p), (−1)^ν ∂f/∂z (p))

for all p in 𝒰, where we note the presence of (−1)^ν. When ν = 0, the above identity simplifies to (11.4.3), in which case, Grad(f)_p = grad(f)_p.

## Theorem 12.2.10 (Level Set of Function). With the above setup, let c be a real number in f(𝒰), and let M = f⁻¹(c). Then:

- (a) If Grad(f) is nowhere-vanishing on M, then M is a regular surface.
- (b) If Grad(f) satisfies [V2], then (M,𝔤) is a regular surface in ℝ³_ν and Grad(f)/‖Grad(f)‖ is a unit normal vector field in 𝔛_{ℝ³_ν}(M).

Remark. We have from Theorem 12.2.1(d) that when ν = 0, the assumptions in parts (a) and (b) are equivalent.

Proof. For brevity, let V = Grad(f).
(a): Since V is nowhere-vanishing on M, so is grad(f). By Theorem 11.4.4, M is a regular surface.
(b): Let p be a point in M, and let v be a vector in T_p(M). By definition, there is a smooth curve λ = (λ¹,λ²,λ³) : (a,b) ⟶ M such that λ(t₀) = p and (dλ/dt)(t₀) = v for some t₀ in (a,b). Since f ∘ λ(t) = c for all t in (a,b), by Theorem 10.1.10,

```
0 = d/dt (f ∘ λ)(t₀) = Σ_i ∂f/∂x^i (p) dλ^i/dt (t₀)
= ⟨(∂f/∂x¹(p), ∂f/∂x²(p), (−1)^ν ∂f/∂x³(p)); (dλ¹/dt(t₀), dλ²/dt(t₀), dλ³/dt(t₀))⟩
= ⟨V_p, v⟩.
```

Because $p$ and $v$ were arbitrary, it follows that $V$ is a normal vector field along $M$ and $T_p(M)\subseteq (\mathbb{R}V_p)^\perp$ for all $p$ in $M$. We have from parts (a) and (c) of Theorem 12.2.1 that $V$ is nowhere-vanishing on $M$ and $W=V/\|V\|$ is a unit normal vector field along $M$. Since $f$ is smooth, so is $V$, and therefore, by Theorem 12.2.1(c), so is $W$; that is, $W$ is a vector field in $\mathfrak{X}_{\mathbb{R}^3_\nu}(M)$. Clearly, $W$ satisfies [V2]. Since $T_p(M)\subseteq (\mathbb{R}V_p)^\perp=(\mathbb{R}W_p)^\perp$, and by Theorem 11.3.1(a), $T_p(M)$ is 2-dimensional, it follows that $(\mathbb{R}W_p)^\perp$ is either 2-dimensional or 3-dimensional. The latter possibility is excluded because we would then have $(\mathbb{R}W_p)^\perp=\mathbb{R}^3_\nu$, hence $\langle W_p,W_p\rangle=0$, which contradicts the fact that $W$ satisfies [V2]. Thus, $(\mathbb{R}W_p)^\perp$ is 2-dimensional, so $T_p(M)=(\mathbb{R}W_p)^\perp$. By Theorem 4.1.2(c), $T_p(M)=(\mathbb{R}V_p)^\perp$ for all $p$ in $M$; that is, $W$ satisfies [V1]. It follows from part (a) and Theorem 12.2.5 that $(M,g)$ is a regular surface in $\mathbb{R}^3_\nu$.

Let $(M,g)$ be a regular surface in $\mathbb{R}^3_\nu$. We have from Theorem 12.2.5 that there is a (not necessarily smooth) unit normal vector field $V$ along $M$ satisfying [V1] and [V2]. As pointed out in the proof of Theorem 12.2.4, since $V$ is a unit vector field, [V2] is equivalent to: $\langle V_p,V_p\rangle=1$ for all $p\in M$, or $\langle V_p,V_p\rangle=-1$ for all $p\in M$. The common value of the $\langle V_p,V_p\rangle$ is denoted by $\epsilon_M$ and called the sign of $M$. Thus,

$$\epsilon_M=\langle V_p,V_p\rangle$$

for all $p\in M$. By Theorem 12.2.3(b), $\epsilon_M$ is independent of the choice of unit normal vector field along $M$ satisfying [V1] and [V2]. We have from Theorem 12.2.2(b) that

$$\epsilon_M=(-1)^{\nu-\operatorname{ind}(g)}.$$

A convenient way to determine $\operatorname{ind}(g)$ that avoids having to construct an orthonormal basis is to find $\epsilon_M$ using (12.2.3) and then compute $\operatorname{ind}(g)$ using (12.2.4). The values of $\nu$, $\operatorname{ind}(g)$, and $\epsilon_M$ are related to each other as follows:

| ν | ind(g) | ε_M |
|---:|---:|---:|
| 0 | 0 | 1 |
| 1 | 1 | 1 |
| 1 | 0 | -1 |

## Theorem 12.2.11.

*Let $(M,g)$ be a regular surface in $\mathbb{R}^3_\nu$, let $(U,\varphi)$ be a chart on $M$, let $(H_1,H_2)$ be the corresponding coordinate frame, and, using Example 11.2.5, view $\varphi(U)$ as a regular surface. Define a map $G:U\longrightarrow\mathbb{R}^3_\nu$, called the coordinate unit normal vector field corresponding to $(U,\varphi)$, by*

$$G_q=\epsilon_M\frac{H_1|_q\times H_2|_q}{\|H_1|_q\times H_2|_q\|}$$

for all $q\in U$, where the vector product is computed with respect to the standard basis for $\mathbb{R}^3_\nu$. Then:
