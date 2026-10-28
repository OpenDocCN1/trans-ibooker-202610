
## MATLAB Notes

MATLAB offers a wealth of linear algebraic algorithms that can be applied to the subspace realization problem including commands to create the Hankel and Toeplitz matrices and their decompositions using the singular-value decomposition (svd), the Gram Schmidt, or QR decomposition that is easily transformed to the lower triangular/orthogonal LQ-transformation used throughout. Other techniques such as the Cholesky factorization for covariance matrices as well as the LDU decomposition methods extend these methods even further. The system identification toolbox (ident) offers a large variety of both subspace (n4sid) and prediction-error methods for state-space models including black-box as well as gray-box representations. Analysis commands enable the performance analysis of the identified models as well as well-founded validation methods. Using the embedded GUI allows the user to easily import the input/output data, perform preliminary preprocessing (e.g. mean and trend removal), partition the data for both identification and validation as well as enable instantaneous analysis of results performing signal estimation plots, residual analysis, frequency domain plots, and transient response analysis. This is quite a powerful toolbox for this problem set. Other implementations of subspace algorithms exist and can be extracted from the texts of [6] and [9].

## References

- 1 Kalman, R. (1963). Mathematical description of linear dynamical systems. SIAM J. Control 82: 152–192.
- 2 Kalman, R., Falb, P., and Arbib, M. (1969). Topics in Mathematical System Theory. New York: McGraw-Hill.
- 3 Ho, B. and Kalman, R. (1966). Effective reconstruction of linear state variable models from input/output data. Regelungstechnik 14: 545–548.
- 4 Candy, J. (2006). Model-Based Signal Processing. Hoboken, NJ: Wiley/IEEE Press.
- 5 Ljung, L. (1999). System Identification: Theory for the User, 2e. Englewood Cliffs, NJ: Prentice-Hall.
- 6 van Overschee, P. and De Moor, B. (1996). Subspace Identification for Linear Systems: Theory, Implementation, Applications. Boston, MA: Kluwer Academic Publishers.
- 7 Verhaegen, M. and Verdult, V. (2007). Filtering and System Identification: A Least-Squares Approach. Cambridge: Cambridge University Press.
- 8 Tangirala, A. (2015). Principles of System Identification: Theory and Practice. Boca Raton, FL: CRC Press.
- 29 Golub, G. and van Loan, C. (1996). *Matrix Computations*, 3e. Baltimore, MD: Johns Hopkins University Press.
- 30 Bretscher, O. (2015). *Linear Algebra with Applications*, 5e. Upper Saddle River, NJ: Pearson Prentice-Hall.
- 31 Sharf, L. (1991). *Statistical Signal Processing: Detection, Estimation and Time Series Analysis*. Reading, MA: Addison-Wesley.
- 32 Swevers, J., Adams, M., and De Moor, B. (1987). A new direct time domain identification method for linear systems using singular value decomposition. In: *Proceedings of the 12th International Seminar on Modal Analysis, 12-3*, 1–20.
- 33 De Moor, B., Vandewalle, J., Vandenberghe, L., and Mieghem, P. (1988). A geometrical strategy for the identification of state space models of linear multivariable systems with singular value decomposition. *Proc. IFAC 88*: 700–704.
- 34 Van Der Veen, A., Deprettere, E.F., and Swindlehurst, A.L. (1993). Subspace-based methods for the identification of linear time-invariant systems. *Proc. IEEE* 81 (9): 1277–1308.
- 35 Katayama, T. and Picci, G. (1994). Realization of stochastic systems with exogenous inputs and subspace identification methods. *Automatica* 35 (10): 75–93.
- 36 Astrom, K. and Eykhoff, P. (1971). System identification—a survey. *Automatica* 7 (2): 123–162.
- 37 Ljung, L. and Soderstrom, T. (1983). *Theory and Practice of Recursive Identification*. Boston, MA: MIT Press.
- 38 Akaike, H. (1974). A new look at the statistical model identification. *IEEE Trans. Autom. Control* 19 (6): 716–723.
- 39 Sakamoto, Y., Ishiguro, M., and Kitagawa, G. (1986). *Akaike Information Criterion Statistics*. Boston, MA: D. Reidel/Kluwer Academic.
- 40 Bauer, D. (2001). Order estimation for subspace methods. *Automatica* 37: 1561–1573.
- 41 Pham Gia, T. and Hung, T. (2001). The mean and median absolute deviations. *Math. Comput. Model.* 34: 921–936.
- 42 Zoubi, A. and Iskander, D. (2004). *Bootstrap Techniques for Signal Processing*. Cambridge: Cambridge University Press.
- 43 Reynders, E. (2012). System identification methods for (operational) modal analysis: review and comparison. *Arch. Comput. Methods Eng.* 19 (1): 51–124.
- 44 Cara, F., Juan, J., Alarcon, E. et al. (2013). Modal contribution and state space order selection in operational modal analysis. *Mech. Syst. Sig. Process.* 38: 276–298.

## Problems

6.1 Classical realization theory is based on the factorization of the Hankel matrix of MIMO impulse response matrices into the full-rank, observability, and controllability matrices, $\mathcal{H}_{K,K'} = \mathcal{O}_K \times \mathcal{C}_{K'}$, show that each row (column) of $\mathcal{H}_{K,K'}$ is dependent if and only if it is a dependent row (column) of $\mathcal{O}_K$ ($\mathcal{C}_{K'}$). (Proof of this result forms the foundation for canonical realization theory.)

6.2 Suppose we are given the following state-space description of a continuous-time system with [10]

$\dot{x}(t) = \begin{bmatrix} -1 & -4/\theta \\ 4\theta & -2 \end{bmatrix} x(t) + \begin{bmatrix} 1 \\ 2\theta \end{bmatrix} u(t)$
$y(t) = \begin{bmatrix} -1 & 2/\theta \end{bmatrix} x(t)$

The continuous-time (subscript “c”) observability and controllability Gramians satisfy the following Lyapunov equations:

$A_c' \mathcal{O}_c + \mathcal{O}_c A_c = -C_c' C_c \qquad \text{(Continuous Gramians)}$
$A_c C_c + C_c A_c' = -B_c B_c'$

- (a) Calculate the observability and controllability Gramians.
- (b) Is this a balanced realization? Show results.
- (c) If so, what values of $\theta$ are required?

6.3 Suppose we are given the transfer function

$H(s) = \frac{4s^2 - 2s - 6}{2s^4 + 2s^3 + 2s^2 + 3s + 1}$

- (a) What order is this system? Verify your results.
- (b) Find a balanced realization using the Ho–Kalman approach?

6.4 We are given the following continuous system:

$\dot{x}(t) = A x(t)$

with

$A = \begin{bmatrix} -4 & -4 \\ 2 & -6 \end{bmatrix}$

(a) Develop the solution to the corresponding *Lyapunov equation*:

\[A'P + PA = -Q\qquad \text{(Lyapunov Equation)}\]

with \(Q=I\).

(b) Is this continuous system stable? Show results.

(c) Validate stability using the calculated eigenvalues.

### 6.5

Consider the following scalar example with impulse response, \(H(k)=\{1,\ 1/2,\ 1/4,\ 1/8,\ 1/16\}\) with \(N=5\). Using the SVD approach, we would like to extract the realization \(\Sigma=(A,b,c)\). Develop the Hankel matrix and apply the SVD–Ho algorithm to extract the system.

### 6.6

We choose \(K>N_x\), then perform an SVD of a given \(\mathcal{H}_{K,K}\) and select a *balanced realization* such that

\[\mathcal{H}_{K,K}=U\Sigma V'=\bigl(U(\Sigma_{N_x}^{1/2})\bigr)\times\bigl((\Sigma_{N_x}^{1/2})V'\bigr)=\mathcal{O}_K\times\mathcal{C}_K\]

where \(N_x\) is the rank of the Hankel matrix. Using the shift-invariant property of \(\mathcal{H}_{K,K}\to\mathcal{H}^{\dagger}_{K,K}\), show that if we select the first \(N_1\) of the \(N_x\)-singular values (\(N_1<N_x\)), then we can extract the \(N_1\)-dimensional realization \(\Sigma_{A_1B_1C_1D_1}\) from the \(N_x\)th. Does this imply that we can extract *all* lower-order systems \((N<N_x)\) from a single \(N_x\)-realization as submatrices? Show this? Are these realizations “nested” (\(A_1\in A_{N_x}\))?

### 6.7

Suppose we have the following discrete transfer function:

\[H(z)=\frac{z+3}{z^2+3z+2}\]

- (a) What is impulse response of this system? (*Hint:* Long division)
- (b) Create the Hankel matrix from the impulse response.
- (c) Using the original Ho–Kalman algorithm, extract the system \(\Sigma_{ABCD}\).
- (d) Using the SVD–Ho–Kalman algorithm, also extract the system \(\Sigma_{ABCD}\).
- (e) Are the realizations equivalent? (*Hint:* Eigenvalues? Impulse responses?)

### 6.8

We are given the following discrete system represented by the transfer function

\[H(z)=\begin{bmatrix}\dfrac{z-0.5}{z^2-1} & \dfrac{1}{z-1}\\[4pt] \dfrac{z^2}{z^2-1} & \dfrac{2z}{z-1}\end{bmatrix}\]

- (a) Calculate the impulse response of this system.
- (b) Create the Hankel matrix.
