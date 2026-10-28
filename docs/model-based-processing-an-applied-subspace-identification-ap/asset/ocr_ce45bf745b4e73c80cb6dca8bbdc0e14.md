perspective; that is, the transfer functions and impulse responses are identical [6–8].

For complex modal case, which is quite common in structural dynamics, the eigenvalues are complex, but still distinct [4–8]. The system matrix can be decomposed using an eigen-decomposition, which now yields complex eigen-pairs along with the corresponding complex eigenvectors to specify the modal similarity transformation matrix $T_M$. Applying this transformation to the system matrix $A_c$ that leads to the modal state-transition matrix for the complex eigen-system as

$$\Phi_M(\tau,\tau_0)=e^{\Lambda(\tau-\tau_0)}=\exp\left\{\begin{bmatrix}\Lambda_1&&0\\&\ddots&\\0&&\Lambda_{N_d}\end{bmatrix}(\tau-\tau_0)\right\}$$

for
$$\Lambda_i=\begin{bmatrix}\sigma_i&\omega_i\\-\omega_i&\sigma_i\end{bmatrix}$$

Thus, the continuous-time, complex modal state-space system is given by

$$\dot{x}(\tau)=A_Mx(\tau)+B_Mu(\tau)$$
$$y(\tau)=C_Mx(\tau)+D_Mu(\tau)$$

where $A_M\in\mathbb{R}^{N_x\times N_x}$, $B_M\in\mathbb{R}^{N_x\times N_u}$, $C_M\in\mathbb{R}^{N_y\times N_x}$, $D_M\in\mathbb{R}^{N_y\times N_u}$ and $N_x=2\times N_d$.

Sampling this system leads to a discrete-time state-space representation that can be transformed back to the continuous-time domain for our application.

The generic linear, time-invariant *state-space* model is defined by its system matrix $A$, input transmission matrix $B$, output or measurement matrix $C$, and direct input feed-through matrix $D$ for *discrete-time* systems as

$$x(t+1)=Ax(t)+Bu(t)\qquad\text{(State)}$$
$$y(t)=Cx(t)+Du(t)\qquad\text{(Output)}$$

for the state $x\in\mathbb{R}^{N_x\times1}$, input $u\in\mathbb{R}^{N_u\times1}$, and output $y\in\mathbb{R}^{N_y\times1}$.

Corresponding to this representation is the *impulse response* termed the Markov parameters [4–8].

$$H_t=CA^{t-1}B+D\delta(t)$$

for $\delta$ the Kronecker delta function.

As in Chapter 3, expanding the state equations, we have

$$x(t)=A^tx(0)+\sum_{k=0}^{t-1}A^{t-k-1}Bu(k-1);\qquad t=0,1,\ldots,K$$