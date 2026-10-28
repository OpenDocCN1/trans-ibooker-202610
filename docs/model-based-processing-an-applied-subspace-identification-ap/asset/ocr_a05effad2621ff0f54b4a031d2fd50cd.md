and the corresponding measurement PSD is

```
S_{yy}(z) = S_{jyj}(z) + S_{j e}(z) + S_{e j}(z) + S_{e e}(z)  (4.58)
```

Using the linear system relations of Chapter 2, we see that

```
S_{yy}(z) = C S_{x x}(z) C' = T(z) S_{ee}(z) T'(z^{-1})  with  S_{ee}(z) = R_{ee}
S_{j e}(z) = C S_{x e}(z) = T(z) S_{ee}(z)  and  S_{e j}(z) = S_{ee}(z) T'(z^{-1})  (4.59)
```

Thus, the measurement PSD is given in terms of the innovations model as

```
S_{yy}(z) = T(z) S_{ee}(z) T'(z^{-1}) + T(z) S_{ee}(z) + S_{ee}(z) T'(z^{-1}) + S_{ee}(z)  (4.60)
```

Since S_{ee}(z) = R_{ee} and R_{ee} ≥ 0, then the following (Cholesky) factorization always exists as

```
R_{ee} = R_{ee}^{1/2} (R_{ee}')^{1/2}  (4.61)
```

Substituting these factors into Eq. (4.60) gives

```
S_{yy}(z) = T(z) R_{ee}^{1/2} (R_{ee}')^{1/2} T'(z^{-1}) + T(z) R_{ee} + R_{ee}(z) T'(z^{-1}) + R_{ee}  (4.62)
```

Combining like terms enables S_{yy}(z) to be written in terms of its spectral factors as

```
S_{yy}(z) = (T(z) R_{ee}^{1/2} + R_{ee}^{1/2}) × ((R_{ee}')^{1/2} T'(z^{-1}) + (R_{ee}')^{1/2})  (4.63)
```

or simply

```
S_{yy}(z) = T_e(z) × T_e'(z^{-1})  (4.64)
```

which shows that the innovations model indeed admits a spectral factorization of the type desired. To show that T_e(z) is the unique, stable, minimum-phase spectral factor, it is necessary to show that |T_e(z)| has all its poles within the unit circle (stable). It has been shown (e.g. [4, 8, 16]) that T_e(z) does satisfy these constraints.

This completes the discussion on the equivalence of the steady-state Kalman filter and the Wiener filter. Next we consider the development of nonlinear processors.

# 4.3 Nonlinear State-Space Model-Based Processors

In this section, we develop a suite of recursive, nonlinear state-space MBPs. We start by using the linearized perturbation model of Section 3.8 and the development of the linear MBP of Section 4.2 to motivate the development of the LZKF. This processor is important in its own right, if a solid reference trajectory is available (e.g. roadmap for a self-driving vehicle). From this processor, it