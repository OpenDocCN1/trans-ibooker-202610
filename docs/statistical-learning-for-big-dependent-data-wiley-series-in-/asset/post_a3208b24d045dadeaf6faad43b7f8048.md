
with

$$e_t = a_t + W S_t^{(h)} a_t,$$

where $S_t^{(h)}$ is a step function and $W$ a lower triangular matrix of size $k \times k$ denoting the impact of the covariance change. This equation implies that the covariance of the innovations $e_t$ of the observed time series changes at the time point $t = h$ from $\Sigma$ to $\Omega = (I + W)\Sigma(I + W)'$. The relation between the observed series, $z_t$, and the unobserved vector ARMA time series, $y_t$, is given by

$$z_t = y_t + \Psi(B) W S_t^{(h)} a_t, \tag{4.41}$$

with $\Psi(B) = \Phi^{-1}(B)\Theta(B)$.

Without loss of generality, we assume that $(I + W)$ is a positive-definite matrix, so that the matrix $W$ is well identified. Let $\Sigma = L_{\Sigma} L_{\Sigma}'$ and $\Omega = L_{\Omega} L_{\Omega}'$ be the Cholesky decompositions of the two covariance matrices. Then

$$\Omega = L_{\Omega} L'_{\Omega} = (I + W)L_{\Sigma} L'_{\Sigma} (I + W)',$$

and

$$L_{\Omega} = (I + W)L_{\Sigma},$$

which implies

$$W = L_{\Omega} L_{\Sigma}^{-1} - I. \tag{4.42}$$

Thus, estimating $W$ by (4.42) results in a well-defined matrix.

To test the significance of a covariance change at $t = h$, suppose that the parameters of the vector ARMA model are known and we compute the residuals via

$$e_t = z_t - \sum_{i=1}^p \Phi_i z_{t-i} + \sum_{j=1}^q \Theta_j e_{t-j}. \tag{4.43}$$

We want to test the null hypothesis $H_0 : W = 0$, which implies that these residuals are iid homoscedastic, versus the alternative hypothesis $H_1 : W \neq 0$, implying that they are heteroskedastic. We propose two statistics: the LR test, which is asymptotically the most powerful test, and the cusum test. Let us define three matrices $S = \sum_{i=1}^T (e_i e_i') / T$, $S_1 = \sum_{i=1}^{h-1} (e_i e_i') / (h-1)$ and $S_2 = \sum_{i=h}^T (e_i e_i') / (T - h + 1)$. The LR statistic for a variance change at and after the time point $t = h$ is given by

$$\text{LR}_h = \log \frac{|S|^T}{|S_1|^{h-1} |S_2|^{T-h+1}}, \tag{4.44}$$

and, under the null hypothesis of no covariance change and that the model is known, the $\text{LR}_h$ statistic has an asymptotic chi-square distribution with $\frac{1}{2}k(k+1)$ degrees of freedom.

An alternative cusum test statistic can be built as follows. Let $A_h = \sum_{i=1}^h e_i' \Sigma^{-1} e_t$ be the multivariate cumulative sum of squares of the residuals, where $h$ is any given value $1 \le h \le T$. Let,

$$C_h = \frac{h}{\sqrt{2kT}} \left( \frac{A_h}{h} - \frac{A_T}{T} \right), \tag{4.45}$$
be the centered and normalized cumulative sum of squares of the sequence \( e_t \).
The asymptotic distribution of this statistic under the hypothesis of no change in the covariance matrix is a Brownian Bridge at \( v = h/T \), which is normal with zero mean and variance \( v(1 - v) \). Thus, to test for the presence of a covariance change at the time point \( h \) the value of the statistic \( C_h \) is compared with percentiles of a zero mean normal distribution with variance \( v(1 - v) \). The impact of the covariance change is estimated by computing the Cholesky decomposition of the matrices \( S_1 \) and \( S_2 \) and using Eq. (4.42)

\(\widehat{W} = L_{S_2} L_{S_1}^{-1} - I.\) (4.46)

### 4.7.2.1 Detecting Multiple Covariance Changes

Multiple covariance changes may occur in a vector time series. With \( r \) changes the observed time series is given by

\( z_t = y_t + \Psi(B)(I + W_r S_t^{(h_r)}) \cdots (I + W_1 S_t^{(h_1)}) a_t,\)

where \(\{h_1, \dots, h_r\}\) are the time indexes of the \( r \) change points and \( W_1, \dots, W_r \) are \( k \times k \) lower triangular matrices denoting the impact of the \( r \) changes. Assuming that the parameters are known, the filtered series of residuals is given by

\( e_t = (I + W_r S_t^{(h_r)}) \cdots (I + W_1 S_t^{(h_1)}) a_t,\)

and the residual covariance matrix of \( e_t \), changes from \( \Sigma \) to \( (I + W_1) \Sigma (I + W_1)' \) at \( t = h_1 \), and to \( (I + W_2)(I + W_1) \Sigma (I + W_1)' (I + W_2)' \) at \( t = h_2 \), and so on. In practice, the parameters of the VARMA model, the number, locations and the sizes of the variance changes are unknown. Let \( \widehat{\text{LR}}_t \) and \( \widehat{C}_t \) be the statistics (4.44) and (4.45), respectively, computed using the estimated residuals which are obtained by (4.43). We define the maximum of these statistics in the series as,

\(\Lambda_{\max}(h_{\max}^{LR}) = \max\{ |\widehat{\text{LR}}_t|, 1 \le t \le T \}, \, \Gamma_{\max}(h_{\max}^C) = \max\{ |\widehat{C}_t|, 1 \le t \le T \},\) (4.47)

where \( h_{\max}^{LR} \) and \( h_{\max}^C + 1 \) are the estimates of the times of change using the LR test and the cusum statistic, respectively. The distribution of \( \Lambda_{\max} \) in Eq. (4.47) is intractable and critical values are obtained by simulation. The distribution of \( \Gamma_{\max} \) in Eq. (4.47) is asymptotically the distribution of \( \sup\{ |M_r^0| : 0 \le r \le 1 \} \), which is given by (see Billingsley, 2013),

\( P\{ \sup |M_r^0| \le a : 0 \le r \le 1 \} = 1 + 2 \sum_{i=1}^\infty (-1)^i \exp(-2 i^2 a^2),\)

and critical values can be obtained from this distribution. If several changes have occurred in the series, we propose two iterative procedures to detect them and estimate their impacts based on the statistics \( \Lambda_{\max} \) and \( \Gamma_{\max} \).

### 4.7.2.2 LR Test

The following procedure is a generalization to the one proposed by Tsay (1988). The algorithm is based on cleaning the series after finding a variance change and proceeds as follows:

- 1. Assuming no variance changes, a vector ARMA model is specified for the observed series \( z_t \). The maximum likelihood estimates of the model are obtained as well as the filtered series of residuals denoted by \( \widehat{e}_t \). Define \( z_t^* = z_t \).

- 2. Compute the statistics $\text{LR}_h$, $h = d + 1, \dots, T - d$, for a given positive integer $d$, using the residuals obtained in step 1. The number $d$ denotes the minimum number of residuals needed to estimate the covariance matrix and we have taken $d$ as the number of parameters to estimate plus one,

\[ d = k + k(p + q) + \frac{k(k+1)}{2} + 1. \]

With the statistics $\text{LR}_h$, the statistic $\Lambda_{\max}(h_{\max}^{LR})$ in Eq. (4.47) is obtained.

- 3. Compare $\Lambda_{\max}(h_{\max}^{LR})$ with a specified critical value $C$ for a given critical level. If $\Lambda_{\max}(h_{\max}^{LR}) < C$, it is concluded that there is no covariance change and the procedure ends. If $\Lambda_{\max}(h_{\max}^{LR}) \ge C$, it is assumed that there is a significant covariance change at time $t = h_{\max}^{LR}$.

- 4. The matrix $\widehat{W}$ is estimated with Eq. (4.46) and a modified residual series is computed as follows:

\[ e_t^* = \begin{cases} \widehat{e_t}, & t < h_{\max}^{LR}, \\ (I + \widehat{W})^{-1} \widehat{e_t}, & t \ge h_{\max}^{LR}, \end{cases} \]

and, with this residual series, a corrected time series is defined by

\[ z_t^* = \begin{cases} z_t, & t < h_{\max}^{LR} \\ \widehat{\Phi}_1 z_{t-1} + \dots + \widehat{\Phi}_p z_{t-p} + e_t^* - \widehat{\Theta}_1 e_{t-1}^* - \dots - \widehat{\Theta}_q e_{t-q}^*, & t \ge h_{\max}^{LR} \end{cases} \]

where the polynomial matrices $\widehat{\Phi}(B)$ and $\widehat{\Theta}(B)$ are the maximum likelihood estimates of the parameters. Then, go back to step 1 considering $z_t^*$ as the observed process.

- 5. When no more covariance changes are found, the model parameters and all the covariance changes detected in the previous steps are estimated jointly, using the model

\[ \Phi(B)z_t = \Theta(B)(I + W_r S_t^{(h_t)}) \cdots (I + W_1 S_t^{(h_1)}) a_t. \quad (4.48) \]

This joint estimation is carried out in two steps. First, estimate the parameters assuming no variance changes and then estimate the matrices $W_r$. After that, correct the series, and repeat these two steps until convergence.

The cusum procedure is a generalization to the one proposed by Inclán and Tiao (1994). The algorithm is based on successive divisions of the series into two pieces when a change is detected and is explained in the Appendix. Some comments on these algorithms are in order. First, the critical values for LR statistic have to be obtained by simulation, while those of the cusum procedure are the asymptotic critical values of the maximum of the absolute value of a Brownian Bridge. Second, in both algorithms we require a minimum distance between variance changes larger than $d$, so that the covariance matrix can be estimated. If several changes were found in an interval smaller than $d$, these changes will be considered as multivariate outliers and estimated by the procedures discussed before. Third, the last step in the LR procedure is needed for avoiding bias in the size of the estimated covariance changes. Note that in step 4, the size of the covariance change is estimated after detecting it. Thus, if there are two covariance changes the impact of the first detected change is estimated without taking into account the second one. Therefore, a joint estimation is needed to reduce biases.
