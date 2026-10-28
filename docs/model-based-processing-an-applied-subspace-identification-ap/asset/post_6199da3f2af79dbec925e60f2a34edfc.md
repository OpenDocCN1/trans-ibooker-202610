
# **Table 4.8** Discrete unscented Kalman filter (UKF) algorithm.

**State: σ-points and weights**

\(\mathcal{X}_0=\hat{x}(t-1|t-1)\), \(W_0=\frac{\kappa}{N_x+\kappa}\)

\(\mathcal{X}_i=\hat{x}(t-1|t-1)+\left(\sqrt{(N_x+\kappa)\tilde{P}(t-1|t-1)}\right)_i\), \(W_i=\frac{1}{2(N_x+\kappa)}\)

\(\mathcal{X}_{i+N_x}=\hat{x}(t-1|t-1)-\left(\sqrt{(N_x+\kappa)\tilde{P}(t-1|t-1)}\right)_i\), \(W_{i+N_x}=\frac{1}{2(N_x+\kappa)}\)

**State prediction**

\(\mathcal{X}_i(t|t-1)=\mathbf{a}[\mathcal{X}_i(t-1|t-1)]+\mathbf{b}[u(t-1)]\)  (Nonlinear State Process)

\(\hat{x}(t|t-1)=\sum_{i=0}^{2N_x}W_i\mathcal{X}_i(t|t-1)\)  (State Regression)

**State error prediction**

\(\tilde{\mathcal{X}}_i(t|t-1)=\mathcal{X}_i(t|t-1)-\hat{x}(t|t-1)\)  (State Error)

\(\tilde{P}(t|t-1)=\sum_{i=0}^{2N_x}W_i\tilde{\mathcal{X}}_i(t|t-1)\tilde{\mathcal{X}}_i'(t|t-1)+R_{ww}(t-1)\)  (Error Covariance Prediction)

**Measurement: σ-points and weights**

\(\hat{\mathcal{X}}_i(t|t-1)=\{\mathcal{X}_i(t|t-1),\ \mathcal{X}_i(t|t-1)+\kappa\sqrt{R_{ww}(t-1)},\ \mathcal{X}_i(t|t-1)-\kappa\sqrt{R_{ww}(t-1)}\}\)

**Measurement prediction**

\(\mathcal{Y}_i(t|t-1)=\mathbf{c}[\hat{\mathcal{X}}_i(t|t-1)]+\mathbf{d}[u(t)]\)  (Nonlinear Measurement)

\(\hat{y}(t|t-1)=\sum_{i=0}^{2N_x}W_i\mathcal{Y}_i(t|t-1)\)  (Measurement Regression)

**Residual prediction**

\(\xi_i(t|t-1)=\mathcal{Y}_i(t|t-1)-\hat{y}(t|t-1)\)  (Predicted Residual)

\(R_{\xi\xi}(t|t-1)=\sum_{i=0}^{2N_x}W_i\xi_i(t|t-1)\xi_i'(t|t-1)+R_{vv}(t)\)  (Residual Covariance Regression)

**Gain**

\(R_{x\xi}(t|t-1)=\sum_{i=0}^{2N_x}W_i\tilde{\mathcal{X}}_i(t|t-1)\xi_i'(t|t-1)\)  (Cross-covariance Regression)

\(\mathcal{K}(t)=R_{x\xi}(t|t-1)R_{\xi\xi}^{-1}(t|t-1)\)  (Gain)

**State update**

\(e(t)=y(t)-\hat{y}(t|t-1)\)  (Innovation)

\(\hat{x}(t|t)=\hat{x}(t|t-1)+\mathcal{K}(t)e(t)\)  (State Update)

\(\tilde{P}(t|t)=\tilde{P}(t|t-1)-\mathcal{K}(t)R_{\xi\xi}(t|t-1)\mathcal{K}'(t)\)  (Error Covariance Update)

**Initial conditions**

\(\hat{x}(0|0)\quad \tilde{P}(0|0)\)

with zero-mean/whiteness values of \((3.85\times10^{-2}<1.73\times10^{-1}/\%\ out)\), while the UKF is certainly comparable at \((5.63\times10^{-2}<1.73\times10^{-1}/\%\ out)\). This completes the example.

### 4.3.5 Practical Nonlinear Model-Based Processor Design: Performance Analysis

The suite of nonlinear processor designs was primarily developed based on Gaussian assumptions. In the linear Kalman filter case, a necessary and sufficient condition for optimality of the filter is that the corresponding innovations or residual sequence must be zero-mean and white (see Section 4.2 for details). In lieu of this constraint, a variety of statistical tests (whiteness, uncorrelated inputs, etc.) were developed evolving from this known property. When the linear Kalman filter was “extended” to the nonlinear case, the same tests can be performed based on approximate Gaussian assumptions. Clearly, when noise is additive Gaussian, these arguments can still be applied for improved design and performance evaluation.

In what might be termed “sanity testing,” the classical nonlinear methods employ the basic statistical philosophy that ”if all of the signal information has been removed (explained) from the measurement data, then the residuals (i.e. innovations or prediction error) should be uncorrelated”; that is, the model fits the data and all that remains is a white or uncorrelated residual (innovations) sequence. Therefore, the zero-mean, whiteness testing, that is, the confidence interval about the normalized correlations (innovations) and the (vector) WSSR tests of Section 4.2 are performed.

![](img/4bb66282061cdabd9d492d7740bb0233_170_0.png)

Figure 4.6  Nonlinear trajectory estimation: unscented Kalman filter (UKF). (a) Trajectory (state) estimates using the EKF (thick dotted), IEKF (thin dotted), and UKF (thick solid). (b) Filtered measurement estimates using the EKF (thick dotted), IEKF (thin dotted), and UKF (thick solid). (c) Zero-mean/whiteness tests for EKF: (1.04 × 10⁻¹ < 1.73 × 10⁻¹ /% out). (d) Zero-mean/whiteness tests for IEKF: (3.85 × 10⁻² < 1.73 × 10⁻¹ /% out). (e) Zero-mean/whiteness tests for UKF: (5.63 × 10⁻² < 1.73 × 10⁻¹ /% out).

Perhaps one of the most important aspects of nonlinear processor design is to recall that processing results *only* in a *single realization* of its possible output. For instance, the conditional mean is just a statistic, but still a *random process*; therefore, if direct measurements are not available, it is best to generate an ensemble of realizations and extract the resulting ensemble statistics using a variety of methods (e.g. bootstrapping [29]). In any case, simple ensemble statistics can be employed to provide an average solution enabling us to generate “what we would expect to observe” when the processor is applied to the actual problem. This approach is viable even for the modern nonlinear unscented Kalman processors. Once tuned, the processor is then applied to the raw measurement data, and ensemble statistics are estimated to provide a thorough analysis of the processor performance.

Statistics can be calculated over the ensemble of processor runs. The *updated* state estimate is obtained directly from the ensemble, that is,

```
x̂(t|t) → {x̂_k(t|t)}; k = 1, ..., K
```

and therefore, the updated state ensemble estimate is

```
x̂(t|t) = (1/K) Σ_{k=1}^K x̂_k(t|t)   (4.93)
```

The corresponding predicted state estimate can be obtained directly from the ensemble as well, that is,

```
x̂(t|t-1) → {x̂_k(t|t-1)}; k = 1, ..., K
```

and, therefore, the predicted state ensemble estimate is

```
x̂(t|t-1) = (1/K) Σ_{k=1}^K x̂_k(t|t-1)   (4.94)
```

The *predicted* measurement estimate is also obtained directly from the ensemble, that is,

```
ŷ(t|t-1) → {ŷ_k(t|t-1)},   k = 1, ..., K
```

where recall

```
ŷ_k(t|t-1) = c[x̂_k(t|t-1)]   (4.95)
```

Thus, the predicted measurement ensemble estimate is

```
ŷ̂(t|t-1) = (1/K) Σ_{k=1}^K ŷ_k(t|t-1)   (4.96)
```

posterior distribution state-by-state at each time-step along with the fact that the number of random samples to characterize the distribution is equal to the number of particles ($N_p$) per time-step.

The PF is a processor that has data on input and produces an estimate of the corresponding posterior distribution on output. These unequally spaced random samples or particles are actually the “location” parameters along with their associated weights that gather or coalesce in the regions of highest probabilities (mean, mode, etc.) providing a nonparametric estimate of the empirical posterior distribution as illustrated in Figure 4.7. With these particles generated, the appropriate weights based on Bayes’ rule are estimated from the data and lead to the desired result. Once the posterior is estimated, then inferences can be performed to extract a wealth of descriptive statistics characterizing the underlying process. In particle filtering, continuous distributions are approximated by “discrete” random measures composed of these weighted particles or point masses where the particles are actually random samples of the unknown or hidden states from the state-space representation and the

![](img/4bb66282061cdabd9d492d7740bb0233_173_0.png)

Figure 4.7 Particle filter representation of “empirical” posterior probability distribution in terms of weights (probabilities) and particles (samples).

weights are the approximate “probability masses” estimated recursively. Here in the figure, we see that associated with each particle, $x_i(t)$ is a corresponding weight or probability mass, $\mathcal{W}_i(t)$ (filled circle). Knowledge of this random measure, $\{x_i(t),\mathcal{W}_i(t)\}$, characterizes an estimate of the instantaneous (at each time $t$) filtering posterior distribution (dashed line). The estimated empirical posterior distribution is a weighted PMF given by

$$\hat{\Pr}(x(t)|Y_t)=\sum_{i=1}^{N_p}\mathcal{W}_i(t)\delta(x(t)-x_i(t))$$

Thus, the PF does *not* involve linearizations around current estimates, but rather approximations of the desired distributions by these discrete random measures in contrast to the Kalman filter that sequentially estimates the conditional mean and covariance used to characterize the “known” (Gaussian) filtering posterior, $\Pr(x(t)|Y_t)$. PFs are a sequential MC methodology based on this “point mass” representation of probability distributions that only requires a state-space representation of the underlying process. This representation provides a set of particles that evolve at each time-step leading to an instantaneous approximation of the target posterior distribution of the state at time $t$ given all of the data up to that time. The posterior distribution then evolves at each time-step as illustrated in Figure 4.8, creating an instantaneous approximation of $t$ vs $x_i$ vs $\hat{\Pr}(X_i(t)|Y_t)$—generating an ensemble of PMFs (see [7] for more details). Statistics are then calculated *across* the ensemble at each time-step to provide estimates of the states. For example, the MAP estimate is simply determined by finding the particle $x_i(t)$ locating to the maximum posterior (weight) at each time-step across the ensemble as illustrated in the offset of Figure 4.8, that is,

$$\hat{X}_{\mathrm{MAP}}(t)=\underset{x_i(t)}{\arg\max}\;\hat{\Pr}(x(t)|Y_t) \tag{4.99}$$

The objective of the PF algorithm is to estimate these weights producing the posterior at each time-step. These weights are estimated based on the concept of importance sampling [7]. *Importance sampling* is a technique to compute statistics with respect to one distribution using random samples drawn from another. It is a method of simulating samples from a proposal or sampling (importance) distribution to approximate a targeted distribution (posterior) by appropriate weighting. For this choice, the weighting function is defined by the ratio

$$W(t):=\frac{\Pr[X_t|Y_t]}{\mathcal{I}(X_t|Y_t)} \tag{4.100}$$

where $\mathcal{I}(\cdot|\cdot)$ is the proposed sampling or *importance distribution* that leads to different PFs depending on its selection (e.g. prior → bootstrap).
