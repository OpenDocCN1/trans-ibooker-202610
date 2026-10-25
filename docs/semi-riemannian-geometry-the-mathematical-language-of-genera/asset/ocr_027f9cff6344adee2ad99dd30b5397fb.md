defined by

C_{kl} = C_k^1 \circ S_l^1.  (6.5.3)

We have from Theorem 6.3.5(b) that

C_{k,k+1} = C_k^1 \circ S_k^1.  (6.5.4)

In order to avoid confusion between metric contractions and the contractions defined in Section 5.4, we recall that the latter are sometimes referred to as ordinary contractions.

# Theorem 6.5.1 (Orthonormal Basis Expression for Metric Contraction). Let (V,𝔤) be a scalar product space with signature (ε₁,…,ε_m), let (e₁,…,e_m) be an orthonormal basis for V, and let 𝒜 be a tensor in 𝒯_s⁰(V). Then:

+   (a) For integers s ≥ 3 and 1 ≤ k < l ≤ s, the tensor C_{kl}(𝒜) in 𝒯_{s-2}⁰(V) is given by

C_{kl}(𝒜)(v₁,…,v_{s-2}) = Σ_i ε_i 𝒜(v₁,…,v_{k-1},e_i,v_k,…,v_{l-2},e_i,v_{l-1},…,v_{s-2})  (6.5.5)

for all vectors v₁,…,v_{s-2} in V.

+   (b) For s = 2, the real number C₁₂(𝒜) is given by

C₁₂(𝒜) = Σ_i ε_i 𝒜(e_i,e_i).

Proof. (a): Let ℰ = (e₁,…,e_m) and 𝔤ℰ = [𝔤_{ij}], and observe that 𝔤^{ij} = 𝔤_{ij} = ε_i δ_{ij}. We have from Theorem 6.3.5(a) and (6.5.3) that C_{kl}(𝒜) has the components

C_{kl}(𝒜)_{j₁…j_{s-2}} = Σ_{pq} 𝔤^{pq} 𝒜_{j₁…j_{k-1}p j_k…j_{l-2}q j_{l-1}…j_{s-2}} = Σ_p ε_p 𝒜_{j₁…j_{k-1}p j_k…j_{l-2}p j_{l-1}…j_{s-2}} = Σ_p ε_p 𝒜(e_{j₁},…,e_{j_{k-1}},e_p,e_{j_k},…,e_{j_{l-2}},e_p,e_{j_{l-1}},…,e_{j_{s-2}})

with respect to ℋ. The result for arbitrary vectors v₁,…,v_{s-2} in V follows from the multilinearity of 𝒜 and C_{kl}(𝒜).