Using (18.7.9), the term in the last row of (18.7.12) can be expressed as

```
∑_j ∂(x^j ∘ σ)/∂s ∇_{∂σ/∂r} (∇_{∂/∂x^j} (∂/∂x^k)) = ∑_j ∂(x^j ∘ σ)/∂s [∑_i ∂(x^i ∘ σ)/∂r ∇_{(∂/∂x^i)∘σ} (∇_{∂/∂x^j} (∂/∂x^k))] = ∑_{ij} ∂(x^i ∘ σ)/∂r ∂(x^j ∘ σ)/∂s ∇_{(∂/∂x^i)∘σ} (∇_{(∂/∂x^j)∘σ} (∂/∂x^k)).
```

Then (18.7.12) becomes

```
∇_{∂/∂r} (∇_{∂/∂s} (∂/∂x^k ∘ σ)) = ∑_j ∂²x^j/∂r∂s ∇_{(∂/∂x^j)∘σ} (∂/∂x^k) + ∑_{ij} ∂(x^i ∘ σ)/∂r ∂(x^j ∘ σ)/∂s ∇_{(∂/∂x^i)∘σ} (∇_{∂/∂x^j} (∂/∂x^k)).
```

Interchanging r and s in the preceding identity and using Theorem 10.1.6 yields

```
∇_{∂/∂s} (∇_{∂/∂r} (∂/∂x^k ∘ σ)) = ∑_j ∂²x^j/∂s∂r ∇_{(∂/∂x^j)∘σ} (∂/∂x^k) + ∑_{ij} ∂(x^i ∘ σ)/∂s ∂(x^j ∘ σ)/∂r ∇_{(∂/∂x^i)∘σ} (∇_{∂/∂x^j} (∂/∂x^k)) = ∑_j ∂²x^j/∂r∂s ∇_{(∂/∂x^j)∘σ} (∂/∂x^k) + ∑_{ij} ∂(x^i ∘ σ)/∂r ∂(x^j ∘ σ)/∂s ∇_{(∂/∂x^j)∘σ} (∇_{∂/∂x^i} (∂/∂x^k)).
```

Subtracting the preceding two identities gives

```
∇_{∂/∂r} (∇_{∂/∂s} (∂/∂x^k ∘ σ)) - ∇_{∂/∂s} (∇_{∂/∂r} (∂/∂x^k ∘ σ)) = ∑_{ij} ∂(x^i ∘ σ)/∂r ∂(x^j ∘ σ)/∂s [∇_{(∂/∂x^i)∘σ} (∇_{∂/∂x^j} (∂/∂x^k)) - ∇_{(∂/∂x^j)∘σ} (∇_{∂/∂x^i} (∂/∂x^k))]  (18.7.13)
```

which is the desired expression for the right-hand side of (18.7.8).