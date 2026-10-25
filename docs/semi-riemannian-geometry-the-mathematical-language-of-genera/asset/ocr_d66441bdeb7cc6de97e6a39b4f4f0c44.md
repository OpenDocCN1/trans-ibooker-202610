(b) If ∇ is symmetric, then J is the unique Jacobi field in 𝔛_M(ρ_{p,v}) such that J(0) = 0 and (∇J/dr)(0) = w.

Remark. That ρ_{p,v}(r) = exp_p(rv) follows from Theorem 18.9.3. By Theorem 18.11.4(b), σ is a geodesic variation of ρ_{p,v}. We have from (18.9.4) that the differential map of exp_p : ℰ_p ⟶ M at rv is

d_{rv}(exp_p) : T_{rv}(T_p(M)) ⟶ T_{exp_p(rv)}(M).

It would be more precise in part (a) to write J(r) = r d_{rv}(exp_p)(w_{rv}). Stating part (a) as above amounts to making the identification T_{rv}(T_p(M)) = T_p(M).

Proof. (a): Setting ψ(s) = v + sw in Theorem 18.11.4(d) yields

∂σ/∂s (r, s) = r d_{r(v+sw)}(exp_p)(w).

Setting s = 0 in the preceding identity and using (18.11.3) gives the result.

(b): Since ∇ is symmetric, it follows from Theorem 18.11.3 and part (a) that J is a Jacobi field in 𝔛_M(ρ_{p,v}). From the identity in part (a), we obtain J(0) = 0 and

∇J/dr (r) = d_{rv}(exp_p)(w) + r ∇/dr (d_{rv}(exp_p)(w)).

By Theorem 18.9.4, d_0(exp_p) is the identity diffeomorphism. Setting r = 0 in the preceding identity yields (∇J/dr)(0) = w. The uniqueness property of J now follows from Theorem 18.11.1.

# Example 18.11.6. Let (M, ∇) be a smooth manifold with a connection, and let γ(r) : [0,1] ⟶ M be a (radial) geodesic. Consider the vector fields J and K in 𝔛_M(γ) defined by

J(r) = dγ/dr (r)      and      K(r) = r dγ/dr (r).

We have

∇/dr (dγ/dr) = 0      and      R∘γ (dγ/dr, dγ/dr) dγ/dr = 0,

where the second identity follows from Theorem 18.7.1(a). It is now easily shown that J and K are Jacobi fields.