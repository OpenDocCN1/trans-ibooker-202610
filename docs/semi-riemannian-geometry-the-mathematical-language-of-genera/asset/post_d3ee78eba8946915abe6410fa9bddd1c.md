
+   [C1] φ : U ⟶ ℝ³ is an immersion.
+   [C2] φ(U) is an open set in M.
+   [C3] φ : U ⟶ φ(U) is a homeomorphism.

Condition [C1] has been discussed in detail. Conditions [C2] and [C3] are far from intuitive, but we can at least say about [C3] that it ensures φ is injective, thereby avoiding the problem of self-intersection discussed above.

When it is necessary to make the components of φ explicit in (U, φ), we use the notation (U, φ = (φⁱ)) or (U, φ = (φ¹, φ², φ³)). We refer to U as the coordinate domain of the chart, and to φ as its coordinate map. For each point p in φ(U), (U, φ) is said to be a chart at p. When φ(U) = M, we say that (U, φ) is a covering chart on M, and that M is covered by (U, φ). Two charts, (U, φ) and (Ũ, φ̃), on M are said to be overlapping if V = φ(U) ∩ φ̃(Ũ) is nonempty. In that case, the map

φ̃⁻¹ ∘ φ|φ⁻¹(V) : φ⁻¹(V) ⟶ φ̃⁻¹(V)

is called a transition map. For brevity, we usually denote

φ̃⁻¹ ∘ φ|φ⁻¹(V) by φ̃⁻¹ ∘ φ.

An atlas for M is a collection 𝔄 = {(Uα, φα) : α ∈ A} of charts on M such that the φα(Uα) form an open cover of M; that is,

M = ⋃_{α ∈ A} φα(Uα).

We are now in a position to replace our preliminary attempt at describing a “smooth surface” with something definitive. A regular surface (in ℝ³) is a pair (M, 𝔄), where M is a topological subspace of ℝ³ and 𝔄 is an atlas for M. A noteworthy feature of this definition is that it places no requirements on the choice of charts making up the atlas other than that their coordinate domains cover M. We usually adopt the shorthand of referring to M as a regular surface, with 𝔄 understood from the context.

## Example 11.2.5 (Chart). Let M be a regular surface, and let (U, φ) be a chart on M. Then φ(U) is a regular surface and (U, φ) is a covering chart. ◇

## Throughout, any chart on a regular surface is viewed as a regular surface.

## Example 11.2.6 (𝕊²). The unit sphere (centered at the origin) is

𝕊² = {(x, y, z) ∈ ℝ³ : x² + y² + z² = 1}.

Recall that

D = {(r, s) ∈ ℝ² : r² + s² < 1}

is the unit open disk. In what follows, we identify ℝ² with the xy-plane in ℝ³. Let us define functions φ₁, …, φ₆ : D ⟶ ℝ³ by:

```
φ₁(x, y) = (x, y, √1 − x² − y²)
φ₂(x, y) = (x, y, −√1 − x² − y²)
φ₃(x, y) = (x, √1 − x² − y², y)
φ₄(x, y) = (x, −√1 − x² − y², y)
φ₅(x, y) = (√1 − x² − y², x, y)
φ₆(x, y) = (−√1 − x² − y², x, y).
```

It can be shown that (D, φ₁), …, (D, φ₆) are charts on S², with S² = φ¹(D) ∪ ⋯ ∪ φ⁶(D). Thus, {(D, φ₁), …, (D, φ₆)} is an atlas for S², hence S² is a regular surface.

## Theorem 11.2.7. A regular surface is not an open set in ℝ³.

Proof. Let M be a regular surface, and let (U, φ) be a chart on M. According to [C2], φ(U) is open in M. By definition, there is an open set 𝒰 in ℝ³ such that φ(U) = M ∩ 𝒰. It follows that M is not open in ℝ³; for if it were, then φ(U) would be open in ℝ³, and then Theorem 9.4.1 and [C3] would give a contradiction.

An implication of Theorem 11.2.7 is that in order to investigate whether a function or map that has a regular surface as its domain is smooth, we need to rely on the extended version of smoothness described at the end of Section 10.1. The next result is an important case in point.

## Theorem 11.2.8 (Smoothness of Inverse Coordinate Map). If M is a regular surface and (U, φ) is a chart on M, then φ⁻¹ : φ(U) ⟶ U is (extended) smooth.

Proof. Let φ = (φ¹, φ², φ³), and let q be a point in U. According to [C2], there is a neighborhood 𝒱 of φ(q) in ℝ³ such that φ(U) = M ∩ 𝒱. It follows from [C1] and Theorem 11.2.2 that J_φ(q) has rank 2. Relabeling coordinates in ℝ³ if necessary, we have from Theorem 2.3.2 and Theorem 2.4.8 that det(J_φ(q)^(1,2)) ≠ 0. Define a smooth map F : U × ℝ ⟶ ℝ³ by

```
F(r, s, t) = φ(r, s) + (0, 0, t) = (φ¹(r, s), φ²(r, s), φ³(r, s) + t),
```

so that F(r, s, 0) = φ(r, s) for all (r, s) in U. In geometric terms, F can be thought of as sending “horizontal slices” of an “infinite cylinder” over U to “horizontal slices” of an “infinite cylinder” over φ(U). In particular, F sends

U × {(0, 0, 0)} to φ(U). See Figure 11.2.3. Since φ is smooth, so is F. We have

$$J_F(q,0)=\begin{bmatrix}\dfrac{\partial \varphi^1}{\partial r}(q)&\dfrac{\partial \varphi^1}{\partial s}(q)&0\\[4pt]\dfrac{\partial \varphi^2}{\partial r}(q)&\dfrac{\partial \varphi^2}{\partial s}(q)&0\\[4pt]\dfrac{\partial \varphi^3}{\partial r}(q)&\dfrac{\partial \varphi^3}{\partial s}(q)&1\end{bmatrix},$$

hence det($J_F(q,0)$) ≠ 0. By Theorem 10.2.1 and Theorem 10.2.3, there is a neighborhood $W \subset U \times \mathbb{R}$ of $(q,0)$ in $\mathbb{R}^3$ and a neighborhood $V$ of $\varphi(q)$ in $\mathbb{R}^3$ such that $F|_W : W \to V$ is a diffeomorphism. Since $V$ and $\mathcal V$ are open sets in $\mathbb{R}^3$, so is $V \cap \mathcal V$, hence $V \cap \mathcal V$ is a neighborhood of $\varphi(q)$ in $\mathbb{R}^3$. Replacing $V$ with $V \cap \mathcal V$, and $W$ with $(F|_W)^{-1}(V \cap \mathcal V)$ if necessary, we assume without loss of generality that $W \subseteq U \times \mathbb{R}$. In a similar way, we assume without loss of generality that $W$ is a “finite cylinder” over a neighborhood $U' \subseteq U$ of $q$ in the $rs$-plane. It follows that $V \cap M$ is a neighborhood of $\varphi(q)$ in $M$ and $\varphi(U') = V \cap M$. Let $\mathcal P : \mathbb{R}^3 \to \mathbb{R}^2$ be the projection map defined by $\mathcal P(x,y,z)=(x,y)$. Since $\mathcal P$ and $(F|_W)^{-1}$ are smooth, by Theorem 10.1.12, so is $\mathcal P \circ (F|_W)^{-1}:V\to U'$. We have

$$\mathcal P\circ(F|_W)^{-1}(\varphi(r,s))=\mathcal P\circ(F|_W)^{-1}(F(r,s,0))=\mathcal P(r,s,0)=(r,s)$$

for all $(r,s)$ in $U'$, hence $\mathcal P\circ(F|_W)^{-1}|_{\varphi(U')}=\varphi^{-1}|_{\varphi(U')}$. Thus, $\mathcal P\circ(F|_W)^{-1}$ is a smooth map on a neighborhood of $\varphi(q)$ in $\mathbb{R}^3$ that restricts to $\varphi^{-1}$ on a neighborhood of $\varphi(q)$ in $M$. Since $q$ was arbitrary, the result follows.

## **Theorem 11.2.9.**

*If $M$ is a regular surface, and $(U,\varphi)$ and $(\widetilde U,\widetilde\varphi)$ are overlapping charts on $M$, then the transition map*

$$\widetilde\varphi^{-1}\circ\varphi:\varphi^{-1}(V)\longrightarrow\widetilde\varphi^{-1}(V)$$

*is a (Euclidean) diffeomorphism, where $V=\varphi(U)\cap\widetilde\varphi(\widetilde U)$.*

*Proof.* We have from [C3] that $\widetilde\varphi^{-1}\circ\varphi$ is bijective. By definition, $\varphi$ is (Euclidean) smooth, and according to Theorem 11.2.8, $\widetilde\varphi^{-1}$ is (extended) smooth. It follows from Theorem 10.1.17 that $\widetilde\varphi^{-1}\circ\varphi$ is (Euclidean) smooth. Similarly, so is $\varphi^{-1}\circ\widetilde\varphi=(\widetilde\varphi^{-1}\circ\varphi)^{-1}$.

## **Theorem 11.2.10.**

*Let $M$ be a regular surface, and let $(U,\varphi)$ be a chart on $M$. If $U'$ is an open set in $U$, then $(U',\varphi|_{U'})$ is a chart on $M$.*

*Proof.* Since $(U,\varphi)$ is a chart on $M$, we have [C1]–[C3] at our disposal. With $U'$ open in $U$, and $U$ open in $\mathbb{R}^2$, by Theorem 9.1.5, $U'$ is open in $\mathbb{R}^2$. According to Theorem 10.1.11(a), $\varphi|_{U'}$ is smooth. Thus, $\varphi|_{U'}:U'\to\mathbb{R}^3$ is a parametrized surface. To show that $(U',\varphi|_{U'})$ is a chart on $M$, we need to prove that:

![](img/d712710a8eb78c0959204afd5dde4c91_248_0.png)

# Figure 11.2.3. Diagram for Theorem 11.2.8

[C1'] φ|U' : U' → ℝ³ is an immersion.

[C2'] φ|U'(U') is an open set in M.

[C3'] φ|U' : U' → φ|U'(U') is a homeomorphism.

The proofs are as follows:

[C1']: This follows from [C1].

[C2']: Since U' is open in U, it follows from [C3] and Theorem 9.1.14 that φ(U') is open in φ(U). According to [C2], φ(U) is open in M. By Theorem 9.1.5, φ|U'(U') = φ(U') is open in M.

[C3']: Let U'' be a subset of U'. By assumption, U' is open in U, and according to [C3], φ : U → φ(U) is a homeomorphism. We therefore have from Theorem 9.1.14 that U'' is open in U' if and only if φ|U'(U'') = φ(U'') is open in φ|U'(U') = φ(U'). The result now follows from Theorem 9.1.14.

By definition, a regular surface is a patchwork of images of parametrized surfaces. The next result shows that instead of images of parametrized surfaces, we can use graphs of smooth functions.

## Theorem 11.2.11. If M is a regular surface and p is a point in M, then there is a chart (V, ψ) at p and a function f in C^∞(V) such that ψ(V) = graph(f),
