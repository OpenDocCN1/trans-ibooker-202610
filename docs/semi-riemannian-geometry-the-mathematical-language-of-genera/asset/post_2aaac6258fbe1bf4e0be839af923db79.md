
Proof. Let ℰ = (e₁, …, eₘ), and let V have signature (ε₁, …, εₘ).

(a): Let u = Σᵢ bⁱ eᵢ, so that [u]ℰ = [b¹ … bᵐ]ᵀ. We have from Theorem 2.4.6 and (8.4.1) that

⟨u, v₁ × ⋯ × vₘ₋₁⟩
= ⟨ Σᵢ bⁱ eᵢ, Σⱼ εⱼ (−1)ʲ⁻¹ det( [[v₁]ℰ … [vₘ₋₁]ℰ]⁽ʲ⁾ᶜ ) eⱼ ⟩
= Σᵢⱼ bⁱ ⟨eᵢ, eⱼ⟩ εⱼ (−1)ʲ⁻¹ det( [[v₁]ℰ … [vₘ₋₁]ℰ]⁽ʲ⁾ᶜ )
= Σᵢ (−1)ⁱ⁻¹ bⁱ det( [[v₁]ℰ … [vₘ₋₁]ℰ]⁽ⁱ⁾ᶜ )
= det( [[u]ℰ [v₁]ℰ … [vₘ₋₁]ℰ] ),

which proves the first equality. By Theorem 1.2.1(d), u = Σᵢ ξⁱ(u) eᵢ, hence

[u]ℰ = [ξ¹(u) … ξᵐ(u)]ᵀ,

with corresponding expressions for [v₁]ℰ, …, [vₘ₋₁]ℰ. Then Theorem 7.2.8 gives

ξ¹ ∧ ⋯ ∧ ξᵐ(u, v₁, …, vₘ₋₁) = det( [ ξ¹(u) ξ¹(v₁) … ξ¹(vₘ₋₁) ; ⋮ ⋮ ⋱ ⋮ ; ξᵐ(u) ξᵐ(v₁) … ξᵐ(vₘ₋₁) ] )
= det( [[u]ℰ [v₁]ℰ … [vₘ₋₁]ℰ] ),

which proves the second equality.

(b): Let

P = [[u]ℰ [v₁]ℰ … [vₘ₋₁]ℰ],

and note that 𝔮ℰ = diag(ε₁, …, εₘ). Then

det( [ ⟨u,u⟩ ⟨u,v₁⟩ … ⟨u,vₘ₋₁⟩ ; ⟨v₁,u⟩ ⟨v₁,v₁⟩ … ⟨v₁,vₘ₋₁⟩ ; ⋮ ⋮ ⋱ ⋮ ; ⟨vₘ₋₁,u⟩ ⟨vₘ₋₁,v₁⟩ … ⟨vₘ₋₁,vₘ₋₁⟩ ] )
= det(Pᵀ 𝔮ℰ P) [Th 3.1.2(b)]
= det(𝔮ℰ) det(P)²
= (−1)ᵛ det(P)² [(4.2.3)]
= (−1)ᵛ ⟨u, v₁ × ⋯ × vₘ₋₁⟩². [part (a)]

## Theorem 8.4.6. Let (V, 𝔤) be a scalar product space of dimension m ≥ 2 and index ν, let ℰ be an orthonormal basis for V, and let v₁, …, vₘ₋₁, w₁, …, wₘ₋₁ be vectors in V. Then:

Since [[v_j]ℰ [v₂]ℰ … [vₘ₋₁]ℰ] has [v_j]ℰ in (at least) two positions, it follows from Theorem 2.4.2 that det([[v_j]ℰ [v₂]ℰ … [vₘ₋₁]ℰ]^{(i)^c}) = 0 for j = 2, …, m − 1. Thus,

det([[v₁]ℰ [v₂]ℰ … [vₘ₋₁]ℰ]^{(i)^c}) = 0

for i = 1, …, m. It follows from (8.4.1) that v₁ × ⋯ × vₘ₋₁ = 0.

(⇐): Since the dimension of span({v₁, …, vₘ₋₁}) is at most m − 1, there is a vector v₀ in V \ span({v₁, …, vₘ₋₁}). Then

⟨v₀, v₁ × ⋯ × vₘ₋₁⟩ = 0
⇔ det([[v₀]ℰ [v₁]ℰ … [vₘ₋₁]ℰ]) = 0 [Th 8.4.5(a)]
⇔ [v₀]ℰ, [v₁]ℰ, …, [vₘ₋₁]ℰ are linearly dependent [Th 2.4.9]
⇔ v₀, v₁, …, vₘ₋₁ are linearly dependent [Th 2.2.1]
⇔ v₁, …, vₘ₋₁ are linearly dependent. [Th 1.1.2]

Thus, if v₁ × ⋯ × vₘ₋₁ = 0, then v₁, …, vₘ₋₁ are linearly dependent.

Whether vectors in a vector space are linearly independent or linearly dependent is unrelated to the presence or absence of a scalar product. This means that in Theorem 8.4.8, linear independence can be checked using any convenient choice of scalar product and orthonormal basis.

## Theorem 8.4.9. Let (V, g) be a scalar product space of dimension m ≥ 2, let ℰ be an orthonormal basis for V, let U be a subspace of V of dimension m − 1, and let (u₁, …, uₘ₋₁) be a basis for U. Then g is nondegenerate on U if and only if ||u₁ × ⋯ × uₘ₋₁|| ≠ 0.

Proof. The matrix of g|_U with respect to (u₁, …, uₘ₋₁) is [⟨u_i, u_j⟩], so

- g is nondegenerate on U
- ⇔ g|_U is nondegenerate on U [by definition]
- ⇔ det([⟨u_i, u_j⟩]) ≠ 0 [Th 3.3.3]
- ⇔ ⟨u₁ × ⋯ × uₘ₋₁, u₁ × ⋯ × uₘ₋₁⟩ ≠ 0 [Th 8.4.6(a)]
- ⇔ ||u₁ × ⋯ × uₘ₋₁|| ≠ 0.

## Theorem 8.4.10. Let (V, g) be a scalar product space of dimension m ≥ 2, and let ℰ be an orthonormal basis for V. Let U be a subspace of V of dimension m − 1 on which g is nondegenerate, let (u₁, …, uₘ₋₁) be a basis for U, and let ε be the sign of ⟨u₁ × ⋯ × uₘ₋₁, u₁ × ⋯ × uₘ₋₁⟩, so that

ε = ⟨ u₁ × ⋯ × uₘ₋₁ / ||u₁ × ⋯ × uₘ₋₁|| , u₁ × ⋯ × uₘ₋₁ / ||u₁ × ⋯ × uₘ₋₁|| ⟩ . (8.4.5)

Then:

- (a) $U^\perp = \mathbb{R}(u_1 \times \cdots \times u_{m-1})$ is a 1-dimensional subspace of $V$.

- (b) $(\epsilon u_1 \times \cdots \times u_{m-1}, u_1, \ldots, u_{m-1})$ is a basis for $V$ that is consistent with $\mathcal{E}$.

- (c) If $(u_1, \ldots, u_{m-1})$ is an orthonormal basis for $U$, then $(\epsilon u_1 \times \cdots \times u_{m-1}, u_1, \ldots, u_{m-1})$ is an orthonormal basis for $V$.

*Remark.* By Theorem 8.4.9, $\langle u_1 \times \cdots \times u_{m-1}, u_1 \times \cdots \times u_{m-1} \rangle \neq 0$, so $\epsilon$ is defined and the denominators in (8.4.5) are nonzero.

*Proof.* (a): We have from Theorem 4.1.2(b) that $U^\perp$ is a 1-dimensional subspace of $V$, from Theorem 8.4.3(a) that $u_1 \times \cdots \times u_{m-1}$ is in $U^\perp$, and from Theorem 8.4.8 that $u_1 \times \cdots \times u_{m-1} \neq 0$. The result follows.

(b): By Theorem 4.1.3, $V = U \oplus U^\perp$. Since $(u_1, \ldots, u_{m-1})$ is a basis for $U$, it follows from Theorem 1.1.2 and part (a) that $\mathcal{U} = (\epsilon u_1 \times \cdots \times u_{m-1}, u_1, \ldots, u_{m-1})$ is a basis for $V$. We have from (2.2.7) and Theorem 8.4.5(a) that

$\det\big([\mathrm{id}_V]^\mathcal{E}_\mathcal{U}\big) = \det\big([\epsilon u_1 \times \cdots \times u_{m-1}]_\mathcal{E}\ [u_1]_\mathcal{E}\ \cdots\ [u_{m-1}]_\mathcal{E}\big)$

$= \epsilon \langle u_1 \times \cdots \times u_{m-1}, u_1 \times \cdots \times u_{m-1} \rangle$

$= |\langle u_1 \times \cdots \times u_{m-1}, u_1 \times \cdots \times u_{m-1} \rangle| > 0,$

so $\mathcal{U}$ is consistent with $\mathcal{E}$.

(c): Since $(u_1, \ldots, u_{m-1})$ is orthonormal, we have from Theorem 8.4.6(b) that $||\epsilon u_1 \times \cdots \times u_{m-1}|| = 1$, so $\epsilon u_1 \times \cdots \times u_{m-1}$ is a unit vector. By Theorem 8.4.3(a), $\langle \epsilon u_1 \times \cdots \times u_{m-1}, u_j \rangle = 0$ for $j = 1, \ldots, m-1$. The result follows. $\square$

## Theorem 8.4.11. Let $(V, \mathfrak{g})$ be a scalar product space of dimension $m \ge 2$, let $\mathcal{E}$ be an orthonormal basis for $V$, let $U$ be a subspace of $V$ of dimension $m-1$ on which $\mathfrak{g}$ is nondegenerate, and let $\mathcal{U} = (u_1, \ldots, u_{m-1})$ and $\mathcal{V} = (v_1, \ldots, v_{m-1})$ be bases for $U$. Then:

- (a)

$u_1 \times \cdots \times u_{m-1} = \det\big([\mathrm{id}_U]^\mathcal{V}_\mathcal{U}\big)\, v_1 \times \cdots \times v_{m-1}.$

- (b)

$\frac{u_1 \times \cdots \times u_{m-1}}{||u_1 \times \cdots \times u_{m-1}||} = \pm \frac{v_1 \times \cdots \times v_{m-1}}{||v_1 \times \cdots \times v_{m-1}||},$

where the positive (negative) sign is chosen if $\mathcal{U}$ and $\mathcal{V}$ are consistent (not consistent).

*Remark.* By Theorem 8.4.9, the denominators in part (b) are nonzero.

*Proof.* (a): Let $P = [\mathrm{id}_U]^\mathcal{V}_\mathcal{U}$ and let $A : U \longrightarrow U$ be the linear isomorphism defined by $A(\mathcal{U}) = \mathcal{V}$. By Theorem 8.4.7,

$v_1 \times \cdots \times v_{m-1} = \det(A)\, u_1 \times \cdots \times u_{m-1},$

hence det(A) = det(P)^{-1}. The result follows.

(b): We have from Theorem 2.4.12 and Theorem 8.4.9 that det(P), ||u₁ × ⋯ × uₘ₋₁||, and ||v₁ × ⋯ × vₘ₋₁|| are each nonzero. Then part (a) gives

u₁ × ⋯ × uₘ₋₁ / ||u₁ × ⋯ × uₘ₋₁|| = det(P)/|det(P)| · v₁ × ⋯ × vₘ₋₁ / ||v₁ × ⋯ × vₘ₋₁|| = sgn(det(P)) · v₁ × ⋯ × vₘ₋₁ / ||v₁ × ⋯ × vₘ₋₁||.

## Theorem 8.4.12. Let (V, g) be a scalar product space of dimension m ≥ 2, let ℰ = (e₁, …, e_m) and ℱ be orthonormal bases for V, and let ⊠ denote the vector product operation corresponding to ℱ. Then

e₁ ⊠ ⋯ ⊠ ê_i ⊠ ⋯ ⊠ e_m = ε_i (−1)^{i−1} det([id_V]_ℰ^ℱ) e_i

for i = 1, …, m, where ^ indicates that an expression is omitted.

Proof. Let U_i be the subspace of V spanned by e₁, …, ê_i, …, e_m. Since g is nondegenerate on ℝe_i, we have from Theorem 4.1.3 that V = ℝe_i ⊕ (ℝe_i)⊥ and g is nondegenerate on (ℝe_i)⊥. By definition, each of e₁, …, ê_i, …, e_m is orthogonal to e_i, so U_i ⊆ (ℝe_i)⊥. From Theorem 1.1.18,

m = dim(V) = dim(ℝe_i) + dim((ℝe_i)⊥) = 1 + dim((ℝe_i)⊥),

hence dim((ℝe_i)⊥) = m − 1 = dim(U_i).

By Theorem 1.1.7(b), U_i = (ℝe_i)⊥, so g is nondegenerate on U_i. It follows from Theorem 4.1.2(c) and Theorem 8.4.10(a) that ℝe_i = U_i⊥ = ℝ(e₁ ⊠ ⋯ ⊠ ê_i ⊠ ⋯ ⊠ e_m),

so e₁ ⊠ ⋯ ⊠ ê_i ⊠ ⋯ ⊠ e_m = c_i e_i for some real number c_i. We have

c_i ε_i = ⟨e_i, c_i e_i⟩ = ⟨e_i, e₁ ⊠ ⋯ ⊠ ê_i ⊠ ⋯ ⊠ e_m⟩ = det([[e_i]_ℱ [e₁]_ℱ … [ê_i]_ℱ … [e_m]_ℱ]) [Th 8.4.5(a)] = (−1)^{i−1} det([[e₁]_ℱ … [e_i]_ℱ … [e_m]_ℱ]) [Th 8.4.3(d)] = (−1)^{i−1} det([id_V]_ℰ^ℱ), [(2.2.7)]

hence c_i = ε_i (−1)^{i−1} det([id_V]_ℰ^ℱ).

The result follows.
