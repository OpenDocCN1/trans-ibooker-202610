# Appendix B

# Projection Theory

# B.1 Projections: Deterministic Spaces

Projection theory plays an important role in subspace identification primarily because subspaces are created by transforming or “projecting” a vector into a lower dimensional space – the subspace [1–3]. We are primarily interested in two projection operators: (i) orthogonal and (ii) oblique or parallel. The orthogonal projection operator “projects” • onto 𝒴 as 𝒫_{•|𝒴} or its complement 𝒫_{•|𝒴}^⊥, while the oblique projection operator “projects” • onto 𝒴 “along” 𝒵 as 𝒫_{•|𝒴∥𝒵}^∥. For instance, orthogonally projecting the vector x, we have 𝒫_{x|𝒴} or 𝒫_{x|𝒴}^⊥, while obliquely projecting x along 𝒵 is 𝒫_{x|𝒴∥𝒵}.

Mathematically, the n-dimensional vector space ℛ can be decomposed into a direct sum of subspaces such that

ℛ = ℛ₁ + ℛ₂

(B.1)

then for the vector r ∈ ℛⁿ, we have

r = r₁ + r₂ for r₁ ∈ ℛ₁ and r₂ ∈ ℛ₂

(B.2)

with r₁ defined as the projection of r onto ℛ₁, while r₂ defined as the projection of r onto ℛ₂.

When a particular projection is termed parallel (∥) or “oblique,” the corresponding projection operator that transforms r onto ℛ₁ along ℛ₂ is defined as 𝒫_{r|ℛ₁∥ℛ₂}^∥, then

r₁ = 𝒫_{r|ℛ₁∥ℛ₂}^∥ or r₂ = 𝒫_{r|ℛ₂∥ℛ₁}^∥

(B.3)

with r₁ defined as the oblique projection of r onto ℛ₁ along ℛ₂, while r₂ defined as the projection of r onto ℛ₂ along ℛ₁.

When the intersection ℛ₁ ∩ ℛ₂ = 0, then the direct sum follows as

𝒫_{r|ℛ₁∪ℛ₁} = 𝒫_{r|ℛ₁∥ℛ₂}^∥ + 𝒫_{r|ℛ₂∥ℛ₁}^∥

(B.4)