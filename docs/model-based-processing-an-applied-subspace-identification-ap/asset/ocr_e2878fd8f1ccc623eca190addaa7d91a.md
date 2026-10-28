are shown in Figure 6.13c with the estimated modal frequencies obtained from the discrete-to-continuous transform ($\mathbb{Z} \to \mathbb{S}$) listed in Figure 6.13d corresponding to those of the simulation.

We show both synthesized data along with their corresponding estimates in Figure 6.14, indicating quite a reasonable “fit” of both the underlying impulse response, synthesized data and their corresponding estimates. The subspace identifier has clearly captured the essence of the signals.

The corresponding Hankel metrics are:

+   Hankel order estimation: $\hat{N}_x = 16$
+   
+   Hankel singular values: $\sigma_i = \{18.8,\ 17.9,\ 10.4,\ 10.6,\ 6.4,\ \ldots,\ 2.3\}$
+   
+   Hankel norm (energy): $\|\mathcal{H}_{K,\kappa}\| = 1.2 \times 10^{-5}$
+   
+   SVD model error: $\mathcal{M}_e = 3.0 \times 10^{-5}$

Next we calculate the various validation metrics to observe how well they predict/validate a reasonable realization of this data. The results are shown in Table 6.6. From the table, we examine the average results of each of the statistics. Over an ensemble of 100-realizations and deterministic subspace identifications, the average residual error is $1.3 \times 10^{-11}$ as expected since the

![](bbox=[0.153, 0.522, 0.856, 0.851])

**Figure 6.14** Subspace identification of eight-mode mechanical system. (a) Raw data and estimated impulse responses. (b) Raw and estimated channel measurements.