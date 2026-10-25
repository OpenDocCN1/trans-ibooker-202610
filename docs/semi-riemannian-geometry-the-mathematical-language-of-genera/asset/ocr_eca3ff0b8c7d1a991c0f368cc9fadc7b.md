# 8.3 Orientation of Scalar Product Spaces

In Section 8.2, we considered orientation of vector spaces. We now expand our coverage to include scalar product spaces.

# Theorem 8.3.1. Let (V,𝔤) be a scalar product space of index ν, let ℰ = (e₁,…,eₘ) and ℋ = (h₁,…,hₘ) be bases for V, with ℰ orthonormal, and let (ξ¹,…,ξᵐ) and (θ¹,…,θᵐ) be the corresponding dual bases. Then:

(a)

det(𝔤_ℋ) = (-1)^ν (det([id_V]_ℋ^ℰ))^2.

(b)

ξ¹ ∧ ⋯ ∧ ξᵐ = ±√|det(𝔤_ℋ)| θ¹ ∧ ⋯ ∧ θᵐ,

where the positive (negative) sign is chosen if ℰ and ℋ are consistent (not consistent).

(c) If ℋ is orthonormal, then

ξ¹ ∧ ⋯ ∧ ξᵐ = ±θ¹ ∧ ⋯ ∧ θᵐ,

where the signs are chosen as in part (b).

Proof. (a): We have from Theorem 3.1.3 that

𝔤_ℋ = ([id_V]_ℋ^ℰ)^T 𝔤_ℰ [id_V]_ℋ^ℰ,

and from (4.2.3) that det(𝔤_ℰ) = (-1)^ν. The result follows.

(b): By Theorem 7.3.3(a),

ξ¹ ∧ ⋯ ∧ ξᵐ = det([id_V]_ℋ^ℰ) θ¹ ∧ ⋯ ∧ θᵐ,

and by part (a), |det(𝔤_ℋ)| = (det([id_V]_ℋ^ℰ))^2, hence

det([id_V]_ℋ^ℰ) = ±√|det(𝔤_ℋ)|,

where the positive (negative) sign is chosen if ℋ and ℰ are consistent (not consistent). The result follows.

(c): This follows from (4.2.3) and part (b).

# Theorem 8.3.2. Let (V,𝔤) be a scalar product space, let ℰ = (e₁,…,eₘ) and ℱ = (f₁,…,fₘ) be orthonormal bases for V, and let ϖ be an orientation multicovector on V. Then

ϖ(e₁,…,eₘ) = ±ϖ(f₁,…,fₘ),

where the positive (negative) sign is chosen if ℰ and ℱ are consistent (not consistent).