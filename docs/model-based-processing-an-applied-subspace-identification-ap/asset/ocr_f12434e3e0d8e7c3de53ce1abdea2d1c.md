That is, attempts to construct all realizations with identical covariance sequences by the choice of all possible positive definite matrices Π will generally not succeed because all Πs do not correspond to the required set {R_ww, R_ww, R_vv} satisfying the positive semidefinite property of Eq. (7.19).

However, it has been shown that if the measurement PSD is positive semidefinite on the unit circle and if the underlying GM system is completely observable, then the Λ ≥ 0 admits a factorization, that is,

```
Λ = [ W ; - ; V ] [W' | V'] ≥ 0  for  S_yy(e^{jΩ}) ≥ 0;  (A, C)  (Observable)
```

(7.20)

Thus, the stochastic realization problem always has a solution when there is a Π ≥ 0 such that Λ ≥ 0 and is guaranteed from the fact that the power spectrum is positive semidefinite (S_yy(z)|_{z=e^{jΩ}} ≥ 0) on the unit circle (see [28, 29] for proof). This result is available in the generalized KSP lemma [15] ensuring that at least one solution to the stochastic realization problem exists, that is, there exists spectral factorization such that

```
S_yy(z) = ([C(zI - A)^{-1} | I_{N_y}] [ W ; - ; V ] ) ( [W' | V'] [ (z^{-1}I - A')^{-1}C' ; - - - ; I_{N_y} ] ) ≥ 0
```

(7.21)

which leads to

```
S_yy(z) = [C(zI - A)^{-1}W + V]_{H_y(z)} × [W'(z^{-1}I - A')^{-1}C' + V']_{H'_y(z^{-1})}
```

(7.22)

or simply

```
S_yy(z) = H_y(z) × H'_y(z^{-1})          (Spectral Factors)    (7.23)
```

With this in mind, and the fact that the Markov parameters correspond to the covariance sequence, then theoretically, if we create a Hankel matrix from the measurement covariance sequence {Λ_ℓ}; ℓ = 0, 1, ..., ∞ of Eq. (7.18) and perform a deterministic realization as in the previous chapter, we obtain the so-called KSP model defined by Σ_KSP := {A, B, C, D} with corresponding transfer function T_KSP(z) = C(zI - A)^{-1}B + D such that the measurement power spectrum can be expressed as a sum decomposition:

```
S_yy(z) = S^+(z) + S^-(z^{-1}) - Λ_0 = C(zI - A)^{-1}B + D + D' + B'(Z^{-1}I - A')^{-1}C'
```

(7.24)