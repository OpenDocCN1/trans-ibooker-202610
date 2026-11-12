After $r$ iterations, we obtain two $n \times r$ matrices $\mathbf{O}$ and $\mathbf{U}$ and two loading matrices $\mathbf{P}$ and $\mathbf{Q}$. We then relate $\mathbf{Y}$ to $\mathbf{X}$ by the linear regression

$\mathbf{U} = \mathbf{O}\boldsymbol{\beta} + \boldsymbol{\epsilon},$

where $\boldsymbol{\epsilon}$ denotes the error term. The fitted value of $\mathbf{Y}$ is then

$\widehat{\mathbf{Y}} = \widehat{\mathbf{U}}\mathbf{Q}' = \mathbf{O}\widehat{\boldsymbol{\beta}}\mathbf{Q}' = \mathbf{X}\mathbf{P}\widehat{\boldsymbol{\beta}}\mathbf{Q}', \quad (7.43)$

where $\mathbf{O} = \mathbf{XP}$ of PCA is used.

To gain insight into the NIPALS algorithm, consider

$\mathbf{p} = \mathbf{X}'\mathbf{u}/\|\mathbf{X}'\mathbf{u}\|$
\(= \mathbf{X}'\mathbf{Y}\mathbf{q}/\|\mathbf{X}'\mathbf{Y}\mathbf{q}\|\)
\(= \mathbf{X}'\mathbf{Y}\mathbf{Y}'\mathbf{o}/\|\mathbf{X}'\mathbf{Y}\mathbf{Y}'\mathbf{o}\|\)
\(= \mathbf{X}'\mathbf{Y}\mathbf{Y}'\mathbf{X}\mathbf{p}/\|\mathbf{X}'\mathbf{Y}\mathbf{Y}'\mathbf{X}\mathbf{p}\|\)
\(= \frac{1}{\lambda}(\mathbf{Y}'\mathbf{X})'(\mathbf{Y}'\mathbf{X})\mathbf{p}. \quad (7.44)\)

Clearly, $\mathbf{p}$ is an eigenvector of the sample covariance matrix of $\mathbf{Y}'\mathbf{X}$. The algorithm in Eq. (7.44) is exactly the updating rule in the Power method for computing the largest eigenvalue-eigenvector of the symmetric matrix $\mathbf{X}'\mathbf{Y}\mathbf{Y}'\mathbf{X}$.\

Besides the NIPALS algorithm, there are alternative methods available to compute the PLS regression. See, for instance, de Jong (1993). Helland (1990) provides an explanation of PLS from a prediction point of view, which is easy to follow when $\mathbf{Y}$ is a scalar dependent variable.

# **PLS algorithm 1: (PLS1)**

+   1. Define the starting values of the $\mathbf{X}$ residuals, $\boldsymbol{\eta}_0$, and $y$ residuals $\mathbf{f}_0$:

\(\boldsymbol{\eta}_0 = \mathbf{X} - \boldsymbol{\mu}_x,\)
\(\mathbf{f}_0 = \mathbf{Y} - \boldsymbol{\mu}_y,\)

where $\boldsymbol{\mu}_X$ denotes the matrix consisting of mean vector of $\mathbf{x}_t$ and $\mu_y$ is the vector consisting of mean of dependent variable $y_{t+h}$. In other words, $\boldsymbol{\eta}_0$ and $\mathbf{f}_0$ are the mean-adjusted $\mathbf{X}$ and $\mathbf{Y}$.\

For $i = 1, 2, \dots$, do the steps 2 to 4 below:

+   2. Introduce the $\mathbf{X}$-scores, $\mathbf{o}_i$, as linear combinations of the $\mathbf{X}$ residuals $\boldsymbol{\eta}_{i-1}$ using the covariances between $\boldsymbol{\eta}_{i-1}$ and $\mathbf{f}_{i-1}$ as weights:

\(\mathbf{o}_i = \boldsymbol{\eta}'_{i-1}\mathbf{w}_i, \quad \text{with } \mathbf{w}_i = \text{Cov}(\boldsymbol{\eta}_{i-1}, \mathbf{f}_{i-1}).\)

+   3. Determine $\mathbf{X}$ loading $\mathbf{p}_i$ and $\mathbf{Y}$ loading $\mathbf{q}_i$ by least squares:

\(\mathbf{p}_i = \text{Cov}(\boldsymbol{\eta}_{i-1}, \mathbf{o}_i)/\text{Var}(\mathbf{o}_i),\)
\(\mathbf{q}_i = \text{Cov}(\mathbf{f}_{i-1}, \mathbf{o}_i)/\text{Var}(\mathbf{o}_i).\)

That is, regress $\boldsymbol{\eta}_{i-1}$ and $\mathbf{f}_{i-1}$ on $\mathbf{o}_i$ to obtain the LS estimates.