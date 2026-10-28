It is also important to realize that “shifting” the Hankel matrix preserves its rank $N_x$. If $H_t = CA^{t-1}B; t = 1, ..., 2N_x$, then it follows that the corresponding rank

$\rho(\mathcal{H}_{N_x,N_x}) = \rho(\mathcal{H}_{N_x+1,N_x}) = \rho(\mathcal{H}_{N_x,N_x+1}) = N_x$ (6.12)

is satisfied [9].

Finally, the realization problem based on an infinite Markov sequence $\{H_t\}_0^\infty$ leads to $\mathcal{H}_{\infty,\infty}$ with rank preserved

$\rho(\mathcal{H}_{\infty,\infty}) = \sup_t \rho(\mathcal{H}_{K,K})$

that is satisfied by a minimal realization with $K = N_x$.

Another interesting system theoretic property that evolves from the observability, controllability, and Hankel matrices is the concept of a balanced realization. Simply stated, a balanced realization is one in which “every state is as controllable as it is observable” [12]. It is a critical property in model order reduction characterizing certain features that uniquely capture the foundation of the underlying system.

We know that if a system is completely observable and controllable, then it is minimal and the Hankel matrix factors as in Eq. (6.7). Therefore, for a stable system, both observability and controllability Gramians must satisfy the following positivity constraints, that is, the Gramian matrices $\Sigma_{AC}$, $\Sigma_{AB}$ are positive definite satisfying the Lyapunov equations:

$\Sigma_{AC} = A'\Sigma_{AC}A + C'C \ge 0$ (Observability Gramian)

$\Sigma_{AB} = A\Sigma_{AB}A' + BB' \ge 0$ (Controllability Gramian) (6.13)

Alternatively, we can express these constraints in terms of the observability and controllability matrices as follows:

$\Sigma_{AC}(K) = \sum_{k=0}^{K} (A^k)'C'C A^k = \mathcal{O}_K' \mathcal{O}_K \ge 0$ (Observability Gramian)

$\Sigma_{AB}(K) = \sum_{k=0}^{K} A^k B B' (A^k)' = C_K C_K' \ge 0$ (Controllability Gramian) (6.14)

Performing a singular-value decomposition (SVD) of the observability and controllability matrices, we obtain

$\mathcal{O}_K = U_\mathcal{O}(K)\Sigma_\mathcal{O}(K)V_\mathcal{O}'(K)$ and $C_K = U_C(K)\Sigma_C(K)V_C'(K)$ (6.15)