
## References

12 Norton, J. (1986). *An Introduction to Identification.* New York: Academic Press.

13 Kalman, R. (1960). A new approach to linear filtering and prediction problems. *Trans. ASME J. Basic Eng.* 82: 34–45.

14 Van Der Veen, A., Deprettere, E.F., and Swindlehurst, A.L. (1993). Subspace-based methods for the identification of linear time-invariant systems. *Proc. IEEE* 81 (9): 1277–1308.

15 Viberg, M. (1995). Subspace based signal analysis using singular value decomposition. *Automatica* 31 (12): 1835–1851.

16 van Overschee, P. and De Moor, B. (1996). *Subspace Identification for Linear Systems: Theory, Implementation, Applications.* Boston, MA: Kluwer Academic Publishers.

17 Verhaegen, M. and Verdult, V. (2007). *Filtering and System Identification: A Least-Squares Approach.* Cambridge: Cambridge University Press.

18 Katayama, T. (2005). *Subspace Methods for System Identification.* London: Springer.

## Problems

**1.1** We are asked to estimate the displacement of large vehicles (semitrailers) when parked on the shoulder of a freeway and subjected to wind gusts created by passing vehicles. We measure the displacement of the vehicle by placing an accelerometer on the trailer. The accelerometer has inherent inaccuracies, which is modeled as

```
y = K_a x + n
```

with y, x, n the measured and actual displacement and white measurement noise of variance \(R_{nn}\), and \(K_a\) the instrument gain. The dynamics of the vehicle can be modeled by a simple mass–spring–damper. (a) Construct and identify the measurement model of this system. (b) Construct and identify the process model and model-based estimator for this problem.

**1.2** Think of measuring the temperature of a liquid in a beaker heated by a burner. Suppose we use a thermometer immersed in the liquid and periodically observe the temperature and record it. (a) Construct a measurement model assuming that the thermometer is linearly related to the temperature, that is, \(y(t) = k \Delta T(t)\). Also model the uncertainty of the visual measurement as a random sequence \(v(t)\) with variance \(R_{vv}\). (b) Suppose we model the heat transferred to the liquid from the burner as

```
Q(t) = C A ΔT(t)
```

where C is the coefficient of thermal conductivity, A is the cross-sectional area, and \(\Delta T(t)\) is the temperature gradient with assumed random uncertainty \(w(t)\) and variance \(R_{ww}\). Using this process model and the models developed above, identify the model-based processor representation.

**1.3** We are given an RLC series circuit driven by a noisy voltage source \(V_{in}(t)\) and we use a measurement instrument that linearly amplifies by K and measures the corresponding output voltage. We know that the input voltage is contaminated by an additive noise source, \(w(t)\), with covariance \(R_{ww}\), and the measured output voltage is similarly contaminated with noise source, \(v(t)\), with \(R_{vv}\).  
(a) Determine the model for the measured output voltage, \(V_{out}(t)\) (measurement model).  
(b) Determine a model for the circuit (process model).  
(c) Identify the general model-based processor structures. In each scheme, specify the models for the process, measurement, and noise.

**1.4** A communications satellite is placed into orbit and must be maneuvered using thrusters to orientate its antennas. Restricting the problem to the single axis perpendicular to the page, the equations of motion are

```
J d²θ/dt² = T_c + T_d
```

where J is the moment of inertia of the satellite about its center of mass, \(T_c\) is the thruster control torque, \(T_d\) is the disturbance torque, and \(\theta\) is the angle of the satellite axis with respect to the inertial reference (no angular acceleration). Develop signal and noise models for this problem and identify each model-based processor component.

**1.5** Consider a process described by a set of linear differential equations

```
d²c/dt² + dc/dt + c = K m
```

The process is to be controlled by a proportional–integral–derivative (PID) control law governed by the equation

```
m = K_p ( e + 1/T_i ∫ e dt + T_d de/dt )
```

and the controller reference signal r is given by

```
r = e + c
```

Suppose the reference is subjected to a disturbance signal, and the measurement sensor, which is contaminated with additive noise, measures the "square" of the output. Develop the model-based signal and noise models for this problem.

**1.6** The elevation of a tracking telescope is controlled by a DC motor. It has a moment of inertia J and damping B due to friction, and the equation of motion is given by

```
J d²θ/dt² + B dθ/dt = T_m + T_d
```

where \(T_m\) and \(T_d\) are the motor and disturbance torques and \(\theta\) is the elevation angle. Assume a sensor transforms the telescope elevation into a proportional voltage that is contaminated with noise. Develop the signal and noise models for the telescope and identify all of the model-based processor components.

**1.7** Suppose we have a two-measurement system given by

```
y = [3; 4] x + v
```

where \(R_{vv} = diag[1, 0.1]\).

(a) What is the batch least-squares estimate (W = I) of the parameter x, if \(y = [7; 21]\)?

(b) What is the batch weighted least-squares estimate of the parameter x with W selected for minimum variance estimation?

**1.8** Calculate the batch and sequential least-squares estimate of the parameter vector x based on two measurements \(y(1)\) and \(y(2)\) where

```
y(1) = C(1)x + v(1) = [2; 1]
```

```
y(2) = c'x + v(2) = 4
```

```
C(1) = [1 1; 0 1],  c'(2) = [1 2],  W = I
```

## 2 Random Signals and Systems

### 2.1 Introduction

We can apply a filter to a random signal, but since its output is still random, we must find a way to eliminate or reduce this randomness in order to employ the powerful techniques available from systems theory. We shall show that techniques from statistics combined with linear systems theory can be applied to extract the desired signal information and reject the disturbance or noise. In this case, the filter is called an estimation filter or simply an estimator that is required to extract the useful information (signal) from noisy or random measurements.

Techniques similar to linear deterministic systems theory hold when the random signal is transformed to its covariance sequence and its Fourier spectrum is transformed to its power spectrum. Once these transformations are accomplished, then the techniques of linear systems theory can be applied to obtain results similar to deterministic signal processing. In fact, we know that the covariance sequence and power spectrum are a discrete Fourier transform (DFT) pair, analogous to a deterministic signal and its corresponding spectrum, that is, we have that

```
Fourier Transform: R_xx(k) ↔ S_xx(Ω)
```

and as in the deterministic case, we can analyze the spectral content of a random signal by investigating its power spectrum.

The power spectral density (PSD) function for a discrete random process is defined as:

```
S_xx(Ω) = lim_{N→∞} E { X(Ω)X*(Ω) / (2N + 1) }    (2.1)
```

where * is the complex conjugate. The expected value operation, \(E\{\cdot\}\), can be thought of as "mitigating" the randomness. Similarly, the covariance¹ of the ... is depicted in Figure 2.2b. Therefore, analogous to deterministic signal processing techniques, the covariance and power spectra enable the powerful methods of linear systems theory to be applied to extract useful signal information for noisy data.

In this chapter, we are concerned with the development of analysis methods for random signals that will prove useful in applications to follow.

¹ It is also common to use the so-called correlation function, which is merely the mean-squared function and identical to the covariance function for a mean of zero.

### 2.2 Discrete Random Signals

A random signal or equivalently a stochastic process finds its roots in probability theory. The idea of an underlying probability or sample space is essential to the treatment of random signals. Recall that a probability/sample space, \(\Xi\), is a collection of samples or experimental outcomes, \(\xi\), which are elements of that space. Certain subsets of \(\Xi\) or collections of outcomes are the events. For example, if we flip a fair coin, then the sample space consists of the set of outcomes, \(\Xi = \{H, T\}\) and the events, \(\{\emptyset, \{H\}, \{T\}\}\). When we attempt to specify how likely a particular event will occur during a trial or experiment, then we define the notion of probability. In terms of our probability space, we define a probability function, \(Pr(\cdot)\), as a function defined on a class of events, which assigns a number to the likelihood of a particular event occurring during an experiment. We must constrain the class of events on which the probability function is defined so that set operations performed produce sets that are still events. Therefore, the class of sets satisfying this constraint is called a field (Borel). If we have a space, \(\Xi\), a field, \(B\), and a probability function, \(Pr(\cdot)\), we can precisely define an experiment as the triple, \(\{\Xi, B, Pr\}\). Stochastic processes can be thought of as a collection of indexed random variables. More formally, a random signal or stochastic process is a two-dimensional function of t and \(\xi\):

```
X(t, ξ),   ξ ∈ Ξ,   t ∈ T    (2.6)
```

where T is a set of index parameters (continuous or discrete) and \(\Xi\) is the sample space.

So we see that a discrete random signal can be precisely characterized by

```
x_i(t) := X(t, ξ_i)    (2.7)
```

A realization of a stochastic process and in fact each distinct value of t can be interpreted as a random variable. Thus, we can consider a stochastic process as simply a sequence of ordered random variables. A collection of realizations of a stochastic process is called an ensemble.

A common representation of a random process is a random walk given in the following example.
