# 3 State-Space Models for Identification

Let us consider a general *deterministic* formulation of a *nonlinear dynamic system* including the output (measurement) model in state-space form¹

$$\dot{x}_t = A(x_t,u_t) = a(x_t) + b(u_t)$$
$$y_t = C(x_t,u_t) = c(x_t) + d(u_t)$$

for $x_t$, $y_t$, and $u_t$ the respective $N_x$-state, $N_y$-output, and $N_u$-input vectors with corresponding system (process), input, measurement (output), and feedthrough functions. The $N_x$-dimensional system and input functions are defined by $a(\cdot)$ and $b(\cdot)$, while the $N_y$-dimensional output and feedthrough functions are given by $c(\cdot)$ and $d(\cdot)$.

In order to specify the solution of the $N_x$th-order differential equations completely, we must specify the above-noted functions along with a set of $N_x$-initial conditions at time $t_0$ and the input for all $t \ge t_0$. Here $N_x$ is the dimension of the “minimal” set of state variables.

If we constrain the state-space representation to be linear in the states, then we obtain the generic continuous-time, *linear, time-varying* state-space model given by

$$\dot{x}_t = A_t x_t + B_t u_t$$
$$y_t = C_t x_t + D_t u_t$$

(3.1)

where $x_t \in \mathbb{R}^{N_x\times1}$, $u_t \in \mathbb{R}^{N_u\times1}$, $y_t \in \mathbb{R}^{N_y\times1}$ and the respective system, input, output, and feedthrough matrices are: $A \in \mathbb{R}^{N_x\times N_x}$, $B \in \mathbb{R}^{N_x\times N_u}$, $C \in \mathbb{R}^{N_y\times N_x}$, and $D \in \mathbb{R}^{N_y\times N_u}$.

An interesting property of the state-space representation is to realize that these models represent a complete generic form for almost any physical system. That is, if we have an *RLC* circuit or a mass-damper-spring (MCK) mechanical system, their dynamics are governed by the identical set of differential equations – only their coefficients differ. Of course, the physical meaning of the states is different, but this is the idea behind state-space – *many physical systems* can be captured by this generic form of differential equations, even though the systems are physically different.

Systems theory, which is essentially the study of dynamic systems, is based on the study of state-space models and is rich with theoretical results exposing the underlying properties of the system under investigation. This is one of the major reasons why state-space models are employed in signal processing, especially when the system is *multivariable*, having multiple inputs and multiple outputs (MIMO). Next we develop the relationship between the state-space representation and input/output relations – the transfer function.

¹ We separate $x_t$ and $u_t$ for future models, but it is not really necessary.