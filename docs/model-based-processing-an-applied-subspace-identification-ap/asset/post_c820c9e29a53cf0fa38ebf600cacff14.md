
where $e(t)$ is the innovations sequence and $K_p(t)$ is the predicted Kalman gain for correlated noise sources $\text{cov}(w, v)$ with state error covariance $\tilde{P}(t)$ given by

$R_{ee}(t) = C\tilde{P}(t)C' + R_{vv}(t)$  (Innovations Covariance)
$K_p(t) = (A\tilde{P}(t)C' + R_{wv}(t))R_{ee}^{-1}(t)$  (Kalman Gain)
$\tilde{P}(t+1) = A\tilde{P}(t)A' - K_p(t)R_{ee}(t)K_p'(t) + R_{ww}(t)$  (Error Covariance)
(7.39)

and we have used the simplified notation: $\hat{x}(t+1|t) \rightarrow \hat{x}(t+1)$ and $\tilde{P}(t+1|t) \rightarrow \tilde{P}(t+1)$.

## 7.4.2 Innovations Power Spectrum

The “transfer function” of the INV model is defined as

$T(z) := \frac{Y(z)}{E(Z)} = C(zI - A)^{-1}K_p$  (7.40)

Calculating the measurement covariance corresponding to $\Sigma_{INV}$ when

$y(t) = e(t) + \hat{y}(t)$

gives

$R_{yy}(\ell) = \text{cov}[y(t+\ell)y(t)] = R_{\hat{y}\hat{y}}(\ell) + R_{\hat{y}e}(\ell) + R_{e\hat{y}}(\ell) + R_{ee}(\ell)$  (7.41)

Taking the $\mathcal{Z}$-transform of this relation, we obtain the measurement PSD in terms of the INV model as

$S_{yy}(z) = S_{\hat{y}\hat{y}}(z) + S_{\hat{y}e}(z) + S_{e\hat{y}}(z) + S_{ee}(z)$  (7.42)

Using the linear system relations of Chapter 2 with

$y(t) = Cx(t) + v(t)$

we see that

$S_{yy}(z) = C S_{\hat{x}\hat{x}}(z) C' = T(z) S_{ee}(z) T'(z^{-1}); \quad S_{ee}(z) = R_{ee}$
$S_{\hat{y}e}(z) = C S_{xe}(z) = T(z) S_{ee}(z) \quad \text{and} \quad S_{e\hat{y}}(z) = S_{ee}(z) T'(z^{-1})$  (7.43)

Thus, the measurement PSD of the INV model is given by

$S_{yy}(z) = T(z) S_{ee}(z) T'(z^{-1}) + T(z) S_{\hat{y}e}(z) + S_{e\hat{y}}(z) T'(z^{-1}) + S_{ee}(z)$  (7.44)

Substituting for $T(z)$ and replacing $S_{ee}(z) = R_{ee}$, the measurement power spectrum can be expressed as

$S_{yy}(z) = \left[ C(zI - A)^{-1} \mid I_{N_y} \right] \begin{bmatrix} K_p R_{ee} K_p' & K_p R_{ee} \\ --- & --- \\ (K_p R_{ee})' & R_{ee} \end{bmatrix} \begin{bmatrix} (z^{-1}I - A')^{-1} C' \\ --- \\ I_{N_y} \end{bmatrix}$  (7.45)

Since $R_{ee} \ge 0$, then the following square-root factorization (Cholesky decomposition) always exists as

$$R_{ee}=R_{ee}^{1/2}(R_{ee}^{1/2})',$$

and therefore Eq. (7.45) admits the following unique spectral factorization (Wiener solution):

$$\begin{aligned}
S_{yy}(z)={}&[C(zI-A)^{-1}K_p\mid I_{N_y}]
\begin{bmatrix}R_{ee}^{1/2}\\--\\R_{ee}^{1/2}\end{bmatrix}
\left[(R_{ee}^{1/2})'\mid(R_{ee}^{1/2})'\right]
\begin{bmatrix}K_p'(z^{-1}I-A')C'\\--\\I_{N_y}\end{bmatrix}\\
={}&[C(zI-A)^{-1}K_pR_{ee}^{1/2}+R_{ee}^{1/2}]
\left[(R_{ee}^{1/2})'K_p'(z^{-1}I-A')C'+(R_{ee}^{1/2})'\right]
\end{aligned}$$

$$T_e(z) \qquad\qquad\qquad T_e'(z^{-1})$$

Thus, using Eq. (7.46) and then Eq. (7.44), we have

$$S_{yy}(z)=T_e(z)\times T_e'(z^{-1}) \qquad\text{(Spectral Factors)}$$

which shows that the INV model indeed admits a proper spectral factorization. To demonstrate that $T_e(z)$ is the unique, stable, minimum-phase spectral factor, it is necessary to show that $|T_e(z)|$ has all its poles within the unit circle (stable). It has been shown (e.g. [1, 4, 33]) that $T_e(z)$ does satisfy these constraints and therefore is in fact the *Wiener solution* in the stationary case. Next, we must show how the INV model (Kalman filter) relates to the Markov parameters in order to specify a stochastic realization. That is, we must show the relationship of $\Sigma_{\mathrm{INV}}$ to the corresponding set of KSP parameters.

## 7.4.3 Innovations Measurement Covariance

In our usual model development, we start with the Gauss–Markov representation given by the model set $\Sigma_{\mathrm{GM}} := \{A,B,C,D,R_{ww},R_{vv},R_{wv},x(0),\mathcal{P}(0)\}$, and a variety of state-space problems are defined within this framework (e.g. state estimation). The same problems can be defined in terms of this model. Therefore, the model set for the INV model is defined as $\Sigma_{\mathrm{INV}} := \{A,B,C,D,K_p,R_{ee},x(0),\mathcal{P}(0)\}$. The only problem here is that we are usually “not” given $\Sigma_{\mathrm{INV}}$, but $\Sigma_{\mathrm{GM}}$. From the covariance equivalence of these models, that is, both satisfy the measurement covariance relation, $\Lambda_{\mathrm{GM}}=\Lambda_{\mathrm{INV}}$, it is easy to show that if we are given $\Sigma_{\mathrm{GM}}$ and we want to obtain $\Sigma_{\mathrm{INV}}$, then we must develop the equivalence relations between the parameters of the model sets.

This solution is given by the set of KSP equations in terms of the INV model. The KSP equations can be solved iteratively to obtain $(K_p,R_{ee})$ by directly implementing the INV model for the *time-invariant* case [32]. Of course, another

approach is simply to execute the steady-state Kalman filter algorithm in prediction form that is equivalent to the KSP equations with Σ_KSP known.

Following the same development as in Section 7.2.4 with the measurement covariance given by the following using the INV model rather than the GM model:

Λ_yy(t, τ) = E { ( CA^{t-τ} x̂(τ) + C Σ_{n=τ}^{t-1} A^{t-n-1} K_p(n) e(n) + e(t) ) × ( C x̂(τ) + e(τ) )' } (7.49)

Expanding this expression and noting that the state and INV are uncorrelated (E{ x e' } = E{ e x' } = 0), therefore, we obtain

Λ_yy(ℓ) = C A^{t-τ} E{ x̂(τ) x̂'(τ) } C' + C Σ_{n=τ}^{t-1} A^{t-n-1} K_p(n) E{ e(n) e'(τ) } + E{ e(t) e'(τ) } (7.50)

Applying the whiteness property of the INV, that is, R_ee(t, τ) = R_ee(τ) δ(t - τ)

and incorporating the shifting property of the Kronecker delta function, we obtain

Λ_yy(t, τ) = { C A^{t-τ} Π̃(τ) C' + C A^{t-τ-1} K_p(τ) R_ee(τ)  t > τ ; C Π̃(τ) C' + R_ee(τ)  t = τ }

For the stationary case with lag ℓ, we have t → τ + ℓ and Λ_yy(t, τ) → Λ_yy(ℓ), as before, so that for t > τ,

Λ_yy(ℓ) = C A^{ℓ} Π̃(ℓ) C' + C A^{ℓ-1} K_p R_ee(ℓ) = C A^{ℓ-1} ( A Π̃(ℓ) C' + K_p(ℓ) R_ee(ℓ) )

This relationship between this realization of the covariance sequence and the stochastic realization is defined by the KSP equations obtained directly from deterministic realization theory or can also be derived by equating the spectral factorization to the sum decomposition [4]. The solution can be stated formally as

GIVEN a minimal realization Σ̃_KSP of {Λ_ℓ}, THEN the model set {Ã, B̃, C̃, D̃, Π̃, R^e_ww, R^e_wv, R^e_vv} is a stochastic realization of the measurement sequence, if there exists a Π̃ ≥ 0 such that the following KSP equations are satisfied:

Π̃ − Ã Π̃ Ã′ = K_p R_ee K′_p
B̃ − Ã Π̃ C̃′ = K_p R_ee
D̃ + D̃′ − C̃ Π̃ C̃′ = R_ee
(7.52)

Therefore, we can define a unique set of noise source covariance matrices in terms of the Kalman gain and INV covariance as follows:

R^e_ww := K_p R_ee K′_p
R^e_wv := K_p R_ee
R^e_vv := R_ee
(7.53)

Solving for K_p in the second KSP equation of Eq. (7.52) and substituting the last equation for the INV covariance R_ee yields

K_p = (B̃ − Ã Π̃ C̃′) × (D̃ + D̃′ − C̃ Π̃ C̃′)^−1
(7.54)

Substituting these expressions into the first equation of Eq. (7.52) shows that the state error covariance Π̃ satisfies a discrete Riccati equation, that is,

Π̃ = Ã Π̃ Ã′ + (B̃ − Ã Π̃ C̃′)(D̃ + D̃′ − C̃ Π̃ C̃′)^−1 (B̃ − Ã Π̃ C̃′)
(Riccati Equation)
(7.55)

guaranteeing a proper stochastic realization.

As before the KSP equations can be solved iteratively: R_ee(i) → R_ee, K(i) → K and Π̃(i) → Π̃ yielding the KSP algorithm using the INV model summarized in Table 7.4 to follow.

It should also be noted in passing that from canonical realization theory the gain and INV covariance represent a canonical realization with the minimal number of parameters: (N_x N_y) to characterize K_p and (½ N_y (N_y + 1)) to characterize R_ee (see [6] for details).

## 7.4.4 Stochastic Realization: Innovations Model

As stated in Section 7.3, any deterministic realization algorithm applied to {Λ_r} such that the Hankel factorization yields a reasonable approach to extract the KSP realization evolving from the INV model. Assuming the data

was generated by the correlated GM model of the previous section with the measurement covariance represented in terms of its parameters, the classical Ho–Kalman–SVD approach can be applied with $\bar{B}_e := A\tilde{\Pi}C' + K_p R_{ee}$

$\Lambda_{K,K} = \mathcal{O}_K \times \bar{C}_K$
$= \begin{bmatrix} C \\ CA \\ \vdots \\ CA^{K-2} \end{bmatrix}_{\mathcal{O}_K} \begin{bmatrix} A\bar{B}_e \mid A^2\bar{B}_e \mid \cdots \mid A^{K-1}\bar{B}_e \end{bmatrix}_{\bar{C}_K}$ (7.56)

and therefore, due to covariance equivalence of $\Sigma_{GM}$ and $\Sigma_{INV}$, the “estimated” KSP model, $\tilde{\Sigma}_{KSP}$, is provided by Ho–Kalman algorithm as

$\tilde{A} \longrightarrow A$
$\tilde{B} \longrightarrow A\tilde{\Pi}C' + R_{uv}$
$\tilde{C} \longrightarrow C$
$(\tilde{D} + \tilde{D}') \longrightarrow C\tilde{\Pi}C' + R_{uv}$ (7.57)

Once the $\tilde{\Sigma}_{KSP}$ model is estimated from the covariance sequence using a deterministic realization algorithm, then the KSP equations can be solved to complete the stochastic realization, that is,

$R_{ee} = \tilde{D} + \tilde{D}' - \tilde{C}\,\tilde{\Pi}\,\tilde{C}'$
$K_p = (\tilde{B} - \tilde{A}\,\tilde{\Pi}\,\tilde{C}') \times R_{ee}^{-1}$ (7.58)

and therefore the noise source covariances become

$R_{ww}^e = K_p R_{ee} K_p'$
$R_{vv}^e = R_{ee}$
$R_{wv}^e = K_p R_{ee}$ (7.59)

We summarize this approach as follows:

- *Perform* a realization (deterministic) to obtain $\tilde{\Sigma}_{KSP}$.
- *Solve* the Riccati equation of Eq. (7.55) to obtain $\tilde{\Pi}$.
- *Calculate* $R_{ee}$ and $K$ from the KSP equations of Eq. (7.58).
- *Determine* the required set of covariances $\{R_{ww}^e, R_{vv}^e, R_{wv}^e\}$ from $R_{ee}$ and $K$ of Eq. (7.59).

Again we see that performing a deterministic realization from the Hankel matrix populated by the covariance sequence $\{\Lambda_r\}$ yields the model $\tilde{\Sigma}_{KSP}$ that is used to estimate $\tilde{\Pi}$ from the corresponding Riccati equation, Kalman gain, and INV covariance enabling the determination of the set of noise
