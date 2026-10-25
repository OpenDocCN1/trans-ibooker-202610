(b): We have

⟨𝒲ₚ(Hᵢ|q), Hⱼ|q⟩ = 𝔥ₚ(Hᵢ|q, Hⱼ|q)    [(12.8.6)]
= 𝔥ᵢⱼ(q)    [(12.8.8)]
= εₘϑᵢⱼ(q)    [part (a)]
= εₘϑⱼᵢ(q)    [Th 12.3.3(d)]
= 𝔥ⱼᵢ(q)    [part (a)]
= 𝔥ₚ(Hⱼ|q, Hᵢ|q)    [(12.8.8)]
= ⟨𝒲ₚ(Hⱼ|q), Hᵢ|q⟩    [(12.8.6)]
= ⟨Hᵢ|q, 𝒲ₚ(Hⱼ|q)⟩.

The result now follows from the observations that (H₁|q, H₂|q) is a basis for Tₚ(M), 𝔤ₚ is bilinear, and 𝒲ₚ is linear.

(c): It was established in connection with (12.8.6) that 𝔥ₚ is bilinear. For vectors v, w in Tₚ(M), we have

𝔥ₚ(v, w) = ⟨𝒲ₚ(v), w⟩    [(12.8.6)]
= ⟨v, 𝒲ₚ(w)⟩    [part (b)]
= ⟨𝒲ₚ(w), v⟩
= 𝔥ₚ(w, v).    [(12.8.6)]

Since v and w were arbitrary, the result follows.

# Theorem 12.8.3. Let (M, 𝔤, 𝔞, 𝔒) be an oriented regular surface in ℝ³ᵥ, and let p be a point in M. Let (U, φ) be a chart in 𝔞 at p, let ℋ be the corresponding coordinate frame, and let q = φ⁻¹(p). Then:

(a)

[𝔥₁₁(q) 𝔥₁₂(q); 𝔥₂₁(q) 𝔥₂₂(q)] = [𝔤₁₁(q) 𝔤₁₂(q); 𝔤₂₁(q) 𝔤₂₂(q)] [𝔴₁¹(q) 𝔴₂¹(q); 𝔴₁²(q) 𝔴₂²(q)].

(b)

[𝒲ₚ]ℋ_q^ℋ_q = [𝔤¹¹(q) 𝔤¹²(q); 𝔤²¹(q) 𝔤²²(q)] [𝔥₁₁(q) 𝔥₁₂(q); 𝔥₂₁(q) 𝔥₂₂(q)].

Proof. (a): Let ℋ = (H₁, H₂). We have

𝔥ᵢⱼ(q) = ⟨𝒲ₚ(Hᵢ|q), Hⱼ|q⟩    [(12.8.6), (12.8.8)]
= ⟨Hᵢ|q, 𝒲ₚ(Hⱼ|q)⟩    [Th 12.8.2(b)]
= ⟨Hᵢ|q, Σₖ 𝔴ⱼᵏ(q) Hₖ|q⟩    [(12.8.4)]
= Σₖ 𝔤ᵢₖ(q) 𝔴ⱼᵏ(q),

from which the result follows.

(b): This follows from (12.8.5) and part (a).