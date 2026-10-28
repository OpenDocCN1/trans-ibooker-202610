
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

where $S^{+}(z) = \sum_{j=0}^{\infty} \Lambda_{j} z^{-j}$. Therefore, from Eq. (7.18), we have a solution (but difficult) to the stochastic realization problem using spectral factorization techniques by

$S_{yy}(z) = H_{y}(z) \times H_{y}'(z^{-1}) = S^{+}(z) + S^{-}(z^{-1}) - \Lambda_{0}$ \tag{7.25}

performing the sum decomposition and then the factorization.

The relationship between this realization of the covariance sequence and the stochastic realization is defined by the so-called KSP equations obtained directly from deterministic realization theory or by equating the spectral factorization of Eq. (7.21) to the sum decomposition of Eq. (7.24) (see [1, 4, 15] for proof), since

$\Lambda_{\ell} = \begin{cases} \underbrace{C \underbrace{A^{\ell-1}}_{\hat{A}} \underbrace{(A\Pi C' + R_{ww})}_{\hat{B}}}_{\hat{C}} \quad \text{for } \ell > 0 \\ \underbrace{C\Pi C' + R_{vv}}_{\hat{D}+D'} \quad \text{for } \ell = 0 \end{cases}$ \tag{7.26}

where $\hat{\Sigma}_{\text{KSP}} = \{\hat{A}, \hat{B}, \hat{C}, \hat{D}\}$ is the underlying KSP model.

This relationship can be stated formally as follows:
GIVEN a minimal realization $\hat{\Sigma}_{\text{KSP}}$ of $\{\Lambda_{j}\}$, THEN the model set $\{\hat{A}, \hat{B}, \hat{C}, \hat{D}, \Pi, R_{ww}, R_{wv}, R_{vv}\}$ is a stochastic realization of the measurement sequence, if there exists a $\Pi \ge 0$ such that the following KSP equations are satisfied (see [15, 30] for proof):

$\Pi - \hat{A}\,\Pi\,\hat{A}' = R_{ww}$
$\hat{D} + D' - \hat{C}\,\Pi\,\hat{C}' = R_{vv}$
$\hat{B} - \hat{A}\,\Pi\,\hat{C}' = R_{wv}$ \tag{7.27}

The key, therefore, is to investigate just how to determine such a $\Pi$ !

This completes the section on classical stochastic realization theory. Next we investigate a variety of ways that can be applied to extract a realization from noisy measurement data.

## 7.3 Classical Stochastic Realization via the Riccati Equation

The classical approach to solve the stochastic realization problem evolves directly from the basic theory required to ensure that such a realization satisfies the positivity constraints imposed by the multichannel power spectrum

symmetric 𝒱 ∈ ℝ^{N_y×N_y} such that 𝒱 ≥ 0 leads to the following unique solution of the KSP equations:

Π* − A Π* A′ = 𝒲 (𝒱 𝒱′) 𝒲′
D + D′ − C Π* C′ = (𝒱 𝒱′)
B − A Π* C′ = 𝒲 (𝒱 𝒱′)
(KSP Equations) (7.32)

Solving for 𝒲 in the last KSP equation of Eq. (7.32) and substituting the second equation for the (𝒱 𝒱′) term yields

𝒲 = (B − A Π* C′) × (D + D′ − C Π* C′)^{−1} (7.33)

Substituting these expressions into the first equation of Eq. (7.32) shows that Π* satisfies a discrete Riccati equation, that is,

Π* = A Π* A′ + (B − A Π* C′)(D + D′ − C Π* C′)^{−1}(B − A Π* C′)′
(Riccati Equation) (7.34)

A realization obtained in this manner is guaranteed to be a stochastic realization, but with the computational expense of solving the discrete Riccati equation. Note that solutions of the Riccati equation are well known and have demonstrated that a unique Π* exists yielding a stable, minimum-phase spectral factor (e.g. see [1, 4, 5, 16] for details). An iterative approach to solving the Riccati equation using the KSP equations directly is summarized in Table 7.1.

Once the Σ̂_KSP model is estimated from the covariance sequence using a classical realization algorithm, then the KSP equations can be developed and used to solve the Riccati equation providing Π* ≥ 0 as its unique, positive definite solution.

Π* = Â Π* Â′ + (B̂ − Â Π* Ĉ′)(D̂ + D̂′ − Ĉ Π* Ĉ′)^{−1}(B̂ − Â Π* Ĉ′)′ (7.35)

### Table 7.1 Iterative KSP algorithm (Riccati equation).

Covariance
(𝒱 𝒱′)(i) = D + D′ − C Π̂*(i) C′
Gain
𝒲(i) = (B − A Π̂*(i) C′)(𝒱 𝒱′)^{−1}(i)
Estimated covariance
Π̂*(i + 1) = A Π̂*(i) A′ + 𝒲(i)(𝒱 𝒱′)(i) 𝒲′(i)
Initial conditions
Π̂*(0)
Stopping rule
|I − Π̂*(i) × (Π̂*)^{−1}(i + 1)| ≤ ε for ε ≪ 1

With the Π* available, we can then use Σ̂_KSP to obtain

(𝒱 𝒱') = D̂ + D̂' − Ĉ Π* Ĉ'
Ŵ = (B̂ − Â Π* Ĉ') × (𝒱 𝒱')⁻¹ (7.36)

and therefore the source covariances are given by

R_ww = 𝒲 (𝒱 𝒱') 𝒲'
R_vv = (𝒱 𝒱')
R_wv = 𝒲 (𝒱 𝒱') (7.37)

We summarize this approach as follows:

- Perform a realization (deterministic) to obtain Σ̂_KSP.
- Solve the Riccati equation of Eq. (7.35) to obtain Π*.
- Calculate 𝒱𝒱' and 𝒲 from the KSP equations of Eq. (7.36).
- Determine the required set of covariances {R_ww, R_vv, R_wv} from Eq. (7.37).

### Table 7.2 Stochastic realization algorithm: Riccati equation approach.

Deterministic realization (Ho–Kalman (SVD) approach)
Λ_{K,K} = 𝒪_K × C̄_K = (U_{N_s} Σ_{N_s}^{1/2}) × (Σ_{N_s}^{1/2} V_{N_s}') (Observability/Controllability Matrices)
KSP system matrix
A = 𝒪_{N_s}^† × 𝒪_{N_s}^↓ = (𝒪_{N_s}' 𝒪_{N_s})⁻¹ 𝒪_{N_s}' × 𝒪_{N_s}^† = Σ_{N_s}^{-1/2} U_{N_s}' × 𝒪_{N_s}^† (System/Process Matrix)
KSP measurement (output) matrix
Ĉ = 𝒪(1 : N_y, 1 : N_x) (Output Matrix)
KSP input transmission matrix
B̂ = C̄(1 : N_x, 1 : N_u) (Input Matrix)
KSP input/output transmission matrix
D̂ + D̂' = Λ_0 (Input–Matrix)
State covariance equation
Π* = Â Π* Â' + (B̂ − Â Π* Ĉ')(D̂ + D̂' − Ĉ Π* Ĉ')⁻¹(B̂ − Â Π* Ĉ')' (Riccati Equation)
KSP equations
(𝒱 𝒱') = D̂ + D̂' − Ĉ Π* Ĉ'
𝒲 = (B̂ − Â Π* Ĉ') × (𝒱 𝒱')⁻¹ (KSP Equations)
Covariance-equations
R_ww = 𝒲 (𝒱 𝒱') 𝒲'
R_vv = (𝒱 𝒱')
R_wv = 𝒲 (𝒱 𝒱') (Covariances)

So we see that performing a deterministic realization from the Hankel matrix populated by the covariance sequence {Λ_r} yields the model Σ̂_KSP that is used to estimate Π* from the corresponding Riccati equation enabling the determination of the set of covariances {R_ww, R_vv, R_wv} from the KSP relations. We summarize the Riccati stochastic realization algorithm in Table 7.2.

This completes the section on the classical approach to solve the stochastic realization problem. However, before we leave this discussion, let us consider an alternative, yet equivalent, way to solve the problem using the well-known Kalman filter based on the underlying INV model developed in Chapter 4.

## 7.4 Classical Stochastic Realization via Kalman Filter

In this section, we present a special case of the Riccati equation solution to solve the stochastic realization problem. This approach evolves from the factorization theorem of Chapter 4 that led to the Wiener/Kalman filter equivalence resulting in the unique, steady-state solution of the Riccati equation. It is well known [5] that this steady-state solution uniquely specifies the optimal Kalman gain. The significance of obtaining a stochastic realization via the Kalman filter is twofold. First, since the Kalman gain is unique (modulo similarity transformations), so is the corresponding stochastic realization. Second, knowledge of this gain specifies a stable, minimum-phase (zeros within the unit circle), multichannel spectral factor [1, 16]. The importance of this approach compared to the previous one is that once the gain is determined, a stochastic realization is guaranteed immediately. However, the price paid for a so elegant solution is again the computational burden of solving a Riccati equation.

### 7.4.1 Innovations Model

The basic state estimation problem is to find the minimum variance estimate of the state vector of the GM model in terms of the currently available measurement sequence y(t). We start with the innovations (INV) model rather than the Gauss–Markov model as before, since it is, in fact, equivalent to the steady-state Kalman filter for stationary processes. That is, the INV representation of the Kalman filter in “prediction form” is given by (see [32] for details)

```
x̂(t + 1) = A x̂(t) + K_p(t) e(t)                     (State Estimate)
y(t) = C x̂(t) + e(t)                                 (Measurement)
ŷ(t) = C x̂(t)                                        (Measurement Estimate)
e(t) = y(t) − ŷ(t)                                   (Innovation)   (7.38)
```
