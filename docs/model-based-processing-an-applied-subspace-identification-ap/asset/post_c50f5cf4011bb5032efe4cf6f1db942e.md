
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

with the output is given by

y(t) = CA^t x(0) + Σ_{k=0}^{t-1} CA^{t-k-1} B u(k-1) + D δ(t)  (8.8)

Expanding this relation further and collecting terms, we obtain

[ y(0) ; y(1) ; ⋮ ; y(K-1) ] = [ C ; CA ; ⋮ ; CA^{t-K-1} ]_{𝒪} x(0) + [ D ⋯ 0 ; CB D ⋯ 0 ; ⋮ ⋮ ⋱ ⋮ ; CA^{K-1}B ⋯ CB D ]_{𝒯} [ u(0) ; u(1) ; ⋮ ; u(K-1) ]  (8.9)

where 𝒪 is the observability matrix of linear systems theory of Section 3.4 [6–8].

Shifting these relations in time leads to the vector input/output relation:

y_K(ℓ) = 𝒪_K x(ℓ) + 𝒯_K u_K(ℓ)  (8.10)

Combining these m-vectors of Eq. (8.10) to create a batch data (block Hankel) matrix over the K-samples, we obtain input/state/output block matrices as

𝒴_{ℓ,m:K} := [ y_m(ℓ) ⋯ y_m(ℓ + K - 1) ]  (8.11)

𝒰_{ℓ,m:K} := [ u_m(ℓ) ⋯ u_m(ℓ + K - 1) ]  (8.12)

𝒳_{ℓ,m:K} := [ x_m(ℓ) ⋯ x_m(ℓ + K - 1) ] = [ x(ℓ) A x(ℓ) ⋯ A^{K-1} x(ℓ) ]  (8.13)

and, therefore, we have the data equation that relates the system model to the data (input and output matrices):

𝒴_{ℓ,m:K} = 𝒪_K 𝒳_{ℓ,m:K} + 𝒯_K 𝒰_{ℓ,m:K}  (8.14)

This expression represents the fundamental relationship for the input–state–output of a linear, time-invariant (LTI) state-space system.

## 8.1.1.1 State-Space Realization

In this subsection, we briefly review the deterministic state-space realization approach based on the system theoretic properties of the underlying Hankel array discussed in Section 6.2. The core of this suite of techniques is founded on the Hankel matrix defined in terms of the impulse response of a LTI system given by

H_t = { C A^{t-1} B for t > 0 ; D for t = 0 }  (8.15)

leading to the (N_x × 2N_x − 1)-Hankel matrix.

Thus, the *realization problem* is GIVEN a set of MIMO transfer function matrices $\{H_{ij}(z)\}$ or impulse response matrices $\{H_k(t)\}$ with corresponding Markov parameters, $H_k = CA^tB + D\delta(t);\ t = 0,1,\ldots,K-1$, FIND the underlying state-space system, $\Sigma := \{A,B,C,D\}$.

## 8.1.2 Deterministic State-Space Realizations

A solution to the *deterministic realization problem* is obtained by performing the SVD of the Hankel matrix, $\mathcal{H}_{m,K}$, where $m \geq N_x$ and $K \geq N_x$ with $N_x$ the dimension of the minimum realization (system) or equivalently the underlying *true* number of states; therefore,

![](img/4bb66282061cdabd9d492d7740bb0233_416_0.png)

From the factorization of the Hankel matrix and its SVD, we have that

![](img/4bb66282061cdabd9d492d7740bb0233_416_1.png)

where $\Sigma_{N_x}$ is the matrix square root obtained by a Cholesky decomposition [9].

From shifting properties (above) of the observability (or controllability) matrix, we can obtain the system matrices as [10–15]

![](img/4bb66282061cdabd9d492d7740bb0233_416_2.png)

where $N_x$ is the number of states, $N_u$ is the dimension of the input vector $u$, and $N_y$ that of the output or measurement vector $y$ with the notation $1:N\rightarrow1,2,\ldots,N$, or equivalently *select* the corresponding rows (columns) 1-to-N of the matrix.

This completes the fundamental background information required to develop the subsequent techniques developed.

### 8.1.2.1 Subspace Approach

The fundamental subspace identification approach is to extract the state-space representation from input/output data extending the realization from impulse response data – still assumed *deterministic*. Input/output data can be handled in a manner similar to the impulse response data discussed previously. In this case, we must return to the data matrices.

We are given input/output data corresponding to a LTI system with vector inputs u ∈ R^{N_u×1} and vector outputs y ∈ R^{N_y×1} along with discrete-time samples, t = 0, 1, …, K.

Suppose we have k-data samples such that k > N_x, then the corresponding block Hankel matrices can be created directly from Eq. (8.10) with the shift k to give both vector input/output (state) relations.

y_k(t) = O_k x(t) + T_k u(t) (8.21)

and the corresponding matrix input/output (state) equation as

Y_{k|2k-1} = O_k X_k + T_k U_{k|2k-1} (8.22)

where the matrices are defined in Eq. (8.10).

The initial states are given by

Y_{0|k-1} = O_k X_0 + T_k U_{0|k-1} (8.23)

Here U_{0|k-1}, Y_{0|k-1} are the past inputs and outputs, while U_{k|2k-1}, Y_{k|2k-1} are the future inputs and outputs, which are all block Hankel matrices [10–15].

The augmented, input/output, data matrix D along with its corresponding LQ-decomposition is constructed as

D_{0|k-1} = [U_{0|k-1}; Y_{0|k-1}] = [I | 0; - | -; T_k | O_k] [U_{0|k-1}; X_0] = [L11 | 0; - | -; L21 | L22] [Q1'; Q2'] (8.24)

These expressions enable the orthogonal decomposition of Y_{0|k-1}; therefore, it follows from Eq. (8.24) that (see Section 6.4 for details)

Y_{0|k-1} Q2 = O_k × X_0 × Q2 = L22 (8.25)

Therefore, performing the SVD of L22, that is,

L22 = [U1 | U2] [Σ1 | 0; - | -; 0 | 0] [V1'; --; V2'] (8.26)

yields

O_k X0 Q2 = U1 × Σ1 × V1' = (U1 Σ1^{1/2}) × (Σ1^{1/2} V1') with O_k underbrace and C_k underbrace (8.27)

and as before the system matrices are A, B, C, D can be extracted by

A = O_{N_s}^# × O_{N_s}^† ;   C = O(1 : N_y, 1 : N_x) (8.28)

The input transmission an direct feed-through matrices B and D can be obtained by solving a set of overdetermined linear equations (least-squares) from the following relations:

```
ℒ × [ Ď 
 -- 
 B̂ ] = U₂' L₂₁ L₁₁⁻¹  (8.29)
```

where ℒ is a block coefficient matrix (see Section 6.4 for the details).

Therefore, we have the Multivariable Output Error State-Space (MOESP) algorithm given by as follows:

- Compute the LQ-decomposition of D of Eq. (8.24).
- Perform the SVD of L₂₂ in Eq. (8.26) to extract 𝒪ₖ.
- Obtain A and C from Eq. (8.28).
- Solve the least-squares problem to obtain B and D from Eq. (8.29).

## 8.1.3 Vibrational System Processing

The overall approach to modal-frequency estimation/tracking is based on the development of robust subspace identification techniques that can be applied to solve this problem as rapidly as possible for potential real-time applications. The main idea is to preprocess a section or window of digitized data and perform a system (vibrational) identification followed by an extraction of the underlying modes from the identified model producing raw estimates of the corresponding modal frequencies. Once the “raw” modal frequencies in each window are extracted, a sequential tracking algorithm (Kalman filter) is applied to “smooth” the estimates that are eventually to be input to an anomaly detection algorithm for monitoring performance.

Typical processing of the system under investigation entails the following steps to:

- Preprocess (outliers, whitening, bandpass filtering, normalization) data relative to the targeted vibrational system information.
- Identify (subspace) underlying vibrational model from the data sets in state-space form Σ = {A, B, C, D}.
- Transform identified model to the modal state-space representation Σ_M = {A_M, B_M, C_M, D_M}.
- Extract modal frequencies and mode shapes from the modal state-space model.
- Track (Kalman filter) the raw frequencies sequentially in real time.
- Postprocess (outlier/tracker) the on-line tracking data.
- Estimate the modal frequency statistics to evaluate performance.
