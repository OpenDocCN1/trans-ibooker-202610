When ν = 0, we say that M is a Riemannian m-manifold, and when m ≥ 2 and ν = 1, M is said to be a Lorentz m-manifold. A semi-Riemannian m-manifold with a connection is a triple (M, 𝔤, ∇), where (M, 𝔤) is a semi-Riemannian m-manifold and ∇ is a connection on M. For the time being, we do not assume that 𝔤 and ∇ are related.

We have already encountered several semi-Riemannian manifolds, although we were not in a position to use such terminology at the time. For example, semi-Euclidean (m, ν)-space ℝᵥᵐ = (ℝᵐ, 𝔰) is a semi-Riemannian m-manifold of index ν. In particular, Euclidean m-space ℝ₀ᵐ = (ℝᵐ, 𝔢) is a Riemannian m-manifold, and Minkowski m-space ℝ₁ᵐ = (ℝᵐ, 𝔪) is a Lorentz m-manifold. With tensor notation now at our disposal, we can express 𝔰 in local coordinates as

```
𝔰 = { Σᵢ=1ᵐ d(xⁱ) ⊗ d(xⁱ)                     if ν = 0
       Σᵢ=1ᵐ⁻ᵛ d(xⁱ) ⊗ d(xⁱ) − Σᵢ=m−ν+1ᵐ d(xⁱ) ⊗ d(xⁱ)   if 1 ≤ ν ≤ m − 1
      − Σᵢ=1ᵐ d(xⁱ) ⊗ d(xⁱ)                     if ν = m. }
```

To our list of semi-Riemannian manifolds, let us add the unit sphere 𝓢² and hyperbolic space ℋ², which are Riemannian 2-manifolds, and the pseudosphere 𝒫², which is a Lorentz 2-manifold. In fact, each of the regular surfaces in ℝ₀³ described in Sections 13.1–13.8 is a Riemannian 2-manifold. We now provide a less obvious example of a Riemannian 2-manifold.

# Example 19.1.1.

Let ℙ² = {(r, s) ∈ ℝ² : s > 0}, which is an open set in the smooth 2-manifold ℝ². It follows from Theorem 14.3.3(b), (14.3.4), and Theorem 14.8.4 that ℙ² is a 2-submanifold of ℝ², and that T₍ᵣ,ₛ₎(ℙ²) can be identified with ℝ². We define a tensor field 𝔭 in 𝒯₂⁰(ℙ²) by

𝔭₍ᵣ,ₛ₎(v, w) = 1/s² ⟨v, w⟩

for all points (r, s) in ℙ² and all vectors v, w in T₍ᵣ,ₛ₎(ℙ²), where ⟨·,·⟩ is the Euclidean inner product. It is easily shown that 𝔭₍ᵣ,ₛ₎ is an inner product for all (r, s) in ℙ². Thus, (ℙ², 𝔭) is a Riemannian 2-manifold, called the Poincaré half-plane. ◇

# Theorem 19.1.2 (Open Set).

If (M, 𝔤) is a semi-Riemannian m-manifold of index ν and U is an open set in M, then (U, 𝔤|ᵤ) is a semi-Riemannian m-submanifold of (M, 𝔤) of index ν.

Proof. By Theorem 14.8.4, U is an m-submanifold of M. From Theorem 14.3.3(b), we have the identification Tₚ(U) = Tₚ(M) for all p in U. It follows that (𝔤|ᵤ)|ₚ is nondegenerate on Tₚ(U) for all p in U, and ind((𝔤|ᵤ)ₚ) = ind(𝔤ₚ) = ν for all p in U. Thus, 𝔤|ᵤ satisfies [G1] and [G2]. □

The remainder of this section presents definitions that will play a role later on.