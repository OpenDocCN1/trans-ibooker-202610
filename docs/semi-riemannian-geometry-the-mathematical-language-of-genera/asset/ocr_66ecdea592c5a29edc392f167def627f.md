called the covariant derivative on λ, such that for all vector fields J, K in 𝔛_M(λ), all functions f in C^∞(M), all vector fields X in 𝔛(M), and all points t in (a,b):

(a)

∇(J+K)/dt (t) = ∇J/dt (t) + ∇K/dt (t).

(b)

∇(fJ)/dt (t) = df/dt (t) J(t) + f(t) ∇J/dt (t).

(c)

∇(X∘λ)/dt (t) = ∇_(dλ/dt)(t) (X).

Remark. The notation ∇_(dλ/dt)(t)(X) in part (c) is justified by the discussion surrounding (18.1.4).

# Theorem 18.3.2 (Local Coordinate Expression for Covariant Derivative on a Curve). Let (M,∇) be a smooth manifold with a connection, let λ(t):(a,b) ⟶ M be a smooth curve, and let J be a vector field in 𝔛_M(λ). Let (U,(x^i)) be a chart on M such that λ((a,b)) ⊂ U, and, in local coordinates, let

J(t) = Σ_i α^i(t) ∂/∂x^i |_{λ(t)}.

Then

∇J/dt (t) = Σ_k ( dα^k/dt (t) + Σ_{ij} d(x^i∘λ)/dt (t) α^j(t) Γ^k_{ij}(λ(t)) ) ∂/∂x^k |_{λ(t)}

for all t in (a,b).

Proof. Theorem 18.3.1 is used repeatedly in what follows. We have

∇/dt ( ∂/∂x^j |_{λ(t)} ) = ∇_(dλ/dt)(t) ( ∂/∂x^j ),

so

∇J/dt (t) = Σ_j ∇/dt ( α^j(t) ∂/∂x^j |_{λ(t)} ) = Σ_j dα^j/dt (t) ∂/∂x^j |_{λ(t)} + Σ_j α^j(t) ∇_(dλ/dt)(t) ( ∂/∂x^j ). (18.3.1)

By Theorem 14.7.1(b),

dλ/dt (t) = Σ_i d(x^i∘λ)/dt (t) ∂/∂x^i |_{λ(t)} ,