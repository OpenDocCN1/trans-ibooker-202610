![](bbox=[0.04, 0.14, 0.92, 0.52])

# Figure 6.6 Orthogonal projection operator: orthogonal complement operation.

and

```
ρ( [ U_{0|k-1} -- X_0 ] ) = N_x + k N_u
```

leads to the rank condition

```
ρ( Y_{0|k-1} P_{U_{0|k-1}}^⊥ ) = ρ( O_K X_0 P_{U_{0|k-1}}^⊥ ) = N_x    (Rank Condition)
```

therefore, it follows immediately that

```
range( Y_{0|k-1} ) ≡ range( O_K )
```

the desired condition (see [7] or [9] for proof).

This “rank condition” provides a powerful result for the identification of the system model Σ_ABCD from deterministic input/output data just as the rank condition for impulse response data enabling the Hankel matrix factorization provides for the identification. It leads to pragmatic numerically efficient linear algebraic methods (to follow) providing a robust solution to solve the problem.

However, before we close this discussion, the system model must be extracted from the output data, which can be accomplished using a truncated SVD decomposition – similar to the method of Section 6.3.2, that is, the SVD