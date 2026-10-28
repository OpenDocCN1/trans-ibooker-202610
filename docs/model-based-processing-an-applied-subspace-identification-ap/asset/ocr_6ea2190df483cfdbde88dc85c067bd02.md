Linear, time-invariant multiple input/output (MIMO) mechanical systems are characterized by the vector–matrix differential equations that can be expressed as

$$M\ddot{\mathbf{d}}(t) + C_d\dot{\mathbf{d}}(t) + K\mathbf{d}(t) = B_p\mathbf{p}(t)\tag{6.31}$$

where $\mathbf{d}$ is the $N_d \times 1$ displacement vector, $\mathbf{p}$ is the $N_p \times 1$ excitation force, and $M$, $C_d$, $K$, are the $N_d \times N_d$ lumped mass, damping, and spring constant matrices characterizing the vibrational process model, respectively. The structure of these matrices, typically, takes the form as

$$M = \begin{bmatrix} M_1 & 0 & 0 & 0 & 0 \\ 0 & M_2 & 0 & 0 & 0 \\ \vdots & \vdots & \ddots & \vdots & \vdots \\ 0 & 0 & 0 & M_{N_d-1} & 0 \\ 0 & 0 & 0 & 0 & M_{N_d} \end{bmatrix}, \quad C_d = [C_{d_q}], \text{ and }$$

$$K = \begin{bmatrix} (K_1+K_2) & -K_2 & 0 & 0 & 0 \\ -K_2 & (K_2+K_3) & \ddots & 0 & 0 \\ 0 & \ddots & \ddots & -K_{N_d-1} & 0 \\ 0 & 0 & -K_{N_d-1} & (K_{N_d-1}+K_{N_d}) & -K_{N_d} \\ 0 & 0 & 0 & -K_{N_d} & K_{N_d} \end{bmatrix}$$

If we define the $2N_d$-state vector as $\mathbf{x}(t) := [\mathbf{d}(t)\quad|\quad \dot{\mathbf{d}}(t)]'$, then the continuous-time state-space representation of this process can be expressed as

$$\dot{\mathbf{x}}(t) = \underbrace{\begin{bmatrix} 0 & | & I \\ --- & | & --- \\ -M^{-1}K & | & -M^{-1}C_d \end{bmatrix}}_{A} \mathbf{x}(t) + \underbrace{\begin{bmatrix} 0 \\ --- \\ M^{-1}B_p \end{bmatrix}}_{B} \mathbf{p}(t)\tag{6.32}$$

The corresponding measurement or output vector relation can be characterized by

$$\mathbf{y}(t) = \mathbf{C_a}\ddot{\mathbf{d}}(t) + \mathbf{C_v}\dot{\mathbf{d}}(t) + \mathbf{C_d}\mathbf{d}(t)\tag{6.33}$$

where the constant matrices: $\mathbf{C_a}$, $\mathbf{C_v}$, $\mathbf{C_d}$ are the respective acceleration, velocity, and displacement weighting matrices of appropriate dimension.

In terms of the state vector relations of Eq. (6.32), we can express the acceleration vector as

$$\ddot{\mathbf{d}}(t) = -M^{-1}K\mathbf{d}(t) - M^{-1}C_d\dot{\mathbf{d}}(t) + M^{-1}B_p\mathbf{p}(t)\tag{6.34}$$

Substituting for the acceleration term in Eq. (6.33), we have that

$$\mathbf{y}(t) = -\mathbf{C_a}M^{-1}[B_p\mathbf{p}(t) - C_d\dot{\mathbf{d}}(t) - K\mathbf{d}(t)] + \mathbf{C_v}\dot{\mathbf{d}}(t) + \mathbf{C_d}\mathbf{d}(t)$$