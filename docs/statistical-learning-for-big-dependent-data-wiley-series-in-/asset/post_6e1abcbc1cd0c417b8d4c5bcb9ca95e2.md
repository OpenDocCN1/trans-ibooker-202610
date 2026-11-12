
![](img/d4c84ae3c2178f4c506af3dd2ed13122_45_0.png)

**Figure 1.17** Time plot of 300 realizations from a time-varying parameter autoregressive model of order 1 in Eq. (1.20), where $\phi(t/T) = 0.2 + 0.4(t/T) - 0.2(t/T)^2$ and $\sigma(t/T) = \exp(t/T)$.

mainly concerned with the covariance matrix, not the lag covariance matrices, which are important in time series analysis. Therefore, the limiting properties of the eigenvalues and eigenvectors of PCA are different for time series data. See a recent paper by Zhang and Tong (2020) and some discussions in Section 1.6. Also, we present in Chapter 6 a more general procedure, called the dynamic principal component analysis (DPCA) for time series data, that takes into account the lag dependence of a vector time series.

There are two main ways to introduce PCA. The first way is variance decomposition, and the second one optimal reconstruction or interpolation. We start with the variance decomposition of independent data at the population level. Let $\boldsymbol{z} = (z_1, ..., z_k)'$ be a $k$-dimensional random vector with mean zero and positive-definite covariance matrix $\mathbf{\Gamma}(0)$. The first population principal component (PC) of $\boldsymbol{z}$ is a linear combination $y_1 = \boldsymbol{c}'_1 \boldsymbol{z}$ satisfying $\boldsymbol{c}'_1 \boldsymbol{c}_1 = 1$ such that $\text{Var}(y_1)$ attains the maximum among all possible linear combinations of $\boldsymbol{z}$ built with vectors of unit length. The second PC of $\boldsymbol{z}$ is defined as a linear combination of $\boldsymbol{z}$, say $y_2 = \boldsymbol{c}'_2 \boldsymbol{z}$ satisfying (i) $\boldsymbol{c}'_2 \boldsymbol{c}_2 = 1$ and (ii) $\boldsymbol{c}'_2 \boldsymbol{c}_1 = 0$ such that $\text{Var}(y_2)$ assumes the maximum variance among all linear combinations of $\boldsymbol{z}$ built with vectors of unit length and such that the second PC is uncorrelated with the first PC. In general, the $i$th PC of $\boldsymbol{z}$ is a non-zero linear combination of $\boldsymbol{z}$, say $y_i = \boldsymbol{c}'_i \boldsymbol{z}$ using a vector of unit length that has the maximum variance among all linear combinations of $\boldsymbol{z}$ and that is uncorrelated to all the previous PCs, that is, $\boldsymbol{c}_i$ satisfies (iii) $\boldsymbol{c}'_i \boldsymbol{c}_i = 1$ and (iv) $\boldsymbol{c}'_i \boldsymbol{c}_j = 0$ for $j = i - 1, ..., 1$. The unit length normalization, $\boldsymbol{c}'_i \boldsymbol{c}_i = 1$, is needed to control the scaling effect; otherwise the maximum has no meaning.

Using properties of positive-definite matrices given in the Appendix, the PCA of a stationary process $\boldsymbol{z}_t$ with positive-definite covariance matrix $\mathbf{\Gamma}(0)$ can be obtained from the spectral decomposition of $\mathbf{\Gamma}(0)$. Specifically, let $(\lambda_i, \boldsymbol{e}_i)$ be the $i$th eigenvalue-eigenvector pair of $\mathbf{\Gamma}(0)$, where the eigenvalues satisfy $\lambda_1 \ge \lambda_2 \ge \cdots \ge \lambda_k$. The spectral decomposition of $\mathbf{\Gamma}(0)$ is

```
$\mathbf{\Gamma}(0) = \lambda_1 \boldsymbol{e}_1 \boldsymbol{e}'_1 + \cdots + \lambda_k \boldsymbol{e}_k \boldsymbol{e}'_k.$
```

Then, the $i$th PCA is $y_{it} = \boldsymbol{e}'_i \boldsymbol{z}_t$. From the definition, $\text{Var}(y_{it}) = \text{Var}(\boldsymbol{e}'_i \boldsymbol{z}_t) = \boldsymbol{e}'_i \Gamma(0) \boldsymbol{e}_i = \lambda_i \boldsymbol{e}'_i \boldsymbol{e}_i = \lambda_i$. Since $\text{tr}[\Gamma(0)] = \sum_{i=1}^k \sigma_{ii} = \sum_{i=1}^k \lambda_i$, the proportion of variability of $\boldsymbol{z}$ explained by its $i$th PC is $\lambda_i / (\sum_{j=1}^k \lambda_j)$. Similarly, the proportion of variability of $\boldsymbol{z}_t$ explained by the first $m$ PCs is $(\sum_{i=1}^m \lambda_i) / (\sum_{j=1}^k \lambda_j)$. If a small $m$ can be found such that the first $m$ PCs explain a large proportion of the variability in $\boldsymbol{z}$, then one can focus analysis on the first $m$ PCs.

Suppose now that, instead of independent variables, we have a random realization $\{\boldsymbol{z}_1, \dots, \boldsymbol{z}_T\}$ of a stationary vector time series $\boldsymbol{z}_t$. For simplicity, we assume that the process is mean-adjusted, i.e. $\sum_{t=1}^T \boldsymbol{z}_t / T = \boldsymbol{0}$. Define the $T \times k$ matrix $\boldsymbol{Z} = [\boldsymbol{z}_{(1)}, \dots, \boldsymbol{z}_{(k)}]$, where $\boldsymbol{z}_{(j)} = (z_{j1}, \dots, z_{jT})'$ is the vector of observations of the $j$th component $z_{jt}$. Then, the covariance matrix of $\boldsymbol{z}_t$ is estimated by the sample covariance matrix $\widehat{\Gamma}(0) = \boldsymbol{Z}' \boldsymbol{Z} / T$ and the eigenvalue-eigenvector pairs $(\lambda_i, \boldsymbol{c}_i)$ of this matrix can be used to construct PCs. The first PC is given by $\boldsymbol{y}_1 = \boldsymbol{Z} \boldsymbol{c}_1$, where

$$\frac{1}{T} \boldsymbol{Z}' \boldsymbol{Z} \boldsymbol{c}_1 = \lambda_1 \boldsymbol{c}_1, \quad \boldsymbol{c}'_1 \boldsymbol{c}_1 = 1.$$

Pre-multiplying the above equation by $\boldsymbol{Z}$, we have

$$\frac{1}{T} \boldsymbol{Z} \boldsymbol{Z}' \boldsymbol{y}_1 = \lambda_1 \boldsymbol{y}_1. \tag{1.21}$$

Therefore, the first PC $\boldsymbol{y}_1$ of $\boldsymbol{z}_t$ is proportional to the eigenvector corresponding to the largest eigenvalue of the matrix $\boldsymbol{Z} \boldsymbol{Z}'$. Other PCs can be obtained in a similar manner.

An alternative approach to introduce PCA is from an optimal reconstruction or interpolation of the data. For simplicity, we consider the sample version with the mean-adjusted stationary time series data as before, i.e. $\{\boldsymbol{z}_1, \dots, \boldsymbol{z}_T\}$. We want to obtain a new time series, $f_{1t}$, defined as a linear combination of $\boldsymbol{z}_t$ that minimizes the error in the reconstruction of the observed data. More formally, let $f_{1t} = \boldsymbol{z}'_t \boldsymbol{\beta}_1$ and $\boldsymbol{f}_1 = \boldsymbol{Z} \boldsymbol{\beta}_1$, where $\boldsymbol{\beta}_1 = (\beta_{11}, \dots, \beta_{k1})'$ and $\boldsymbol{f}_1 = (f_{11}, \dots, f_{1T})'$. The coefficients $\beta_{i1}$ are obtained by minimizing the objective function

$$L(\boldsymbol{\beta}_1, \boldsymbol{f}_1) = \sum_{t=1}^T \sum_{i=1}^k (z_{it} - \beta_{i1} f_{1t})^2. \tag{1.22}$$

Since both $f_{1t}$ and $\boldsymbol{\beta}_1$ are unknown, the solution to Eq. (1.22) is not well-defined, because $\beta_{i1} f_{1t} = (\beta_{i1} / h) (h f_{1t}) = \beta^*_{i1} f^*_{1t}$, where $h$ is an arbitrary non-zero real number. To overcome the difficulty of non-uniqueness, we assume that $f_{1t}$ satisfies $\sum_{t=1}^T f_{1t}^2 = 1$. The scale of $f_{1t}$ is then identified, and we can take the partial derivative of $L$ with respect to $\beta_{i1}$. By equating the partial derivatives to zero, we have

$$\hat{\beta}_{i1} = \frac{\sum_{t=1}^T z_{it} f_{1t}}{\sum_{t=1}^T f_{1t}^2} = \sum_{t=1}^T z_{it} f_{1t}, \quad i=1, \dots, k, \tag{1.23}$$

or, in vector form, $\widehat{\boldsymbol{\beta}}_1 = \boldsymbol{Z}' \boldsymbol{f}_1$. Using $\boldsymbol{f}_1 = \boldsymbol{Z} \boldsymbol{\beta}_1$, we obtain

$$\widehat{\boldsymbol{\beta}}_1 = \boldsymbol{Z}' \boldsymbol{Z} \widehat{\boldsymbol{\beta}}_1,$$
and, hence, \(\hat{\beta}_1\) is an un-normalized eigenvector of the matrix \(\mathbf{Z}'\mathbf{Z}\).
Similarly, we can take the partial derivative of \(L\) with respect to \(f_{1t}\) and obtain

\(f_{1t} = \frac{\sum_{i=1}^k z_{it} \beta_{1i}}{\sum_{i=1}^k \beta_{1i}^2}, \quad t = 1, \dots, T.\)

The numerator of Eq. (1.24) can be written as \(\mathbf{z}'_t(\mathbf{Z}'\mathbf{f}_1)\). Letting \(c = \sum_{i=1}^k \beta_{1i}^2\), plugging
Eq. (1.23) into Eq. (1.24), and using matrix notation, we have

\(\mathbf{ZZ}'\mathbf{f}_1 = c\mathbf{f}_1.\)

This implies that \(\mathbf{f}_1\) is an eigenvector of the matrix \(\mathbf{ZZ}'\) associated with eigenvalue \(c\).
The function in Eq. (1.22) is an objective function of least squares estimation, and
the fitted value is given by \(\mathbf{ZZ}'\mathbf{f}_1\). Therefore, the value of objective function evaluated
at the estimates becomes

\(\begin{aligned} \widehat{L} &= \sum_{t=1}^T \sum_{i=1}^k z_{it}^2 - [\mathbf{ZZ}'\mathbf{f}_1]'(\mathbf{ZZ}'\mathbf{f}_1) \\ &= \sum_{t=1}^T \sum_{i=1}^k z_{it}^2 - \mathbf{f}_1'\mathbf{ZZ}'(c\mathbf{f}_1) \\ &= \sum_{t=1}^T \sum_{i=1}^k z_{it}^2 - c^2, \end{aligned}\)

where we have used \(\mathbf{f}'_1\mathbf{f}_1 = \sum_{t=1}^T f_{1t}^2 = 1\). Consequently, to achieve the minimum value
of the objective function \(\widehat{L}\), we choose \(c\) to be the largest eigenvalue of \(\mathbf{ZZ}'\). The vector \(\mathbf{f}_1\) is not equal to the first PC \(\mathbf{y}_1\), because it has variance 1, whereas the PC
has variance \(\lambda_1\), but apart from this scaling effects they are identical, as both are
proportional to the eigenvectors of the matrix \(\mathbf{ZZ}'\), as shown in (1.21) and (1.25).

For the second sample PC, we can follow a similar procedure by replacing the
observed data \(\mathbf{z}_t\) by the residual \(\boldsymbol{\epsilon}_t = \mathbf{z}_t - \hat{\beta}_1 f_{1t}\). This process can be repeated until all
sample PCs are introduced.

In summary, both approaches led to the same result with a difference in scale. In
the first approach, the linear combinations of maximum variance are defined by unit
norm vectors that are the eigenvectors of the covariance matrix and the PCs have
different variances. In the second approach, the optimal interpolations are defined
by vectors that are proportional to the eigenvalues of the covariance matrix and the
linear combinations are standardized to have unit variance.

## 1.5.1 Discussion

We have introduced PCA using both the variance decomposition and the reconstruction
approaches. However, there is a subtle difference between the two versions. In
the first case we assume stationarity. On the other hand, for a given data set, the
reconstruction approach can be applied to any time series.
