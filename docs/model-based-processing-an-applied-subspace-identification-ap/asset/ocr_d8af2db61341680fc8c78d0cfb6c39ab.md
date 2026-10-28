and accuracy of the model parameters. The measurements can be estimated from manufacturer’s specifications of the measurement instrumentation given by $K_r = -8.1 \times 10^{-5} \text{ kg Da}^{-1}$, $K_H = 0.5 \text{ kg Da}^{-1}$, $H = 2.5 \text{ m}$, $a = 0.021 \text{ m}^3$, $b = 0.328 \text{ m}^2$, $g = 9.8 \text{ m s}^{-2}$, $P = 0.173$, $m_p = 1448 \text{ kg m}^{-3}$, $m_m = 983 \text{ kg}$. The uncertainty in the measurements is approximated from typical specification. Using these model parameter values, we estimate the nominal $\Delta P_A$ and $\Delta P_B$ to be $3.6 \times 10^4$ and $3.0 \times 10^4 \text{ N m}^{-2}$. From the set of model parameters, the process and measurement models are given by

$\frac{d}{dt}x(t) = \begin{bmatrix} -1.43 \times 10^{-5} & 0 \\ 0 & 0 \end{bmatrix} x(t) + \begin{bmatrix} 1 \\ 0 \end{bmatrix} u(t) + w(t)$

and the measurement model as

$y(t) = \begin{bmatrix} 29.8 & -0.623 \\ 0 & 24.9 \end{bmatrix} x(t) + v(t)$

For our gauges, we have that the measurement covariance matrix is

$R_{vv} = \text{diag} \left[ 5.06 \times 10^4 \; 1.4 \times 10^5 \right] \quad (\text{N}^2 \text{ m}^{-4})$

We implement a discrete system using the matrix exponential series approximation for the discrete-time matrices, resulting in the discrete model at a sampling interval of 0.1 day as

$x(t) = \begin{bmatrix} 0.999 & 0 \\ 0 & 1 \end{bmatrix} x(t-1) + \begin{bmatrix} 1 \\ 0 \end{bmatrix} u(t-1) + w(t-1)$
$y(t) = \begin{bmatrix} 29.8 & -0.623 \\ 0 & 24.9 \end{bmatrix} x(t) + v(t)$
$R_{ww} = \text{diag} \left[ 10 \; 10 \right]$, 
$R_{vv} = \text{diag} \left[ 5.06 \times 10^4 \; 1.4 \times 10^5 \right]$ with initial conditions $\hat{x}(0|0) = \begin{bmatrix} 988 & 1455 \end{bmatrix}'$ and $	ilde{P}(0|0) = \text{diag} \left[ 0.01 \; 0.01 \right]$.

+   a) Using the discrete model, simulate the storage tank for a 25-day period and generate the measured outputs.
+   b) Using this synthesized data, identify a “black-box” state-space model from this data by a subspace method. Extract a stochastic realization $\Sigma_{INV}$.
+   c) Repeat this identification using a pseudo-random sequence as the deterministic input. Compare these identifications.
+   d) Design a Kalman filter from the “known” discrete state-space model and apply it to the data generated in (a).
+   e) Design a Kalman filter from the identified “black-box” state-space model and apply it to the data generated in (a).
+   f) Compare the Kalman filter designs. Do they perform equivalently? How do their design criteria compare (zero-mean, whiteness)?