# 3. Exponential model:

\(\gamma_Y(\mathbf{h}, \boldsymbol{\theta}) = \begin{cases} 0, & \mathbf{h} = \mathbf{0}, \\ b_0 + b_1 [1 - \exp(-\|\mathbf{h}\|/b_2)], & \mathbf{h} \neq \mathbf{0}, \end{cases}\)

where \(\boldsymbol{\theta} = (b_0, b_1, b_2)'\) with \(b_i \ge 0\) for \(i = 1, 2, 3\).

In some applications, anisotropic variograms are needed if the underlying physical process evolves differently in space, that is, different dynamic dependences of the process occur in different directions in space. For instance, the spatial dependence of air pollutants is likely affected by the geographical features surrounding the monitoring stations so that it can be different in different directions. In some cases, the anisotropy can be corrected by an invertible linear transformation of the lag vector \(\mathbf{h}\). If \(2\gamma_Y(.)\) can be written as

\(2\gamma_Y(\mathbf{h}) = 2\gamma_Y^*(\|\mathbf{A}\mathbf{h}\|), \quad \mathbf{h} \in R^d, \quad (9.15)\)

where \(\mathbf{A}\) is an invertible \(d \times d\) matrix and \(\gamma_Y^*(.)\) has domain \(R\), it is said to be *geometrically anisotropic*.

# 9.3.3 Stationary Covariance Function

Instead of using variogram in Eq. (9.9), if the spatial process \(Y(\mathbf{s})\) satisfies the following two conditions:

\(C_y(\mathbf{h}) = \text{Cov}[Y(\mathbf{s} + \mathbf{h}), Y(\mathbf{s})], \quad \text{for all } \mathbf{s}, \mathbf{s} + \mathbf{h} \in D, \quad (9.16)\)

and

\(E[Y(\mathbf{s})] = \mu, \quad \text{for all } \mathbf{s} \in D, \quad (9.17)\)

then it is said to be a second-order stationary process in \(D\) with stationary covariance function \(C_y(.)\). Similarly to time series analysis, one can also use the correlation function

\(\rho_y(.) = C_y(.) / C_y(\mathbf{0})\)

in studying a covariance stationary spatial process. The covariance function \(C_y(.)\) must satisfy the nonnegative-definiteness condition

\(\sum_{i=1}^{\ell} \sum_{j=1}^{\ell} \alpha_i \bar{\alpha}_j C_y(\mathbf{s}_i - \mathbf{s}_j) \ge 0, \quad (9.18)\)

for any positive integer \(\ell\), any set of spatial locations \(\{\mathbf{s}_1, \dots, \mathbf{s}_\ell\}\), and any set of complex numbers \(\{\alpha_1, \dots, \alpha_\ell\}\). Note that the nonnegative-definiteness condition of Eq. (9.18) must hold for all complex numbers \(\{\alpha_i\}\), not only those that sum to zero. Consequently, stationary covariance function is stronger than stationary variogram. If \(C_y(\mathbf{h}) = C_y^*(\|\mathbf{h}\|)\), then it is said to be *isotropic*, otherwise it is *anisotropic*, where \(C_y^*(.)\) has domain \(R\). Note that the condition in Eq. (9.16), which implies that the process has a finite and constant variance, is not needed for a process to have a stationary variogram.