On the other hand, since Δ is alternating,

Δ(P₁, …, Pⱼ, …, Pᵢ, …, Pₘ) = −Δ(P₁, …, Pᵢ, …, Pⱼ, …, Pₘ).

Thus,

Δ(P₁, …, Pᵢ, …, Pⱼ, …, Pₘ) = −Δ(P₁, …, Pᵢ, …, Pⱼ, …, Pₘ),

from which the result follows.

(c) ⇒ (a): For 1 ≤ i < j ≤ m, we have

0 = Δ(P₁, …, Pᵢ + Pⱼ, …, Pᵢ + Pⱼ, …, Pₘ)
= Δ(P₁, …, Pᵢ, …, Pᵢ, …, Pₘ) + Δ(P₁, …, Pᵢ, …, Pⱼ, …, Pₘ)
+ Δ(P₁, …, Pⱼ, …, Pᵢ, …, Pₘ) + Δ(P₁, …, Pⱼ, …, Pⱼ, …, Pₘ)
= Δ(P₁, …, Pᵢ, …, Pⱼ, …, Pₘ) + Δ(P₁, …, Pⱼ, …, Pᵢ, …, Pₘ),

from which the result follows.

To prove (c) ⇔ (d), we replace the assertion in part (d) with the following logically equivalent assertion:
(d′): If P₁, …, Pₘ are linearly dependent matrices in Matₘ×₁, then Δ(P₁, …, Pₘ) = 0.

(c) ⇒ (d′): Since P₁, …, Pₘ are linearly dependent, one of them can be expressed as a linear combination of the others, say, P₁ = Σᵢ₌₂ᵐ aⁱ Pᵢ. Then

Δ(P₁, …, Pₘ) = Δ( Σᵢ₌₂ᵐ aⁱ Pᵢ, P₂, …, Pₘ ) = Σᵢ₌₂ᵐ aⁱ Δ(Pᵢ, P₂, …, Pₘ).

Since Δ(Pᵢ, P₂, …, Pₘ) has Pᵢ in (at least) two positions, Δ(Pᵢ, P₂, …, Pₘ) = 0 for i = 2, …, m. Thus, Δ(P₁, …, Pₘ) = 0.

(d′) ⇒ (c): If two (or more) of P₁, …, Pₘ are equal, then P₁, …, Pₘ are linearly dependent, so Δ(P₁, …, Pₘ) = 0.

A function Δ : (Matₘ×₁)ᵐ ⟶ ℝ is said to be a determinant function (on Matₘ×₁) if it is both multilinear and alternating.

# Theorem 2.4.3. Let Δ : (Matₘ×₁)ᵐ ⟶ ℝ be a determinant function, and let P₁, …, Pₘ be matrices in Matₘ×₁, with

P₁ = [p₁¹; ⋮; p₁ᵐ],      …      Pₘ = [pₘ¹; ⋮; pₘᵐ].

Then

Δ(P₁, …, Pₘ) = Δ(E₁, …, Eₘ) Σ_{σ ∈ Sₘ} sgn(σ) p₁^{σ(1)} … pₘ^{σ(m)}.