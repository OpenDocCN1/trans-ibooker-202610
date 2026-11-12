where the parameters include (a) the \(k \times r\) elements of \(\mathbf{P}\), (b) the \(T \times r\) elements of \(\mathbf{F}\), and (c) the \(k\) diagonal elements of \(\unknown_e\). Assuming \(\unknown_e = \sigma^2\mathbf{I}\), maximizing Eq. (6.18) is equivalent to PCA, and assuming \(\unknown_e\) a positive definite matrix to generalized PCs. The minimization of Eq. (6.18) can be carried out by putting the model in a state space form and computing the likelihood function using the Kalman filter. The observation equation for the state space representation is simply Eq. (6.1). The transition equation is obtained by assuming a VAR(\(p_1\)) and a VAR(\(p_2\)) model for the factors and noises, respectively. See, Chapter 2 for state-space model and Kalman filter. This approach is useful for a small \(k\) and has the advantage of allowing the series to contain different periodicities, but it is less useful when \(k\) is large. The EM algorithm has also been used for estimating FMs but, again, is not practical for large \(k\).

The following iterative procedure, proposed by Bai and Li (2016), can be carried out easily with some initial estimates of \(\mathbf{P}\) and \(\mathbf{F}\) to perform the approximate ML estimation:

+   1. Given the present estimates \(\widehat{\mathbf{P}},\widehat{\mathbf{F}}\), the residuals are computed by \(\widehat{\mathbf{n}}_t = (\mathbf{I} - \widehat{\mathbf{P}}\widehat{\mathbf{P}'})\mathbf{z}_t\), and the noise variances, \(\text{var}(n_{it}) = \widehat{\sigma}_i^2\), which are used to form a diagonal matrix \(\widehat{\unknown}_e\).

+   2. Autoregressive (AR) models are fitted to each residual series, \(\widehat{\phi}_i(B)\widehat{n}_{it} = a_{it}\), \(i = 1, \dots, k\).

+   3. The vector of loadings for each series, \((\mathbf{p}_{t\cdot})'\), that is, the \(i\)th row of the matrix \(\widehat{\mathbf{P}}\), is updated with the following regression
+   
+   \(\widehat{\phi}_i(B)z_{it} = (\mathbf{p}_{t\cdot}')\widehat{\phi}_i(B)\widehat{\mathbf{f}}_t + \epsilon_{it}.\)

+   4. The factors are updated by using the current vector of loadings and running the regression
+   
+   \(\widehat{z}_{it}/\widehat{\sigma}_i = (\mathbf{p}_{t\cdot}')\widehat{\mathbf{f}}_t/\widehat{\sigma}_i + \epsilon_{it}^*.\)

+   5. With the new factors and loadings, go to step 1 and iterate until convergence.

# 6.2.5 Selecting the Number of Factors

Several tests have been proposed in the literature to select the number of factors and we will present here the most often used ones. The first class of tests is based on checking the rank of the autocovariance matrices. Note that for the EDFM, the maximum rank must be \(r\) and this maximum is achieved also by the sum of some autocovariance matrices with \(h > 0\). These tests are less useful if there is strong autocorrelation in the noises (e.g. the ADFM), because then the lag covariance matrices can be full rank. Also, these types of tests work well for \(T >> k\), that is, with a small number of long time series, but are less powerful when \(k\) is relatively large compared with \(T\). A second class of tests make use of the idea of separating the large eigenvalues of the covariance matrices from the small ones and selecting \(r\) as the number of large eigenvalues. A third way to select the number of factors is to use a model selection criterion. We describe these three methods next. The selection of \(r\) for the case of \(k \ge T\) can be carried out by testing for high-dimensional white noises discussed in Section 6.10.