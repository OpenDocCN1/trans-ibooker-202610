# Theorem 14.7.3. If M is a smooth manifold and p is a point in M, then

```
T_p(M) = { dλ/dt (t0) : λ(t) : (a,b) ⟶ M is smooth, λ(t0) = p, t0 ∈ (a,b) }.
```

Proof. This follows from Theorem 14.7.2 and (14.7.4).

Let M be a smooth manifold, let p be a point in M, and let v be a vector in T_p(M). We have from the preceding theorem that there is a smooth curve λ(t) : (a,b) ⟶ M such that λ(t0) = p and (dλ/dt)(t0) = v for some t0 in (a,b). Let X be a vector field in 𝔛(M). Then X_p is a vector in T_p(M), hence there corresponds such a smooth curve. We will make use of these observations frequently.

# Theorem 14.7.4. Let M and N be smooth manifolds, let F : M ⟶ N be a smooth map, let p be a point in M, and let v be a vector in T_p(M). Then:

+   (a)

```
d_p(F)(v) = d(F ∘ λ)/dt (t0),
```

where λ(t) : (a,b) ⟶ M is any smooth curve such that λ(t0) = p and (dλ/dt)(t0) = v for some t0 in (a,b).

+   (b) If ψ(t) : (a,b) ⟶ M is a smooth curve, then

```
d_{ψ(t)}(F)( dψ/dt (t) ) = d(F ∘ ψ)/dt (t)
```

for all t in (a,b).

Remark. Since F ∘ λ, F ∘ ψ : (a,b) ⟶ N are smooth curves, the assertions makes sense.

Proof. (a): For a function g in C^∞(N), we have

```
d_p(F)(v)(g) = v(g ∘ F)    [(14.4.1)]
= dλ/dt (t0)(g ∘ F)
= d(g ∘ F ∘ λ)/dt (t0)    [(14.7.5)]
= d(F ∘ λ)/dt (t0)(g).    [(14.7.5)]
```

Since g was arbitrary, the result follows.

(b): This follows from part (a).

# 14.8 Submanifolds

Having defined smooth manifolds, it is natural to consider subsets with corresponding properties. Let M̄ be a smooth m̄-manifold, and let M be a subset of