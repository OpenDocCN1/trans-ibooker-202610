

Figure 8.30 Chirp data: (a) True chirp signal and disturbance with spectrum. (b) Noisy (−40 dB) chirp signal and disturbance with spectrum.

![](img/4bb66282061cdabd9d492d7740bb0233_476_0.png)

![](img/4bb66282061cdabd9d492d7740bb0233_476_1.png)

## 8.5.2 Model-Based Identification: Linear Chirp Signals

Thus, the model-based linear *chirp estimation* problem can simply be stated as

**GIVEN** a set of noisy measurement data $\{y(t_k)\}$, $k=0,1,\ldots,K$ contaminated with broadcast disturbances $\{d(t_k)\}$, extraneous audio signals $\{e(t_k)\}$ and Gaussian noise $\{v(t_k)\}$, **FIND** the best estimate of the chirp signal $\hat{s}(t_k)$ or equivalently the parameters $\{\hat{\alpha},\hat{f}_o,\hat{f}_f\}$.

The estimated chirp is obtained directly from its parameter estimates as

$$\hat{s}(t_k)=\hat{\alpha}\cos\left(\hat{f}_o\cdot t+2\pi\left(\frac{\hat{\beta}}{2}\cdot t^2\right)\right)$$  (8.141)

for

$$\hat{\beta}=\frac{\hat{f}_f-\hat{f}_o}{t_f}$$  (8.142)

Thus, the solution to estimating the linear chirp in the midst of these disturbances reduces to a problem of MBID.

### 8.5.2.1 Gauss–Markov State-Space Model: Linear Chirp

We choose to apply the UKF of Section 4.3 as a MBID-technique to solve this nonlinear problem by modeling the parameter variations as *random walks*, that is, in state-space form we have that

$$\mathbf{f}(t_k)=\mathbf{f}(t_{k-1})+\mathbf{w}_f(t_{k-1})$$
$$\alpha(t_k)=\alpha(t_{k-1})+w_\alpha(t_{k-1})$$  (8.143)

where $\mathbf{f}\in\mathbb{R}^{N_f\times1}$ is the vector of frequency parameters, $\alpha$ is the scalar amplitude and $\mathbf{w}\in\mathbb{R}^{N_w\times1}$ a zero-mean, multivariate Gaussian vector process with covariance $\mathbf{R}_{ww}(t_k)$. Here $\mathbf{f}=[f_o\ f_f]^T$ and $\mathbf{w}_f=[w_o\ w_f]^T$. The corresponding measurement model is given in Eq. (8.141).

Generally, the UKF algorithm provides the updated signal (parameter) estimates as²

$$\hat{\mathbf{f}}(t_k|t_k)=\hat{\mathbf{f}}(t_k|t_{k-1})+\mathbf{K}_f(t_k)e(t_k|t_{k-1})$$
$$\hat{\alpha}(t_k|t_k)=\hat{\alpha}(t_k|t_{k-1})+\mathbf{K}_\alpha(t_k)e(t_k|t_{k-1})$$
$$e(t_k|t_{k-1})=y(t_k)-\hat{y}(t_k|t_{k-1})-d(t_k)-e(t_k)$$
$$\hat{y}(t_k|t_{k-1})=\hat{s}(t_k|t_k)=\hat{\alpha}(t_k|t_{k-1})$$
$$\qquad\times\cos\left(2\pi\left(\hat{f}_o(t_k|t_{k-1})\cdot t_k+\frac{\hat{\beta}(t_k|t_{k-1})}{2}\cdot t_k^2\right)\right)$$
$$\mathbf{K}(t_k)=\mathbf{R}_{fe}(t_k|t_{k-1})\mathbf{R}_{ee}^{-1}(t_k|t_{k-1})$$  (8.144)

² The notation $\hat{\theta}(t_k|t_{k-1})$ means the estimate of $\theta$ at time $t_k$ based on all the data up to time $t_{k-1}$, $Y_{k-1}=\{y(t_j)\};\ k=0,\ldots,K$.

with the instantaneous frequency (linear chirp) where the amplitude is estimated by $\hat{\alpha}(t_k|t_k)$ and

$$\hat{f}(t_k|t_{k-1}) := \hat{f}_o(t_k|t_k) \cdot t_k + \frac{\hat{\beta}(t_k|t_k)}{2} \cdot t_k^2$$

$$\hat{\beta}(t_k|t_k) = \frac{\hat{f}_f(t_k|t_k) - \hat{f}_o(t_k|t_k)}{t_f}$$  (8.145)

Since we are transmitting the chirp signal, we have a good estimate of the starting values for $\alpha$, $f_o$, $f_f$, $t_f$ that can be used in the signal estimation problem. An interesting property of the UKF processor is that the innovations (residual errors) should be approximately zero-mean and white (uncorrelated) indicating that the model "matches" the data.

Thus, the received data can be considered to consist of extraneous and broadcast disturbances along with random measurement noise. We developed a set of synthesized measurement data by filtering (low pass) a snip-it of raw measurements to extract the extraneous disturbance with its power spectrum shown in Figure 8.32b. Next we synthesized a unit amplitude ($\alpha = 1$) chirp and broadcast disturbances. That is, we assume a chirp signal was swept over the entire record length sweeping up from $f_o = 20$ kHz to $f_f = 30$ kHz for a period of $t_f = 19.7$ ms at a sampling interval of $\Delta t = 4.8 \mu s$. From the power spectrum of Figure 8.30a, we see that the extraneous disturbance is in the frequency range of 0–19 kHz (Figure 8.32) with the broadcast (sinusoid) disturbance at 2.48 kHz. For our simulation, we chose amplitudes of 0.2 and 0.5, respectively. The simulated data and spectrum (without instrumentation noise) are shown in Figure 8.32a. Note that all of the amplitude data was scaled down from actual measurements. For the simulation, we chose a measurement model of

$$y(t_k) = 1.0 \cos(2\pi(2 \times 10^4 t_k + 3 \times 10^4 t_k^2)) + 0.5 \sin(2\pi 2.48 \times 10^4 t_k)$$
$$+0.25 e(t_k) + v(t_k)$$  (8.146)

for $v \sim \mathcal{N}(0, 1 \times 10^{-1})$. We show the simulated noisy measurement (input to the UKF-processor) and spectrum in Figure 8.32b with instrumentation noise included obscuring the chirp signal completely.

Next we applied the UKF processor to estimate the instantaneous frequency and compared it in steady state ($\tilde{f} = E{\hat{f}(t_k|t_k)}$) dynamically at each instant as shown in Figure 8.31. Here we see that the model can track the parameters as indicated by its estimate of the instantaneous frequency and the resulting chirp with the corresponding error. The UKF processor is "tuned," since the innovations statistics are zero-mean/white using the statistical zero-mean/whiteness test as well as the WSSR test indicating a reasonably tuned processor. The WSSR processor performs a dynamic whiteness test by sliding a finite length (parameter) window through the data and tests for whiteness as long as the statistic lies below the threshold (see Section 4.2 for details).

![](img/4bb66282061cdabd9d492d7740bb0233_479_0.png)

**Figure 8.31** Model-based CHIRP identification: (a) Estimated chirp. (b) Linear chirp: true, steady-state, dynamic.

## 8.5.3 Model-Based Identification: FSK Signals

In this subsection, we develop the model-based identifier for the FSK signal discussed earlier. Recall that the FSK signal can be parameterized as

$$s_m(t_k)=\alpha\cos 2\pi(f_o+m\Delta f)t_k,\qquad m=0,\ldots,M-1$$

(8.147)

where the amplitude is $\alpha=\sqrt{\frac{2\mathcal{E}_s}{\tau}}$, the carrier frequency is $f_o$ and the frequency separation is $\Delta f$. From the signal estimation perspective, we need to estimate only the amplitude and separation, since the carrier is well known as well as the number of transmitted frequencies $M$. Therefore, the model-based FSK *estimation* problem including the disturbances (as before) can be stated as

**GIVEN** a set of noisy measurement data $\{y(t_k)\}$, $k=0,1,\ldots,K$ contaminated with broadcast disturbances $\{d(t_k)\}$, extraneous disturbances $\{e(t_k)\}$, and Gaussian noise $\{v(t_k)\}$, **FIND** the best estimate of the FSK signal $\hat{s}(t_k)$ or equivalently the parameters $\{\hat{\alpha},\Delta\hat{f}\}$.

For this case, the measurement model is

$$y(t_k)=s(t_k)+d(t_k)+e(t_k)+v(t_k)$$

and the estimated FSK signal is obtained directly from its parameter estimates as

$$\hat{s}(t_k)=\hat{\alpha}\sum_{m=0}^{M-1}\cos 2\pi(f_o+m\Delta\hat{f})t_k\times G_{t_k}(\mathcal{T}_b)$$

Thus, the MBID solution to estimating the FSK in the midst of these disturbances again reduces to a problem of parameter estimation as before in the linear chirp case.

### 8.5.3.1 Gauss–Markov State-Space Model: FSK Signals

We perform a MBID applying the UKF (UKF) to this nonlinear problem by modeling the parameter variations as *random walks*, that is, in state-space form we have that

$$\Delta f(t_k)=\Delta f(t_{k-1})+w_f(t_{k-1})$$
$$\alpha(t_k)=\alpha(t_{k-1})+w_\alpha(t_{k-1})$$

where $\Delta f$ is the frequency separation parameter, $\alpha$ is the scalar amplitude, and $\mathbf{w}\in\mathbb{R}^{2\times1}$, a zero-mean, multivariate Gaussian vector process with covariance $\mathbf{R}_{\mathbf{w}}(t_k)$. Here $\mathbf{w}=[w_f\mid w_\alpha]'$. The corresponding measurement model is given in Eq. (8.148).

Generally, the UKF algorithm provides the updated signal (parameter) estimates as

$$\Delta\hat{f}(t_k|t_k)=\Delta\hat{f}(t_k|t_{k-1})+K_f(t_k)e(t_k|t_{k-1})$$
$$\hat{\alpha}(t_k|t_k)=\hat{\alpha}(t_k|t_{k-1})+K_\alpha(t_k)e(t_k|t_{k-1})$$
$$e(t_k|t_{k-1})=y(t_k)-\hat{y}(t_k|t_{k-1})-d(t_k)-e(t_k)$$
$$\hat{y}(t_k|t_{k-1})=\hat{s}(t_k|t_k)=\hat{\alpha}(t_k|t_{k-1})$$
$$\qquad\times\sum_{m=0}^{M-1}\cos 2\pi(f_o+m\Delta\hat{f}(t_k|t_{k-1}))t_k\times G_{t_k}(\mathcal{T}_b)$$
$$\mathbf{K}(t_k)=\mathbf{R}_{fe}(t_k|t_{k-1})\mathbf{R}_{ee}^{-1}(t_k|t_{k-1})$$

Since we are transmitting the FSK signal, we have a good estimate of the starting values for $\alpha$, $f_o$, $M$ that can be used in the signal estimation/detection problem. Recall that the processor is tuned when the innovations are zero-mean/white and not when these statistical conditions are not met. Therefore, the processor can be used as an *anomaly detector* (as before) when these conditions are not satisfied. We discuss this property subsequently.

Thus, the received data can be considered to consist of extraneous and broadcast disturbances along with random measurement noise.

