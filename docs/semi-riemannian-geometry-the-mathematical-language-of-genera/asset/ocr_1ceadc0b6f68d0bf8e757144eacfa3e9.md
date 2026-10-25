# Chapter 6

# Tensors on Scalar Product Spaces

# 6.1 Contraction of Tensors

In Section 5.4, we discussed contraction of tensors over vector spaces. In the setting of scalar product spaces, there is more to say.

Theorem 6.1.1 (Orthonormal Basis Expression for Ordinary Contraction). Let (V,g) be a scalar product space with signature (ε₁,…,εₘ), let (e₁,…,eₘ) be an orthonormal basis for V, and let 𝒜 be a tensor in 𝒯ₛ¹(V). Then:

+   (a) For integers s ≥ 2 and 1 ≤ l ≤ s − 1, the tensor Cₗ¹(𝒜) in 𝒯ₛ₋₁⁰(V) is given by
+   
+   Cₗ¹(𝒜)(v₁,…,vₛ₋₁) = Σᵢ εᵢ⟨ℛₛ⁻¹(𝒜)(v₁,…,vₗ₋₁,eᵢ,vₗ,…,vₛ₋₁), eᵢ⟩
+   
+   for all vectors v₁,…,vₛ₋₁ in V, where ℛₛ is given by (5.3.1).

+   (b) For s = 1, the real number C₁¹(𝒜) is given by
+   
+   C₁¹(𝒜) = Σᵢ εᵢ⟨ℛ⁻¹(𝒜)(eᵢ), eᵢ⟩,
+   
+   where ℛ is given by (5.3.2).

Proof. (a): By Theorem 5.4.5, the components of Cₗ¹(𝒜) with respect to (e₁,…,eₘ) are

Cₗ¹(𝒜)(eⱼ₁,…,eⱼₛ₋₁) = Cₗ¹(𝒜)ⱼ₁…ⱼₛ₋₁ = Σₚ 𝒜ⱼ₁…ⱼₗ₋₁ p p jₗ…ⱼₛ₋₁. (6.1.1)