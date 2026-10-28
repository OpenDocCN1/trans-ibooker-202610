This relationship shows that obliquely projecting the future data nullifies not only the inputs but also the INV, while enabling the extraction of the observability and estimated state directly from the given data.

Thus, the state vector 𝒳_f provides the basis for the intersection of the past and future subspaces as discussed in Section 6.4.2 and can be extracted from this projection. This is a special state vector. It is, in fact, a “state estimate” – the output of a Kalman filter providing the best estimate of the state based on the available data! In contrast to the recursive filter developed in Section 4.2 that requires the entire stochastic realization Σ_INV = {A, B, C, D, R^e_ww, R^e_wv, R^e_vv} as well as the initial state and error covariance (x̂(0), P̃(0)) to be applied, this realization, extracted using SID techniques is numerical, only requiring input/output data for its development and application. In fact, it is not a steady-state Kalman filter processor as in Section 4.2, but one that processes the data matrices in parallel, “column-by-column,” in a batch manner [27]. This is a rather remarkable property of the SID approach!

The numerical Kalman filter⁶ evolves directly from the oblique projection and can be placed in the following closed-form input/output structure as [27]

```
x̂(k) = [A^k − Ω_k 𝒪_k | C_AB^−(k) − Ω_k 𝒯_k | Ω_k] ⎡ x̂(0) ⎤ ⎢ --- ⎥ ⎢ u(0) ⎥ ⎢  :  ⎥ ⎢ u(k−1) ⎥ ⎢ --- ⎥ ⎢ y(0) ⎥ ⎢  :  ⎥ ⎣ y(k−1) ⎦ (7.110)
```

where Ω_k = (C_AK^−(k) − A^k P̃(0)𝒪'_k) (L_k − 𝒪_k P̃(0)𝒪'_k). Expanding this expression over K-samples (columns) with x̂_f := [x̂(k) x̂(k+1) ⋯ x̂(k+K−1)]', we obtain

```
x̂_f = [A^k − Ω_k 𝒪_k | C_AB^−(k) − Ω_k 𝒯_k | Ω_k] ⎡ x̂(0) ⎤ ⎢ --- ⎥ ⎢ 𝒰_0|k−1 ⎥ ⎢ --- ⎥ ⎣ 𝒴_0|k−1 ⎦ (7.111)
```

or grouping the data as before, we have

```
x̂_f = [A^k − Ω_k 𝒪_k | (C_AB^−(k) − Ω_k 𝒯_k | Ω_k)] ⎡ x̂(0) ⎤ ⎢ --- ⎥ ⎣ D_0|k−1 ⎦ (7.112)
```

for x̂(0) := [x̂_0(0) x̂_1(0) ⋯ x̂_{K−1}(0)]'