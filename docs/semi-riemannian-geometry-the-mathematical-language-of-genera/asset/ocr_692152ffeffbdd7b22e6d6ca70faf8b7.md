# 13.10 Hyperbolic Space in ℝ³₁

The set

```
ℋ² = {(x, y, z) ∈ ℝ³ : x² + y² − z² = −1, z > 0}
```

is the same upper sheet of the hyperboloid of two sheets described in Section 13.7, and just as before, it is a regular surface. Since

```
⎛∂f/∂x(x,y)⎞² + ⎛∂f/∂y(x,y)⎞² − 1 = − 1/(x² + y² − 1)
```

and, by definition, x² + y² > 1, the condition of Theorem 12.2.8 is satisfied. Thus, ℋ² is a regular surface in ℝ³₁. Alternatively, this comes from directly from Theorem 12.2.12(a). Theorem 12.10.2 gives:

```
H₁ = (1, 0, x/√(x² + y² + 1))    H₂ = (0, 1, y/√(x² + y² + 1))
```

```
𝒩 ∘ φ = (x, y, √(x² + y² + 1)) = φ
```

```
𝔤_ℋ = 1/(x² + y² + 1) [ y² + 1  −xy ; −xy  x² + 1 ]
```

```
𝔥_ℋ = −𝔤_ℋ
```

```
𝒦 ∘ φ = −1
```

```
ε_ℋ² = −1.
```

We note that ε_ℋ² = −1 agrees with Theorem 12.2.12(d). In the context of ℝ³₁, we refer to ℋ² as hyperbolic space.

We can also parametrize ℋ² as a surface of revolution. In the notation of Theorem 11.4.3, let

```
ρ(t) = sinh(t)
```

```
h(t) = cosh(t)
```

```
φ(t, φ) = (sinh(t) cos(φ), sinh(t) sin(φ), cosh(t))
```

```
U = (0, +∞) × (−π, π).
```

From the well-known identity sinh²(t) − cosh²(t) = −1, we obtain ρ̇(t)² − ḣ(t)² = 1. Evidently, the condition of Theorem 12.2.9 is satisfied, so once again we see that ℋ² is a regular surface in ℝ³₁. Theorem 12.10.4 gives:

```
H₁ = (cosh(t) cos(φ), cosh(t) sin(φ), sinh(t))
```

```
H₂ = (−sinh(t) sin(φ), sinh(t) cos(φ), 0)
```

```
𝒩 ∘ φ = (sinh(t) cos(φ), sinh(t) sin(φ), cosh(t)) = φ
```