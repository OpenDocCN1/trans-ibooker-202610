where \( h_{T+1|T} = \boldsymbol{x}'_{T+1}(\boldsymbol{X}'_T \boldsymbol{X}_T)^{-1} \boldsymbol{x}_{T+1} \), and

\( (\boldsymbol{X}'_{T+1} \boldsymbol{X}_{T+1})^{-1} = (\boldsymbol{X}'_T \boldsymbol{X}_T)^{-1} [\boldsymbol{I} - \boldsymbol{x}_{T+1} \boldsymbol{x}'_{T+1} (\boldsymbol{X}'_T \boldsymbol{X}_T)^{-1} / (1 + h_{T+1|T})] \frac{\hat{\sigma}^2_{a,T}}{\hat{\sigma}^2_{a,T+1}}, \quad (2.103) \)

that are similar to Eqs. (2.99) and (2.101).

# ## 2.8.3 Maximum Likelihood (ML) Estimation

The ML estimation of an ARMA model for a stationary series \(\omega_t\) is carried out by decomposing the joint probability density function of the data as products of conditional and marginal density functions and maximizing it with respect to the parameters. The data are treated as fixed. The joint density function of \(\boldsymbol{\omega}^T_1\) is

\( f(\boldsymbol{\omega}^T_1) = f(\omega_1)f(\omega_2|\omega_1)f(\omega_3|\omega_2,\omega_1) \cdots f(\omega_T|\omega_{T-1}, \dots, \omega_1). \)

and, under joint normality, all the conditional distributions involved are normal and their expectations are the 1-step ahead predictions

\( E(\omega_t|\omega_{t-1}, \dots, \omega_1) = \widehat{\omega}_{t|-1}(1) = \omega_{t|t-1}. \)

Let \( e_{t|t-1} \) be the 1-step ahead prediction error of \(\omega_t\) using the information of \(\boldsymbol{\omega}^{t-1}_1\) and giving the parameters of the process. Thus,

\( e_{t|t-1} = \omega_t - \omega_{t|t-1}. \)

These prediction errors are highly related to the innovations \( a_t \) of the process, but they are not identical because they depend on the initial values. Calling \(\sigma^2 v_{t|t-1}\) the variance of these prediction errors, the joint density function of the data for an ARMA process can be written as

\( f(\boldsymbol{\omega}_T) = \prod_{t=1}^T \sigma^{-1} v^{-1/2}_{t|t-1} (2\pi)^{-1/2} \exp \left\{ - \frac{1}{2\sigma^2} \sum_{t=1}^T \frac{(\omega_t - \omega_{t|t-1})^2}{v_{t|t-1}} \right\}, \)

where \( v_{1|0} \) and \( \boldsymbol{\omega}_{1|0} \) are initial values. Taking natural logarithm and calling \( \boldsymbol{\beta} = (\mu, \sigma^2, \phi_1, \dots, \phi_p, \theta_1, \dots, \theta_q) \) the vector of parameters to be estimated, the log likelihood function of the ARMA model to be maximized is

\( L(\boldsymbol{\beta}) = -\frac{1}{2} T \ln \sigma^2 - \frac{1}{2} \sum_{t=1}^T \ln v_{t|t-1} - \frac{1}{2} \sum_{t=1}^T \frac{e^2_{t|t-1}}{\sigma^2 v_{t|t-1}}. \quad (2.104) \)

In practice, the likelihood function can be efficiently computed for given parameter values by putting the model in the state-space form and using the Kalman filter. Readers are referred to the algorithm discussed in Section 2.4, which provides a fast-recursive way to compute \(\omega_{t|t-1}\) and \(\sigma^2 v_{t|t-1}\). The maximization of the exact likelihood function is carried out using a nonlinear optimization algorithm (see Box at al., 2015; Shumway and Stoffer, 2017).