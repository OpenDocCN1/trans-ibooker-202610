be the centered and normalized cumulative sum of squares of the sequence \( e_t \).
The asymptotic distribution of this statistic under the hypothesis of no change in the
covariance matrix is a Brownian Bridge at \( v = h/T \), which is normal with zero mean
and variance \( v(1 - v) \). Thus, to test for the presence of a covariance change at the
time point \( h \) the value of the statistic \( C_h \) is compared with percentiles of a zero mean
normal distribution with variance \( v(1 - v) \). The impact of the covariance change is
estimated by computing the Cholesky decomposition of the matrices \( S_1 \) and \( S_2 \) and
using Eq. (4.42)

\(\widehat{W} = L_{S_2} L_{S_1}^{-1} - I.\) (4.46)

# 4.7.2.1 Detecting Multiple Covariance Changes

Multiple covariance changes
may occur in a vector time series. With \( r \) changes the observed time series is
given by

\( z_t = y_t + \Psi(B)(I + W_r S_t^{(h_r)}) \cdots (I + W_1 S_t^{(h_1)}) a_t,\)

where \(\{h_1, \dots, h_r\}\) are the time indexes of the \( r \) change points and \( W_1, \dots, W_r \) are
\( k \times k \) lower triangular matrices denoting the impact of the \( r \) changes. Assuming that
the parameters are known, the filtered series of residuals is given by

\( e_t = (I + W_r S_t^{(h_r)}) \cdots (I + W_1 S_t^{(h_1)}) a_t,\)

and the residual covariance matrix of \( e_t \), changes from \( \Sigma \) to \( (I + W_1) \Sigma (I + W_1)' \) at
\( t = h_1 \), and to \( (I + W_2)(I + W_1) \Sigma (I + W_1)' (I + W_2)' \) at \( t = h_2 \), and so on. In practice,
the parameters of the VARMA model, the number, locations and the sizes of the
variance changes are unknown. Let \( \widehat{\text{LR}}_t \) and \( \widehat{C}_t \) be the statistics (4.44) and (4.45),
respectively, computed using the estimated residuals which are obtained by (4.43).
We define the maximum of these statistics in the series as,

\(\Lambda_{\max}(h_{\max}^{LR}) = \max\{ |\widehat{\text{LR}}_t|, 1 \le t \le T \}, \, \Gamma_{\max}(h_{\max}^C) = \max\{ |\widehat{C}_t|, 1 \le t \le T \},\) (4.47)

where \( h_{\max}^{LR} \) and \( h_{\max}^C + 1 \) are the estimates of the times of change using the LR
test and the cusum statistic, respectively. The distribution of \( \Lambda_{\max} \) in Eq. (4.47) is
intractable and critical values are obtained by simulation. The distribution of \( \Gamma_{\max} \)
in Eq. (4.47) is asymptotically the distribution of \( \sup\{ |M_r^0| : 0 \le r \le 1 \} \), which is
given by (see Billingsley, 2013),

\( P\{ \sup |M_r^0| \le a : 0 \le r \le 1 \} = 1 + 2 \sum_{i=1}^\infty (-1)^i \exp(-2 i^2 a^2),\)

and critical values can be obtained from this distribution. If several changes have
occurred in the series, we propose two iterative procedures to detect them and esti-
mate their impacts based on the statistics \( \Lambda_{\max} \) and \( \Gamma_{\max} \).

# 4.7.2.2 LR Test

The following procedure is a generalization to the one proposed
by Tsay (1988). The algorithm is based on cleaning the series after finding a variance
change and proceeds as follows:

+   1. Assuming no variance changes, a vector ARMA model is specified for the
+   observed series \( z_t \). The maximum likelihood estimates of the model are
+   obtained as well as the filtered series of residuals denoted by \( \widehat{e}_t \). Define \( z_t^* = z_t \).