So we see that performing a deterministic realization from the Hankel matrix populated by the covariance sequence {Λ_r} yields the model Σ̂_KSP that is used to estimate Π* from the corresponding Riccati equation enabling the determination of the set of covariances {R_ww, R_vv, R_wv} from the KSP relations. We summarize the Riccati stochastic realization algorithm in Table 7.2.

This completes the section on the classical approach to solve the stochastic realization problem. However, before we leave this discussion, let us consider an alternative, yet equivalent, way to solve the problem using the well-known Kalman filter based on the underlying INV model developed in Chapter 4.

# 7.4 Classical Stochastic Realization via Kalman Filter

In this section, we present a special case of the Riccati equation solution to solve the stochastic realization problem. This approach evolves from the factorization theorem of Chapter 4 that led to the Wiener/Kalman filter equivalence resulting in the unique, steady-state solution of the Riccati equation. It is well known [5] that this steady-state solution uniquely specifies the optimal Kalman gain. The significance of obtaining a stochastic realization via the Kalman filter is twofold. First, since the Kalman gain is unique (modulo similarity transformations), so is the corresponding stochastic realization. Second, knowledge of this gain specifies a stable, minimum-phase (zeros within the unit circle), multichannel spectral factor [1, 16]. The importance of this approach compared to the previous one is that once the gain is determined, a stochastic realization is guaranteed immediately. However, the price paid for a so elegant solution is again the computational burden of solving a Riccati equation.

# 7.4.1 Innovations Model

The basic state estimation problem is to find the minimum variance estimate of the state vector of the GM model in terms of the currently available measurement sequence y(t). We start with the innovations (INV) model rather than the Gauss–Markov model as before, since it is, in fact, equivalent to the steady-state Kalman filter for stationary processes. That is, the INV representation of the Kalman filter in “prediction form” is given by (see [32] for details)

```
x̂(t + 1) = A x̂(t) + K_p(t) e(t)                     (State Estimate)
y(t) = C x̂(t) + e(t)                                 (Measurement)
ŷ(t) = C x̂(t)                                        (Measurement Estimate)
e(t) = y(t) − ŷ(t)                                   (Innovation)   (7.38)
```