defined for s ≥ 1 by

```
d_p(F)^*(ℬ)(v₁, …, v_s) = ℬ(d_p(F)(v₁), …, d_p(F)(v_s))    (15.12.1)
```

for all tensors ℬ in 𝒯_s^0(T_{F(p)}(N)) and all vectors v₁, …, v_s in T_p(M). Pullback by F (for covariant tensor fields) is the family of linear maps

```
F^* : 𝒯_s^0(N) ⟶ 𝒯_s^0(M)
```

defined for s ≥ 1 by

```
F^*(𝒜)_p(v₁, …, v_s) = d_p(F)^*(𝒜_{F(p)})(v₁, …, v_s)
= 𝒜_{F(p)}(d_p(F)(v₁), …, d_p(F)(v_s))    (15.12.2)
```

for all tensor fields 𝒜 in 𝒯_s^0(N), all points p in M, and all vectors v₁, …, v_s in T_p(M), where the second equality follows from setting ℬ = 𝒜_{F(p)} in (15.12.1). We refer to F^*(𝒜) as the pullback of 𝒜 by F.

To give meaning to F^* when s = 0, recall from (15.6.3) that 𝒯_0^0(N) = C^∞(N). We therefore define

```
F^*(g)(p) = g(F(p)) = F^•(g)(p)
```

for all functions g in C^∞(N) and all p in M; that is, we define

```
F^* = F^•.    (15.12.3)
```

# Theorem 15.12.1. Let M, N, and P be smooth manifolds, let F : M ⟶ N and G : N ⟶ P be smooth maps, let 𝒜, ℬ and 𝒞 be tensor fields in 𝒯_s^0(N) and 𝒯_{s'}^0(N), respectively, and let g be a function in C^∞(N). Then:

+   (a) F^*(𝒜 + ℬ) = F^*(𝒜) + F^*(ℬ).
+   (b) F^*(g𝒜) = F^*(g)F^*(𝒜).
+   (c) F^*(𝒜 ⊗ 𝒞) = F^*(𝒜) ⊗ F^*(𝒞).
+   (d) (G ∘ F)^* = F^* ∘ G^*.

Since g ⊗ 𝒜 = g𝒜, part (b) of Theorem 15.12.1 follows from part (c). Identity (15.12.3) has several implications: Theorem 15.9.2 follows from Theorem 15.12.1(d); the identity in Theorem 15.10.4 can be expressed as

```
F^*(Y(g)) = F^*(Y)(F^*(g))
```

for all vector fields Y in 𝔛(N) and all functions g in C^∞(N); Theorem 15.11.1(b) follows from Theorem 15.12.1(b); and (15.11.4) can be expressed as

```
F^*(d(g)) = d(F^*(g))
```

for all functions g in C^∞(N).