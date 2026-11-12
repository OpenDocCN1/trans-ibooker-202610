
observable. The third type of inference is *smoothing*, which makes use of information available in $F_T$ to estimate $\alpha_t$, where $T > t$. The Kalman filter algorithm discussed next is a useful tool to make those statistical inferences. The algorithm can also be used to evaluate efficiently the log likelihood function in parameter estimation.

### 2.5.3 The Kalman Filter

The Kalman filter is a fast-recursive algorithm for forecasting and parameter estimation of a linear system presented in an SSM. The filter has a straightforward Bayesian interpretation, and it can be derived easily by properties of the multivariate normal distribution. The algorithm operates in three steps. First, we predict the future state using available information about the current state. Second, we forecast a new data point using the observation equation. Third, once a new data is observed the state vector is updated using information contained in the new data. These three steps are iterated to form the Kalman filter algorithm.

Assume that we have observed the data $F_{t-1} = \{z_1, \dots, z_{t-1}\}$ and from which we have computed an estimator of the state vector, $\widehat{\alpha}_{t-1}$. We wish to predict the next state vector, say $\widehat{\alpha}_{t|t-1}$, using the available information in $F_{t-1}$. The prediction, under the mean squared error loss, is the conditional expectation of $\alpha_t$ in (2.63) given $F_{t-1}$. That is,

$\widehat{\alpha}_{t|t-1} = \Omega_t \widehat{\alpha}_{t-1},$ (2.68)

where it is understood that $\widehat{\alpha}_{t-1|t-1} = \widehat{\alpha}_{t-1}$. Let $S_{t|t-1}$ denote the conditional covariance matrix of $\widehat{\alpha}_{t|t-1}$ given by

$S_{t|t-1} = E[(\alpha_t - \widehat{\alpha}_{t|t-1})(\alpha_t - \widehat{\alpha}_{t|t-1})' | F_{t-1}].$

By (2.68) and (2.63),

$\alpha_t - \widehat{\alpha}_{t|t-1} = \Omega_t(\alpha_{t-1} - \widehat{\alpha}_{t-1}) + u_t.$

Plugging the above expression into the definition of $S_{t|t-1}$ and using $S_{t-1} = S_{t-1|t-1}$, we obtain

$S_{t|t-1} = \Omega_t S_{t-1} \Omega_t' + R_t.$ (2.69)

This equation has a clear intuitive interpretation: the uncertainty in predicting a new state with available information $F_{t-1}$ is the sum of the uncertainty that we had with respect to the previous state using the same information, measured by $S_{t-1}$, and the uncertainty of the noise of the state equation, $R_t$. The matrix $\Omega_t$ appears in order to relate the components of the state vectors at time $t-1$ and time $t$. If this matrix was the identity, $\Omega_t = I$, the state vector would evolve like a random walk, and the uncertainty in state estimation would increase continuously through the sum of matrices $R_t$. Since $\Omega_t$ is generally not the identity matrix the increase in uncertainty depends on its structure. For example, if we assume an AR(1) model, then the state vector is scalar, and $\Omega_t = \phi < 1$. The variance of the estimation follows the process $s_{t|t-1} = \phi^2 s_{t-1} + \sigma^2$ and only a portion of the uncertainty at $t-1$ is transferred to time $t$.

The second step of the Kalman filter is to predict the new observation $z_t$ given $F_{t-1}$. This prediction is calculated again using the conditional expectation given $F_{t-1}$ and we obtain, by the observation equation,

$\widehat{z}_{t|t-1} = E(z_t | F_{t-1}) = H_t' \widehat{\alpha}_{t|t-1}.$ (2.70)

The uncertainty of this prediction depends on the covariance matrix of the prediction errors, namely,
\( e_t = z_t - \hat{z}_{t|t-1} \).

Define
\( P_{t|t-1} = E[e_t e_t'],\) (2.71)

which can be obtained as follows. Subtracting the prediction (2.70) from the observation equation (2.62), we have
\( e_t = z_t - \hat{z}_{t|t-1} = H_t(\alpha_t - \hat{\alpha}_{t|t-1}) + \epsilon_t.\) (2.72)

Plugging this expression into the definition of \( P_{t|t-1} \), we obtain
\( P_{t|t-1} = H_t S_{t|t-1} H_t' + V_t.\) (2.73)

Equation (2.73) indicates that the uncertainty of the prediction \( \hat{z}_{t|t-1} \) is the sum of the uncertainty in the state vector and the measurement error. The prediction error that comes from the state estimation is modulated depending on the matrix \( H_t \). If \( H_t \) is the identity matrix, meaning that the observation \( z_t \) is the measurement of the state vector plus a random error, the measurement error of the observation is added to the error of the state variable.

The third and final step in the filter is to update the state estimation in light of the new information. Assume that \( z_t \) has been observed and the available information becomes \( F_t = (F_{t-1}, z_t) \). The new state estimation, \( \hat{\alpha}_t = \hat{\alpha}_{t|t} = E(\alpha_t|F_t) \), is calculated by regression (or by properties of the multivariate normal) with

\( E(\alpha_t|F_{t-1}, z_t) = E(\alpha_t|F_{t-1})\)
\( + \text{Cov}(\alpha_t, z_t|F_{t-1})\text{Var}(z_t|F_{t-1})^{-1}(z_t - E(z_t|F_{t-1})).\) (2.74)

In the above equation, the expectations \( E(\alpha_t|F_{t-1}) = \hat{\alpha}_{t|t-1} \) and \( E(z_t|F_{t-1}) = \hat{z}_{t|t-1} \) are known, as is the matrix \( \text{Var}(z_t|F_{t-1}) = P_{t|t-1} \). All that remains to be calculated is the covariance between the state and the new observation, which is given by

\( \text{Cov}(\alpha_t, z_t|F_{t-1}) = E[(\alpha_t - \hat{\alpha}_{t|t-1})(z_t - \hat{z}_{t|t-1})'] = E[(\alpha_t - \hat{\alpha}_{t|t-1})e_t'],\)

and by Eq. (2.72)

\( \text{Cov}(\alpha_t, z_t|F_{t-1}) = E[(\alpha_t - \hat{\alpha}_{t|t-1})((\alpha_t - \hat{\alpha}_{t|t-1})'H_t' + e_t')] = S_{t|t-1}H_t'.\) (2.75)

Equation (2.75) holds because the observation error \( e_t' \) is white noise and independent of \( \alpha_t - \hat{\alpha}_{t|t-1} \). Plugging this covariance into (2.74), we can write:
\( \hat{\alpha}_t = \hat{\alpha}_{t|t-1} + K_t(z_t - \hat{z}_{t|t-1}),\) (2.76)

where \( K_t \) is the matrix of regression coefficients, which is called the *Kalman gain* of the filter, and is given by
\( K_t = S_{t|t-1}H_t'P_{t|t-1}^{-1}.\)

Equation (2.76) indicates that the revision we make on the previous state estimation depends on the prediction error, \( e_t = z_t - \hat{z}_{t|t-1} \). If this error is zero, we do not modify the estimation because no new information is available. Otherwise, we make a modification in the state estimation that depends on the quotient of the uncertainty in the state estimation, \( S_{t|t-1} \), and the precision of the prediction error, \( P_{t|t-1}^{-1} \). The matrix \( H_t' \) allows us to compare these two matrices. An equivalent way of writing Eq. (2.76) is

\(\hat{\alpha}_t = (I - K_t H_t)\hat{\alpha}_{t|t-1} + K_t z_t,\)

which indicates that the state estimation is a linear combination of the two sources of information that are available to us. On the one hand, the prior estimation, \( \hat{\alpha}_{t|t-1} \), and, on the other, the observation \( z_t \). It can be proved that the weights of the two information sources are equal to their relative precision. The conditional covariance matrix of this estimation is

\( S_t = E[(\alpha_t - \hat{\alpha}_t)(\alpha_t - \hat{\alpha}_t)'|F_t]. \)

Replacing \( \hat{\alpha}_t \) with its expression in Eq. (2.76), we have

\( S_t = E[(\alpha_t - \hat{\alpha}_{t|t-1} - K_t e_t)(\alpha_t - \hat{\alpha}_{t|t-1} - K_t e_t)'|F_t], \)

and using (2.71) and (2.75) we finally obtain

\( S_t = S_{t|t-1} - S_{t|t-1}H_t'P_{t|t-1}^{-1}H_tS_{t|t-1} = (I - K_t H_t)S_{t|t-1}. \quad (2.77) \)

The system formed by Eqs. (2.68)-(2.70), (2.76), and (2.77) is referred to as the Kalman filter algorithm. To start the algorithm, an initial value must be given for the state variables, \( \alpha_0 \), and for the covariance matrix \( S_0 \). These initial values are not crucial, because the filter does not depend heavily on the initial conditions. The reader interested in the implementation of the algorithm can consult Young (1984), Harvey (1990), and Gómez and Maravall (1994). Corresponding to the Kalman filter algorithm, there are recursive algorithms for filtering and smoothing too. Interested readers are referred to Tsay (2010, chapter 11), among other textbooks on SSMs.

## 2.6 FORECASTING WITH LINEAR MODELS

### 2.6.1 Computing Optimal Predictors

Given a realization of a scalar time series \( z_t \) with \( T \) equally spaced observations, say \( z_T = (z_1, \dots, z_T) \), our goal is to predict \( z_{T+h} \), where \( h \) is a positive integer referring to as the *forecast horizon* and \( T \) is called the *forecast origin*. In this section, we focus on linear predictors, which are linear combinations of the available data, and employ the minimum squared prediction error (MSPE) criterion. That is, we use a square loss function. This is mainly for simplicity and is also commonly used in the literature. Denote the optimal point forecast by \( \hat{z}_T(h) \). It is easy to see that, under the MSPE criterion, we have \( \hat{z}_T(h) = E(z_{T+h}|z_T) \) and the prediction error, \( e_T(h) \), is given by

\( e_T(h) = z_{T+h} - \hat{z}_T(h). \quad (2.78) \)

Assume that the data are generated by an ARIMA(\( p, d, q \)) process, \( \phi_p(B)\nabla^d z_t = c + \theta_q(B)a_t \), with known parameters, including the variance of \( a_t \). Let \( \varphi_m(B) = \phi_p(B)\nabla^d \) be the AR operator of order \( m = p + d \) obtained by multiplying
