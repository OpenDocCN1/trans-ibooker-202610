while the likelihood is determined from the nonlinear pressure-field measurement model:

```
p(r_s, z_ℓ) = c[Φ_i(z_ℓ; Θ)] + v(z_ℓ)  (8.128)
```

Assuming additive Gaussian noise, the likelihood probability is given by

```
Pr[p(r_s, z_ℓ)|Φ_i(z_ℓ)] = 1/√(2πR_vv) × exp{ -1/(2R_vv) (p(r_s, z_ℓ) - c[Φ_i(z_ℓ; Θ)])² }  (8.129)
```

Thus, we estimate the posterior distribution using a sequential Monte Carlo approach and construct a bootstrap particle filter using the following steps:

+   - Initialize: Φ_i(0), w_i ~ N(0, R_wv), W_i(0) = 1/N_p; i = 1, ..., N_p
+   - State transition: Φ_i(z_ℓ; Θ) = { A(z_{ℓ-1}) Φ_i(z_{ℓ-1}) + w_i(z_{ℓ-1})  (Case i)
+     a[Φ_i(z_{ℓ-1}; Θ)] + w_i(z_{ℓ-1})  (Case ii) }
+   - Likelihood probability: Pr[p(r_s, z_ℓ)|Φ_i(z_ℓ)] of Eq. (8.129)
+   - Weights: W_i(z_ℓ; Θ) = W_i(z_{ℓ-1}; Θ) × Pr[p(r_s, z_ℓ)|Φ_i(z_ℓ)];
+   - Normalize: W_i(z_ℓ; Θ) = W_i(z_ℓ; Θ) / Σ_{i=1}^{N_p} W_i(z_ℓ; Θ)
+   - Resample: Φ̃_i(z_ℓ; Θ) ⟷ Φ_i(z_ℓ; Θ)
+   - Posterior: P̂_r[Φ(z_ℓ; Θ)|P_ℓ] = Σ_{i=1}^{N_p} W_i(z_ℓ; Θ) × δ(Φ(z_ℓ; Θ) - Φ_i(z_ℓ; Θ))

+   - MAP estimate: Φ̂_i(z_ℓ; Θ)_MAP = argmax_i P̂_r[Φ_i(z_ℓ; Θ)|P_ℓ]
+   - MMSE (CM) estimate: Φ̂_i(z_ℓ; Θ)_MMSE = 1/N_p Σ_{i=1}^{N_p} W_i(z_ℓ; Θ) × Φ_i(z_ℓ; Θ).

# 8.4.4 Model-Based Ocean Acoustic Processing

In this section, we discuss the development of the propagators for the Hudson Canyon experiment performed in 1988 in the Atlantic with the primary goal of investigating acoustic propagation (transmission and attenuation) using continuous-wave data [43, 44]. The Hudson Canyon is located off the coast of New Jersey in the area of the Atlantic Margin Coring project borehole 6010. The seismic and coring data are combined with sediment properties measured at that site. Excellent agreement was determined between the model and data, indicating a well-known, well-documented shallow water experiment with bottom interaction and yielding ideal data sets for investigating the applicability of an MBP to measured ocean acoustic data. The experiment was performed at low frequencies (50–600 Hz) in shallow water of 73 m depth during a period of calm sea state. A calibrated acoustic source was towed at roughly 36 m depth along the 73 m isobath radially to distances of 4–26 km. The ship speed was between 2 and 4 knots. The fixed vertical