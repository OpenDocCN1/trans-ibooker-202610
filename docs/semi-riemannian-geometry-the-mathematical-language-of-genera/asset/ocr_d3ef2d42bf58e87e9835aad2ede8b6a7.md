# Example 6.2.3. For a tensor 𝒜 in 𝒯₂²(V), the tensor F₁²(𝒜) in 𝒯₃¹(V) is given by

F₁²(𝒜)(η¹, v₁, v₂, v₃) = 𝒜(η¹, v₁^F, v₂, v₃).

By Theorem 3.3.1(a), hⱼ^F = Σₚ gⱼₚ θᵖ, so F₁²(𝒜) has the components

```
F₁²(𝒜)ⁱ_{jkl} = F₁²(𝒜)(θⁱ, hⱼ, hₖ, hₗ) = 𝒜(θⁱ, hⱼ^F, hₖ, hₗ)
= 𝒜(θⁱ, Σₚ gⱼₚ θᵖ, hₖ, hₗ) = Σₚ gⱼₚ 𝒜(θⁱ, θᵖ, hₖ, hₗ)
= Σₚ gⱼₚ 𝒜ⁱᵖ_{kl}
```

with respect to ℋ.

# Theorem 6.2.4. Let (V, g) be a scalar product space, let ℋ be a basis for V, and let g_ℋ = [gᵢⱼ]. Let 𝒜 be a tensor in 𝒯ₛʳ(V), and let 1 ≤ k ≤ r and 1 ≤ l ≤ s+1 be integers. Then the tensor Fₗᵏ(𝒜) in 𝒯_{s+1}^{r-1}(V) has the components

```
Fₗᵏ(𝒜)^{i₁...i_{k-1} p i_k...i_{r-1}}_{j₁...j_{l-1} j_{l+1}...j_{s+1}} = Σₚ gⱼₗₚ 𝒜^{i₁...i_{k-1} p i_k...i_{r-1}}_{j₁...ĵₗ...j_{s+1}}
```

with respect to ℋ.

# Proof. The proof is an elaboration of Example 6.2.3. We consider only the case [F2]. The tensor Fₗᵏ(𝒜) in 𝒯_{s+1}^{r-1}(V) is given by

```
Fₗᵏ(𝒜)(η¹,...,η^{r-1}, v₁,...,v_{s+1})
= 𝒜(η¹,...,η^{k-1}, vₗ^F, η^k,...,η^{r-1}, v₁,...,v̂ₗ,...,v_{s+1})
```

for all covectors η¹,...,η^{r-1} in V* and all vectors v₁,...,v_{s+1} in V. Let ℋ = (h₁,...,h_m), and let (θ¹,...,θ^m) be its dual basis. By Theorem 3.3.1(a), hⱼ^F = Σₚ gⱼₚ θᵖ, so Fₗᵏ(𝒜) has the components

```
Fₗᵏ(𝒜)^{i₁...i_{r-1}}_{j₁...j_{s+1}} = Fₗᵏ(𝒜)(θ^{i₁},...,θ^{i_k},...,θ^{i_{r-1}}, h_{j₁},...,h_{j_{s+1}})
= 𝒜(θ^{i₁},...,θ^{i_{k-1}}, h_{jₗ}^F, θ^{i_k},...,θ^{i_{r-1}}, h_{j₁},...,h_{j_{l-1}}, h_{j_{l+1}},...,h_{j_{s+1}})
= 𝒜(θ^{i₁},...,θ^{i_{k-1}}, Σₚ gⱼₗₚ θᵖ, θ^{i_k},...,θ^{i_{r-1}}, h_{j₁},...,h_{j_{l-1}}, h_{j_{l+1}},...,h_{j_{s+1}})
= Σₚ gⱼₗₚ 𝒜(θ^{i₁},...,θ^{i_{k-1}}, θᵖ, θ^{i_k},...,θ^{i_{r-1}}, h_{j₁},...,h_{j_{l-1}}, h_{j_{l+1}},...,h_{j_{s+1}})
= Σₚ gⱼₗₚ 𝒜^{i₁...i_{k-1} p i_k...i_{r-1}}_{j₁...ĵₗ...j_{s+1}}
```

with respect to ℋ.