∑ₚ 𝔤ⁱᵏᵖ hₚ, so Sₗᵏ(𝒜) has the components

Sₗᵏ(𝒜)ⁱ¹...ⁱʳ⁺¹
j₁...jₛ₋₁
= Sₗᵏ(𝒜)(θⁱ¹, ..., θⁱᵏ, ..., θⁱʳ⁺¹, hⱼ₁, ..., hⱼₗ, ..., hⱼₛ₋₁)
= 𝒜(θⁱ¹, ..., θⁱᵏ⁻¹, θⁱᵏ⁺¹, ..., θⁱʳ⁺¹, hⱼ₁, ..., hⱼₗ₋₁, θⁱᵏˢ, hⱼₗ, ..., hⱼₛ₋₁)
= 𝒜( θⁱ¹, ..., θⁱᵏ⁻¹, θⁱᵏ⁺¹, ..., θⁱʳ⁺¹, hⱼ₁, ..., hⱼₗ₋₁, ∑ₚ 𝔤ⁱᵏᵖ hₚ, hⱼₗ, ..., hⱼₛ₋₁ )
= ∑ₚ 𝔤ⁱᵏᵖ 𝒜(θⁱ¹, ..., θⁱᵏ⁻¹, θⁱᵏ⁺¹, ..., θⁱʳ⁺¹, hⱼ₁, ..., hⱼₗ₋₁, hₚ, hⱼₗ, ..., hⱼₛ₋₁)
= ∑ₚ 𝔤ⁱᵏᵖ 𝒜ⁱ₁...îᵢₖ...ⁱʳ⁺¹
j₁...jₗ₋₁ p jₗ...jₛ₋₁

with respect to ℋ.

# Theorem 6.3.5. Let (V,𝔤) be a scalar product space, let ℋ be a basis for V, and let 𝔤_ℋ⁻¹ = [𝔤ⁱʲ]. Let 𝒜 be a tensor in 𝒯ₛʳ(V), and let r ≥ 0 and s ≥ 2 be integers. Then:

+   (a) For integers 1 ≤ k < l ≤ s and 1 ≤ n ≤ r, the components of the tensor Cₖⁿ ∘ Sₗⁿ(𝒜) in 𝒯ₛ₋₂ʳ(V) with respect to ℋ are
+   Cₖⁿ ∘ Sₗⁿ(𝒜)ⁱ₁...ⁱʳ
+   j₁...jₛ₋₂ = ∑ₚq 𝔤ᵖq 𝒜ⁱ₁...ⁱʳ
+   j₁...jₖ₋₁ p jₖ...jₗ₋₂ q jₗ₋₁...jₛ₋₂. (6.3.2)

+   (b) For integers 1 ≤ k ≤ s and 1 ≤ n ≤ r, the components of the tensor Cₖⁿ ∘ Sₖⁿ(𝒜) in 𝒯ₛ₋₂ʳ(V) with respect to ℋ are
+   Cₖⁿ ∘ Sₖⁿ(𝒜)ⁱ₁...ⁱʳ
+   j₁...jₛ₋₂ = Cₖⁿ ∘ Sₖ₊₁ⁿ(𝒜)ⁱ₁...ⁱʳ
+   j₁...jₛ₋₂
+   = ∑ₚq 𝔤ᵖq 𝒜ⁱ₁...ⁱʳ
+   j₁...jₖ₋₁ p q jₖ...jₛ₋₂. (6.3.3)

Proof. By Theorem 6.3.4, the components of the tensor Sₗⁿ(𝒜) in 𝒯ₛ₋₁ʳ⁺¹(V) with respect to ℋ are
Sₗⁿ(𝒜)ⁱ₁...ⁱʳ⁺¹
j₁...jₛ₋₁ = ∑_q 𝔤ⁱₙ q 𝒜ⁱ₁...îₙ...ⁱʳ⁺¹
j₁...jₗ₋₁ q jₗ...jₛ₋₁. (6.3.4)

(a): We have
Cₖⁿ ∘ Sₗⁿ(𝒜)ⁱ₁...îₙ...ⁱʳ⁺¹
j₁...ĵₗ...jₛ₋₁ = ∑ₚ Sₗⁿ(𝒜)ⁱ₁...ⁱₙ₋₁ p îₙ₊₁...ⁱʳ⁺¹
j₁...jₖ₋₁ p jₖ₊₁...jₛ₋₁ [Th 5.4.5]
= ∑ₚ ( ∑_q 𝔤ᵖq 𝒜ⁱ₁...îₙ...ⁱʳ⁺¹
j₁...jₖ₋₁ p jₖ₊₁...jₗ₋₁ q jₗ...jₛ₋₁ ) [(6.3.4)]
= ∑ₚq 𝔤ᵖq 𝒜ⁱ₁...îₙ...ⁱʳ⁺¹
j₁...jₖ₋₁ p jₖ₊₁...jₗ₋₁ q jₗ...jₛ₋₁.