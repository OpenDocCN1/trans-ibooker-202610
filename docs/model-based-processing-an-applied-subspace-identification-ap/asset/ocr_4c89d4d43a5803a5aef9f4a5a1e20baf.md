this technique is to use an AR spectral estimator and then perform this averaging.

This completes the discussion of the MVDR PSD estimation technique. In the next section, we discuss methods designed primarily to extract sinusoidal signals from noise.

# 2.5.2.4 Multiple Signal Classification (MUSIC) Spectral Estimation

In this section, we investigate a solution to the harmonics (sinusoids) in noise parameter estimation problem, which is prevalent in a variety of applications especially in spatial processing for direction-of-arrival and target localization problems [26, 27].

A complex harmonic model is defined by

```
s(t) = Σ_{i=1}^{N_s} A_i e^{jΩ_i t + φ_i}  (2.69)
```

where A_i is complex, A_i = |A_i| e^{-jθ_i}, φ_i is random and uniformly distributed as φ ~ U(-π, π) and Ω_i is the harmonic frequency. The set of N_s-complex amplitudes and harmonic frequencies, [{A_i}, {Ω_i}], i = 1, ..., N_s is assumed deterministic, but unknown.

The corresponding measurement model is characterized by the complex harmonic signal in additive random noise

```
y(t) = s(t) + n(t)  (2.70)
```

with n(t), a zero-mean random sequence with variance, σ_n^2. Therefore, its autocorrelation function is given by

```
R_yy(k) = R_ss(k) + R_nn(k) = Σ_{i=1}^{N_s} P_i e^{jΩ_i k} + σ_n^2  for P_i := |A_i|^2  (2.71)
```

since the signal is uncorrelated with the noise. Expanding this relation to incorporate the N_s-harmonics, we obtain

```
R_yy(k) = { Σ_{i=1}^{N_s} P_i + σ_n^2 δ(k)  k = 0 ; Σ_{i=1}^{N_s} P_i e^{jΩ_i k}  k ≠ 0 }  (2.72)
```

There are a number of approaches in which this problem can be attacked such as maximum likelihood parameter estimation, Prony's technique, linear prediction, but here we choose to use the power method [20, 26], that is, the cost function is based on maximizing the output power in the signal and the resulting function is related to its power spectrum. All peaks of the spectrum locate the associated harmonic frequencies of the model.