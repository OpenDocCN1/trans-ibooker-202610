It is easily shown that Π_{λ(0)}^{λ(2π)}((0, s₀, 0)) = (0, s₀, 0) if and only if θ₀ = π/2; that is, if and only if the image of λ lies on the “equator”. We therefore have the surprising finding that parallel translation of v along the entire trajectory of a latitude curve other than the equator does not send v to itself. This is a manifestation of the “curvature” of the sphere.

The intersection of S² and a plane through the origin is called a great circle of S², the equator being the obvious example. The parametrization we selected at the outset features the equator, but this was a matter of convenience. Due to the symmetry of S², any great circle would do.

# Theorem 18.5.5 (Diffeomorphism Invariance of Parallelism of a Curve).

Let (M, ∇) be a smooth manifold with a connection, let λ(t) : (a, b) ⟶ M be a smooth curve, and let g(u) : (c, d) ⟶ (a, b) be a diffeomorphism. Then the vector field J in 𝔛_M(λ) is parallel if and only if the vector field J ∘ g in 𝔛_M(λ ∘ g) is parallel.

Proof. This follows from Theorem 10.2.4(a) and Theorem 18.3.3(a).

# Theorem 18.5.6.

Let (M, ∇) be a smooth manifold with a connection, let λ : (a, b) ⟶ M be a smooth curve, and let t₀ and t be points in (a, b). Then Π_{λ(t₀)}^{λ(t)} : T_{λ(t₀)}(M) ⟶ T_{λ(t)}(M) is a linear isomorphism, and its inverse

(Π_{λ(t₀)}^{λ(t)})⁻¹ : T_{λ(t)}(M) ⟶ T_{λ(t₀)}(M)

is given by

(Π_{λ(t₀)}^{λ(t)})⁻¹(v) = Π_{λ(t)}^{λ(t₀)}(v)

for all vectors v in T_{λ(t)}(M).

Proof. Let v, w be vectors in T_{λ(t₀)}(M). By Theorem 18.5.1, there are parallel vector fields J, K in 𝔛_M(λ) such that J(t₀) = v and K(t₀) = w. For a real number c, we have

∇(cJ + K)/dt (t) = c ∇J/dt (t) + ∇K/dt (t) = 0

for all t in (a, b). Thus, cJ + K is a parallel vector field in 𝔛_M(λ) such that (cJ + K)(t₀) = cJ(t₀) + K(t₀) = cv + w.

It follows from the uniqueness property in Theorem 18.5.1 that Π_{λ(t₀)}^{λ(t)}(cv + w) = (cJ + K)(t) = cJ(t) + K(t) = c Π_{λ(t₀)}^{λ(t)}(v) + Π_{λ(t₀)}^{λ(t)}(w).

Thus, Π_{λ(t₀)}^{λ(t)} is linear.

Let v and w be vectors in T_{λ(t₀)}(M) and T_{λ(t)}(M), respectively. It follows from the properties of ordinary differential equations that w is the parallel translate of v if and only if v is the parallel translate of w. Thus, Π_{λ(t₀)}^{λ(t)} is a bijective map, and therefore a linear isomorphism, with inverse (Π_{λ(t₀)}^{λ(t)})⁻¹ = Π_{λ(t)}^{λ(t₀)}.