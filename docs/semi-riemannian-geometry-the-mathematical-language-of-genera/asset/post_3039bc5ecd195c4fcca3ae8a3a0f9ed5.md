
+   (b) *If dim(U) = dim(V), then U = V.*
+   (c) *If dim(U) < dim(V), then any unordered basis for U can be extended to an unordered basis for V. That is, given an unordered basis {h₁, …, hₖ} for U, there are vectors hₖ₊₁, …, hₘ in V such that {h₁, …, hₖ, hₖ₊₁, …, hₘ} is an unordered basis for V.*

*Proof.* (a): We proceed in steps. For the first step, let u₁ be a vector in U. If span({u₁}) = U, we are done. If not, for the second step, let u₂ be a vector in U\span({u₁}). It follows from Theorem 1.1.2 that u₁, u₂ are linearly independent. If span({u₁, u₂}) = U, we are done, and so on. By Theorem 1.1.6(b), this process ends after k ≤ m steps. Then u₁, …, uₖ are linearly independent and span U, which is to say that {u₁, …, uₖ} is an unordered basis for U.

(b): Let ℋ and ℱ be bases for U and V, respectively, and suppose U ≠ V. Since U = span(ℋ), there is a vector v in V\span(ℋ). By Theorem 1.1.2, ℋ ∪ {v} is linearly independent. We have from Theorem 1.1.5 that

card(ℋ ∪ {v}) > card(ℋ) = dim(U) = dim(V) = card(ℱ),

which contradicts Theorem 1.1.6(b).

(c): Given the unordered basis {h₁, …, hₖ} for U, the algorithm described in part (a) can be used to find vectors hₖ₊₁, …, hₘ in V such that {h₁, …, hₖ, hₖ₊₁, …, hₘ} is an unordered basis for V. □

## Throughout the remainder of Part I, unless stated otherwise, all vector spaces are finite-dimensional.

Let V be a vector space, and let {h₁, …, hₘ} be an unordered basis for V. The m-tuple (h₁, …, hₘ) is said to be an ordered basis for V, as is any m-tuple derived from (h₁, …, hₘ) by permuting h₁, …, hₘ. For example, (h₁, h₂, …, hₘ) and (h₂, h₁, …, hₘ) are distinct ordered bases for V.

## Example 1.1.8 (ℝᵐ). Let eᵢ be the vector in ℝᵐ defined by

eᵢ = (0, …, 0, 1, 0, …, 0),

where 1 is in the i-th position and 0s are elsewhere for i = 1, …, m. For real numbers a¹, …, aᵐ, we have

a¹ e₁ + ⋯ + aᵐ eₘ = (a¹, …, aᵐ),

from which it follows that e₁, …, eₘ span ℝᵐ and are linearly independent. We refer to {e₁, …, eₘ} as the standard unordered basis for ℝᵐ, and to (e₁, …, eₘ) as the standard ordered basis for ℝᵐ. Thus, not surprisingly, ℝᵐ has dimension m. ◇

## Throughout the remainder of Part I, unless stated otherwise, all bases are ordered.

from which it follows that A is unique.

Existence. Let us define A : V ⟶ W using (1.1.1) for all vectors v in V. The uniqueness of the m-tuple (a¹, ..., aᵐ) ensures that A is well-defined. Clearly, A(H) = (w₁, ..., wₘ). Let u = b¹h₁ + ⋯ + bᵐhₘ be a vector in V, and let c be a real number. Then

cv + u = (ca¹ + b¹)h₁ + ⋯ + (caᵐ + bᵐ)hₘ,

hence

A(cv + u) = (ca¹ + b¹)A(h₁) + ⋯ + (caᵐ + bᵐ)A(hₘ) = (ca¹ + b¹)w₁ + ⋯ + (caᵐ + bᵐ)wₘ = c(a¹w₁ + ⋯ + aᵐwₘ) + (b¹w₁ + ⋯ + bᵐwₘ) = cA(v) + A(u).

Thus, A is linear.

From the point of view of linear structure, isomorphic vector spaces are indistinguishable. In fact, it is easily shown using Theorem 1.1.10 that all m-dimensional vector space are isomorphic. More than that, they are all isomorphic to ℝᵐ. The isomorphism constructed with the help of Theorem 1.1.10 depends on the choice of bases for the vector spaces. However, we will see an instance in Section 1.2 where an isomorphism can be defined without having to resort to such an arbitrary choice.

Let V and W be vector spaces, and let A : V ⟶ W be a linear map. The kernel of A is defined by

ker(A) = {v ∈ V : A(v) = 0},

and the image of A by

im(A) = {A(v) ∈ W : v ∈ V}.

It is easily shown that ker(A) is a subspace of V, and im(A) is a subspace of W. The nullity of A is defined by

null(A) = dim(ker(A)),

and the rank of A by

rank(A) = dim(im(A)).

The nullity and rank of a linear map satisfy an important identity.

## Theorem 1.1.11 (Rank–Nullity Theorem). If V and W are vector spaces and A : V ⟶ W is a linear map, then

dim(V) = rank(A) + null(A). (1.1.2)

**Proof.** By Theorem 1.1.7(c), any basis $(h_1,\ldots,h_k)$ for $\ker(A)$ can be extended to a basis $(h_1,\ldots,h_k,h_{k+1},\ldots,h_m)$ for $V$. We claim that $(A(h_{k+1}),\ldots,A(h_m))$ is a basis for $\operatorname{im}(A)$. Let $v$ be a vector in $V$. Since $\mathcal H$ spans $V$, we have $v=a^1h_1+\cdots+a^mh_m$ for some real numbers $a^1,\ldots,a^m$. Then

$$A(v)=a^1A(h_1)+\cdots+a^kA(h_k)+a^{k+1}A(h_{k+1})+a^mA(h_m)$$
$$=a^{k+1}A(h_{k+1})+a^mA(h_m),$$

hence $A(h_{k+1}),\ldots,A(h_m)$ span $\operatorname{im}(A)$. Suppose

$$c^{k+1}A(h_{k+1})+\cdots+c^mA(h_m)=0$$

for some real numbers $c^{k+1},\ldots,c^m$. Then $A(c^{k+1}h_{k+1}+\cdots+c^mh_m)=0$, so $c^{k+1}h_{k+1}+\cdots+c^mh_m$ is in $\ker(V)$. Since $h_1,\ldots,h_k$ span $\ker(A)$, there are real numbers $b^1,\ldots,b^k$ such that

$$b^1h_1+\cdots+b^kh_k=c^{k+1}h_{k+1}+\cdots+c^mh_m,$$

hence

$$b^1h_1+\cdots+b^kh_k+(-c^{k+1})h_{k+1}+\cdots+(-c^m)h_m=0.$$

From the linear independence of $h_1,\ldots,h_k,h_{k+1},\ldots,h_m$, we have $c^{k+1}=\cdots=c^m=0$. Thus, $A(h_{k+1}),\ldots,A(h_m)$ are linearly independent. This proves the claim. It follows that

$$\operatorname{rank}(A)=\dim(\operatorname{im}(A))=m-k=\dim(V)-\dim(\ker(A))$$
$$=\dim(V)-\operatorname{null}(A).$$

As an example of the rank–nullity identity, consider the linear map $A:\mathbb R^3\longrightarrow\mathbb R^2$ given by $A(x,y,z)=(x+y,0)$. Then

$$\ker(A)=\{(x,y,z)\in\mathbb R^3:x+y=0\}$$

and

$$\operatorname{im}(A)=\{(x,y)\in\mathbb R^2:y=0\}.$$

In geometric terms, $\ker(A)$ is a plane in $\mathbb R^3$ and $\operatorname{im}(A)$ is a line in $\mathbb R^2$. Thus, $\operatorname{null}(A)=2$ and $\operatorname{rank}(A)=1$, which agrees with Theorem 1.1.11.

In the notation of Theorem 1.1.11, we observe from (1.1.2) that $\operatorname{rank}(A)\leq\dim(V)$. Thus, a linear map at best “preserves” dimension, but never increases it.

## **Theorem 1.1.12.**

*If $V$ and $W$ are vector spaces and $A:V\longrightarrow W$ is a linear map, then the following are equivalent:*

-   (a) $\operatorname{rank}(A)=\dim(V)$.
-   (b) $\operatorname{null}(A)=0$.
-   (c) $\ker(A)=\{0\}$.
-   (d) $A$ is injective.

Proof. (a) ⇔ (b) ⇔ (c): By Theorem 1.1.11,

rank(A) = dim(V)
⇔ null(A) = 0
⇔ dim(ker(A)) = 0
⇔ ker(A) = {0}.

(c) ⇒ (d): For vectors v, w in V, we have

A(v) = A(w)
⇔ A(v − w) = 0
⇔ v − w is in ker(A)
⇒ v − w = 0.

(d) ⇒ (c): Clearly, 0 is in ker(V). For a vector v in V, we have

v is in ker(A)
⇔ A(v) = 0
⇔ A(v) = A(0)
⇒ v = 0.

## Theorem 1.1.13. Let V and W be vector spaces, let ℋ be a basis for V, and let A : V ⟶ W be a linear map. Then:

-   (a) A is a linear isomorphism if and only if A(ℋ) is a basis for W.
-   (b) If A is a linear isomorphism, then dim(V) = dim(W).

Proof. Let ℋ = (h₁, …, hₘ).

(a)(⇒): Since A is surjective, for each vector w in W, there is a vector v in V such that A(v) = w. Let v = a¹h₁ + ⋯ + aᵐhₘ for some real numbers a¹, …, aᵐ. Then

w = A(v) = a¹ A(h₁) + ⋯ + aᵐ A(hₘ),

so A(ℋ) spans W. Suppose b¹A(h₁) + ⋯ + bᵐA(hₘ) = 0 for some real numbers b¹, …, bᵐ. Then A(b¹h₁ + ⋯ + bᵐhₘ) = 0, hence b¹h₁ + ⋯ + bᵐhₘ is in ker(A). Since A is injective, it follows from Theorem 1.1.12 that b¹h₁ + ⋯ + bᵐhₘ = 0, hence b¹ = ⋯ = bᵐ = 0. Thus, A(ℋ) is linearly independent.

(a)(⇐): Let w be a vector in W. Since A(ℋ) spans W, we have w = b¹A(h₁) + ⋯ + bᵐA(hₘ) for some real numbers b¹, …, bᵐ. Then w = A(b¹h₁ + ⋯ + bᵐhₘ), so A is surjective. Let v = a¹h₁ + ⋯ + aᵐhₘ be a vector in ker(A). Then 0 = A(v) = a¹A(h₁) + ⋯ + aᵐA(hₘ). Since A(ℋ) is linearly independent, it follows that a¹ = ⋯ = aᵐ = 0, so v = 0. Thus, ker(A) = {0}. By Theorem 1.1.12, A is injective.

(b): This follows from part (a).

We pause here to comment on the way proofs are presented when there is an equation or other type of display that stretches over several lines of text. The necessary justification for logical steps in such displays, whether it be equation numbers, theorem numbers, example numbers, and so on, are often provided in brackets at the end of corresponding lines. In order to economize on space, “[Theorem x.y.z]” and “[Example x.y.z]” are abbreviated to “[Th x.y.z]” and “[Ex x.y.z]”. The proof of the next result illustrates these conventions.

## Theorem 1.1.14. If V and W are vector spaces of dimension m and A : V ⟶ W is a linear map, then the following are equivalent:

-   (a) A is a linear isomorphism.
-   (b) A is injective.
-   (c) A is surjective.
-   (d) rank(A) = m.

Proof. (a) ⇒ (b): This is true by definition.

(b) ⇔ (c): By Theorem 1.1.11,

dim(W) = dim(V) = rank(A) + null(A) = dim(im(A)) + null(A),

hence

W = im(A)
⇔ null(A) = 0 [Th 1.1.7(b)]
⇔ A is injective. [Th 1.1.12]

(c) ⇒ (a): Since A is surjective, we have from (b) ⇔ (c) that A is also injective.

(d) ⇔ (b): This follows from Theorem 1.1.12.

Let V be a vector space, and let U₁,…,Uₖ be subspaces. The sum of U₁,…,Uₖ is denoted by U₁ + ⋯ + Uₖ and defined by

U₁ + ⋯ + Uₖ = {u₁ + ⋯ + uₖ : u₁ ∈ U₁,…, uₖ ∈ Uₖ}.

For example, ℝ(1,0) + ℝ(0,1) = ℝ². It is easily shown that

U₁ + ⋯ + Uₖ = span(U₁ ∪ ⋯ ∪ Uₖ),

from which it follows that U₁ + ⋯ + Uₖ is the smallest subspace of V containing each of U₁,…,Uₖ, in the sense that any subspace containing each of U₁,…,Uₖ also contains U₁ + ⋯ + Uₖ. We observe that

U₁ + ⋯ + Uₖ + {0} = U₁ + ⋯ + Uₖ,

which shows that adding the zero vector spaces does not change a sum. For vectors v₁,…,vₖ in V, we have the following connection between spans and sums:

span({v₁,…,vₖ}) = ℝv₁ + ⋯ + ℝvₖ.
