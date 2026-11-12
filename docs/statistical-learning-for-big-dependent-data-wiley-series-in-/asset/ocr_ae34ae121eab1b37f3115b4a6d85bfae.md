Thus, \(\boldsymbol{w}_t\) follows a stationary VARMA(1,2) model. However, from Eq. (3.67) and denoting the MA matrix polynomial of \(\boldsymbol{w}_t\) by \(\theta^*(B)\), we can see that

\(|\theta^*(1)| = \begin{vmatrix} 1 - 0.5 - 0.005 & -0.5 + 0.245 \\ 0.495 & 1 - 1 - 0.255 \end{vmatrix} = \begin{vmatrix} 0.495 & -0.255 \\ 0.495 & -0.255 \end{vmatrix} = 0.\)

This implies that one is a solution of the determinant equation \(|\theta^*(B)| = 0\). Consequently, the VARMA(1,2) model for \(\boldsymbol{w}_t\) is non-invertible.

The fact that differencing individual components of a co-integrated vector process with co-integrating rank less than the dimension results in a non-invertible VARMA model is referred to as *over-differencing* in the time series literature. Over-differencing can lead to complications in modeling co-integrated processes. For instance, since \(\boldsymbol{w}_t\) is non-invertible, it does not have a finite-order VAR approximation. Therefore, one often needs a high-order VAR model to achieve good approximation in modeling \(\boldsymbol{w}_t\) in practice. Another complication is that the solutions of the ML function of a non-invertible model are on the boundary of the parameter space and, hence, the conventional asymptotic theory for MLE would not apply. Interested readers are referred to Tanaka (2017, chapter 7) and the references therein.

To avoid over-differencing, the idea of error-correction was proposed in Engle and Granger (1987). Details are given in the next section.

# 3.6 ERROR-CORRECTION MODELS

Consider the VARMA model in Eq. (3.1), but assume that 1 might be a solution of the determinant equation \(|\phi(B)| = 0\). In other words, we assume \(\boldsymbol{z}_t\) can be an integrated process of order 1. Let \(\nabla \boldsymbol{z}_t = \boldsymbol{z}_t - \boldsymbol{z}_{t-1} = (I - B)\boldsymbol{z}_t\) be the first-differenced series of \(\boldsymbol{z}_t\). By straightforward algebraic calculation, e.g. Tsay (2014, chapter 5), we have

\(\nabla \boldsymbol{z}_t = \Pi \boldsymbol{z}_{t-1} + \sum_{i=1}^{p-1} \Phi_i^* \nabla \boldsymbol{z}_{t-i} + \boldsymbol{c}_0 + \theta(B)\boldsymbol{a}_t, \quad (3.68)\)

where \(\Pi = \sum_{i=1}^p \Phi_i - I = -\phi(1)\) and \(\Phi_i^* = -(\phi_{i+1} + \cdots + \phi_p)\) for \(i = 1, \dots, p-1\). Eq. (3.68) is called an *error-correction model* (ECM) for the integrated process \(\boldsymbol{z}_t\). Note that this model is invertible because we do not alter the MA matrix polynomial. However, if \(|\phi(1)| = 0\), then the coefficient matrix \(\Pi\) is singular. In general, we have the following cases:

+   1. If rank(\(\Pi\)) = 0, then \(\Pi = \mathbf{0}\) and \(\boldsymbol{z}_t\) has \(k\) unit roots. It is not co-integrated.
+   
+   2. If rank(\(\Pi\)) = \(k\), then \(\Pi\) is nonsingular and \(\boldsymbol{z}_t\) is a stationary process.
+   
+   3. If rank(\(\Pi\)) = \(m\) with \(1 \le m < k\), then \(\boldsymbol{z}_t\) has \(\nu = k - m\) unit roots and there exist \(m\) co-integrating vectors. In this case, one can write \(\Pi = \boldsymbol{\alpha}\boldsymbol{\beta}'\), where both \(\boldsymbol{\alpha}\) and \(\boldsymbol{\beta}\) are \(k \times m\) real-valued matrices of rank \(m\). Let \(\boldsymbol{w}_t = \boldsymbol{\beta}'\boldsymbol{z}_t\), which is an \(m\)-dimensional stationary process. In other words, columns of \(\boldsymbol{\beta}\) are the co-integrating vectors of \(\boldsymbol{z}_t\). Note that \(\boldsymbol{\alpha}\boldsymbol{\beta}' = (\boldsymbol{\alpha}\boldsymbol{P})(\boldsymbol{P}\boldsymbol{\beta})'\) for any \(m \times m\) orthogonal matrix \(\boldsymbol{P}\). Therefore, co-integrating vectors are not uniquely defined if \(m > 1\). The column space of \(\boldsymbol{\beta}\) is unique, however.