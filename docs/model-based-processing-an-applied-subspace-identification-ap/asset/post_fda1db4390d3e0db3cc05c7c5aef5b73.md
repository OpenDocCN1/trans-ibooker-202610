
![](img/4bb66282061cdabd9d492d7740bb0233_468_0.png)

**Figure 8.23** Modal function tracking with adaptive modal coefficient estimation: raw experimental data (DATA), unscented Kalman filter (UKF), maximum a posteriori (MAP) (circles), and conditional mean (CM) (squares) particle filters.

Figure 8.24 Adaptive modal coefficient parameter estimation data (MODEL) from the Hudson Canyon experiment using the MAP particle filter (Parameter Est.).

![](img/4bb66282061cdabd9d492d7740bb0233_470_0.png)

Figure 8.25 Raw pressure-field data/enhanced data (DATA) from the Hudson Canyon experiment for a 23-element hydrophone vertical array using particle filter estimators with adaptive wavenumber processing: maximum a posteriori (MAP), conditional mean (CM), and the corresponding innovations (ERROR) sequence.

We also illustrate the multimodal aspect of the oceanic data by observing the modal function posterior probability PDF estimates for mode 5 illustrated in Figure 8.27. It is clear from the plots that for each depth multiple peaks appear in the posterior estimates. The wavenumber PDF estimate corresponding to mode 5 is shown in Figure 8.28. Again we note the multiple, well-defined peaks in the posterior distribution leading to the MAP parameter estimate.

This completes the analysis of the synthesized Hudson Canyon experiment and the PF processing performance.

## 8.4.5 Summary

In this study, we have discussed the development of environmentally adaptive processors capable of tracking modes and enhancing the raw pressure-field measurements obtained from a vertical hydrophone array in shallow water. The parametric adaption was based on simultaneously estimating either the

![](img/4bb66282061cdabd9d492d7740bb0233_471_0.png)

Figure 8.26 Modal function tracking for adaptive wavenumber estimation: Hudson Canyon data (MODEL) of a 23-element array, unscented Kalman filter (UKF), maximum a posteriori (MAP), and conditional mean (CM)(squares) particle filters.

![](img/4bb66282061cdabd9d492d7740bb0233_471_1.png)

Figure 8.27 Probability mass function (PMF) posterior estimation (mode 5) surface for Hudson Canyon 23-element array data (particle vs time vs probability).

## Mode no. 5: wavenumber posterior distribution

![](img/4bb66282061cdabd9d492d7740bb0233_472_0.png)

Figure 8.28 Probability mass function (PMF) posterior estimation (wavenumber 5) surface for Hudson Canyon 23-element array data (particle vs time vs probability).

modal coefficients or the horizontal wavenumbers along with the modes and pressure-field as the environmental parameters of interest. These wavenumber parameters were more challenging from a processor design perspective because of their increased sensitivity to environmental change compared to the modal coefficients. We chose a Bayesian sequential design because of the varying nature of the shallow ocean and applied a normal-mode model in state-space form to create a forward propagator. The algorithms applied were the UKF and the particle filter, both modern approaches applied to this problem (see Chapter 4). We compared their performance and found slightly better results of the PF over a 100-member ensemble. These results with more details can be found in a recent paper [48].

## 8.5 MBID for Chirp Signal Extraction

Chirp signals have evolved primarily from radar/sonar signal processing applications specifically attempting to estimate the location of a target in surveillance/tracking volume [49, 50] and have recently been introduced to the nondestructive evaluation (NDE) area [51]. The chirp, which is essentially a sinusoidal signal whose phase changes instantaneously at each time sample,

has an interesting property in that its correlation approximates an impulse function [52, 53]. It is well known that a matched-filter detector in radar/sonar estimates the target range by cross-correlating a replicant of the transmitted chirp with the measurement data reflected from the target back to the radar/sonar receiver, yielding a maximum peak corresponding to the echo time and therefore enabling the desired range estimate.

In this application, we perform the same operation as a radar or sonar system, that is, we transmit a “chirp-like pulse” into the target medium and extract it from noisy measurements. Our problem is complicated by the presence of disturbance signals from surrounding broadcast stations as well as extraneous sources of interference in our frequency bands and, of course, the ever-present random instrumentation noise [51]. First, we model the two signals of high interest: the chirp and FSK signals and then develop a model-based identifier to extract (estimate) them from noisy data.

### 8.5.1 Chirp-like Signals

#### 8.5.1.1 Linear Chirp

A chirp signal is a sinusoidal signal characterized by its amplitude α and phase φ, that is,

```
s(t) = α × cos(φ(t))
```

where the instantaneous phase is defined by

```
φ(t) = 2πf(t) + φ(tₒ)
```

for f(t) the corresponding instantaneous frequency. The rate of change of the phase is defined by its derivative

```
d/dt φ(t) = 2πf'(t) =: ωᵢ(t)
```

where ωᵢ is defined as the instantaneous angular radian frequency.

The frequency can be characterized by a number of different relations, one of which is the “linear” relation

```
f(t) = β × t + fₒ
```

where fₒ is the initial (sweep) frequency at time tₒ and β is the rate given by

```
β = (f_f − fₒ) / t_f
```

where f_f is the final (sweep) frequency in the time window bounded by the final (sweep) time t_f.

For the linear chirp, the corresponding phase is found by integrating Eq. (8.132):

$$\phi(t)=\phi(t_o)+2\pi\int_{t_o}^{t} f(\alpha)\,d\alpha=\phi(t_o)+2\pi\left(f_o t+\frac{\beta t^2}{2}\right)$$ (8.135)

The corresponding *sinusoidal chirp* signal is then given by (see spectrum in Figure 8.29a)

$$s(t)=\alpha\cos\left(2\pi\left(f_o\cdot t+\frac{\beta}{2}\cdot t^2\right)\right)$$ (8.136)

For our problem, we have disturbances and noise that contaminate the measurement, that is, assuming a sampled-data representation $(t=t_k)$, we have

$$y(t_k)=s(t_k)+d(t_k)+e(t_k)+v(t_k)$$ (8.137)

![](img/4bb66282061cdabd9d492d7740bb0233_474_0.png)

![](img/4bb66282061cdabd9d492d7740bb0233_474_1.png)

**Figure 8.29** True chirp and FSK spectra (20–30 kHz sweep): (a) Chirp spectrum. (b) FSK spectrum ($\Delta f=1.25$ kHz-steps).

where $s$ is the transmitted chirp, $d$ is potential broadcast disturbances, $e$ is an extraneous disturbances, and $v$ is a zero-mean, Gaussian random noise process with variance $R_{vv}(t_k)$. A chirp signal and spectrum with disturbances and noise is shown in Figure 8.30a for a chirp signal swept between 20 and 30 kHz with disturbances and noise illustrated in (b).

### 8.5.1.2 Frequency-Shift Key (FSK) Signal

An alternative for a linear frequency modulation (FM) chirp is FSK modulation deemed the simplest form of FM [54] with the spectrum illustrated in Figure 8.29b. In the $M$-ary case, $M$ different frequencies are used to transmit a coded information sequence. The choice of frequency separation $\Delta f$ is defined by the FSK-signal

$$s_m(t_k)=\sqrt{\frac{2\mathcal{E}_s}{\mathcal{T}}}\cos 2\pi(f_0+m\Delta f)t_k,\qquad m=0,\ldots,M-1 \tag{8.138}$$

where $\mathcal{E}_s=m\times\mathcal{E}_b$ is defined as the energy/symbol, $\mathcal{T}=m\times\mathcal{T}_b$ is the symbol interval and $\Delta f=f_m-f_{m-1}$ with $\mathcal{E}_b$ is the signal energy/bit and $\mathcal{T}_b$ is the duration of the bit interval. Usually, $M$-ary FSK is used to transmit a block of $n=\log_2 M$ bits/signal. Here the $M$-FSK signals have equal energy $\mathcal{E}_s$.

The frequency separation $\Delta f$ determines the degree of discrimination possible of the $M$ transmitted signals. Each of these signals can be represented as orthogonal unit-vectors $\mathbf{u}_m$ scaled by $\sqrt{\mathcal{E}_s}$, that is, $\mathbf{s}_m:=\sqrt{\mathcal{E}_s}\mathbf{u}_m$, where the basis functions are defined $b_m(t_k)=\sqrt{\frac{2}{\mathcal{T}}}\cos 2\pi(f_0+m\Delta f)t_k$. The minimum distance between the pairs of signal vectors is given by $d=\sqrt{\mathcal{E}_s}$. We will incorporate this FSK representation to develop our model-based identifier.

In order to represent the FSK signal in the time domain, we must introduce the gate function $G_{t_k}(\Delta\tau)$ defined

$$G_{t_k}(\Delta\tau):=\mu(t_k-m\Delta\tau)-\mu(t_k-(m+1)\Delta\tau);\qquad m=0,\ldots,M-1 \tag{8.139}$$

for $\mu(\cdot)$ a unit-step function and $\Delta\tau=\frac{1}{\Delta f}$ the reciprocal of the frequency separation or equivalently $\mathcal{T}_b$ the bit interval duration. With this operator available, we can now represent the temporal FSK signal as

$$\begin{aligned}s(t_k)&=\sum_{m=0}^{M-1}s_m(t_k)\times G_{t_k}(\mathcal{T}_b)\\&=\sqrt{\frac{2\mathcal{E}_s}{\mathcal{T}}}\sum_{m=0}^{M-1}\cos 2\pi(f_0+m\Delta f)t_k\,[\mu(t_k-m\mathcal{T}_b)-\mu(t_k-(m+1)\mathcal{T}_b)]\end{aligned}\tag{8.140}$$

This model will be used to extract the FSK signal.
