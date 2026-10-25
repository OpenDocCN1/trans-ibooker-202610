
Since ζ was arbitrary, η^♯ = Σ_i a^i h_i. To prove the succinct version directly:

[η^♯]_ℋ = [S(η)]_ℋ = [S]_Θ^ℋ [η]_Θ   [Th 2.2.4] = 𝔤_ℋ^{-1} [η]_Θ .   [Th 4.5.3]

We see from (4.5.3) and (4.5.4) why taking the sharp of η to obtain η^♯ is classically referred to as raising an index by 𝔤. The flat map 𝔽 and sharp map S therefore lower and raise indices, respectively. In the mathematical literature, these maps are colorfully referred to as the musical isomorphisms.

A scalar product space and its dual scalar product space are isomorphic under the flat map and sharp map. The remaining results of this section show that there is also a close connection between other familiar structures.

## Theorem 4.5.5. Let (V,𝔤) be a scalar product space with signature (ε₁,…,ε_m), let (e₁,…,e_m) be an orthonormal basis for V, and let (ξ¹,…,ξ^m) be its dual basis. Then, for i = 1,…,m:

- (a) e_i^𝔽 = ε_i ξ^i.
- (b) ξ^i♯ = ε_i e_i.

Proof. (a): This follows from Theorem 3.3.1(a).
(b): This follows from Theorem 4.5.3(a) or from part (a).

## Theorem 4.5.6. Let (V,𝔤) be a scalar product space, let ℰ be an orthonormal basis for V, and let Ξ be its dual basis. Then:

- (a) Ξ is an orthonormal basis for (V*, 𝔤*).
- (b) (V,𝔤) and (V*,𝔤*) have the same signature.

Proof. Let V have signature (ε₁,…,ε_m), and let ℰ = (e₁,…,e_m) and Ξ = (ξ¹,…,ξ^m). By (4.5.1) and Theorem 4.5.5(b), ⟨ξ^i, ξ^j⟩* = ⟨ξ^i♯, ξ^j♯⟩ = ε_i ε_j ⟨e_i, e_j⟩ = { ε_i if i = j, 0 if i ≠ j }, hence ⟨ξ^i, ξ^j⟩* = ⟨e_i, e_j⟩ for i, j = 1,…,m. The result follows.

## 4.6 Inner Product Spaces

Having discussed in some detail the features of scalar product spaces, we now specialize to the type of scalar product space that is usually the focus in introductory linear algebra.

Let V be a vector space, and let 𝔤 be a bilinear function on V. We say that 𝔤 is an inner product on V, and that the pair (V,𝔤) is an inner product space, if 𝔤 is symmetric and positive definite on V. Recall from Section 3.1 that 𝔤 is symmetric on V if ⟨v,w⟩ = ⟨w,v⟩ for all vectors v,w in V, and that 𝔤 is positive definite on V if ⟨v,v⟩ > 0 for all nonzero vectors v in V.

Suppose $(V,\mathfrak{g})$ is in fact an inner product space. If $v$ is a vector in $V$ such that $\langle v,w\rangle = 0$ for all vectors $w$ in $V$, then $\langle v,v\rangle = 0$, hence $v = 0$. Thus, $\mathfrak{g}$ is nondegenerate on $V$, demonstrating that $(V,\mathfrak{g})$ is automatically a scalar product space. In the present context, the norm corresponding to $\mathfrak{g}$, given by (4.1.1), simplifies to the function

```
||·|| : V → ℝ
```

defined by

```
||v|| = √⟨v,v⟩ = √q(v)    (4.6.1)
```

for all vectors $v$ in $V$. Since an inner product space is a type of scalar product space, the definitions of orthogonal, orthogonal $k$-tuple, orthogonal basis, orthonormal, orthonormal $k$-tuple, and orthonormal basis given in Section 4.2 also apply here. Let $(e_1,...,e_m)$ be an orthonormal basis for $V$. Since $\mathfrak{g}$ is an inner product, $\langle e_i,e_i\rangle = 1$ for $i = 1,...,m$. Thus, ind$(\mathfrak{g}) = 0$ and the signature of $\mathfrak{g}$ is an $m$-tuple of $+1$s.

The fact that an inner product is positive definite distinguishes it from a scalar product space in ways that are fundamental. For example, in an inner product space, only the zero vector is orthogonal to itself; or equivalently, only the zero vector has zero norm. Perhaps most important of all, in an inner product space, the inner product is positive definite (hence nondegenerate) on every subspace. Consequently, pairing any subspace of an inner product with the corresponding restriction of the inner product yields an inner product space.

## Example 4.6.1 (ℝ₀ᵐ). Continuing with Example 4.1.1, let ν = 0. In this case, we denote 𝔰 by 𝔢. Then

```
𝔢 : ℝᵐ × ℝᵐ → ℝ
```

is given by

```
𝔢((x¹,...,xᵐ),(y¹,...,yᵐ)) = x¹y¹ + ⋯ + xᵐyᵐ.
```

We refer to

```
ℝ₀ᵐ = (ℝᵐ,𝔢)
```

as Euclidean m-space and to 𝔢 as the Euclidean inner product. It is easily shown that 𝔢 is in fact an inner product on ℝᵐ. Following Example 4.1.1, the standard basis for ℝ₀ᵐ is simply the standard basis for ℝᵐ. The norm corresponding to 𝔢 is given by

```
||(x¹,...,xᵐ)|| = √((x¹)² + ⋯ + (xᵐ)²)
```

and called the Euclidean norm. The reader is likely familiar with ||·|| as a measure of “length” in ℝᵐ, a notion to be explored further in Section 9.3. ◇

## Theorem 4.6.2. Let (V,𝔤) be a scalar product space of index ν. Then 𝔤 is an inner product if and only if ν = 0.

Proof. (⇒): This follows from above remarks.
(⇐): Let $(e_1,\dots,e_m)$ be an orthonormal basis for $V$, and let $v$ be a vector in $V$, with $v = \sum_i a^i e_i$. Since $\nu = 0$,

$\langle v, v \rangle = \left\langle \sum_i a^i e_i, \sum_j a^j e_j \right\rangle = \sum_{ij} \langle e_i, e_j \rangle a^i a^j = \sum_i (a^i)^2,$

hence $\langle v, v \rangle \ge 0$, with equality if and only if each $a^i = 0$, that is, if and only if $v = 0$.

## Theorem 4.6.3. If $(V,\mathfrak{g})$ is an inner product space and $\mathcal{H}$ is a basis for $V$, then $\det(\mathfrak{g}_{\mathcal{H}}) > 0$.

Proof. We have from Theorem 4.2.5 and Theorem 4.6.2 that $\det(\mathfrak{g}_{\mathcal{H}}) = \det(A)^2 \ge 0$. By Theorem 3.3.3, $\det(\mathfrak{g}_{\mathcal{H}}) \ne 0$.

We encountered a version of the Cauchy–Schwarz inequality in Theorem 3.2.6, the proof of which was somewhat involved. For an inner product space, we get a stronger result with much less effort.

## Theorem 4.6.4 (Cauchy–Schwarz Inequality). If $(V,\mathfrak{g})$ is an inner product space and $v, w$ are vectors in $V$, then

$\langle v, w \rangle^2 \le \langle v, v \rangle \langle w, w \rangle,$
(4.6.2)

with equality if and only if $v, w$ are linearly dependent.

Proof. The result is trivial if either $v = 0$ or $w = 0$, so assume $v, w \ne 0$. Since

$\langle w, w \rangle \bigl(\langle v, v \rangle \langle w, w \rangle - \langle v, w \rangle^2\bigr)$
$= \bigl\langle \langle w, w \rangle v - \langle v, w \rangle w, \langle w, w \rangle v - \langle v, w \rangle w \bigr\rangle \ge 0,$
(4.6.3)

we have
$\langle v, v \rangle \langle w, w \rangle - \langle v, w \rangle^2 \ge 0,$
from which (4.6.2) follows. If equality holds in (4.6.2), then equality holds in (4.6.3), that is,

$\bigl\langle \langle w, w \rangle v - \langle v, w \rangle w, \langle w, w \rangle v - \langle v, w \rangle w \bigr\rangle = 0,$

hence $\langle w, w \rangle v - \langle v, w \rangle w = 0$, so $v, w$ are linearly dependent. Conversely, if $w = c v$ for some nonzero real number $c$, then
$\langle v, v \rangle \langle w, w \rangle = \langle v, w/c \rangle \langle c v, w \rangle = \langle v, w \rangle^2.$

## Theorem 4.6.5 (Triangle Inequality). If $(V,\mathfrak{g})$ is an inner product space and $v, w$ are vectors in $V$, then

$\|v + w\| \le \|v\| + \|w\|,$

with equality if and only if $v, w$ are linearly dependent.

**Proof.** By Theorem 4.6.4,

\[
\langle v,w\rangle \le |\langle v,w\rangle|=\sqrt{\langle v,w\rangle^2}\le \sqrt{\langle v,v\rangle}\sqrt{\langle w,w\rangle}=\|v\|\,\|w\|,
\]

hence

\[
\begin{aligned}
\|v+w\|^2&=\langle v+w,v+w\rangle\\
&=\langle v,v\rangle+2\langle v,w\rangle+\langle w,w\rangle\\
&\le \|v\|^2+2\|v\|\|w\|+\|w\|^2=(\|v\|+\|w\|)^2.
\end{aligned}
\]

so $\|v+w\|\le \|v\|+\|w\|$. There is equality if and only if $\langle v,w\rangle=\|v\|\|w\|$, which is equivalent to $\langle v,w\rangle\ge 0$ and $\langle v,w\rangle^2=\langle v,v\rangle\langle w,w\rangle$, which in turn, by Theorem 4.6.4, is equivalent to $v,w$ being linearly dependent.

In geometric terms, the triangle inequality has the following intuitive interpretation: in an inner product space, it is shorter to walk diagonally across a rectangular field than to “go around the corner”. As we will see in Section 4.8, this commonplace observation depends crucially on the properties of the inner product.

## Example 4.6.6 $(\mathbb{R}_0^m)$. For vectors $(x^1,\ldots,x^m)$ and $(y^1,\ldots,y^m)$ in $\mathbb{R}_0^m$, the Cauchy–Schwarz inequality and triangle inequality become

\[
\left(\sum_i x^i y^i\right)^2\le \left(\sum_i (x^i)^2\right)\left(\sum_i (y^i)^2\right)
\]

and

\[
\sqrt{\sum_i (x^i+y^i)^2}\le \sqrt{\sum_i (x^i)^2}+\sqrt{\sum_i (y^i)^2},
\]

respectively.

## Theorem 4.6.7 (Pythagora’s Theorem).

If $(V,g)$ is an inner product space and $U$ is a subspace of $V$, then

\[
\|v\|^2=\|P_U(v)\|^2+\|P_{U^\perp}(v)\|^2,
\]

where $P_U$ and $P_{U^\perp}$ are the orthogonal projection maps defined in Section 4.1.

**Proof.** By Theorem 4.1.4,

\[
\begin{aligned}
\|v\|^2&=\langle P_U(v)+P_{U^\perp}(v),P_U(v)+P_{U^\perp}(v)\rangle\\
&=\langle P_U(v),P_U(v)\rangle+\langle P_{U^\perp}(v),P_{U^\perp}(v)\rangle\\
&=\|P_U(v)\|^2+\|P_{U^\perp}(v)\|^2.
\end{aligned}
\]

The next result is the inner product space counterpart of **Theorem 4.2.3.**

## Theorem 4.6.8.

If $(V,g)$ is an inner product space and $(e_1,\ldots,e_k)$ is an orthogonal (but not necessarily orthonormal) $k$-tuple of vectors in $V$, then $e_1,\ldots,e_k$ are linearly independent.
