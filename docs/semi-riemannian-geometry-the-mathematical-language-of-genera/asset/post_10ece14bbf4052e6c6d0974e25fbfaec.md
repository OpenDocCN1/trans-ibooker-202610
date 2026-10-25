
Proof. Let $(e_1,\dots,e_m)$ be an orthonormal basis for $V$ that is positively oriented with respect to $\mathfrak{O}$, and let $(\xi^1,\dots,\xi^m)$ be its dual basis. Since both $(e_1,\dots,e_m)$ and $\mathcal{H}$ are positively oriented, they are consistent. The result now follows from Theorem 8.3.1(b) and Theorem 8.3.4.

Let $(V,\mathfrak{g},\mathfrak{O})$ be an oriented scalar product space of dimension $m \ge 2$, and let $\Omega_V$ be its volume multicovector. By Theorem 8.3.4, $\Omega_V$ induces $\mathfrak{O}$. Let $U$ be a subspace of $V$ of dimension $m-1$ on which $\mathfrak{g}$ is nondegenerate. We have from Theorem 4.1.2(b) that $U^\perp$ is a 1-dimensional subspace of $V$, and from Theorem 4.1.3 that $V = U \oplus U^\perp$. Let $u$ be a unit vector in $U^\perp$, so that $\mathbb{R}u = U^\perp$, hence $V = U \oplus \mathbb{R}u$. Since $\mathfrak{g}$ is nondegenerate on $U$, $\mathfrak{g}|_U$ is a scalar product on $U$. By parts (a) and (b) of Theorem 8.2.5, $i_u(\Omega_V)|_U$ is an orientation multicovector on $U$ that induces a certain orientation of $U$ denoted by $\mathfrak{O}_U$. Thus, $(U,\mathfrak{g}|_U,\mathfrak{O}_U)$ is an oriented scalar product space of dimension $m-1$. Let $\Omega_U$ be its volume multicovector. The question arises as to whether the orientation multicovectors $i_u(\Omega_V)|_U$ and $\Omega_U$ are related. As the next result shows, they are one and the same.

## Theorem 8.3.6. With the above setup:

(a)

$i_u(\Omega_V)|_U = \Omega_U.$

(b) More generally, if $v$ is a vector in $V$ and $\epsilon$ is the sign of $\langle u,u\rangle$, then

$i_v(\Omega_V)|_U = \epsilon\langle u,v\rangle\Omega_U.$

Proof. (a): Let $(e_1,\dots,e_{m-1})$ be an orthonormal basis for $U$ that is positively oriented with respect to $\mathfrak{O}_U$. We have from Theorem 8.2.5(a) that $(u,e_1,\dots,e_{m-1})$ is an orthonormal basis for $V$ that is positively oriented with respect to $\mathfrak{O}$, and then from Theorem 8.3.4 (applied to $V$) that $\Omega_V(u,e_1,\dots,e_{m-1}) = 1$. By definition,

$i_u(\Omega_V)|_U(e_1,\dots,e_{m-1}) = \Omega_V(u,e_1,\dots,e_{m-1})$,

so $i_u(\Omega_V)|_U(e_1,\dots,e_{m-1}) = 1$. The result now follows from the uniqueness of $\Omega_U$, as given by Theorem 8.3.4 (applied to $U$).

(b): Since $V = U \oplus \mathbb{R}u$, we have $v = (v - \bar{v}) + \bar{v}$, where $v - \bar{v}$ and $\bar{v}$ are (unique) vectors in $U$ and $\mathbb{R}u$, respectively. By Theorem 7.4.1(a),

$i_v(\Omega_V)|_U = i_{v - \bar{v}}(\Omega_V)|_U + i_{\bar{v}}(\Omega_V)|_U. \quad (8.3.1)$

Since $w_1,\ldots,w_{m-1}$ were arbitrary,

$$i_{v-\bar v}(\Omega_V)|_U=0.$$

For the second term, we have from Theorem 4.1.5 that

$$\bar v=\mathcal P_{\mathop{\rm Rm}(U)}(v)=\epsilon\langle v,u\rangle u,$$

and then from Theorem 7.4.1(b) and part (a) that

$$i_{\bar v}(\Omega_V)|_U=\epsilon\langle v,u\rangle i_u(\Omega_V)|_U=\epsilon\langle v,u\rangle\Omega_U.$$

Substituting (8.3.2) and (8.3.3) into (8.3.1) gives the result.

## 8.4 Vector Products

In this section, we generalize the well-known vector product in $\mathbb R^3$ to an arbitrary scalar product space.

Let $(V,g)$ be a scalar product space with signature $(\epsilon_1,\ldots,\epsilon_m)$, let $\mathcal E=(e_1,\ldots,e_m)$ be an orthonormal basis for $V$, and let $v_1,\ldots,v_{m-1}$ be vectors in $V$. The **vector product** (or cross product) of $v_1,\ldots,v_{m-1}$ (in that order) with respect to $\mathcal E$ is defined by

$$v_1\times\cdots\times v_{m-1}=\sum_{i=1}^{m}\epsilon_i(-1)^{i-1}\det\left(\left[\, [v_1]_{\mathcal E}\ \cdots\ [v_{m-1}]_{\mathcal E}\,\right]^{(i)c}\right)e_i.$$

where $(i)^c$ is the multi-index

$$(i)^c=(1,\ldots,\hat{i},\ldots,m).$$

Clearly, the vector product of given vectors depends on the order in which they are taken and the choice of orthonormal basis.

> In this chapter, the vector product $\times$ on $V$ is computed with respect to a given orthonormal basis $\mathcal E$ for $V$.

> Throughout, the vector product $\times$ on $\mathbb R^m_\nu$ is computed with respect to the standard basis for $\mathbb R^m_\nu$.

We obtain a computationally convenient alternative to (8.4.1) as follows. Let

$$v_j=\sum_i a_j^i e_i$$

for $j=1,\ldots,m-1$, so that from (2.2.3),

$$\left[\,[v_1]_{\mathcal E}\ \cdots\ [v_{m-1}]_{\mathcal E}\,\right]=\begin{bmatrix}a_1^1&\cdots&a_{m-1}^1\\ \vdots&\ddots&\vdots\\ a_1^m&\cdots&a_{m-1}^m\end{bmatrix}.$$

Then (8.4.1) can be expressed as the formal identity

v₁ × ⋯ × vₘ₋₁ = det( ⎛⎜⎝ ε₁e₁ a¹₁ ⋯ a¹ₘ₋₁ ⋮ ⋮ ⋱ ⋮ εₘeₘ aᵐ₁ ⋯ aᵐₘ₋₁ ⎞⎟⎠ ), (8.4.3)

provided we expand the “determinant” along the first column.

When m = 2, even though the notation for the left-hand sides of (8.4.1) and (8.4.3) simplifies to a single vector, the right-hand sides can still be computed. Let v = a¹e₁ + a²e₂. Then the “vector product” of v is

det( ⎛⎜⎝ ε₁e₁ a¹ ε₂e₂ a² ⎞⎟⎠ ) = ε₁a²e₁ − ε₂a¹e₂.

When the context is meaningful, this quantity has the same properties as a vector product when m ≥ 3.

### Example 8.4.1 (Vector Product on ℝ₀² and ℝ₀³).

Let (e₁, e₂) be the standard basis for ℝ₀², and let (a¹, a²) be a vector in ℝ₀². The “vector product” of (a¹, a²) is (a², −a¹).

Now let (e₁, e₂, e₃) be the standard basis for ℝ₀³, and let v = (a¹, a², a³) and w = (b¹, b², b³) be vectors in ℝ₀³. Then

[v]ℰ = ⎡a¹⎤   and   [w]ℰ = ⎡b¹⎤
          ⎢a²⎥             ⎢b²⎥
          ⎣a³⎦             ⎣b³⎦

hence

v × w = det( ⎛⎜⎝ e₁ a¹ b¹ e₂ a² b² e₃ a³ b³ ⎞⎟⎠ ) = (det(⎡a² b²⎤), −det(⎡a¹ b¹⎤), det(⎡a¹ b¹⎤)) = (a²b³ − a³b², a³b¹ − a¹b³, a¹b² − a²b¹).

In particular,

e₁ × e₂ = e₃,      e₁ × e₃ = −e₂,      and      e₂ × e₃ = e₁.

### Example 8.4.2 (Vector Product on ℝ₁² and ℝ₁³).

Let (e₁, e₂) be the standard basis for ℝ₁², and let (a¹, a²) be a vector in ℝ₁². The “vector product” of (a¹, a²) is (a², a¹).

Now let (e₁, e₂, e₃) be the standard basis for ℝ₁³, and let v = (a¹, a², a³) and w = (b¹, b², b³) be vectors in ℝ₁³. Then

[v]ℰ = ⎡a¹⎤   and   [w]ℰ = ⎡b¹⎤
          ⎢a²⎥             ⎢b²⎥
          ⎣a³⎦             ⎣b³⎦

hence

v × w = det( ⎛⎜⎝ e1 a1 b1 e2 a2 b2 −e3 a3 b3 ⎞⎟⎠ ) = ( det( [a2 b2; a3 b3] ), −det( [a1 b1; a3 b3] ), −det( [a1 b1; a2 b2] ) ) = (a2 b3 − a3 b2, a3 b1 − a1 b3, a2 b1 − a1 b2).

In particular,

e1 × e2 = −e3,      e1 × e3 = −e2,      and      e2 × e3 = e1.

The vector product has a number of interesting algebraic properties, several of which, not surprisingly, are expressed in terms of determinants and wedge products.

## Theorem 8.4.3. Let (V,g) be a scalar product space of dimension m ≥ 2, let ℰ be an orthonormal basis for V, let v1,...,vm−1,w be vectors in V, and let c be a real number. Then, for i,j = 1,...,m−1:

- (a) ⟨vi, v1 × ⋯ × vm−1⟩ = 0.
- (b) v1 × ⋯ × c vi × ⋯ × vm−1 = c(v1 × ⋯ × vi × ⋯ × vm−1).
- (c) v1 × ⋯ × (c vi + w) × ⋯ × vm−1 = c(v1 × ⋯ × vi × ⋯ × vm−1) + v1 × ⋯ × w × ⋯ × vm−1.
- (d) v1 × ⋯ × vi × ⋯ × vj × ⋯ × vm−1 = −(v1 × ⋯ × vj × ⋯ × vi × ⋯ × vm−1).
- (e) If vi = vj, then v1 × ⋯ × vi × ⋯ × vj × ⋯ × vm−1 = 0.

Proof. (a): From (8.4.1) and (8.4.2),

⟨vi, v1 × ⋯ × vm−1⟩ = ⟨∑j ai j ej, ∑k εk (−1)k−1 det( [ [v1]ℰ … [vi]ℰ … [vm−1]ℰ ]^(k)c ) ek ⟩ = ∑jk ai j ⟨ej, ek⟩ εk (−1)k−1 det( [ [v1]ℰ … [vi]ℰ … [vm−1]ℰ ]^(k)c ) = ∑j (−1)j−1 ai j det( [ [v1]ℰ … [vi]ℰ … [vm−1]ℰ ]^(j)c ) = det( [ [vi]ℰ [v1]ℰ … [vi]ℰ … ] ).

Since (at least) two columns of [[v_i]_E  [v_1]_E … [v_i]_E … [v_{m-1}]_E] are equal, it follows from Theorem 2.4.2 that its determinant equals 0.

(b)–(d): Straightforward.

In light of Theorem 8.4.3(b), we drop parentheses and denote c(v_1 × ⋯ × v_i × ⋯ × v_{m-1}) by c v_1 × ⋯ × v_i × ⋯ × v_{m-1}.

The next result is the main reason for considering vector products.

## Theorem 8.4.4. Let (V,g) be a scalar product space of dimension m ≥ 2, let E be an orthonormal basis for V, let v_1,…,v_{m-1} be vectors in V, and let U be the subspace of V spanned by v_1,…,v_{m-1}. Then v_1 × ⋯ × v_{m-1} is a vector in U^⊥.

Proof. This follows from Theorem 8.4.3(a).

At first glance, Theorem 8.4.4 appears to promise more than it actually delivers. To make the result truly informative, we need v_1 × ⋯ × v_{m-1} to be a nonzero vector and U^⊥ to be a 1-dimensional subspace of V. These conditions can be met with further assumptions: see Theorem 8.4.8 and Theorem 8.4.10(a).

It was remarked above that the vector product depends on the order in which vectors are taken and the choice of orthonormal basis. According to Theorem 8.4.3(d), changing the order of vectors at most affects the sign of the resulting vector product. In view of (8.4.1), it might be expected that computing with respect to a different orthonormal basis would have a significant impact on results. As Theorem 8.4.4 shows, this is not necessarily the case: regardless of the choice of orthonormal basis, the vector product is always in the perp of the subspace spanned by the constituent vectors. In fact, several of the results to follow exhibit this same feature: see Theorem 8.4.5, Theorem 8.4.6, Theorem 8.4.8, and Theorem 8.4.9.

## Theorem 8.4.5. Let (V,g) be a scalar product space of dimension m ≥ 2 and index ν. Let E be an orthonormal basis for V, let (ξ^1,…,ξ^m) be its dual basis, and let u, v_1,…,v_{m-1} be vectors in V. Then:
