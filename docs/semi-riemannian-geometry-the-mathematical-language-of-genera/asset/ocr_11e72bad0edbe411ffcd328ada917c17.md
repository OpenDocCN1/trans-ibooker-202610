(c): Since ⋆(ω) is a form in Λ^{m−s}(M),

```
δ(⋆(ω)) = (−1)^{m(m−s+1)+1+ν} ⋆ d ⋆ (⋆(ω))      [(20.2.1)]
          = (−1)^{m(m−s+1)+1+ν} ⋆ d(⋆²(ω))
          = (−1)^{[m(m−s+1)+1+ν]+[s(m−s)+ν]} ⋆ d(ω).      [Th 20.1.1(c)]
```

Since

```
[m(m−s+1)+1+ν] + [s(m−s)+ν] = m(m+1) + 2ν + 1 − s²,
```

because m(m+1) and 2ν are even, and since s² is even (odd) precisely when s is even (odd),

```
(−1)^{[m(m−s+1)+1+ν]+[s(m−s)+ν]} = (−1)^{s+1}.
```

The result follows.

# Example 20.2.2 (Electrodynamics).

We continue with Example 16.1.10, except that ℝ⁴ is replaced with ℝ₁⁴. The covector field ξ and forms ζ and φ, now viewed as elements of 𝔛*(ℝ₁⁴) and Λ²(ℝ₁⁴), respectively, are given by

```
ξ = α₁ d(x) + α₂ d(y) + α₃ d(z),
```

```
ζ = β₁ d(y) ∧ d(z) + β₂ d(z) ∧ d(x) + β₃ d(x) ∧ d(y),
```

and

```
φ = ξ ∧ d(t) + ζ
   = α₁ d(x) ∧ d(t) + α₂ d(y) ∧ d(t) + α₃ d(z) ∧ d(t)
     + β₁ d(y) ∧ d(z) + β₂ d(z) ∧ d(x) + β₃ d(x) ∧ d(y).
```

We seek an expression for δ(φ). Setting m = 4, ν = 1, and s = 2 in (20.2.1) gives δ = ⋆ d ⋆. Using Example 8.5.6 yields

```
⋆(φ) = α₁ ⋆(d(x) ∧ d(t)) + α₂ ⋆(d(y) ∧ d(t)) + α₃ ⋆(d(z) ∧ d(t))
        + β₁ ⋆(d(y) ∧ d(z)) + β₂ ⋆(d(z) ∧ d(x)) + β₃ ⋆(d(x) ∧ d(y))
      = α₁ d(y) ∧ d(z) + α₂ d(z) ∧ d(x) + α₃ d(x) ∧ d(y)
        − [β₁ d(x) ∧ d(t) + β₂ d(y) ∧ d(t) + β₃ d(z) ∧ d(t)],
```

hence

```
d⋆(φ) = d(α₁) ∧ d(y) ∧ d(z) + d(α₂) ∧ d(z) ∧ d(x) + d(α₃) ∧ d(x) ∧ d(y)
         − [d(β₁) ∧ d(x) ∧ d(t) + d(β₂) ∧ d(y) ∧ d(t) + d(β₃) ∧ d(z) ∧ d(t)].
```