
Proof. Since v is timelike, it follows from Theorem 4.8.2 that ℝv is timelike, and then from Theorem 4.8.4(b) that (ℝv)⊥ is spacelike. Thus, 𝔤 is nondegenerate on (ℝv)⊥. By Theorem 4.1.2(c) and Theorem 4.1.3, V = (ℝv)⊥ ⊕ ℝv.

In the notation of Theorem 4.8.5, since (ℝv)⊥ is spacelike and ℝv is timelike, it follows that 𝔤 is positive definite on (ℝv)⊥ and negative definite on ℝv. Thus, V = (ℝv)⊥ ⊕ ℝv is a direct sum of the type given by Theorem 4.1.6(a).

## Theorem 4.8.6. Let (V,𝔤) be a Lorentz vector space, and let v, w be lightlike vectors in V. Then v and w are orthogonal if and only if they are linearly dependent.

Proof. (⇒): Let (e₁, …, eₘ) be an orthonormal basis for V. Then eₘ is timelike, so we have from Theorem 4.8.5 that v and w can be expressed as

v = v̄ + a eₘ      and      w = w̄ + b eₘ,    (4.8.2)

where v̄, w̄ are (spacelike) vectors in (ℝ eₘ)⊥ and a, b are real numbers. Then

0 = ⟨v, v⟩ = ⟨v̄, v̄⟩ − a²
0 = ⟨w, w⟩ = ⟨w̄, w̄⟩ − b²
0 = ⟨v, w⟩ = ⟨v̄, w̄⟩ − ab,

hence

a² = ⟨v̄, v̄⟩      b² = ⟨w̄, w̄⟩      ab = ⟨v̄, w̄⟩,    (4.8.3)

so

⟨v̄, w̄⟩² = ⟨v̄, v̄⟩⟨w̄, w̄⟩.    (4.8.4)

Since eₘ is timelike, we have from Theorem 4.8.2 that ℝ eₘ is timelike, and then from Theorem 4.8.4(b) that (ℝ eₘ)⊥ is spacelike. Thus, 𝔤 is an inner product on (ℝ eₘ)⊥. It follows from Theorem 4.6.4 and (4.8.4) that

v̄ = c w̄    (4.8.5)

for some real number c. Then (4.8.5) and the second and third identities in (4.8.3) give ab = b² c. We have b ≠ 0; for if not, then w = w̄, where w is lightlike and w̄ is spacelike, which is a contradiction. Thus,

a = b c.    (4.8.6)

Combining (4.8.2), (4.8.5), and (4.8.6) yields v = c w.
(⇐): If v, w are linearly dependent, then w = c v for some real number c, hence ⟨v, w⟩ = c⟨v, v⟩ = 0.

## Theorem 4.8.7. If (V,𝔤) is a Lorentz vector space and U is a subspace of V of dimension m ≥ 2, then the following are equivalent:
- (a) U is timelike.
- (b) U contains two linearly independent lightlike vectors.
- (c) $U$ contains a timelike vector.

**Proof.** (a) ⇒ (b): By assumption, $U$ is timelike, so $\mathbf{g}$ is a (Lorentz) scalar product on $U$. By Theorem 4.2.4, there is an orthonormal basis $(e_1,\ldots,e_k)$ for $U$. Then $e_1$ is a spacelike unit vector, $e_k$ is a timelike unit vector, and $e_1$ and $e_k$ are orthogonal. It follows that

$\langle e_1+e_k,e_1+e_k\rangle=0\qquad\text{and}\qquad\langle e_1-e_k,e_1-e_k\rangle=0,$

so $e_1+e_k$ and $e_1-e_k$ are lightlike. Suppose

$0=a(e_1+e_k)+b(e_1-e_k)=(a+b)e_1+(a-b)e_k$

for some real numbers $a,b$. Since $e_1,e_k$ are linearly independent, we have $a=b=0$. Thus, $e_1+e_k,e_1-e_k$ are linearly independent.

(b) ⇒ (c): By assumption, $U$ contains two linearly independent lightlike vectors $v$ and $w$. Then

$\langle v+w,v+w\rangle=2\langle v,w\rangle\qquad\text{and}\qquad\langle v-w,v-w\rangle=-2\langle v,w\rangle.$

It follows from Theorem 4.8.6 that $\langle v,w\rangle\ne0$, so either $v+w$ or $v-w$ is timelike.

(c) ⇒ (a): By assumption, $U$ contains a timelike vector $v$. It follows from Theorem 4.8.2 that $\mathbb{R}v$ is timelike, and then from Theorem 4.8.4(b) that $(\mathbb{R}v)^\perp$ is spacelike. Since $\mathbb{R}v\subset U$, we have $U^\perp\subset(\mathbb{R}v)^\perp$, and then Theorem 4.8.3 that $U^\perp$ is spacelike. By Theorem 4.1.2(c) and Theorem 4.8.4(a), $U=U^{\perp\perp}$ is timelike.

## Theorem 4.8.8.

If $(V,\mathbf{g})$ is a Lorentz vector space and $U$ is a subspace of $V$, then the following are equivalent:
- (a) $U$ is lightlike.
- (b) $U$ contains a lightlike vector but not a timelike vector.
- (c) $U\cap\Lambda=\mathbb{R}v\setminus\{0\}$ for some lightlike vector $v$ in $U$.

**Proof.** If $V$ is 1-dimensional, the result is trivial, so assume $\dim(V)\ge2$.

(a) ⇒ (b): By assumption, $U$ is lightlike. Then $\sigma$ is degenerate on $U$, so there is a nonzero vector $v$ in $U$ such that $\langle v,w\rangle=0$ for all vectors $w$ in $U$. In particular, $\langle v,v\rangle=0$, hence $v$ is lightlike. Furthermore, $U$ does not contain a timelike vector; for if it did, then, by Theorem 4.8.7, $U$ would be timelike, which contradicts the assumption on $U$.

(b) ⇒ (c): By assumption, $U$ contains a lightlike vector $v$. Then $U\cap\Lambda$ does not contain a lightlike vector that is linearly independent of $v$; for if it did, then, by Theorem 4.8.7, $U$ would contain a timelike vector, which contradicts the assumption on $U$. Thus, $U\cap\Lambda=\mathbb{R}v\setminus\{0\}$.

(c) ⇒ (a): By assumption, $U\cap\Lambda=\mathbb{R}v\setminus\{0\}$ for some lightlike vector $v$ in $U$. We have from Theorem 4.8.3 that $U$ is not spacelike. Since $U\cap\Lambda$ is contained in the 1-dimensional subspace $\mathbb{R}v$ of $V$, it does not contain two linearly independent (lightlike) vectors. By Theorem 4.8.7, $U$ is not timelike. Therefore, $U$ is lightlike.

## Theorem 4.8.9. If (V,𝔤) is a Lorentz vector space and U is a subspace of V of dimension m ≥ 2, then U contains a nonzero spacelike vector.

Proof. There are three cases to consider.

Case 1. U is spacelike.
By Theorem 4.8.3, any nonzero vector in U is spacelike.

Case 2. U is timelike.
Let dim(V) = m. According to Theorem 4.8.7, U contains a timelike vector v. Since a timelike vector is nonzero, dim(ℝv) = 1, so Theorem 4.1.2(b) gives dim((ℝv)⊥) = m − 1. By assumption, dim(U) ≥ 2. We have from Theorem 1.1.15 that

```
m ≥ dim((ℝv)⊥ + U)
= dim((ℝv)⊥) + dim(U) − dim((ℝv)⊥ ∩ U)
≥ m + 1 − dim((ℝv)⊥ ∩ U),
```

hence dim((ℝv)⊥ ∩ U) ≥ 1. Thus, (ℝv)⊥ ∩ U is not the zero subspace. It follows from Theorem 4.8.2 that ℝv is timelike, and then from Theorem 4.8.4(b) that (ℝv)⊥ is spacelike. By Theorem 4.8.3, any nonzero vector in (ℝv)⊥ ∩ U is spacelike.

Case 3. U is lightlike.
By Theorem 4.8.8, U contains a lightlike vector v. Let w be a vector in U\ℝv. There are three cases to consider: w is either spacelike, timelike, or lightlike. If w is spacelike, then we are done. Suppose w is not spacelike. By Theorem 4.8.8, w cannot be timelike, so it is lightlike. It follows from Theorem 1.1.2 that v, w are linearly independent, and then from Theorem 4.8.6 that they are not orthogonal. Thus,

```
⟨v + w, v + w⟩ = 2⟨v, w⟩ ≠ 0,
```

so the vector v + w in U is nonzero and not lightlike. By Theorem 4.8.8, v + w cannot be timelike, hence it is spacelike.

The following table summarizes some of the above findings on Lorentz vector spaces. As can be seen, the observations for 2-dimensional subspaces of ℝ₁³ made in Example 4.8.1 hold for Lorentz vector spaces in general.

| Type of subspace (dimension ≥ 2) | Types of vectors in subspace (excluding zero vector) | | |
| | Spacelike | Timelike | Lightlike |
| Spacelike | yes | no | no |
| Timelike | yes | yes | yes |
| Lightlike | yes | no | yes |

Looking again at Figure 4.8.2, we confirm using Theorem 4.8.3, Theorem 4.8.7, and Theorem 4.8.8, respectively, that $\Pi_1$ is spacelike, $\Pi_2$ is timelike, and $\Pi_3$ is lightlike.

We conclude this section with yet another version of the Cauchy–Schwarz inequality. It is labeled “reversed” because of the direction of the inequality. This is a reminder of how differently behaved a Lorentz vector space is compared to an inner product space.

## Theorem 4.8.10 (Reversed Cauchy–Schwarz Inequality).

*If $(V,g)$ is a Lorentz vector space and $v,w$ are timelike vectors, then*

\[\langle v,w\rangle^2 \ge \langle v,v\rangle\langle w,w\rangle,\]

with equality if and only if $v,w$ are linearly dependent.

*Proof.* Since $v$ is timelike, it follows from Theorem 4.8.2 that $\mathbb{R}v$ is timelike, and then from Theorem 4.8.4(b) that $(\mathbb{R}v)^\perp$ is spacelike. Thus, $g$ is an inner product on $(\mathbb{R}v)^\perp$. By Theorem 4.8.5, $w=\overline{w}+cv$, where $\overline{w}$ is a (spacelike) vector in $(\mathbb{R}v)^\perp$ and $c$ is a real number. Then

\[\langle v,w\rangle=c\langle v,v\rangle \qquad\text{and}\qquad \langle w,w\rangle=\langle\overline{w},\overline{w}\rangle+c^2\langle v,v\rangle,\]

hence
\[
\langle v,w\rangle^2=c^2\langle v,v\rangle^2
=\langle v,v\rangle\bigl(\langle w,w\rangle-\langle\overline{w},\overline{w}\rangle\bigr)
=\langle v,v\rangle\langle w,w\rangle-\langle v,v\rangle\langle\overline{w},\overline{w}\rangle.
\]

Since $v$ and $w$ are timelike, $\langle v,v\rangle\langle w,w\rangle>0$, and because $v$ is timelike and $\overline{w}$ is spacelike, $\langle v,v\rangle\langle\overline{w},\overline{w}\rangle\le 0$. It follows that $\langle v,w\rangle^2\ge\langle v,v\rangle\langle w,w\rangle$, with equality if and only if $\langle\overline{w},\overline{w}\rangle=0$. Since $g$ is an inner product on $(\mathbb{R}v)^\perp$, $\langle\overline{w},\overline{w}\rangle=0$ if and only if $\overline{w}=0$, which is equivalent to $w=cv$, which in turn is equivalent to $v$ and $w$ being linearly dependent.

## 4.9 Time Cones

Let $(V,g)$ be a Lorentz vector space. The **time cone** of $V$ is denoted by $\mathcal{T}$ and defined to be the set of timelike vectors in $V$:

\[\mathcal{T}=\{v\in V:\langle v,v\rangle<0\}.\]

In $\mathbb{R}^m_1$, the time cone is denoted by $\mathcal{T}_m$. For example,

\[\mathcal{T}_3=\{(x,y,z)\in\mathbb{R}^3_1:x^2+y^2-z^2<0\}.\]

Recall from Example 4.8.1 that the light cone of $\mathbb{R}^3_1$ is

\[\Lambda_3=\{(x,y,z)\in\mathbb{R}^3_1:x^2+y^2-z^2=0\}\setminus\{(0,0,0)\}.\]

Thus, $\mathcal{T}_3$ is “inside” $\Lambda_3$.
