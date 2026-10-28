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