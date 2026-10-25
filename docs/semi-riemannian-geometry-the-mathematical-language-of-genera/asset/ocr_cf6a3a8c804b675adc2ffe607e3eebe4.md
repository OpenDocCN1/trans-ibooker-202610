of 𝒜 and ℬ. Unlike with addition, the product of tensors does not require 𝒜 and ℬ to have the same rank. Usually, 𝒜 ⊗ ℬ ≠ ℬ ⊗ 𝒜, so the tensor product is generally not commutative.

The next result gives the basic algebraic properties of tensors.

# Theorem 5.1.2. Let V be a vector space, let 𝒜, 𝒜₁, 𝒜₂ and ℬ, ℬ₁, ℬ₂ and 𝒞 be tensors in 𝒯ₛʳ(V) and 𝒯ₛ'ʳ'(V) and 𝒯ₛ''ʳ''(V), respectively, and let c be a real number. Then:

+   (a) (𝒜₁ + 𝒜₂) ⊗ ℬ = 𝒜₁ ⊗ ℬ + 𝒜₂ ⊗ ℬ.
+   (b) 𝒜 ⊗ (ℬ₁ + ℬ₂) = 𝒜 ⊗ ℬ₁ + 𝒜 ⊗ ℬ₂.
+   (c) (c𝒜) ⊗ ℬ = c(𝒜 ⊗ ℬ) = 𝒜 ⊗ (cℬ).
+   (d) (𝒜 ⊗ ℬ) ⊗ 𝒞 = 𝒜 ⊗ (ℬ ⊗ 𝒞).

Proof. Straightforward.

In view of Theorem 5.1.2(d), we drop parentheses around tensor products and, for example, denote (𝒜 ⊗ ℬ) ⊗ 𝒞 and 𝒜 ⊗ (ℬ ⊗ 𝒞) by 𝒜 ⊗ ℬ ⊗ 𝒞, with corresponding notation for tensor products of more than three terms. By forming the tensor product of collections of vectors and covectors, we can construct tensors of arbitrary rank. To illustrate, from vectors w₁,…,wᵣ in V and covectors ζ¹,…,ζˢ in V*, we obtain the tensor w₁ ⊗ ⋯ ⊗ wᵣ ⊗ ζ¹ ⊗ ⋯ ⊗ ζˢ in 𝒯ₛʳ(V). Then (5.1.4) gives

w₁ ⊗ ⋯ ⊗ wᵣ ⊗ ζ¹ ⊗ ⋯ ⊗ ζˢ(η¹,…,ηʳ, v₁,…,vₛ)
= w₁(η¹)⋯wᵣ(ηʳ) ζ¹(v₁)⋯ζˢ(vₛ)
for all all covectors η¹,…,ηʳ in V* and all vectors v₁,…,vₛ in V.

Let V be a vector space, let 𝒜 be a tensor in 𝒯ₛʳ(V), let ℋ = (h₁,…,hₘ) be a basis for V, and let Θ = (θ¹,…,θᵐ) be its dual basis. The components of 𝒜 with respect to ℋ are defined by

𝒜ⁱ¹…ⁱʳ
j₁…jₛ = 𝒜(θⁱ¹,…,θⁱʳ, hⱼ₁,…,hⱼₛ)

for all 1 ≤ i₁,…,iᵣ ≤ m and 1 ≤ j₁,…,jₛ ≤ m.

# Theorem 5.1.3 (Basis for 𝒯ₛʳ(V)). With the above setup: