The *unscented transformation* (UT) is a technique for calculating the statistics of a random variable that has been transformed, establishing the foundation of the processor. A set of samples or *sigma-points* are chosen so that they capture the specific properties of the underlying distribution. Each of the σ-points is nonlinearly transformed to create a set of samples in the new space. The statistics of the transformed samples are then calculated to provide the desired estimates.

Consider propagating an $N_x$-dimensional random vector, **x**, through an arbitrary nonlinear transformation **a**[·] to generate a new random vector [24]

**z** = **a**[**x**]  (4.90)

The set of σ-points, {$\mathcal{X}_i$}, consists of $N_x + 1$ vectors with appropriate weights, {$W_i$}, given by $\Sigma = \{\mathcal{X}_i, W_i : i = 0, ..., N_x\}$. The weights can be positive or negative, but *must satisfy the normalization constraint* that

$\sum_{i=0}^{N_x} W_i = 1$

The problem then becomes
**GIVEN** these σ-points and the nonlinear transformation **a**[·], **FIND** the statistics of the transformed samples, $\mu_z \equiv E\{\mathbf{z}\}$ and $\mathbf{R}_{zz} = \text{Cov}(\mathbf{z})$

The UT approach is to

+   1) Nonlinearly *transform* each point to obtain the set of new σ-points: $\mathcal{Z}_i = \mathbf{a}[\mathbf{X}_i]$
+   2) Estimate the posterior *mean* by its weighted average: $\mu_{\mathbf{z}} = \sum_{i=0}^{N_x} W_i \mathcal{Z}_i$
+   3) Estimate the posterior *covariance* by its weighted outer product:
+   
+   $\mathbf{R}_{zz} = \sum_{i=0}^{N_x} W_i (\mathcal{Z}_i - \mu_z)(\mathcal{Z}_i - \mu_z)'$

The critical issues to decide are as follows: (i) $N_x$, the number of σ-points; (ii) $W_i$, the weights assigned to each σ-point; and (iii) where the σ-points are to be located. The σ-points should be selected to capture the “most important” properties of the random vector, **x**. This parameterization captures the *mean* and *covariance* information and permits the direct propagation of this information through the arbitrary set of nonlinear functions. Here we accomplish this (approximately) by generating the discrete distribution having the same first and second (and potentially higher) moments where each point is directly transformed. The mean and covariance of the transformed ensemble can then be computed as *the estimate* of the nonlinear transformation of the original distribution (see [7] for more details).

The UKF is a recursive processor developed to eliminate some of the deficiencies (see [7] for more details) created by the failure of the linearization process