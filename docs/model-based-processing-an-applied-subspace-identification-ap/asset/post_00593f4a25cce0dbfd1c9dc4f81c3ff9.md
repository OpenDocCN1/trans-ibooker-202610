
![](img/4bb66282061cdabd9d492d7740bb0233_424_0.png)

**Figure 8.3** Postprocessing modal frequency tracking ensemble statistics: (a) Modal frequency scatter plot with 99.9% confidence limits about the mean. (b) Modal frequency histogram indicating high probabilities in identified modal frequency bins.

Figure 8.3b, we observe the corresponding modal frequency histogram with most of the identified frequency bins heavily populated, indicating very high probabilities of the modal frequencies estimated by the subspace tracker.

## 8.1.5 Summary

This study has illustrated the development of a model-based modal tracking scheme capable of on-line processing of structural vibrational responses. By applying both system identification methods to extract a modal model and state estimation techniques to track the modal frequencies, a sequential anomaly detector was also developed subsequently and applied successfully to noisy anomalous data (see Ref. [3] for details).

## 8.2 MBID for Scintillator System Characterization

Figure 8.4 Postprocessed modal frequency tracking of (a) 12-mode, (b) 13-mode, (c) 14-mode identified state-space model results.

Scintillator/photomultiplier systems are prevalent in many instruments to measure a variety of nuclear events. When γ energy from a nuclear event irradiates the scintillator, event radiation interacts with the scintillator material generating photons, which are detected by the photomultiplier tube (PMT)

the absorption of a single photoelectron, thereby amplifying the original photoelectron from stage to stage with a typical gain factor of $10^7$ [18]. These electrons are then collected at the anode of the PMT producing the output voltage pulse. It is this pulse that is of most interest, since it is proportional to the original incident photon energy emitted from the unknown source.

The shape of the voltage pulse at the PMT output is governed by the prompt fluorescence intensity of the scintillator crystal at a time $t$ following the initial excitation with an exponential decay time $\tau_s$ such that the intensity is given (simply) by

$$i(t)=i_0 e^{-t/\tau_s}$$  (8.33)

which is usually a few nanoseconds. A full description of the output pulse must also take the rise time at the anode into account, which is usually around 3–4 times larger (faster) than its fall $\tau_a$. Then, the overall voltage output pulse of the scintillator system is given by [17]

$$i(t)=i_0\left(e^{-t/\tau_a}-e^{-t/\tau_s}\right)$$  (8.34)

Using the parameters for a plastic scintillator from [17] ($\tau_s=1.7$ ns, $\tau_a=0.2$ ns, $i_0=1$), the resulting pulse is shown in Figure 8.6. Next, we develop the model of Eq. (8.34) from an equivalent electrical circuit representation.

### 8.2.1 Scintillation Pulse Shape Model

Following Knoll [17], the shape of the voltage pulse at the anode of the PMT following an event depends on the time constant of the anode circuit and the decay time of the scintillator. For this application, we would like a very fast output pulse, and, therefore, the design calls for the anode circuit time constant to be much smaller than that of the scintillator decay time. The equivalent anode circuit (ideal) can be realized as a parallel resistor/capacitor connection with the input current modeling the fluorescence as a simple exponential as in Eq. (8.33). Here the lumped capacitance ($C$) represents that of the anode and cable connections as well as the circuit input capacitance, while the resistance ($R$) is a physical resistor or the input impedance of the circuit load. The input electron current arriving at the PMT anode is given by Eq. (8.33) with the initial current as a function of the total charge collected and given by

$$i(t)=\frac{Q}{\tau_s}$$  (8.35)

Thus, writing the node equation using Kirchhoff’s current law, we have that total input current is the sum of the currents through the resistance and capacitance as

$$i(t)=i_R(t)+i_C(t)=C\frac{de(t)}{dt}+\frac{e(t)}{R}$$  (8.36)

![](img/4bb66282061cdabd9d492d7740bb0233_428_0.png)

![](img/4bb66282061cdabd9d492d7740bb0233_428_1.png)

Figure 8.6 Typical scintillator/photomultiplier response: (a) Pulse. (b) Bode plot.

The solution of this differential equation follows using Laplace transforms to give the output voltage signature for the scintillator as

$e(t) = K_{as}[e^{-t/\tau_a} - e^{-t/\tau_s}] \quad \text{for} \quad K_{as} = \frac{Q}{C}\left(\frac{\tau_a}{\tau_a - \tau_s}\right)$
(8.37)

![](img/4bb66282061cdabd9d492d7740bb0233_429_0.png)

# (a)

# Photo cathode:

# Anode:

H_s(s):= Y_s(s)/ξ̃(s) = ( K_S / (s + 1/τ_s) )

h_s(t) = K_s e^{-t/τ_s}

H_a(s):= E(s)/Y_s(s) = ( K_a / (s + 1/τ_a) )

h_a(t) = K_a e^{-t/τ_a}

# (b)

Figure 8.7 Cascaded scintillator/photomultiplier system model: (a) Diagram. (b) Transfer function/impulse response.

Thus, the output voltage pulse e(t) has the identical form of the physical model of Eq. (8.34).

### 8.2.2 Scintillator State-Space Model

In order to develop the simulation for the scintillator, we choose to use the state-space representation by developing the underlying differential equation governing our scintillator model of Eq. (8.36), which is easily obtained from the cascade of subsystems shown in Figure 8.7.

The photocathode system is governed by the differential equation that evolves from the transfer function

```
di(t)/dt = -1/τ_s i(t) + K_s ξ̃(t)      (Photoelectron Current)
y_s(t) = i(t)                         (Output Current)      (8.38)
```

where ξ̃ is the input excitation (assumed to be an impulse-like signal).

The anode of the photomultiplier is modeled by the RC-circuit and is governed by the Kirchhoff current relation of Eq. (8.36)

```
de(t)/dt = 1/τ_a e(t) + K_i(t)  for τ_a = RC      (Anode Voltage)
y_a(t) = e(t)                                   (Output Voltage)      (8.39)
```

If we define the state vector x := [i(t) | e(t)]′, then we obtain the vector–matrix equation

ẋ(t) = [ -1/τ_s | 0 ; - - - ; 0 | -1/τ_a ] x(t) + [ K_s ; - ; K_a/C ] u(t)    (States)
y(t) = [0 | 1] x(t)    (Measurement)    (8.40)

or simply

d x(t)/dt = A_c x(t) + B_c u(t)
y(t) = C_c x(t)    (8.41)

the usual state-space representation of a LTI system with A_c, B_c, C_c the respective N_x × N_x, N_x × N_u, N_y × N_x continuous-time system, input and output matrices [9] with the corresponding impulse response, H_t = C_c e^{A_c (t-τ)} B_c and transfer function given by H(s) = C_c (sI − A_c)^{-1} B_c completing the state-space representation of the scintillator–photomultiplier system.

### 8.2.3 Scintillator Sampled-Data State-Space Model

Sampled-data A/D conversion implies that t → t_k and therefore the state-transition matrix is Φ(t_k, t_0) = e^{A_c (t_k - t_0)} with state solution [19, 20]

x(t_k) = Φ(t_k, t_{k-1}) x(t_{k-1}) + ∫_{t_{k-1}}^{t_k} Φ(t_k, τ) B_c u(τ) dτ    (8.42)

Define the sampled-data system with the input assumed piecewise-constant, that is, u(t_{k-1}) ≤ u(t) < u(t_k) and B := ∫_{t_{k-1}}^{t_k} Φ(t_k, τ) B_c dτ leading to the discrete sampled-data system given by

x(t_k) = A x(t_{k-1}) + B u(t_{k-1})    (8.43)

where A := e^{A_c (t_k - t_{k-1})} = e^{A Δt_k} for Δt_k := t_k - t_{k-1} with B := ∫_{t_{k-1}}^{t_k} e^{A_c (t_k - τ)} dτ × B_c.

The corresponding sampled measurement system given by

y(t_k) = C x(t_k)    (8.44)

We developed an impulse response simulation of this system, and the results are shown in Figure 8.8, where we see the states, the current i and the voltage v and the output y.

![](img/4bb66282061cdabd9d492d7740bb0233_431_0.png)

Figure 8.8 Deterministic state-space model output for cascaded scintillator/photomultiplier system simulation.

### 8.2.4 Gauss–Markov State-Space Model

Since we are using digitized data, then a sampled-data representation of this continuous-time system will be a better approach to characterizing this system. Therefore, using the same approach of the deterministic system given above, we obtain

**x**(tₖ) = A**x**(tₖ₋₁) + B**u**(tₖ₋₁) + **w**(tₖ₋₁)
**y**(tₖ) = C**x**(tₖ) + v(tₖ)  (8.45)

where both additive process and measurement noises are zero-mean, Gaussian processes with respective covariance matrices R_ww(tₖ₋₁), R_vv(tₖ). The corresponding statistics are given by (see [9] for more details)

**m**ₓ(tₖ) = A**m**ₓ(tₖ₋₁) + B**u**(tₖ₋₁)
**P**ₓₓ(tₖ) = A**P**ₓₓ(tₖ₋₁)A' + R_ww(tₖ₋₁)
**m**ᵧ(tₖ) = C**m**ₓ(tₖ)
**R**ᵧᵧ(tₖ) = C**P**ₓₓ(tₖ)C' + R_vv(tₖ)  (8.46)
