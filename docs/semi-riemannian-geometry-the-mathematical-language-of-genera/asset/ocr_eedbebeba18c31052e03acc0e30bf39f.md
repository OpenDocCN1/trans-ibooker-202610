Proof. Consider the diffeomorphism g(u) : (a/ε, b/ε) ⟶ (a, b) given by g(u) = εu. By Theorem 18.8.5, γ_{p,v} ∘ g(u) : (a/ε, b/ε) ⟶ M is a geodesic. Since γ_{p,v} is maximal, so is γ_{p,v} ∘ g, otherwise γ_{p,v} could be extended. We have from Theorem 18.8.2(a) that γ_{p,v} ∘ g(0) = γ_{p,v}(0) = p, and from Theorem 14.4.5 and Theorem 18.8.2(a) that

```
d(γ_{p,v} ∘ g)/du (0) = dg/du (0) dγ_{p,v}/dt (g(0)) = ε dγ_{p,v}/dt (0) = εv.
```

It follows from Theorem 18.8.2 that γ_{p,εv} = γ_{p,v} ∘ g. Thus, γ_{p,εv}(u) = γ_{p,v}(εu) for all u in (a/ε, b/ε).

# Example 18.8.7 (Smooth 2-Manifold).

For smooth 2-manifolds with a symmetric connection, the geodesic equations in Theorem 18.8.1 simplify to

```
d²(x¹ ∘ λ)/dt² (t) + (d(x¹ ∘ λ)/dt (t))² Γ¹₁₁(λ(t)) + 2 d(x¹ ∘ λ)/dt (t) d(x² ∘ λ)/dt (t) Γ¹₁₂(λ(t)) + (d(x² ∘ λ)/dt (t))² Γ¹₂₂(λ(t)) = 0
```

and

```
d²(x² ∘ λ)/dt² (t) + (d(x¹ ∘ λ)/dt (t))² Γ²₁₁(λ(t)) + 2 d(x¹ ∘ λ)/dt (t) d(x² ∘ λ)/dt (t) Γ²₁₂(λ(t)) + (d(x² ∘ λ)/dt (t))² Γ²₂₂(λ(t)) = 0.
```

Viewing a regular surface as a smooth 2-manifold with the connection defined in Example 18.1.1, we have from Example 18.6.1 that the connection is symmetric. Thus, the above geodesic equations apply.

# Example 18.8.8 (Pln).

Let us view the regular surface Pln, discussed in Section 13.1, as a smooth 2-manifold with the connection defined in Example 18.1.1. Suppose γ(t) = (γ¹, γ²) : ℝ ⟶ ℝ² is a geodesic. Since the Christoffel symbols have constant value 0, by Theorem 18.8.1, the geodesic equations are (d²γᵏ/dt²)(t) = 0 for k = 1,2. These have the solutions γᵏ(t) = aᵏ + bᵏ t for any choice of real numbers a¹, b¹, a², b². Thus,

```
γ(t) = (a¹, a²) + t(b¹, b²).
```

Not surprisingly, the image of γ is a straight line. This demonstrates that the maximal geodesics in Pln with a given starting point are the straight lines through that point.