
but after substituting for $\mathcal{U}_{0|k-1}$ and applying the orthogonality conditions $U'_{\mathrm{N}}\mathcal{O}_{k}=0$ and $U'_{\mathrm{N}}L_{22}=0$, we have

$U'_{\mathrm{N}}\mathcal{T}_{k}(L_{11}Q'_{1})=U'_{\mathrm{N}}L_{21}Q'_{1}$

postmultiply by $Q_{1}$ and using its orthonormal property $(Q'_{1}Q_{1}=I)$ gives

$Z := U'_{\mathrm{N}}\mathcal{T}_{k}=U'_{\mathrm{N}}L_{21}\times L_{11}^{-1}$ (6.90)

analogous to Eq. (6.77) with $Z\to\mathcal{Z}$. Stacking and reordering the vectors (as before) leads to an overdetermined set of linear equations and a least-squares solution with $\tilde{Z}\to\tilde{\mathcal{Z}}$ to give

$\begin{bmatrix}\hat{D}\\--\\\hat{B}\end{bmatrix}=\mathcal{U}^{\#}\times\tilde{Z}$ (6.91)

where $\mathcal{U}$ is the block coefficient matrix equivalent to Eq. (6.81) ($\mathcal{U}\to\mathbf{U}$).

Therefore, we have the Multivariable Output Error State-Space (MOESP⁸) algorithm given by

+   - Compute the LQ-decomposition of $D$ of Eq. (6.82).
+   - Perform the SVD of $L_{22}$ in Eq. (6.87) to extract $\mathcal{O}_{k}$.
+   - Obtain $A$ and $C$ from Eq. (6.89).
+   - Solve the least-squares problem to obtain $B$ and $D$ from Eq. (6.91).

We summarize the deterministic MOESP algorithm in Table 6.3.

## 6.5 Deterministic Subspace Realization: Oblique Projections

Finally, for the deterministic realization problem from input/output data, we develop an alternative technique, the parallel or oblique projection method based on both past and future data. This approach differs from the MOESP-method in that it first estimates the state sequence directly from the data through a projection of the row space of block Hankel matrices. That is, in this approach, the row space of the state sequence can be obtained using the SVD; here the key element of oblique subspace identification is based on extracting:

$\mathcal{X}_{k}=[\mathbf{x}(k)\ \mathbf{x}(k+1)\ \cdots\ \mathbf{x}(K+k-1)]$ State–Space Sequence

8 Note that if the instrument variable is selected as both $\mathcal{U}_{0|k-1}$ and $\mathcal{Y}_{0|k-1}$, the past inputs and outputs, then the PO-MOESP technique evolves rather than the PI-MOESP [28].

and

```
U_{k|2k-1} := [ u(k) u(k+1) ... u(K+k-1); u(k+1) u(k+2) ... u(K+k); : : : :; u(2k-1) u(2k) ... u(K+2k-2) ] (6.93)
```

Leading to the corresponding data equation given by

```
Y_{k|2k-1} = O_k X_k + T_k U_{k|2k-1} (6.94)
```

where the block (Hankel) matrices are defined as before to give

```
Y_{k|2k-1} = [ y_k(k) y_k(k+1) ... y_k(K+k-1) ] ∈ R^{k N_y × K}
U_{k|2k-1} = [ u_k(k) u_k(k+1) ... u_k(K+k-1) ] ∈ R^{k N_u × K}
X_k = [ x(k) x(k+1) ... x(K+k-1) ] ∈ R^{N_x × K}
```

Here U_{0|k-1}, Y_{0|k-1}, X_0 are defined as the past inputs, outputs, and states (U_p, Y_p, X_0), while U_{k|2k-1}, Y_{k|2k-1}, X_k are the future inputs, outputs, and states (U_f, Y_f, X_f), which are all block Hankel matrices as before, that is,

```
U_p := U_{0|k-1};    Y_p := Y_{0|k-1};    X_p := X_0
U_f := U_{k|2k-1};    Y_f := Y_{k|2k-1};    X_f := X_k (6.95)
```

Similar to the input/output relations developed previously in Eq. (6.63), the corresponding state equations can be developed by expanding the solution of the deterministic state-space model:

```
x(k+j) = A^k x(j) + Σ_{ℓ=0}^{k-1} A^{k-ℓ-1} B u(ℓ + j) = A^k x(j) + A^{k-1} B u(j) + A^{k-2} B u(j+1) + ... + B u(k+j-1)
```

Expanding this relation over $K$-columns with $j = 0$, then

$$[x(k)\ \cdots\ x(K+k-1)] = A^k[x(0)\ \cdots\ x(K-1)] + C_{AB}^{\leftarrow}(k)[u_k(0)\ \cdots\ u_k(K-1)]$$

that is, we now have

$$\mathcal{X}_k = A^k\mathcal{X}_0 + C_{AB}^{\leftarrow}(k)\mathcal{U}_{0|k-1}$$

or in terms of our definitions (above), the set of future states in terms of past states $\mathcal{X}_p$, inputs $\mathcal{U}_p$ becomes

$$\mathcal{X}_f = A^k\mathcal{X}_p + C_{AB}^{\leftarrow}(k)\mathcal{U}_p$$

Therefore, the set of “deterministic” data equations for the past/future inputs/outputs/states are as follows:

$$\mathcal{Y}_p = \mathcal{O}_k\mathcal{X}_p + \mathcal{T}_k\mathcal{U}_p \qquad \text{(Past Outputs)}$$

$$\mathcal{Y}_f = \mathcal{O}_k\mathcal{X}_f + \mathcal{T}_k\mathcal{U}_f \qquad \text{(Future Outputs)}$$

$$\mathcal{X}_f = A^k\mathcal{X}_p + C_{AB}^{\leftarrow}(k)\mathcal{U}_p \qquad \text{(Future States)}$$

with the corresponding data matrices, past and future, defined by $D_p := D_{0|k-1}$ and $D_f := D_{k|2k-1}$ where

$$D_p = \begin{bmatrix}\mathcal{U}_p\\---\\\mathcal{Y}_p\end{bmatrix} \quad\text{and}\quad D_{0|k-1} = \begin{bmatrix}\mathcal{U}_{0|k-1}\\---\\\mathcal{Y}_{0|k-1}\end{bmatrix};$$

$$D_f = \begin{bmatrix}\mathcal{U}_f\\---\\\mathcal{Y}_f\end{bmatrix} \quad\text{and}\quad D_{k|2k-1} = \begin{bmatrix}\mathcal{U}_{k|2k-1}\\---\\\mathcal{Y}_{k|2k-1}\end{bmatrix}.$$

With these input/state/output relations available, we see the relationship between the future state vector with the corresponding past and future data as

$$\operatorname{span}\{\mathcal{X}_f\} = \operatorname{span}\{D_p\} \cap \operatorname{span}\{D_f\}$$

under the assumption of a persistently excited, completely controllable and observable system $(\rho(\mathcal{O}_k) = \rho(\mathcal{C}_k) = N_x)$ along with the constraint on the initial

![](img/4bb66282061cdabd9d492d7740bb0233_300_0.png)

**Figure 6.7** Oblique projections: Projection $\mathcal{P}^{\parallel}_{\mathcal{Y}_f|\mathcal{D}_p\circ\mathcal{U}_f}$ of future outputs $\mathcal{Y}_f$ onto past data $(\mathcal{D}_p)$ along the future inputs $(\mathcal{U}_f)$.

Another way to express this idea from an algebraic rather than geometric perspective is that the “vectors in the row space of $\mathcal{Y}_f$ are the sum of linear combinations of vectors in the row space of the state $\mathcal{X}_f$ and with those in the row space of the future inputs $\mathcal{U}_f$.”

In contrast, the oblique projection matrix can also be defined in terms of orthogonal projections as well [6, 8]

$\mathcal{P}^{\parallel}_{\mathcal{Y}_f|\mathcal{D}_p\circ\mathcal{U}_f} := [\mathcal{P}^{\perp}_{\mathcal{Y}_f|\mathcal{U}_f}]\,[\mathcal{P}^{\perp}_{\mathcal{D}_p|\mathcal{U}_f}]^{\#}\mathcal{D}_p = [\mathcal{P}^{\perp}_{\mathcal{Y}_f|\mathcal{U}_f}]\,[\mathcal{D}_p\mathcal{P}^{\perp}_{\mathcal{U}_f}]^{\#}\mathcal{D}_p$  (6.109)

Substituting Eq. (6.107) for the “future” output into this expression, we have

$\mathcal{P}^{\parallel}_{\mathcal{Y}_f|\mathcal{D}_p\circ\mathcal{U}_f} = [ (\mathcal{O}_k\Theta_p\mathcal{D}_p + \mathcal{T}_k\mathcal{U}_f)\mathcal{P}^{\perp}_{\mathcal{U}_f} ] [\mathcal{D}_p\mathcal{P}^{\perp}_{\mathcal{U}_f}]^{\#}\mathcal{D}_p$
$= \mathcal{O}_k\Theta_p[\mathcal{D}_p\mathcal{P}^{\perp}_{\mathcal{U}_f}][\mathcal{D}_p\mathcal{P}^{\perp}_{\mathcal{U}_f}]^{\#}\mathcal{D}_p$  (6.110)

or simply from Eq. (6.106), we have

$\mathcal{P}^{\parallel}_{\mathcal{Y}_f|\mathcal{D}_p\circ\mathcal{U}_f} = \mathcal{O}_k\Theta_p\mathcal{D}_p = \mathcal{O}_k\mathcal{X}_f$  (6.111)

This is the underlying concept of the N4SID-approach when applied to the deterministic realization problem. This relationship shows that obliquely projecting the future data nullifies the inputs, while enabling the extraction of the observability and future states directly from the given data. Therefore, the state vector provides a basis for the intersection of the past and future subspaces and can be extracted from this projection.

Performing the SVD on Eq. (6.111), we obtain the partitioned signal and noise (uncertainty) relationship as

```
P_{Y_f|D_p}^{\parallel} \circ U_f = [U_S \mid U_N] \begin{bmatrix} \Sigma_S & 0 \\ - - - & - - - \\ 0 & \Sigma_N \end{bmatrix} \begin{bmatrix} V_S' \\ -- \\ V_N' \end{bmatrix} = U_S \Sigma_S V_S' + U_N \Sigma_N V_N' \quad \text{Signal Subspace} \quad \text{Noise Subspace} \quad (6.112)
```

Assuming the uncertainties are “negligible” (Σ_N << Σ_S) and selecting a balanced deterministic realization (Σ_S = Σ_S^{1/2} × (Σ_S')^{1/2}), we obtain the critical N4SID subspace relations:

```
P_{Y_f|D_p}^{\parallel} \circ U_f \approx U_S \Sigma_S^{1/2} (\Sigma_S')^{1/2} V_S' \in \mathcal{R}^{kN_y \times K} \quad (Projection) \n\underbrace{O_k}_{O_k} \underbrace{X_f}_{X_f} \nO_k = U_S \Sigma_S^{1/2} \in \mathcal{R}^{kN_y \times N_x} \quad (Observability) \nX_f = (\Sigma_S')^{1/2} V_S' \in \mathcal{R}^{N_x \times K} \quad (States) \quad (6.113)
```

### 6.5.2 Numerical Algorithms for Subspace State-Space System Identification (N4SID) Algorithm

Again, a numerically efficient approach to extract the system Σ_ABCD from input/output data is to employ the LQ-decomposition of the block Hankel data matrices of the past D_p and future input/output data matrices {U_f, Y_f} that will lead us to the oblique projection of Y_f onto D_p along U_f [9, 34]

```
D := \begin{bmatrix} U_f \\ -- \\ D_p \\ -- \\ Y_f \end{bmatrix} = \underbrace{\begin{bmatrix} L_{11} & 0 & 0 \\ -- & -- & -- \\ L_{21} & L_{22} & 0 \\ -- & -- & -- \\ L_{31} & L_{32} & L_{33} \end{bmatrix}}_{L} \times \underbrace{\begin{bmatrix} Q_1' \\ -- \\ Q_2' \\ -- \\ Q_3' \end{bmatrix}}_{Q} \quad (6.114)
```

with L_{11} ∈ R^{kN_u × kN_u}, L_{21} ∈ R^{k(N_u+N_y) × kN_u}, L_{22} ∈ R^{k(N_u+N_y) × k(N_u+N_y)}, L_{31} ∈ R^{kN_y × kN_u}, L_{32} ∈ R^{kN_y × k(N_u+N_y)}, L_{33} ∈ R^{kN_y × kN_y}; and Q_1' ∈ R^{kN_u × K}, Q_2' ∈ R^{k(N_u+N_y) × K}, Q_3' ∈ R^{kN_y × K}.
