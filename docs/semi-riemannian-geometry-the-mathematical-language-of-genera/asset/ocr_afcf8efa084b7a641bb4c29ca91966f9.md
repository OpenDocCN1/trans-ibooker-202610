# Theorem 15.8.4.

Let M be a smooth m-manifold, and let U be an open set in M. If (X₁, ..., Xₘ) is a frame on U and (ω¹, ..., ωᵐ) is its dual frame, then

```
ω^{j₁} ∧ ⋯ ∧ ω^{jₛ}(X_{i₁}, ..., X_{iₛ}) = { 1 if (i₁, ..., iₛ) = (j₁, ..., jₛ) 0 if (i₁, ..., iₛ) ≠ (j₁, ..., jₛ) }
```

for all 1 ≤ i₁ < ⋯ < iₛ ≤ m and 1 ≤ j₁ < ⋯ < jₛ ≤ m. In particular, if (U, (xⁱ)) is a chart on M, and (∂/∂x¹, ..., ∂/∂xᵐ) and (d(x¹), ..., d(xᵐ)) are the corresponding coordinate and dual coordinate frames, then

```
d(x^{j₁}) ∧ ⋯ ∧ d(x^{jₛ}) ( ∂/∂x^{i₁}, ..., ∂/∂x^{iₛ} ) = { 1 if (i₁, ..., iₛ) = (j₁, ..., jₛ) 0 if (i₁, ..., iₛ) ≠ (j₁, ..., jₛ) }
```

for all 1 ≤ i₁ < ⋯ < iₛ ≤ m and 1 ≤ j₁ < ⋯ < jₛ ≤ m.

Proof. This is the manifold version of Theorem 7.2.9.

# Theorem 15.8.5.

Let M be a smooth m-manifold, let (U, (xⁱ)) be a chart on M, and let (d(x¹), ..., d(xᵐ)) be the corresponding dual coordinate frame. If ω is a form in Λᵐ(U), then there is a uniquely determined function f in C^∞(U) such that ω = f d(x¹) ∧ ⋯ ∧ d(xᵐ).

Proof. Let (∂/∂x¹, ..., ∂/∂xᵐ) be the coordinate frame corresponding to (U, (xⁱ)), and, for brevity, let Ω = d(x¹) ∧ ⋯ ∧ d(xᵐ). For each point p in U, by definition, ω_p is a covector in T*_p(U). It follows from Theorem 7.2.12(d) that ω_p = f(p) Ω_p for some real number f(p). The assignment p ⟼ f(p) defines a function f : U ⟶ ℝ such that ω = f Ω. Since ω is in Λᵐ(U), by definition, ω(X₁, ..., Xₘ) is a function in C^∞(U) for all vector fields X₁, ..., Xₘ in 𝔛(U). In particular, this is so for ∂/∂x¹, ..., ∂/∂xᵐ. By Theorem 15.8.4,

```
f = f Ω( ∂/∂x¹, ..., ∂/∂xᵐ ) = ω( ∂/∂x¹, ..., ∂/∂xᵐ )
```

hence f is in C^∞(U). The uniqueness of f follows from the preceding computations.

# 15.9 Pushforward and Pullback of Functions

Let M be a smooth manifold, and let F : M ⟶ N be a diffeomorphism. Pushforward by F (for functions) is the map

```
F_• : C^∞(M) ⟶ C^∞(N)
```

defined by

```
F_•(f) = f ∘ F^{-1} (15.9.1)
```

for all functions f in C^∞(M).