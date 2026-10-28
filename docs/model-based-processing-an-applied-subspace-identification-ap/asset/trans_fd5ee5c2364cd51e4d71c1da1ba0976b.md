

With this in mind, we develop the processor directly from the LZKF. If, instead of using the reference trajectory, we choose to linearize about each new state estimate as soon as it becomes available, then the EKF algorithm results. The reason for choosing to linearize about this estimate is that it represents the best information we have about the state and therefore most likely results in a better reference trajectory (state estimate). As a consequence, large initial estimation errors do not propagate; therefore, linearity assumptions are less likely to be violated. Thus, if we choose to use the current estimate \hat{x}(t|\alpha), where \alpha is t − 1 or t, to linearize about instead of the reference trajectory x^*(t), then the EKF evolves. That is, let

```
x^*(t) \equiv \hat{x}(t|\alpha) \quad \text{for } t-1 \le \alpha \le t \tag{4.79}
```

Then, for instance, when \alpha = t − 1, the predicted perturbation is

```
\delta \hat{x}(t|t-1) = \hat{x}(t|t-1) - x^*(t)|_{x^*=\hat{x}(t|t-1)} = 0 \tag{4.80}
```

Thus, it follows immediately that when x^*(t) = \hat{x}(t|t), then \delta \hat{x}(t|t) = 0 as well. Substituting the current estimate, either prediction or update into the LZKF algorithm, it is easy to see that each of the difference terms [\hat{x} - x^*] is null, resulting in the EKF algorithm. That is, examining the prediction phase of the linearized algorithm, substituting the current available updated estimate, \hat{x}(t-1|t-1), for the reference and using the fact that (u^*(t) = u(t) \forall t), we have

```
\hat{x}(t|t-1) = a[\hat{x}(t-1|t-1)] + A[\hat{x}(t-1|t-1)] \times [\hat{x}(t-1|t-1) - \hat{x}(t-1|t-1)] \underbrace{0}_{} + b[u(t-1)] + B[u(t-1)][u(t-1) - u(t-1)] \underbrace{0}_{}
```

yielding the prediction of the EKF

```
\hat{x}(t|t-1) = a[\hat{x}(t-1|t-1)] + b[u(t-1)] \tag{4.81}
```

Now with the predicted estimate available, substituting it for the reference in Eq. (4.76) gives the innovations sequence as

```
e(t) = y(t) - c[\hat{x}(t|t-1)] - C[\hat{x}(t|t-1)][\hat{x}(t|t-1) - \hat{x}(t|t-1)] \underbrace{0}_{} - d[u(t)] - D[u(t)][u(t) - u(t)] \underbrace{0}_{} = y(t) - c[\hat{x}(t|t-1)] - d[u(t)] \tag{4.82}
```

where we have the new predicted or filtered measurement expression

```
\hat{y}(t|t-1) \equiv c[\hat{x}(t|t-1)] - d[u(t)] \tag{4.83}
```

The updated state estimate is easily obtained by substituting the predicted estimate for the reference ($\hat{x}(t|t-1) \to x^*(t)$) in Eq. (4.77)

$$\delta\hat{x}(t|t)=\delta\tilde{x}(t|t-1)+K(t)e(t)$$

$$[\hat{x}(t|t)-\hat{x}(t|t-1)]=[\hat{x}(t|t-1)-\hat{x}(t|t-1)]+K(t)e(t)$$

$$\hat{x}(t|t)=\hat{x}(t|t-1)+K(t)e(t) \tag{4.84}$$

The covariance and gain equations are identical to those in Table 4.5, but with the Jacobian matrices $A$, $B$, $C$, and $D$ linearized about the predicted state estimate, $\hat{x}(t|t-1)$. Thus, we obtain the discrete EKF algorithm summarized in Table 4.6. Note that the covariance matrices, $\tilde{P}$, and the gain, $K$, are now functions of the current state estimate, which is the approximate conditional mean estimate and therefore is a single realization of a stochastic process. Thus, ensemble Monte Carlo (MC) techniques should be used to evaluate estimator performance. That is, for new initial conditions selected by a Gaussian random number generator (either $\hat{x}(0|0)$ or $\tilde{P}(0|0)$), the algorithm is executed generating a set of estimates, which should be averaged over the entire ensemble using this approach to get an “expected” state, etc. Note also in practice that this

**Table 4.6** Extended Kalman filter (EKF) algorithm.

| Section | Equation | Description |
|---|---|---|
| *Prediction* | $\hat{x}(t|t-1)=a[\hat{x}(t-1|t-1)]+b[u(t-1)]$ | State Prediction |
|  | $\tilde{P}(t|t-1)=A[\hat{x}(t|t-1)]\tilde{P}(t-1|t-1)A^T[\hat{x}(t|t-1)]+R_{\omega\omega}(t-1)$ | Covariance Prediction |
| *Innovation* | $e(t)=y(t)-c[\hat{x}(t|t-1)]-d[u(t)]$ | Innovations |
|  | $R_{ee}(t)=C[\hat{x}(t|t-1)]\tilde{P}(t|t-1)C^T[\hat{x}(t|t-1)]+R_{vv}(t)$ | Innovations Covariance |
| *Gain* | $K(t)=\tilde{P}(t|t-1)C^T[\hat{x}(t|t-1)]R_{ee}^{-1}(t)$ | Gain or Weight |
| *Update* | $\hat{x}(t|t)=\hat{x}(t|t-1)+K(t)e(t)$ | State Update |
|  | $\tilde{P}(t|t)=[I-K(t)C[\hat{x}(t|t-1)]]\tilde{P}(t|t-1)$ | Covariance Update |
| *Initial conditions* | $\hat{x}(0|0)\quad \tilde{P}(0|0)$ |  |
| *Jacobians* | $A[x(t|t-1)]\equiv\left.\frac{da[x(t-1)]}{dx(t-1)}\right|_{x=\hat{x}(t|t-1)}$ | $C[\hat{x}(t|t-1)]\equiv\left.\frac{dc[x(t)]}{dx(t)}\right|_{x=\hat{x}(t|t-1)}$ |

The EKF algorithm is then given by

ˆx(t|t − 1) = (1 − 0.05△T)ˆx(t − 1|t − 1) + 0.04△T ˆx²(t − 1|t − 1)
˜P(t|t − 1) = [1 − 0.05△T + 0.08△T x(t − 1)]² ˜P(t − 1|t − 1)
e(t) = y(t) − ˆx²(t|t − 1) − ˆx³(t|t − 1)
Rₑₑ(t) = [2ˆx(t|t − 1) + 3ˆx²(t|t − 1)]² ˜P(t|t − 1) + 0.09
K(t) = (˜P(t|t − 1)[2ˆx(t|t − 1) + 3ˆx²(t|t − 1)]) Rₑₑ⁻¹(t)
ˆx(t|t) = ˆx(t|t − 1) + K(t)e(t)
˜P(t|t) = (1 − K(t)[2ˆx(t|t − 1) + 3ˆx²(t|t − 1)]) ˜P(t|t − 1)
ˆx(0|0) = 2.3 and ˜P(0|0) = 0.01

An EKF run is depicted in Figure 4.4. Here we see that the state estimate begins tracking the true state after the initial transient. The estimation error is reasonable (~ 1% lie outside the limits), indicating the filter is performing properly for this realization. The filtered measurement and innovations are shown in Figure 4.4b and lie within the predicted limits. The innovations are zero-mean (6.3 × 10⁻² < 11.8 × 10⁻²) and white (0% lie outside the limits) as shown in Figure 4.4c, indicating proper tuning. Comparing the EKF to the linearized Kalman filter (LZKF) of Section 4.3.1 shows that it performs slightly worse in terms of predicted covariance limits for the estimated measurement and innovations. Most of this error is caused by the initial conditions of the processor.

Running an ensemble of 101 realizations of this processor yields similar results for the ensemble estimates: state estimation error increased to (~ 2% outside limits), innovations zero-mean test increased slightly (6.7 × 10⁻² < 12 × 10⁻²), and whiteness was identical. This completes the example.

## 4.3.3 Nonlinear Model-Based Processor: Iterated–Extended Kalman Filter

In this section, we discuss an extension of the EKF to the IEKF. This development from the Bayesian perspective coupled to numerical optimization is complex and can be found in [2]. Here we first heuristically motivate the technique and then apply it. This algorithm is based on performing “local” iterations (not global) at a point in time, t to improve the reference trajectory and therefore the underlying estimate in the presence of significant measurement nonlinearities [3]. A local iteration implies that the inherent recursive structure of the processor is retained providing updated estimates as the new measurements are made available.

To develop the iterated–extended processor, we start with the linearized processor update relation substituting the “linearized” innovations of Eq. (4.76) of

the LZKF, that is,

```
ˆx(t|t) = ˆx(t|t − 1) + K(t; x*(t)) ×[y(t) − c[x*(t)] − C[x*(t)](ˆx(t|t − 1) − x*(t)) − d[u*(t)] − D[u*(t)](u(t) − u*(t))]
(4.85)
```

where we have explicitly shown the dependence of the gain on the reference trajectory, x*(t) through the measurement Jacobian. The EKF algorithm linearizes about the most currently available estimate, x*(t) = ˆx(t|t − 1) and u*(t) = u(t) in this case. Theoretically, the updated estimate, ˆx(t|t), is a better estimate and closer to the true trajectory. Suppose we continue and relinearize about ˆx(t|t) when it becomes available and then recompute the corrected estimate and so on. That is, define the (i + 1)th iterated estimate as ˆx_{i+1}(t|t), then the corrected or updated iterator equation becomes

```
ˆx_{i+1}(t|t) = ˆx(t|t − 1) + K_i(t) [y(t) − c[ˆx_i(t|t)] − C[ˆx_i(t|t)](ˆx(t|t − 1) − ˆx_i(t|t)) − d[u(t)] − D[u(t)](u(t) − u(t))]
K_i(t)
0
(4.86)
```

Now if we start with the 0th iterate as the predicted estimate, that is, ˆx_0 ≡ ˆx(t|t − 1), then the EKF results for i = 0. Clearly, the corrected estimate in this iteration is given by

```
ˆx_1(t|t) = ˆx(t|t − 1) + K_0(t) [y(t) − c[ˆx(t|t)] − C[ˆx(t|t − 1)] × (ˆx(t|t − 1) − ˆx(t|t − 1)) − d[u(t)]]
0
(4.87)
```

where the last term in this expression is null leaving the usual innovations. Also note that the gain is reevaluated on each iteration as are the measurement function and Jacobian. The iterations continue until there is little difference in consecutive iterates. The last iterate is taken as the updated estimate. The complete (updated) iterative loop is given by

```
e_i(t) = y(t) − c[ˆx_i(t|t)] − d[u(t)]
R_{e_i e_i}(t) = C[ˆx_i(t|t)] ˜P(t|t − 1) C'[ˆx_i(t|t)] + R_{vv}(t)
K_i(t) = ˜P(t|t − 1) C'[ˆx_i(t|t)] R_{e_i e_i}^{-1}(t)
ˆx_{i+1}(t|t) = ˆx(t|t − 1) + K_i(t)[e_i(t) − C[ˆx_i(t|t)](ˆx(t|t − 1) − ˆx_i(t|t))]
˜P_i(t|t) = (I − K_i(t) C[ˆx_i(t|t)]) ˜P(t|t − 1)
(4.88)
```

