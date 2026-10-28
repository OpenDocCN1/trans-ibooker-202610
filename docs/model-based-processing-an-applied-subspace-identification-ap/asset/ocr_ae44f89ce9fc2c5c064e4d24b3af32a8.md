where
$R_{ww} = \frac{(0.1)^2}{\Delta t} = 0.1\ (V)^2 \quad \text{and} \quad R_{vv} = 0.01\ (V)^2$

We first convert the system into a sampled-data (discrete) representation yielding the following discrete-time Gauss–Markov model:

$x(t) = \begin{bmatrix} 0.98 & 0.09 \\ -0.36 & 0.801 \end{bmatrix} x(t-1) + \begin{bmatrix} -0.019 \\ -0.36 \end{bmatrix} u(t-1) + \begin{bmatrix} -0.019 \\ -0.36 \end{bmatrix} w(t-1)$
$y(t) = [1 \ |\ 0] x(t) + v(t)$

where
$R_{ww} = 0.1 (V)^2 \quad \text{and} \quad R_{vv} = 0.01 (V)^2$

+   a) Using the discrete model, simulate the RLC circuit for a unit pulse train and generate the measured output voltage, $y(t)$ (measurement model).
+   b) Using this synthesized data, identify a “black-box” state-space model from this data using a subspace method. Extract a stochastic realization $\Sigma_{INV}$.
+   c) Repeat this identification using a pseudo-random sequence as the deterministic input. Compare these identifications.
+   d) Design a Kalman filter from the “known” discrete state-space model and apply it to the data generated in (a).
+   e) Design a Kalman filter from the identified “black-box” state-space model and apply it to the data generated in (a).
+   f) Compare the Kalman filter designs. Do they perform equivalently? How does their design criteria compare? (zero-mean, whiteness).

# 7.14 Consider the design of a processor for a storage tank containing plutonium-nitrate [32]. We would like to design Kalman filter capable of predicting the amount (mass) of plutonium present in the tank at any instant in order to estimate the total mass at the end of each day.

The level of solution in the tank at any time is attributed to the dynamics of evaporation of the water and nitric acid as well as radiolysis effects. The process model relating the solution mass $m(t)$ to the plutonium concentration $P(t)$, that is,
$\frac{d}{dt} m(t) = -K_H - K_r m(t) P(t)$

where $m$ is the solution mass (kg), $K_H$ is the evaporation rate of the water and acid solution, (kg d$^{-1}$), $K_r$ is the radiolysis constant, (kg (water)/day per kg ($Pu$)), $P$ is the plutonium concentration kg (Pu)/kg (solution).

Since the change in plutonium concentration is very slow due to the radiolysis effects, we assume that it is constant, $P(t) = P \quad \forall t.$