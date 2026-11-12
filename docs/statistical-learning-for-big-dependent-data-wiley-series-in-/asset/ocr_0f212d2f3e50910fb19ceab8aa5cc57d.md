In practice, the choice between using VAR or VARMA models depends on the dimension and sample size of the problem under study. If the dimension \( k \) is not large, say \( k \le 10 \), then VARMA models can be used. For other cases, VAR models are often preferred for their simplicity both in understanding the dynamic structure of the data and in estimation. We discuss VARMA models for high-dimensional time series later.

# 3.4.2 Properties of VARMA Models

We summarize the key properties of VARMA models of Eq. (3.1) below:

+   • Stationarity condition: All zeros of the determinant equation \( |\phi(B)| = 0 \) are greater than 1 in absolute value.
+   The VMA representation of a stationary VARMA model is
+   \[
+   z_t = \mu + a_t + \sum_{i=1}^{\infty} \psi_i a_{t-i},
+   \]
+   (3.60)
+   where \(\mu = E(z_t)\) and the \(\psi\)-weight coefficient \(\psi_i\) is given by \([\phi(B)]^{-1}\theta(B) = \psi(B) = I + \sum_{i=1}^{\infty} \psi_i B^i\).

+   • Invertibility condition: All zeros of the determinant equation \( |\theta(B)| = 0 \) are greater than 1 in absolute value.
+   The VAR representation of an invertible VARMA model is
+   \[
+   z_t = c + \sum_{i=1}^{\infty} \pi_i z_{t-i} + a_t,
+   \]
+   (3.61)
+   where \(c = [\theta(1)]^{-1}c_0\) and the \(\pi\)-weight coefficient matrix \(\pi_i\) is given by \(\pi(B) = I - \sum_{i=1}^{\infty} \pi_i B^i = [\theta(B)]^{-1}\phi(B)\).

+   • Moment condition: For a stationary VARMA process \( z_t \) of Eq. (3.1), we have
+   1. \( E(z_t) = \mu \) such that \( \phi(1)\mu = c_0 \). That is, \( \mu = [\phi(1)]^{-1}c_0 \).
+   2. \( \Gamma_z(\ell) = \sum_{i=1}^p \phi_i \Gamma_z(\ell-i) - \sum_{j=\ell}^q \theta_j \Sigma_a \psi'_{\ell-j} \), for \( \ell = 1, \dots, q \), where \( \psi_0 = I \).
+   3. \( \Gamma_z(\ell) = \sum_{i=1}^p \phi_i \Gamma(\ell-i) \) for \( \ell > q \).

If we consider the third moment condition jointly for \( \ell = q + 1, \dots, q + p \), then we have a system of \( p \) moment conditions, which is referred to as the *generalized Yule-Walker equations* of a VARMA(\( p, q \)) model.

# 3.4.3 Modeling VARMA Process

There are several approaches available in the literature for modeling VARMA processes. One approach, proposed by Tiao and Tsay (1983), is to fit a large VAR that provides consistent estimates of the residuals and incorporate these estimated residuals to find the MA order. They proposed to use a table of extended cross-correlation matrices to obtain the order of the ARMA process. The second approach is based on the structure of covariance matrix of two long vectors of time series, consisting of the past and the future of the process under study, referred to the Hankel matrix, and