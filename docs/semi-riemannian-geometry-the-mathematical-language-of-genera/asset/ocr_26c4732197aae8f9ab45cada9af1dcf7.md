We observed in Section 12.3 that the Christoffel symbols are intrinsic. It follows from (12.9.2) and (12.9.3) that the same is true of the Riemann curvature tensor.

# Example 12.9.3 (S²).

Substituting into (12.9.3) the values of the Christoffel symbols for S² given in Section 13.4 yields

R¹₁₂₂ = sin²(θ)    R¹₂₁₂ = −sin²(θ)
R²₁₂₁ = −1        R²₂₁₁ = 1,

with the remaining Rˡᵢⱼₖ equal to 0. Observe that the Rˡᵢⱼₖ are independent of φ, as would be expected due to the symmetry of the sphere.

It is a remarkable feature of (12.9.2) that no partial derivatives of the component functions appear in the expression. This crucial observation underlies the next two results.

# Theorem 12.9.4.

If (M,𝔤) is a regular surface in ℝ³_ν, then R is determined pointwise on M in the following sense: for all points p in M, if X,Y,Z and X̃,Ỹ,Z̃ are vector fields in 𝔛(M) such that

(X_p, Y_p, Z_p) = (X̃_p, Ỹ_p, Z̃_p),

then

(R(X,Y)Z)_p = (R(X̃,Ỹ)Z̃)_p.

Proof. In local coordinates, let

X ∘ φ = Σ_i α^i H_i    and    X̃ ∘ φ = Σ_i α̃^i H_i.

Then X_p = X̃_p translates into α^i(p) = α̃^i(p) for i = 1,2, with corresponding identities for the other two vector fields. The result now follows from (12.9.2).

# Theorem 12.9.5.

If (M,𝔤) is a regular surface in ℝ³_ν, then R is C^∞(M)-multilinear in the following sense: for all vectors fields X,Y,Z,W in 𝔛(M) and all functions f in C^∞(M),

R(fX + W, Y)Z = f R(X,Y)Z + R(W,Y)Z,

with corresponding identities for the other two arguments of R.

Proof. This follows from reasoning similar to that used in the proof of Theorem 12.9.4.

# Theorem 12.9.6.

Let (M,𝔤,𝒪) be an oriented regular surface in ℝ³_ν, let 𝒩 be the Gauss map, and let X,Y be vector fields in 𝔛(M). Then:

(a)

⟨D_X(Y), 𝒩⟩ = ⟨𝒲(X), Y⟩.