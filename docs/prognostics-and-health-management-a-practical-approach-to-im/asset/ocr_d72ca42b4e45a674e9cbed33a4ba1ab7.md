and its logarithm is

```
l(θ₁, ..., θₙ) = Σₖ₌₁ᴺ ln(f(tₖ | θ₁, ..., θₙ))
```

The Fisher information is now a matrix, the negative Hessian of l(θ₁, ..., θₙ):

```
I(θ₁, ..., θₙ) = ( -∂²l/∂θ₁²  -∂²l/∂θ₁∂θ₂  ...  -∂²l/∂θ₁∂θₙ ; -∂²l/∂θ₂∂θ₁  -∂²l/∂θ₂²  ...  -∂²l/∂θ₂∂θₙ ; ⋮  ⋮  ⋱  ⋮ ; -∂²l/∂θₙ∂θ₁  -∂²l/∂θₙ∂θ₂  ...  -∂²l/∂θₙ² )
```

It can be shown that the inverse matrix of I(θ₁, ..., θₙ) gives the covariance matrix of (θ₁, ..., θₙ).

# Example 1.11 Let X now be a normal variable with unknown parameters μ and σ². From Example 1.9, we know that

```
l(μ, V) = -N/2 ln(V) - N ln(√2π) - 1/(2V) Σₖ₌₁ᴺ (tₖ - μ)²
```

where we use the simplifying notation V = σ². Notice that

```
∂l/∂μ = -1/(2V) Σₖ₌₁ᴺ 2(tₖ - μ)(-1) = 1/V Σₖ₌₁ᴺ (tₖ - N μ)
```

The first order condition implies that the likelihood estimate of μ is

```
μ * = 1/N Σₖ₌₁ᴺ tₖ = t̄
```

Similarly

```
∂l/∂V = -N/(2V) + 1/(2V²) Σₖ₌₁ᴺ (tₖ - μ)²
```