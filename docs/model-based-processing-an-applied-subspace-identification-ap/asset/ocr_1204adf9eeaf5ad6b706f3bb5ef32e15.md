and define the model error metric for the ith-measurement channel as

```
ε_i = sqrt( (∑_{t=1}^K e_i^2(t)) / (∑_{t=1}^K y_i^2(t)) ); i = 1, ..., N_y      (Model Error Metric)      (6.136)
```

to evaluate the performance of the model response {y_M(t)} relative to the response data {y(t)} [6, 7].

This metric can also be calculated over an N-ensemble of data for each measurement channel as well to provide statistics enabling a reasonable estimate of model quality. Here we have generated a data ensemble {y_n(t)}; n = 1, ..., N with the corresponding ensemble model error for the nth realization defined by

```
ε_n(t) := y_n(t) - y_M^n(t)      (Ensemble Model Error)      (6.137)
```

Ensemble statistics and confidence intervals can be estimated from the sample mean and variance estimates across the realizations {ε_n(t)}; n = 1, ..., N as

```
μ_e(t) := Ê{ε_n(t)} = (1/N) ∑_{n=1}^N ε_n(t)
σ_e(t) := Ê{(ε_n(t) - μ_e(t))} = (1/N) ∑_{n=1}^N ε_n^2(t) - μ_e^2(t)      (6.138)
```

enabling a confidence interval estimate

```
I_e(t) = μ_e(t) ± 1.96 σ_e(t)      for      N > 30
```

for each measurement channel realization providing another means of validating the identified model.

Another statistic that can be applied to validate the performance of the identified model is the median absolute deviation (MAD) statistic because of its inherent robustness property relative to the usual ensemble mean/standard deviation approach [41]: