
The approach we chose for our problem is to estimate these weights based on the concept of importance sampling (see Section 4.3). Recall that importance sampling is a technique to compute statistics with respect to one distribution using random samples drawn from another. It is a method of simulating samples from a proposal or sampling (importance) distribution to be used to approximate a targeted distribution (joint posterior) by appropriate weighting. For this choice, the weighting function is defined by

\[
\mathcal{W}(z_\ell; \Theta) := \frac{\Pr[\Phi(z_\ell; \Theta)|P_\ell]}{q[\Phi(z_\ell; \Theta)|P_\ell]} \tag{8.123}
\]

where \(q[\cdot]\) is the proposed sampling or importance distribution.

For the “sequential” case, we have the weighting function

\[
\mathcal{W}(z_\ell; \Theta) \propto \left( \frac{\Pr[p(r_s, z_\ell)|\Phi(z_\ell; \Theta)] \times \Pr[\Phi(z_\ell; \Theta)|\Phi(z_{\ell-1}; \Theta)]}{q[\Phi(z_\ell; \Theta)|\Phi(z_{\ell-1}; \Theta), P_\ell]} \right) \times \mathcal{W}(z_{\ell-1}; \Theta) \tag{8.124}
\]

where \(\propto\) means proportional up to a normalizing constant (evidence).

There are a variety of PF algorithms available, each evolving by a particular choice of the sampling or importance distribution, but the simplest is the bootstrap technique that we apply to our problem. Here the importance distribution is selected as the transition prior, that is,

\[
q[\Phi(z_\ell; \Theta)|\Phi(z_{\ell-1}; \Theta), P_\ell] = \Pr[\Phi(z_\ell; \Theta)|\Phi(z_{\ell-1}; \Theta)] \tag{8.125}
\]

and substituting into Eq. (8.124) we obtain

\[
\mathcal{W}(z_\ell; \Theta) = \Pr[p(r_s, z_\ell)|\Phi(z_\ell; \Theta)] \times \mathcal{W}(z_{\ell-1}; \Theta) \tag{8.126}
\]

Thus, we see that once the underlying posterior is available, the estimates of important statistics can be inferred directly. For instance, the MAP estimate is simply found by locating a particular particle \(\hat{\Phi}_i(z_\ell)\) corresponding to the maximum of the PMF, that is, \(\hat{\Phi}_i(z_\ell; \Theta)_{\mathrm{MAP}}\), while the conditional mean estimate is calculated by integrating the posterior to give \(\hat{\Phi}_i(z_\ell; \Theta)_{\mathrm{CM}}\).

For the bootstrap implementation, we need only to draw noise samples from the state and parameter distributions and use the dynamic models above (normal-mode/random walk) in Eq. (8.114) to generate the set of particles, \(\{\Phi_i(z_\ell; \Theta)\} \rightarrow \{\Phi_i(z_\ell), \Theta_i(z_\ell)\}\) for \(i = 1, \dots, N_p\). That is, both sets of particles are generated from the augmented models (linear/nonlinear) for each individual case (adaptive modal coefficients or adaptive wavenumbers) from

\[
\Phi_i(z_\ell; \Theta) = \begin{cases} \mathbf{A}(z_{\ell-1})\Phi_i(z_{\ell-1}) + \mathbf{w}_i(z_{\ell-1}) & \text{(Case i: modal coefficients)} \\ \mathbf{a}[\Phi_i(z_{\ell-1}; \Theta)] + \mathbf{w}_i(z_{\ell-1}) & \text{(Case ii: wavenumbers)} \end{cases} \tag{8.127}
\]

while the likelihood is determined from the nonlinear pressure-field measurement model:

\[
p(r_s, z_\ell) = c[\Phi_i(z_\ell; \Theta)] + v(z_\ell) \tag{8.128}
\]

Assuming additive Gaussian noise, the likelihood probability is given by

\[
\Pr[p(r_s, z_\ell)|\Phi_i(z_\ell)] = \frac{1}{\sqrt{2\pi R_{vv}}} \times \exp\left\{ -\frac{1}{2R_{vv}} \left(p(r_s, z_\ell) - c[\Phi_i(z_\ell; \Theta)]\right)^2 \right\} \tag{8.129}
\]

Thus, we estimate the posterior distribution using a sequential Monte Carlo approach and construct a bootstrap particle filter using the following steps:

- Initialize: \(\Phi_i(0), \mathbf{w}_i \sim \mathcal{N}(0, \mathbf{R}_{ww}), W_i(0) = 1/N_p; \quad i = 1, \dots, N_p\)
- State transition: \(\Phi_i(z_\ell; \Theta) = \begin{cases} \mathbf{A}(z_{\ell-1})\Phi_i(z_{\ell-1}) + \mathbf{w}_i(z_{\ell-1}) & \text{(Case i)} \\ \mathbf{a}[\Phi_i(z_{\ell-1}; \Theta)] + \mathbf{w}_i(z_{\ell-1}) & \text{(Case ii)} \end{cases}\)
- Likelihood probability: \(\Pr[p(r_s, z_\ell)|\Phi_i(z_\ell)]\) of Eq. (8.129)
- Weights: \(W_i(z_\ell; \Theta) = W_i(z_{\ell-1}; \Theta) \times \Pr[p(r_s, z_\ell)|\Phi_i(z_\ell)];\)
- Normalize: \(W_i(z_\ell; \Theta) = W_i(z_\ell; \Theta) / \sum_{i=1}^{N_p} W_i(z_\ell; \Theta)\)
- Resample: \(\tilde{\Phi}_i(z_\ell; \Theta) \leftrightarrow \Phi_i(z_\ell; \Theta)\)
- Posterior: \(\hat{\Pr}[\Phi(z_\ell; \Theta)|P_\ell] = \sum_{i=1}^{N_p} W_i(z_\ell; \Theta) \times \delta(\Phi(z_\ell; \Theta) - \Phi_i(z_\ell; \Theta))\)
- MAP estimate: \(\hat{\Phi}_i(z_\ell; \Theta)_{\mathrm{MAP}} = \arg\max_i \hat{\Pr}[\Phi_i(z_\ell; \Theta)|P_\ell]\)
- MMSE (CM) estimate: \(\hat{\Phi}_i(z_\ell; \Theta)_{\mathrm{MMSE}} = \frac{1}{N_p} \sum_{i=1}^{N_p} W_i(z_\ell; \Theta) \times \Phi_i(z_\ell; \Theta)\)

## 8.4.4 Model-Based Ocean Acoustic Processing

In this section, we discuss the development of the propagators for the Hudson Canyon experiment performed in 1988 in the Atlantic with the primary goal of investigating acoustic propagation (transmission and attenuation) using continuous-wave data [43, 44]. The Hudson Canyon is located off the coast of New Jersey in the area of the Atlantic Margin Coring project borehole 6010. The seismic and coring data are combined with sediment properties measured at that site. Excellent agreement was determined between the model and data, indicating a well-known, well-documented shallow water experiment with bottom interaction and yielding ideal data sets for investigating the applicability of an MBP to measured ocean acoustic data. The experiment was performed at low frequencies (50–600 Hz) in shallow water of 73 m depth during a period of calm sea state. A calibrated acoustic source was towed at roughly 36 m depth along the 73 m isobath radially to distances of 4–26 km. The ship speed was between 2 and 4 knots. The fixed vertical

Metrics are estimated and processor performance analyzed. Since each run of the PF is a random realization, that is, the process noise inputs are random, an ensemble of results are estimated with its statistics presented. In this way, we can achieve a detailed analysis of the processor performance prior to fielding an operational version. We constrain our discussion results to processing the noisy experimental pressure-field measurements.

We performed a series of “tuning” runs for both the UKF and PF. Here we primarily adjusted the process noise covariance matrix (\(R_{ww}\)) for each of the modal functions and then executed a 100-member ensemble of realizations using these parameters. The particle filter was designed with the same parameters, and 1500 particles were used to characterize the posterior PMF at each depth. Resampling (see Section 4.3) was applied at every iteration of the PF to avoid any potential degradation.

First we investigate the enhancement capabilities of the PF in estimating the pressure-field over a 100-member ensemble shown in Figure 8.22. The resulting figures show the averaged PF estimates. We observe the raw data (DATA) as well as both MAP and conditional mean (CM) estimates. Both estimators are capable of tracking the field quite well and even filter the erratic measurements near the bottom of the channel.

![](img/4bb66282061cdabd9d492d7740bb0233_466_0.png)

**Figure 8.22** Raw/enhanced pressure-field (DATA) data from the Hudson Canyon experiment using particle filter estimators with adaptive modal coefficients: maximum a posteriori (MAP), conditional mean (CM), and the corresponding innovations (ERROR) sequence.

The innovations or residuals (ERROR) are also shown in the figure. Both estimators are capable of tracking and enhancing the pressure-field. Using classical performance (sanity tests) metrics on the innovations sequence (ERROR), the zero-mean/whiteness tests, both processors satisfy the criteria of unbiasedness (\(Z_M\): \(6.2 \times 10^{-4} < 4.9 \times 10^{-1}\)) and uncorrelated innovations, that is, less than 5% exceeding the bound (6.3%). The weighted sum-squared residual (WSSR) test of Section 4.2 is also applied with satisfactory results, that is, *no samples exceed the threshold*, indicating a functionally “tuned” processor. The UKF processor also produced reasonable results for the enhanced pressure-field (not shown).

Ensemble mode tracking results are shown in Figure 8.23 for each of the modal function estimators, the PF (MAP/CM) and the UKF. In Figure 8.23 we observe that the performance of the PF appears to track the modes quite well compared to the UKF. It is interesting to note that the modal coefficient estimates are constantly being adapted (adjusted) by the processor throughout the runs attesting to the nonstationary nature of the ocean statistics as illustrated in Figure 8.24.

This completes the analysis of the Hudson Canyon experimental data for the adaptive (modal coefficient) PF processing performance.

### 8.4.4.2 Adaptive PF Design: Wavenumbers

As before in the modal coefficient case, we investigate the enhancement capabilities of the PF in estimating the pressure-field over a 100-member ensemble shown in Figure 8.25. Using 1500 particles, we see the raw hydrophone data (dashed line) from the experiment as well as both MAP estimates (circles) and conditional mean (CM) estimates (dotted line with circles). Both estimators appear to track the field quite well (true (mean) solution in dashes). The corresponding innovations (residual) sequence is also shown (diamonds). Classically, both estimators produced satisfactory zero-mean/statistical whiteness test as well as the WSSR tests indicating a “tuned” processor [9].

The ensemble mode tracking results are shown in Figure 8.26 for each of the modal function estimators, the PF (MAP/CM) and the UKF. In Figure 8.26, we observe that the performance of the PF appears to track the modes quite well and better than the UKF. The root-mean-squared (modal tracking) error for each mode is quite reasonable on the order of \(10^{-5}\) again confirming their performance. It is interesting to note that the wavenumber estimates are constantly being adapted (adjusted) by the processor throughout the runs attesting to the nonstationary nature of the ocean statistics. The ensemble average wavenumber estimates are very reasonable: (0.206, 0.197, 0.181, 0.173, 0.142; (TRUE) 0.208, 0.199, 0.183, 0.175, 0.142. The PF and CM ensemble estimates are very close to the true values adapting to the changing ocean environment yet still preserving wavenumber values on average.
