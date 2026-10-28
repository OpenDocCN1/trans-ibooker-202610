theoretic properties in mind, we can now define the deterministic realization problem.

The fundamental problem of extracting the state-space system Σ_ABCD is called the realization problem. Simply stated, the deterministic realization problem is GIVEN a transfer function matrix H(z) or equivalently a set of impulse response matrices {H_t} with corresponding Markov parameters, H_t = CA^{t-1}B + Dδ(t); t = 0, …, ∞, FIND the underlying minimal state-space system, Σ_ABCD := {A, B, C, D}.

These properties evolve completely from the development of the Hankel matrix of systems theory [2], which plays an intrinsic role in the solution of the deterministic realization problem. Note that the Hankel matrix is to state-space identification as the Toeplitz matrix is to signal processing identification (e.g. spectral estimation [11]). Note that they are related through an exchange matrix that is cross diagonal or antidiagonal made up of ones in the scalar case and identity matrices in the multivariable case [9].

We define the (K × N_y N_u) × (K × N_y N_u) block Hankel matrix (K → ∞) by

```
ℋ_{K,K} := [ H₁  H₂  ⋯  H_K ; H₂  H₃  ⋯  H_{K+1} ; ⋮   ⋮   ⋯   ⋮ ; H_K  H_{K+1} ⋯ H_{2K-1} ] (6.5)
```

If Σ_ABCD is a realization of {H_t}, then based on the Markov parameters, the Hankel matrix can be factored as the product of the controllability and observability matrices:

```
ℋ_{K,K} = [ CB  CAB  ⋯ CA^{K-1}B ; CAB  CA²B  ⋯ CA^K B ; ⋮   ⋮   ⋯   ⋮ ; CA^{K-1}B  CA^{K-2}B ⋯ CA^{2K-2}B ] = [ C ; CA ; ⋮ ; CA^{K-1} ] [ B | AB | ⋯ | A^{K-1}B ] (6.6)
```

or simply

```
ℋ_{K,K} = 𝒪_K × 𝒞_K (6.7)
```

Another interesting property of the Hankel matrix utilized by realization algorithms is its inherent shift-invariant property, that is, multiplication by the