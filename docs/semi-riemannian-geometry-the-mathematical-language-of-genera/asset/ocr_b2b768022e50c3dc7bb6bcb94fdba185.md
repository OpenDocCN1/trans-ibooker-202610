In ℝ₀³, with standard coordinates (x, y, z), this becomes

Lap(f) = ∂²f/∂x² + ∂²f/∂y² + ∂²f/∂z² = lap(f),

and in ℝ₁⁴, with standard coordinates (x, y, z, t), it is

Lap(f) = ∂²f/∂x² + ∂²f/∂y² + ∂²f/∂z² − ∂²f/∂t².

In the physics literature, Lap in the context of ℝ₁⁴ is usually denoted by □ (sometimes by □²) and called the d’Alembert operator.

# 20.8 Laplace–de Rham Operator

Let (M, 𝔤, 𝔒) be an oriented semi-Riemannian manifold. The Laplace–de Rham operator is the family of linear maps

Δ : Λˢ(M) ⟶ Λˢ(M)

defined for 1 ≤ s ≤ m by

Δ = dδ + δd;  (20.8.1)

that is,

Δ(ω) = dδ(ω) + δd(ω)

for all forms ω in Λˢ(M). Recall from Theorem 16.1.1(c) that d² = 0, and from Theorem 20.2.1(a) that δ² = 0. Thus,

(d + δ) ∘ (d + δ) = d² + dδ + δd + δ² = Δ,

which can be expressed as

Δ = (d + δ)².

The next result shows that, except for a sign, the Laplace–de Rham operator is a generalization of the Laplace operator.

# Theorem 20.8.1. If (M, 𝔤, 𝔒) is an oriented semi-Riemannian manifold and f is a function in C^∞(M), then

Lap(f) = −Δ(f).

Proof. We have

Lap(f) = Div(Grad(f))  [(20.7.1)]
= −δ(Grad(f)^♭)  [Th 20.4.3(b)]
= −δd(f)  [(20.3.2)]
= −(dδ + δd)(f)  [(20.2.2)]
= −Δ(f).  [(20.8.1)]