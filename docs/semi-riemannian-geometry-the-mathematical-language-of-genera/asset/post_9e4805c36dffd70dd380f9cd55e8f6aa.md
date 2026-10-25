
covariant tensor fields. We seek a corresponding pullback for differential forms. An observation is that Λ^s(M) is a subspace of T_s^0(M), Λ^s(N) is a subspace of T_s^0(N), and F*(Λ^s(N)) is a subspace of Λ^s(M), so we can proceed by restricting the maps defined in (15.12.1)–(15.12.3). Pullback by F (for differential forms) is the family of linear maps

F* : Λ^s(N) ⟶ Λ^s(M)

defined for s ≥ 1 by

F*(ω)_p(v1,…,vs) = d_p(F)*(ω_{F(p)})(v1,…,vs) = ω_{F(p)}(d_p(F)(v1),…,d_p(F)(vs))

for all differential forms ω in Λ^s(N), all points p in M, and all vectors v1,…,vs in T_p(M). We refer to F*(ω) as the pullback of ω by F. As before, when s = 0, we define F* = F•.

## Theorem 15.13.1. Let M, N, and P be smooth manifolds, let F : M ⟶ N and G : N ⟶ P be smooth maps, let ω, ξ, and ζ be forms in Λ^s(N) and Λ^{s'}(N), respectively, and let g be a function in C^∞(N). Then:
- (a) F*(ω + ξ) = F*(ω) + F*(ξ).
- (b) F*(gω) = F*(g)F*(ω).
- (c) F*(ω ∧ ζ) = F*(ω) ∧ F*(ζ).
- (d) (G ∘ F)* = F* ∘ G*.

## Theorem 15.13.2. Let M be a smooth m-manifold, and let f^1,…,f^s be functions in C^∞(M), where s ≤ m. Let (U,(x^j)) be a chart on M, and let (∂/∂x^1,…,∂/∂x^m) and (d(x^1),…,d(x^m)) be the corresponding coordinate and dual coordinate frames. Then, in local coordinates,

d(f^1) ∧ ⋯ ∧ d(f^s) = Σ_{1≤j1<⋯<js≤m} det[ ∂f^1/∂x^1 … ∂f^1/∂x^m ; ⋮ ⋱ ⋮ ; ∂f^s/∂x^1 … ∂f^s/∂x^m ]_{(j1,…,js)} d(x^{j1}) ∧ ⋯ ∧ d(x^{js}).

Proof. In local coordinates, let

With the aid of (14.5.4) and Theorem 15.8.3(a), the left-hand side of the preceding identity can be expressed as

d(f¹) ∧ ⋯ ∧ d(fˢ) ( ∂/∂x^{i₁}, …, ∂/∂x^{iₛ} ) = det ⎛⎜⎝ d(f¹)(∂/∂x^{i₁}) … d(f¹)(∂/∂x^{iₛ}) ⋮ ⋱ ⋮ d(fˢ)(∂/∂x^{i₁}) … d(fˢ)(∂/∂x^{iₛ}) ⎞⎟⎠ = det ⎛⎜⎝ ∂f¹/∂x^{i₁} … ∂f¹/∂x^{iₛ} ⋮ ⋱ ⋮ ∂fˢ/∂x^{i₁} … ∂fˢ/∂x^{iₛ} ⎞⎟⎠ = det ⎛⎜⎝ ∂f¹/∂x¹ … ∂f¹/∂xᵐ ⋮ ⋱ ⋮ ∂fˢ/∂x¹ … ∂fˢ/∂xᵐ ⎞⎟⎠_{(i₁,…,iₛ)}.

Thus,

α_{j₁,…,jₛ} = det ⎛⎜⎝ ∂f¹/∂x¹ … ∂f¹/∂xᵐ ⋮ ⋱ ⋮ ∂fˢ/∂x¹ … ∂fˢ/∂xᵐ ⎞⎟⎠_{(j₁,…,jₛ)}. (15.13.2)

Substituting (15.13.2) into (15.13.1) gives the result.

## Theorem 15.13.3 (Pullback of Differential Form). Let M be a smooth m-manifold, let N be a smooth n-manifold, let F : M ⟶ N be a smooth map, and let (U, (xⁱ)) and (V, (yʲ)) be charts on M and N, respectively, such that U ∩ F⁻¹(V) is nonempty. Let ω be a form in Λˢ(N), and, in local coordinates, let

ω = Σ_{1 ≤ j₁ < ⋯ < jₛ ≤ n} α_{j₁…jₛ} d(y^{j₁}) ∧ ⋯ ∧ d(y^{jₛ}), where s ≤ min(m, n). Then:

(a) F*(ω) = Σ_{1 ≤ i₁ < ⋯ < iₛ ≤ m} [ Σ_{1 ≤ j₁ < ⋯ < jₛ ≤ n} (α_{j₁…jₛ} ∘ F) det( (J_F)_{(i₁,…,iₛ)}^{(j₁,…,jₛ)} ) ] · d(x^{i₁}) ∧ ⋯ ∧ d(x^{iₛ}), where

J_F = ⎡ ∂(y¹ ∘ F)/∂x¹ … ∂(y¹ ∘ F)/∂xᵐ ⎤ ⋮ ⋱ ⋮ ⎣ ∂(yⁿ ∘ F)/∂x¹ … ∂(yⁿ ∘ F)/∂xᵐ ⎦ (15.13.3)

is the corresponding Jacobian matrix.

(b) If $s = m = n$ and $\omega = \alpha\, d(y^1) \wedge \cdots \wedge d(y^m)$, then

$$F^*(\omega) = (\alpha \circ F)\,\det(J_F)\, d(x^1) \wedge \cdots \wedge d(x^m).$$

Proof. (a): We have from Theorem 15.13.1 that

$$F^*(\omega) = F^*\left(\sum_{1 \le j_1 < \cdots < j_s \le n} \alpha_{j_1 \dots j_s}\, d(y^{j_1}) \wedge \cdots \wedge d(y^{j_s})\right)$$ 

$$= \sum_{1 \le j_1 < \cdots < j_s \le n} F^*(\alpha_{j_1 \dots j_s})\, F^*\left(d(y^{j_1}) \wedge \cdots \wedge d(y^{j_s})\right), \qquad (15.13.4)$$ 

and from (15.9.2) and (15.12.3) that

$$F^*(\alpha_{j_1 \dots j_s}) = \alpha_{j_1 \dots j_s} \circ F^*. \qquad (15.13.5)$$ 

We also have

$$F^*\left(d(y^{j_1}) \wedge \cdots \wedge d(y^{j_s})\right)$$ 

$$= F^*\left(d(y^{j_1})\right) \wedge \cdots \wedge F^*\left(d(y^{j_s})\right) \qquad [\text{Th 15.13.1(c)}]$$

$$= d\left(F^*(y^{j_1})\right) \wedge \cdots \wedge d\left(F^*(y^{j_s})\right) \qquad [\text{Th 15.11.2, (15.12.3)}]$$

$$= d(y^{j_1} \circ F) \wedge \cdots \wedge d(y^{j_s} \circ F), \qquad [(15.9.2),\ (15.12.3)]$$

and from Theorem 15.13.2 that

$$d(y^{j_1} \circ F) \wedge \cdots \wedge d(y^{j_s} \circ F)$$ 

$$= \sum_{1 \le i_1 < \cdots < i_s \le m} \det\left(\begin{bmatrix} \dfrac{\partial(y^{j_1} \circ F)}{\partial x^1} & \cdots & \dfrac{\partial(y^{j_s} \circ F)}{\partial x^m} \\ \vdots & \ddots & \vdots \\ \dfrac{\partial(y^{j_s} \circ F)}{\partial x^1} & \cdots & \dfrac{\partial(y^{j_s} \circ F)}{\partial x^m} \end{bmatrix}_{(i_1,\dots,i_s)}\right) \cdot d(x^{i_1}) \wedge \cdots \wedge d(x^{i_s})$$

$$= \sum_{1 \le i_1 < \cdots < i_s \le m} \det\left(\begin{bmatrix} \dfrac{\partial(y^1 \circ F)}{\partial x^1} & \cdots & \dfrac{\partial(y^1 \circ F)}{\partial x^m} \\ \vdots & \ddots & \vdots \\ \dfrac{\partial(y^n \circ F)}{\partial x^1} & \cdots & \dfrac{\partial(y^n \circ F)}{\partial x^m} \end{bmatrix}_{(i_1,\dots,i_s)}^{(j_1,\dots,j_s)}\right) \cdot d(x^{i_1}) \wedge \cdots \wedge d(x^{i_s})$$

$$= \sum_{1 \le i_1 < \cdots < i_s \le m} \det\left((J_F)_{(i_1,\dots,i_s)}^{(j_1,\dots,j_s)}\right) d(x^{i_1}) \wedge \cdots \wedge d(x^{i_s}).$$

Thus,

$$F^*\left(d(y^{j_1}) \wedge \cdots \wedge d(y^{j_s})\right)$$ 

$$= \sum_{1 \le i_1 < \cdots < i_s \le m} \det\left((J_F)_{(i_1,\dots,i_s)}^{(j_1,\dots,j_s)}\right) d(x^{i_1}) \wedge \cdots \wedge d(x^{i_s}). \qquad (15.13.6)$$

Substituting (15.13.5) and (15.13.6) into (15.13.4) and reversing summations gives the result.
(b): This follows from part (a).

## Example 15.13.4 (Polar Coordinates).

Let $(r,s)$ be standard coordinates on $\mathbb{R}^2$, and let $(\rho,\phi)$ be polar coordinates on

$$U = \{(\rho,\phi) \in \mathbb{R}^2 : \rho > 0, 0 \le \phi < 2\pi\}.$$

Let $F : U \longrightarrow \mathbb{R}^2$ be the smooth map given by

$$F(\rho,\phi) = (\rho \cos(\phi), \rho \sin(\phi)),$$

and consider the form

$$\omega_{(r,s)} = d(r) \wedge d(s)$$

in $\Lambda^2(\mathbb{R}^2)$. Setting $(U,(x^i)) = (U,(\rho,\phi))$ and $(V,(y^j)) = (\mathbb{R}^2,(r,s))$ in Theorem 15.13.3(b) yields

$F^*(d(r) \wedge d(s)) = \det\begin{pmatrix} \dfrac{\partial(r \circ F)}{\partial \rho} & \dfrac{\partial(r \circ F)}{\partial \phi} \\ \dfrac{\partial(s \circ F)}{\partial \rho} & \dfrac{\partial(s \circ F)}{\partial \phi} \end{pmatrix} d(\rho) \wedge d(\phi)$

$= \det\begin{pmatrix} \dfrac{\partial(\rho \cos(\phi))}{\partial \rho} & \dfrac{\partial(\rho \cos(\phi))}{\partial \phi} \\ \dfrac{\partial(\rho \sin(\phi))}{\partial \rho} & \dfrac{\partial(\rho \sin(\phi))}{\partial \phi} \end{pmatrix} d(\rho) \wedge d(\phi)$

$= \det\begin{pmatrix} \cos(\phi) & -\rho \sin(\phi) \\ \sin(\phi) & \rho \cos(\phi) \end{pmatrix} d(\rho) \wedge d(\phi)$

$= \rho \, d(\rho) \wedge d(\phi).$

## 15.14 Contraction of Tensor Fields

This brief section presents the manifold versions of several of the results in Section 5.4.

## Theorem 15.14.1.

If $M$ is a smooth manifold, then there is a unique $C^\infty(M)$-linear map

$$\mathrm{C}_1^1 : \mathcal{T}_1^1(M) \longrightarrow C^\infty(M),$$

called $(1,1)$-contraction, such that

$$\mathrm{C}_1^1(X \otimes \omega) = \omega(X)$$

for all vector fields $X$ in $\mathfrak{X}(M)$ and all covector fields $\omega$ in $\mathfrak{X}^*(M)$.

## Theorem 15.14.2. Let \(M\) be a smooth manifold, let \(\mathcal A\) be a tensor field in \(\mathcal T^1_1(M)\), and, in local coordinates, let

$$\mathcal A=\sum_{ij} A^i_j\frac{\partial}{\partial x^i}\otimes d(x^j).$$

Then

$$C^1_1(\mathcal A)=\sum_i A^i_i.$$

## Theorem 15.14.3. If \(M\) is a smooth manifold, and \(1\leq k\leq r\) and \(1\leq l\leq s\) are integers, then there is a unique \(C^\infty(M)\)-linear map

$$C^k_l:\mathcal T^r_s(M)\longrightarrow\mathcal T^{r-1}_{s-1}(M),$$

called **\((k,l)\)-contraction**, such that

$$C^k_l(X_1\otimes\cdots\otimes X_r\otimes\omega^1\otimes\cdots\otimes\omega^s)$$
$$=X_k(\omega^l)X_1\otimes\cdots\otimes\widehat{X_k}\otimes\cdots\otimes X_r\otimes\omega^1\otimes\cdots\otimes\widehat{\omega^l}\otimes\cdots\otimes\omega^s$$

for all vector fields \(X_1,\ldots,X_r\) in \(\mathfrak X(M)\) and all covector fields \(\omega^1,\ldots,\omega^s\) in \(\mathfrak X^*(M)\), where \(\widehat{\phantom X}\) indicates that an expression is omitted.

## Theorem 15.14.4. Let \(M\) be a smooth manifold, let \(\mathcal A\) be a tensor field in \(\mathcal T^r_s(M)\), let \(\omega^1,\ldots,\omega^r\) be covector fields in \(\mathfrak X^*(M)\), and let \(X_1,\ldots,X_s\) be vector fields in \(\mathfrak X(M)\). Then there is a contraction \(C\) on \(M\) such that

$$\mathcal A(\omega^1,\ldots,\omega^r,X_1,\ldots,X_s)=C(X_1\otimes\cdots\otimes X_s\otimes\mathcal A\otimes\omega^1\otimes\cdots\otimes\omega^r).$$
