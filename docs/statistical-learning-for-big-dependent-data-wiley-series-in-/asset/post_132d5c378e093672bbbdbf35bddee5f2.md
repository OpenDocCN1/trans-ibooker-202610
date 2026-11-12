
which implies that $(QQ')^{-1} = I$ and, hence, $QQ' = I$. Again, $Q$ is an orthonormal matrix. The two approaches lead to the same requirement on $Q$. The prior discussion states that the DFM in Eq. (6.1) is not yet uniquely identified. In fact, for any \( r \times r \) orthonormal matrix $Q$, we have

$$z_t = P f_t + n_t = P^* f_t^* + n_t.$$

where $P^* = P Q^{-1}$ and $f_t^* = Q f'_t$. Note that a \( r \times r \) orthonormal matrix $Q$ has $r(r+1)/2$ independent elements and it represents a rotation in the $r$-dimensional space.

In this chapter, we adopt the condition that $P'P = I$. Since the common factor $f_t$ is not observable (also known as latent), we further assume, without loss of generality, that $\Gamma_f(0)$ is a diagonal matrix. In other words, we can perform orthogonalization on the latent common factor $f_t$ if needed. With the aforementioned assumptions, the DFM is identified under rotations. Note that a rotation of the uncorrelated factors with an arbitrary orthogonal matrix $f_t^* = A f_t$, where $A A' = I$ is orthogonal, may lead to a covariance matrix of the new factors, $E(f_t^{*'} f_t^{*}) = A \Gamma_f(0) A'$ that need not be diagonal. However, the rotation would keep the orthonormal condition of the new loading matrix, because $(P^*)' P^* = (A^{-1})' P' P A^{-1} = (A A')^{-1} = I$. Of course, once the identified model is fitted to the data we can always explore if a rotation of the factors leads to a model that is easier to understand. Also, as mentioned before, one can always perform orthogonalization on $f_t$ so that the resulting common factors have a diagonal covariance matrix.

In addition to the structural Eq. (6.1), we also need to specify the model for the common factors and the noises. The factors are assumed to be linear stationary series that can be well approximated by a VARMA model,

$$\Phi(B) f_t = \Theta(B) a_t, \quad (6.4)$$

where $\Phi(B) = I - \Phi_1 B - \cdots - \Phi_p B^p$ and $\Theta(B) = I - \Theta_1 B - \cdots - \Theta_q B^q$ and, for stationarity and invertibility, the roots of the determinant equations $|\Phi(B)| = 0$ and $|\Theta(B)| = 0$ are outside the unit circle. The factors must have a diagonal covariance matrix and, for simplicity, all the polynomial matrices of the VARMA representation (6.4) are diagonal \( r \times r \) matrices and \( a_t \sim N_r(0, \Sigma_a) \), where $\Sigma_a$ is the diagonal variance-covariance matrix and $E(a_t a'_{t-h}) = 0$, for \( h \neq 0 \). Also, we assume that the factors are uncorrelated with the idiosyncratic noise, that is, $E(a_t n'_{t-h}) = 0$, for all \( h = 0, \pm 1, \pm 2, \dots \).

There are two types of assumptions that are commonly imposed on the idiosyncratic or specific component $n_t$. In the first case the idiosyncratic component is simply a white noise process, that is, $n_t = e_t$, where $e_t$ is a sequence of independent and identically distributed random vectors with mean zero and covariance matrix $\Sigma_e$. In addition, as stated before, $\{e_t\}$ is uncorrelated with $f_t$ or equivalently $\{e_t\}$ is uncorrelated with $\{a_t\}$, i.e. $E(a_t e'_{t-h}) = 0$, for all \( h = 0, \pm 1, \pm 2, \dots \). With this assumption for $n_t$, the model is perfectly identified (except for rotations) and it is called the exact DFM or EDFM. This model was proposed by Peña and Box (1987). For large $k$ the number of parameters in the covariance matrix is $k(k+1)/2$ that can be larger than $T$, and in this case one often further assumes that $\Sigma_e$ is diagonal to improve the efficacy in estimation. In real applications, the sample covariance matrix of the estimated noise term $\hat{n}_t$ is singular regardless of the sample size $T$.

The second type of assumptions is for the high-dimensional case in which $k$ can approach infinity. Then, $n_t$ may have some weak serial and cross-sectional correlations, so that the largest eigenvalue of the covariance matrix of $n_t$ is relatively small compared with the smallest variance of the common factors. Under such assumptions, the DFM is identifiable when both $T$ and $k$ go to infinity. The resulting model is called approximate DFM or ADFM. The ADFM even allows for some weak cross-correlations between the common factors and the noises if those correlations converge to zero asymptotically. In general, to identify an ADFM, one requires that all serial or cross-dependencies disappear for large $k$ and $T$, so that the model would approach an EDFM in the limit. We provide some further details of the model in the next section. The ADFM was proposed by Geweke (1977), Chamberlain (1983), and Chamberlain and Rothschild (1983), and studied by Forni et al (2000), Stock and Watson (2002a, 2002b) and Bai and Ng (2002).

## 6.1.1 Properties of the Covariance Matrices

For a stationary time series, the available information to estimate a DFM is contained in the covariance matrices of the series. Then, from Eq. (6.1), calling \(\Gamma_f(h)\) and \(\Gamma_n(h)\) to the lag h covariances of the factors and the noises, we have

$$\Gamma_z(h) = P\Gamma_f(h)P' + \Gamma_n(h), \quad h \ge 0.\tag{6.5}$$

This moment equation plays an important role in studying DFM. In practice, care must be exercised because autocovariance matrices depend on the scale. If one uses standardized process of $z_t$, then the results are likely to be different from those based on the original scale. As in classic factor analysis, if the series are not in the same measurement units standardizing them makes the result free from scaling effects. When all series are in the same scale, those with larger variances have heavier effects on the results. Thus, depending on the data and the objective of the analysis we can decide whether one should or should not standardize the observed process $z_t$. We recommend to try both and compare the results.

### 6.1.1.1 The Exact DFM

In this case, $k$ is finite and $n_t = e_t$ is white noise so that Eq. (6.5) gives

$$\Gamma_z(0) = P\Gamma_f(0)P' + \Sigma_e,\tag{6.6}$$

$$\Gamma_z(h) = P\Gamma_f(h)P', \quad h > 0.\tag{6.7}$$

From Eq. (6.7), the rank of \(\Gamma_z(h)\) is the same as that of \(\Gamma_f(h)\) because P is full rank. Consequently, the number of common factors $r = \max_h\{\text{rank}[\Gamma_z(h)] \mid h > 0\}$. Therefore, checking ranks of the sample autocovariance matrices of $z_t$ provides information about the number of common factors. More details are given in the next section.

Next, since all dynamic dependence of the model is driven by the common factor $f_t$, it seems natural to further assume that no linear combination of the common factors is a white noise series. Furthermore, if the latent common factors $f_{it}$ of $f_t$ are independent of each other, then \(\Gamma_f(h)\) is a diagonal matrix for all $h$. Consequently, from Eqs. (6.6) and (6.7), $\Gamma_z(h)$ is symmetric for all $h \ge 0$. These features of the autocovariance matrices of $z_t$ can be used to verify the existence of an EDFM with independent common factors.

With further assumptions on the covariance matrix $\Sigma_e$ of the specific component $n_t$ in Eq. (6.6), we have the following two interesting cases:

- I. The covariance matrix $\Sigma_e = \sigma^2 I$: Post-multiplying Eq. (6.6) by the $j$th column $p_j$ of $P$, and using $P'P = I$ and some algebra, we have

$$\Gamma_z(0)p_j = (\gamma_{f_j}^2 + \sigma^2)p_j,$$

where $\gamma_{f_j}^2$ is the variance of the $j$th common factor $f_{jt}$ and $1 \le j \le r$. Therefore, in this particular instance, the $j$th column $p_j$ is an eigenvector of $\Gamma_z(0)$ associated with eigenvalue $\gamma_{f_j}^2 + \sigma^2$. Moreover, let $P_\perp$ be the $k \times (k-r)$ orthogonal complement of $P$ in $R^k$ such that $(P_\perp)'P = 0$, a $(k-r) \times r$ zero matrix. Then, post-multiplying Eq. (6.6) by the $j$th column $p_j^*$ of $P_\perp$, we have

$$\Gamma_z(0)p_j^* = \sigma^2 p_j^*,$$

where $1 \le j \le k - r$. Thus, $p_j^*$ is an eigenvector of $\Gamma_z(0)$ associated with eigenvalue $\sigma^2$. Based on the prior discussion, we see that, in this particular case, the $r$ eigenvectors of $\Gamma_z(0)$ associated with the $r$ largest eigenvalues form the loading matrix of the EDFM.

- II. The covariance matrix $\Sigma_e = \text{diag}\{\sigma_1^2, \dots, \sigma_k^2\}$. Let $\sigma^2 = \max_{1 \le j \le k}\{\sigma_j^2\}$ be the maximum variance of the noises. We have

$$\Sigma_e = \sigma^2 I + D_0,$$

where $D_0 = \text{diag}\{\sigma_1^2 - \sigma^2, \dots, \sigma_k^2 - \sigma^2\}$. Without loss of generality, assume that $\gamma_{f,r}^2$ be the smallest variance of the common factors $f_{jt}$ for $1 \le j \le r$. Then, using the same method as that of Case I, we obtain

$$\Gamma_z(0)p_r = (\gamma_{f,r}^2 + \sigma^2)(p_r + \delta_r), \quad (6.8)$$

where $\delta_r = D_0 p_r / (\gamma_{f,r}^2 + \sigma^2)$. From Eq. (6.8), we see that $p_r$ is no longer an eigenvector of $\Gamma_z(0)$ because $\delta_r$ is not a zero vector in general. However, if $\gamma_{f,r}^2$ is much larger than $\sigma^2$, then $\delta_r$ would be small or even ignorable, and $p_r$ would approximately be an eigenvector of $\Gamma_z(0)$ with eigenvalue $\gamma_{f,r}^2 + \sigma^2$. To summarize, let $\sigma^2 / \gamma_{f,r}^2$ be the noise-to-signal ratio of the EDFM under study, where $\gamma_{f,r}^2$ is the smallest variance of the common factor and $\sigma^2$ is the largest variance of the noises. Then, the results of Case I would continue to hold in Case II approximately provided that the noise-to-signal ratio $\sigma^2 / \gamma_{f,r}^2$ is very small. In summary, the model would be more useful and easier to identify if the noise-to-signal ratio is small (or equivalently, the signal-to-noise ratio is large).
