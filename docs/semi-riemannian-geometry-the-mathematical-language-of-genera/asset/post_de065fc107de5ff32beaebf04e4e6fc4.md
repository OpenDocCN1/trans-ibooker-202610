
The volume of M is obtained by taking f to be the function in C^∞(M) with constant value 1:

vol(M) = ∫_M Ω_M.

## Example 19.10.3 (Area of Regular Surface in ℝ₀³).

Let (M, 𝔤, 𝔒) be a compact oriented regular surface in ℝ₀³, let (U, φ) be a positively oriented chart on M, and let ℋ = (H₁, H₂) be the corresponding coordinate frame. The term “chart” is used here in the sense of Section 11.2: U is an open set in ℝ² and φ : U ⟶ ℝ₀³ is a smooth map. In the present setting, we refer to “volume” as “area”. Working through the definitions in Section 16.7 and using Theorem 19.10.2, we find that

area(φ(U)) = ∬_U √det(𝔤_ℋ) dr¹ dr².

We have from Theorem 4.6.3 that det(𝔤_ℋ) > 0 on U, so the above integral is well-defined. By Theorem 8.4.6(b), ‖H₁ × H₂‖ = √det(𝔤_ℋ), hence

area(φ(U)) = ∬_U ‖H₁ × H₂‖ dr¹ dr².

This is the definition of area familiar from the integral calculus of two real variables.

## Example 19.10.4 (𝓢²).

Let us view (𝓢², 𝔢|𝓢²) as a Riemannian 2-submanifold of the Riemannian 3-manifold ℝ₀³ = (ℝ³, 𝔢). Recall the results of Section 13.4, and consider the chart (U, φ = (θ, φ)) on 𝓢², where U = (0, π) × (−π, π) and φ : U ⟶ 𝓢² is given by

φ(θ, φ) = (sin(θ) cos(φ), sin(θ) sin(φ), cos(θ)).

From

𝔤_ℋ(θ, φ) = [1 0; 0 sin²(θ)]

and Theorem 19.10.2, the volume form is given by

Ω_𝓢²|_(θ,φ) = √det(𝔤_ℋ(θ, φ)) d(θ) ∧ d(φ) = sin(θ) d(θ) ∧ d(φ).

Since φ(U) equals 𝓢² except for a portion of a great circle (which has an area of 0), the area of 𝓢² is

area(𝓢²) = ∬_U Ω_𝓢²|_(θ,φ) dθ dφ = ∫_{-π}^{π} ∫_0^{π} sin(θ) dθ dφ = 4π.

## 19.11 Orientation of Hypersurfaces

In this section, we summarize and extend some of the earlier results on the orientation of hypersurfaces.

vector field in 𝔛_M̄(M ∩ U ∩ V). See Figure 19.11.1(b), where G is the “vector product of ∂/∂x¹”, and therefore orthogonal to ∂/∂x¹.

Let (M ∩ Ũ, (x̃¹, …, x̃m−1)) be another (positively oriented) chart in 𝔐, let X̃ = (∂/∂x̃¹, …, ∂/∂x̃m−1) be the corresponding (positively oriented) coordinate frame on M ∩ Ũ, let Ẽ be a positively oriented orthonormal frame on a neighborhood Ṽ in M̄ that intersects M ∩ Ũ, and let G̃|_{M∩Ũ∩Ṽ} be the corresponding unit normal vector field in 𝔛_M̄(M ∩ Ũ ∩ Ṽ). Suppose M ∩ U ∩ V intersects M ∩ Ũ ∩ Ṽ. Since 𝒳 and X̃ are positively oriented, they are consistent, and for the same reason, so are Ẽ and Ẽ. It follows from Theorem 8.4.11(b) and Theorem 8.4.13(b) that G_p = G̃_p for all p in M ∩ (U ∩ V) ∩ (Ũ ∩ Ṽ). We can therefore assign to each point p in M a vector 𝒩_p that is unit normal at p, and do so in such a way that there is agreement whenever coordinate domains and the domains of orthonormal frames overlap with M. This defines a unit normal vector field 𝒩 along M. By construction, 𝒩 is smooth on a neighborhood of every point in M, so it is smooth on M. Thus, 𝒩 is a unit normal vector field in 𝔛_M̄(M).

(⇐): Let V be a unit normal vector field in 𝔛_M̄(M). Since V is a nowhere-tangent vector field in 𝔛_M̄(M), it follows from Theorem 16.6.9 that V induces an orientation of M. By definition, there is a consistent smooth atlas for M that induces the orientation, and therefore, by definition, M is orientable.

## Theorem 19.11.2.

Let (M̄, ḡ, 𝔇) be an oriented semi-Riemannian m̄-manifold or an oriented semi-Riemannian m̄-manifold with boundary, where m̄ ≥ 2, and let Ω_M̄ be its volume form. Let (M, ḡ|_M) be an orientable semi-Riemannian hypersurface of M̄ or an orientable semi-Riemannian hypersurface with boundary of M̄. Let V be a unit normal vector field in 𝔛_M̄(M) (as given by Theorem 19.11.1), and let 𝔇_M be the orientation of M induced by i_V(Ω_M̄)|_M (as given by Theorem 16.6.9), so that (M, ḡ|_M, 𝔇_M) is an oriented semi-Riemannian hypersurface of M̄ or an oriented semi-Riemannian hypersurface with boundary of M̄. Let Ω_M be its volume form. Then:

(a)

 i_V(Ω_M̄)|_M = Ω_M.

(b) More generally, if X is a vector field in 𝔛_M̄(M), then

 i_X(Ω_M̄)|_M = ε_M⟨X, V⟩ Ω_M.

Proof. This is the manifold version of Theorem 8.3.6.

We noted above that for smooth manifolds with boundary, Theorem 17.2.5 ensures the existence of an outward-pointing smooth vector field along the boundary. In the more specialized setting of semi-Riemannian manifolds, the outward-pointing smooth vector field can be upgraded to a unit normal smooth vector field, as we now show.

## Theorem 19.11.3 (Existence of Outward-Pointing Vector Field).

Let (M, g) be a semi-Riemannian m-manifold with boundary, where m ≥ 2, and

or $\mathcal{N}|_{\partial M\cap M}= -G|_{\partial M\cap M}$. This shows that $\mathcal{N}$ is smooth on a neighborhood in $\partial M$ of every point in $\partial M$. Thus, $\mathcal{N}$ is smooth on $\partial M$.

## **Theorem 19.11.4.**

Let $(M,\epsilon|_M)$ be an orientable Riemannian hypersurface with boundary of $\mathbb{R}_0^3=(\mathbb{R}^3,\epsilon)$, let $V=(V^1,V^2,V^3)$ be a unit normal vector field in $\mathfrak{X}_{\mathbb{R}_0^3}(M)$ (as given by Theorem 19.11.1), and let $\mathcal{D}_M$ be the orientation of $M$ induced by $V$, so that $(M,\epsilon|_M,\mathcal{D}_M)$ is an oriented Riemannian hypersurface with boundary of $\mathbb{R}_0^3$. Let $\Omega_M$ be its volume form. Let $W$ be the unique outward-pointing unit normal vector field in $\mathfrak{X}(\partial M)$ (as given by Theorem 19.11.3), and let $\mathcal{D}_{\partial M}$ be the (Stokes) orientation induced by $W$, so that $(\partial M,\epsilon|_{\partial M},\mathcal{D}_{\partial M})$ is an oriented Riemannian hypersurface of $(M,\epsilon|_M,\mathcal{D}_M)$. Let $\Omega_{\partial M}$ be its volume form, and let $U=V\times W$. Then:

+   (a) For all points $p$ in $M$ and all vectors $v,w$ in $T_p(M)$,
+   
+   $$
+   \Omega_M|_p(v,w)=\langle V_p,v\times w\rangle
+   $$
+   
+   $$
+   =\bigl(V^1(p)\,d(y)\wedge d(z)+V^2(p)\,d(z)\wedge d(x)
+   +V^3(p)\,d(x)\wedge d(y)\bigr)(v,w).
+   $$

+   (b) For all points $q$ in $\partial M$ and all vectors $u$ in $T_q(\partial M)$,
+   
+   $$
+   \Omega_{\partial M}|_q(u)=\langle U_q,u\rangle.
+   $$

+   (c) For all points $q$ in $\partial M$, $(U_q,V_q,W_q)$ and $(V_q,W_q,U_q)$ are orthonormal bases for $\mathbb{R}_0^3$ that have the standard orientation.

+   (d) $U$ is the unique unit vector field in $\mathfrak{X}(\partial M)$ that is positively oriented with respect to $\mathcal{D}_{\partial M}$.

+   (e) The function $\Omega_{\partial M}(U)$ in $C^\infty(\partial M)$ has constant value $1$.

**Proof.** (a): Let $v=(a^1,a^2,a^3)$ and $w=(b^1,b^2,b^3)$, and let $\Omega_{\mathbb{R}_0^3}=d(x)\wedge d(y)\wedge d(z)$ be the volume form on $\mathbb{R}_0^3$, where, as usual, $\mathbb{R}_0^3$ is assumed to have the standard orientation. Then

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

where the first equality follows from Theorem 19.11.2(a), the fourth and fifth equalities from Theorem 8.4.5(a), and the last equality from Theorem 7.2.8.

(b): We have

```
Ω_{∂M}|_q(u) = (i_W(Ω_M)|_{∂M})|_q(u) = Ω_M|_q(W_q, u)
= ⟨V_q, W_q × u⟩ = ⟨V_q × W_q, u⟩ = ⟨U_q, u⟩,
```

where the first equality follows from Theorem 19.11.2(a), the third equality from part (a), and the fourth equality from Theorem 8.4.5(a).

(c): By definition, V_q is a unit vector in T_q(M)⊥, W_q is a unit vector in T_q(M), and U_q = V_q × W_q. It follows from Theorem 8.4.6(b) that U_q is a unit vector. Thus, (U_q, V_q, W_q), hence (V_q, W_q, U_q), is an orthonormal basis for ℝ₀³. By Theorem 8.4.10(b), (U_q, V_q, W_q) has the standard orientation, so (V_q, U_q, W_q) has the opposite orientation, hence (V_q, W_q, U_q) has the standard orientation. See Figure 19.11.2.

![](img/d712710a8eb78c0959204afd5dde4c91_572_0.png)

# Figure 19.11.2. Diagram for Theorem 19.11.4

(d): Let u be a nonzero vector in T_q(∂M). It follows from item (v) in the introduction to this section that (u) is a basis for T_q(∂M) that is positively oriented with respect to 𝔇_{∂M}(q) if and only if (V_q, W_q, u) is a basis for ℝ₀³ that has the standard orientation. We therefore have from part (c) that (U_q) is a basis for T_q(∂M) that is positively oriented with respect to 𝔇_{∂M}(q). Since q was arbitrary, the result follows.

(e): By part (b),

```
Ω_{∂M}|_q(U_q) = ⟨U_q, U_q⟩ = 1
```

for all q in ∂M. The result follows.

We note that, taken together, parts (d) and (e) of the preceding theorem are consistent with Theorem 19.10.1. As an example of part (a), the outward-pointing unit normal vector field for the unit sphere S² is given by V_{(x,y,z)} = (x, y, z), so

```
Ω_{S²}|_{(x,y,z)} = x d(y) ∧ d(z) + y d(z) ∧ d(x) + z d(x) ∧ d(y).
```
