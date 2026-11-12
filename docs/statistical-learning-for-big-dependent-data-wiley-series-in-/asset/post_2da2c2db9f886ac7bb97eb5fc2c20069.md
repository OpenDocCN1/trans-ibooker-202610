
Cheng et al. (2016) proposed a shrinkage estimation for level shifts. Missing value estimation in DFM has been studied by Bai and Ng (2019).

## 6.8 DFM WITH CLUSTER STRUCTURE

Often, there are factors that affect all the series, whereas others are group specific and affect only some group of the series. For instance, we may have groups of countries with various degrees of economic developments, inter-connected industrial sectors, and bilateral trade. Then, the dynamic evolution of economic time series of those countries may be affected by some general factors, that reflect the evolution of the global economy, and some specific factors that are group-dependent. In this case, we have a DFM with cluster structure (DFMCS). Such models have been studied, among others, by Wang (2010), Hallin and Liška (2011), Lin and Ng (2012), Bonhomme and Manresa (2015), Su et al. (2016), Ando and Bai (2016, 2017), and Alonso et al. (2020).

Partition the $k$-dimensional time series as $z_t = (z'_{1t}, z'_{2t}, \dots, z'_{ct})'$, where $z_{it}$ is a $k_i$-dimensional time series such that $\sum_{i=1}^c k_i = k$. In other words, we have $c$ clusters of time series and the $i$th cluster has $k_i$ time series. The DFMCS can be written as

$$
z_t = P_0 f_{0t} + \sum_{i=1}^c P_i f_{it} + n_t, \quad (6.49)
$$

where $n_t$ is the vector of noises, $f_{0t} = (f_{01t}, \dots, f_{0r_0 t})'$ is an $r_0$-dimensional vector of global factors, $P_0$ is a $k \times r_0$ global loading matrix. $f_{it} = (f_{i,1t}, \dots, f_{i,r_i t})'$ is an $r_i$-dimensional common factor of the $i$th cluster and $P_i$ is a $k \times r_i$ loading matrix for the $i$th cluster such that $P_i = [0'_{i-1}, w'_i, 0'_{c-i}]'$, where $0_{i-1}$ is a zero matrix of dimension $(\sum_{j=1}^{i-1} k_j)$-by-$r_i$ provided that $i > 1$, $w_i$ is a $k_i \times r_i$ loading matrix for the $i$th cluster, and $0_{c-i}$ is another zero matrix of dimension $(\sum_{j=i+1}^c k_j)$-by-$r_i$ provided that $i < c$. In this way, $P_i f_{it}$ only affects time series in the $i$th cluster. The total number of factors is $r = r_0 + r_1 + \dots + r_c$.

The identification of the DFMCS in Eq. (6.49) has been studied by Wang (2010) and the conditions needed are: (1) $P'_0 P_0 = I_{r_0}$, where $I_{r_0}$ is the identity matrix of order $r_0$; (2) $P'_i P_i = w'_i w_i = I_{r_i}$ for $i = 1, \dots, c$; (3) $P'_0 P_i = 0_{r_0 \times r_i}$; and (4) the covariance matrix of the $r = \sum_{j=1}^c r_j$ factors is diagonal. Note that also, by definition, $P'_i P_j = 0_{r_i \times r_j}$, for $i \neq j$. We can write this model as a standard DFM. Letting $f_t = (f'_{0t}, f'_{1t}, \dots, f'_{ct})'$, and $P = [P_0 | P_1 | \dots | P_c]$, we have

$$
z_t = P f_t + n_t, \quad (6.50)
$$

and the previous conditions implied the usual identification restriction $P'P = I_r$.

The assumptions for the idiosyncratic term and the factors are the same as before. The idiosyncratic term or noise, $n_t = (n_{1t}, \dots, n_{kt})'$, is a general sequence of stationary time series with mean $0_k$ and weak serial dependency as in the ADFM. The global and specific factors are orthogonal to each other and follow a diagonal VARMA. Additionally, we assume that both innovation processes appearing in the FM are uncorrelated for all lags. However, the number of clusters and the allocation of the series to the clusters are unknown.

The estimation of a DFMCS requires obtaining the following parameters: (1) the number of global factors, \(r_0\), the number of groups, \(c\), and the number of specific factors in each of the \(c\) groups, \(r_1, ..., r_c\); (2) the label variable \(g_i \in \{1, ..., c\}\), indicating to which group the series belongs, and we call \(G\) the \(k \times 1\) vector with components \(g_i\), for \(i = 1, ..., k\); (3) the loading matrices of the global and specific factors, \(P_0, P_1, ..., P_c\) and the time series of these factors, \(f_{0t}, f_{1t}, ..., f_{ct}\). Given the estimated factors and noises, where \(\hat{n}_t = z_t - \hat{P} f_t\), estimators of the parameters of the scalar ARMA models for the factors and noises can be obtained.

### 6.8.1 Fitting DFMCS

The procedure we consider has the following three steps:

- 1. An initial set of factors and their loadings are estimated with all the time series and used to build the common component of each time series. Then, a cluster by dependency using the generalized cross-correlation (GCC) method (proposed in Chapter 5) is applied to these common components to find the groups.

- 2. A new set of factors and their loadings are estimated in each group of time series found in the previous step. All the factors found in the groups as well as those found in step 1 are compared and classified as global or specific;

- 3. The effect of the global factors is removed from each of the time series and the group-specific residuals obtained are used to re-estimate the specific factors. Finally, groups are checked for possible recombination.

In the first step, the initial estimation of the factors and loadings is made with PCA applied to the sample covariance matrix of the time series \(\hat{\Gamma}_z(0)\). As we have seen, the eigenvectors associated to the \(r_*\) largest eigenvalues of this matrix provide us with an estimate of the factor loading matrix, \(\hat{P}\), with columns \(\hat{p}_1 \dots \hat{p}_{r_*}\). The number of factors, \(r_*\), is determined by using the test proposed by Ahn and Horenstein (2013) based on the ratios of consecutive eigenvalues of the matrix \(\hat{\Gamma}_z(0)\). Note that the matrix \(\hat{P}\) is expected to include all the global factors and some (or all) of the group-specific factors, so that the number of factors in this matrix, \(r_*\), is in general larger than the true number of global factors, \(r_0\). Also, in practice, when the factors are of different degree of strength, the results of ratio of eigenvalues tests are usually better if they are applied twice, first to detect a few dominant factors and second to find the others, as suggested by Lam and Yao (2012). Then, the factors are estimated by \(\hat{f}_{it} = \hat{p}'_i z_t\), for \(i = 1, \dots, r_*\), and the common component by \(\mathbf{f}_t = \hat{\mathbf{P}} \hat{\mathbf{p}}' \mathbf{z}_t\).

The groups are now built applying a hierarchical clustering algorithm with single linkage to the dissimilarity matrix using the GCC. The number of clusters is obtained by a modification of the Silhouette algorithm adding the restriction that the clusters must have a minimum size. We implement this restriction by omitting time series in the dendrogram analysis that have a relatively small dependency with the rest (for instance, the 90% percentile of the dendrogram's unions). Once the groups are formed, the omitted time series are assigned to the closer cluster in the single linkage sense. In this way, we obtain an estimated value of \(c\), the number of groups, and an estimator of the vector \(G\) that gives the allocation of the series to the \(c\) groups.

In the second step, we use the series in the groups found in the previous step to estimate new sets of factors and their loadings. Let \(r^s_1, \dots, r^s_c\) be the number of factors found in each group by using the Ahn and Horenstein (2013) test applied to the eigenvalues of the sample covariance matrices of the time series in each group. The specific loading matrices \(\widehat{P}_i\) of dimension \(k \times r^s_i\) and columns \(\widehat{p}_{i1}, \dots, \widehat{p}_{ir^s_i}\) are built by adding to the eigenvectors corresponding to the largest \(r^s_i\) eigenvalues in the \(i\)th group, a set of zero values for the observations not included in the group. The factors in each group are estimated by \(\widehat{f}^s_{ij,t} = \widehat{p}'_{ij} z_t\), with \(j = 1, \dots, r^s_i\). These group factors are expected to include all the specific factors and some (or all) of the global factors.

Next, in order to decide whether a factor is global or specific, we compare the set of \(r_*\) factors found in step 1 and the set of \(\sum_{i=1}^c r^s_i\) factors found in this second step. Note that the factors contained in the first set may be a rotation of the factors contained in the second set and, therefore, it is not evident which ones should be classified as global and which as specific. Consequently, we first decide if each factor \(f_t\) in the first set of \(r_*\) factors is global or specific by applying the following three simple rules:

- 1. If \(f_t\) does not belong to any of the second set of factors then it is a global factor.

- 2. If \(f_t\) belongs to only one of the sets of the second set of factors then it is a specific factor in this group.

- 3. If \(f_t\) belongs to more than one of the second set of factors then it is a global factor.

We decide if a factor, \(f_t\), belongs to a set of specific factors by computing the empirical canonical correlation between the factor, \(f_t\), and the ones in the set, \(\widehat{f}^s_{i1,t}, \dots, \widehat{f}^s_{ir^s_i,t}\), with \(i = 1, \dots, c\). When the empirical canonical correlation of factor \(f_t\) with elements of the set is higher than some threshold value, \(\rho_0\), we say that \(f_t\) belongs to this set. The threshold value of \(\rho_0 = 0.9\) seems to work well in our Monte Carlo exercise. Afterwards, we check if any of the groups with \(r^s_1, \dots, r^s_c\) factors include any factor that does not belong to the set of factors found in step 2. If this is the case, the factor is classified as specific factor in the corresponding group.

In the third step, we compute the residuals \(v_t = z_t - \widehat{P}_0 \widehat{f}_{0t}\), where \(\widehat{f}_{0t}\) is the vector of estimated global factors obtained in step 3 and \(\widehat{P}_0\) is the loading matrix corresponding to these factors, and the specific factors are re-estimated using the series \(v_{it}\) corresponding to each group. Then, we verify that the groups obtained are due to different specific factors and not due to differences between factor loadings in a global factor. This is made by checking whether all the groups have at least one specific factor. We may face the following cases: (1) All the \(c\) groups found include at least one specific factor, and we have a DFMCS with \(c\) groups; (2) \(c_1\) groups, (\(1 \le c_1 < c\)) contain specific factors, and \(c_2 = c - c_1\) groups only contain global factors, then we have a DFMCS with \(c_1 + 1\) groups; and (3) All the groups only contain global factors, then we have the standard DFM.

To justify this last verification to check that the groups are generated by different specific factors and not for different loadings of the global factors, consider the simple model

$$
\begin{bmatrix} z_{1t} \\ z_{2t} \end{bmatrix} = \begin{bmatrix} a \mathbf{1}_{k_1} \\ b \mathbf{1}_{k_2} \end{bmatrix} f_t + \begin{bmatrix} n_{1t} \\ n_{2t} \end{bmatrix},
$$

where \(z_{it}\) is \(k_i \times 1\), with \(k = k_1 + k_2\), and \(\mathbf{1}_{k_i} = (1, \dots, 1)'\) is also \(k_i \times 1\). In this model, there are two groups of time series of similar dependency. For simplicity,
