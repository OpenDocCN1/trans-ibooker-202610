see some specific examples later. It suffices to say now that the state vector \(\boldsymbol{\alpha}_t\) contains information needed to predict \(\boldsymbol{z}_{t+1}\) at the forecast origin \(t\). Consider a simple zero-mean AR(2) model,

\(z_t = \phi_1 z_{t-1} + \phi_2 z_{t-2} + a_t.\)

The 1-step ahead prediction of \(z_{t+1}\) at the forecast origin \(t\) is \(\widehat{z}_t(1) = \phi_1 z_t + \phi_2 z_{t-1}\). This quantity can be computed at the forecast origin because we know the model and have \(z_t\) and \(z_{t-1}\). Thus, given the model, to compute the 1-step ahead prediction of an AR(2) model at the forecast origin \(t\), we need two quantities \(z_t\) and \(z_{t-1}\). Therefore, we can define the state vector as \(\boldsymbol{\alpha}_t = (z_t, z_{t-1})'\). Since \(z_t\) is the first element of \(\boldsymbol{\alpha}_t\), the observation equation is simply

\(z_t = [1, 0]\boldsymbol{\alpha}_t,\)

implying that the variance of \(\boldsymbol{\varepsilon}_t\) is zero. The state equation for the AR(2) model is also easy to construct as

\(\boldsymbol{\alpha}_t = \begin{bmatrix} z_t \\ z_{t-1} \end{bmatrix} = \begin{bmatrix} \phi_1 & \phi_2 \\ 1 & 0 \end{bmatrix} \begin{bmatrix} z_{t-1} \\ z_{t-2} \end{bmatrix} + \begin{bmatrix} a_t \\ 0 \end{bmatrix} .\)

For the SSM in Eqs. (2.62) and (2.63), we assume that the matrices \(\boldsymbol{H}_t\) and \(\boldsymbol{\Omega}_t\) are known for all \(t\). In real applications, their parameters can consistently be estimated from the data under certain conditions. We discuss estimation later. For now, we provide further information concerning the generality of the SSM. Assume that we observe the position of a satellite in space at each time \(t\) by measuring the variable \(\boldsymbol{z}_t\), which is a three-dimensional vector (e.g. longitude, latitude, and height) and may contains some measurement errors, which are assumed to be independent of the actual location of the satellite. We also assume that satellite's position depends on a set of state variables, \(\boldsymbol{\alpha}_t\), which are not observable such as the gravity of earth and the speed and acceleration of the satellite. Our observed data \(\boldsymbol{z}_t\) is related to \(\boldsymbol{\alpha}_t\) via some aerodynamic force approximated by \(\boldsymbol{H}_t\), i.e.

\(\boldsymbol{z}_t = \boldsymbol{H}_t \boldsymbol{\alpha}_t + \boldsymbol{\varepsilon}_t,\)

where \(\boldsymbol{\varepsilon}_t\) denotes the measurement errors. Since the satellite would move in certain smooth manner, the state variable \(\boldsymbol{\alpha}_t\) should depend on its past value at time \(t - 1\) and some additional disturbance at time \(t\). It is then reasonable to assume that \(\boldsymbol{\alpha}_t\) can be modeled by the state transition equation in (2.63). Consequently, the SSM in Eqs. (2.62) and (2.63) could be used to study the process \(\boldsymbol{z}_t\). In this particular example, we do not provide a precise definition of the state vector \(\boldsymbol{\alpha}_t\). In this process, we often seek a state vector \(\boldsymbol{\alpha}_t\) that attains the minimum dimension in the sense that any state vector with dimension less than \(p\) cannot adequately represent the system under study. The aforementioned simple AR(2) example serves as an example as no state vector of dimension 1 can adequately represent the \(z_t\) process.

Structural time series models can be put into a state-space form. For instance, consider a monthly time series with periodicity \(s = 12\) and irregular component \(I_t = a_t\), we can define the state variable as

\(\boldsymbol{\alpha}_t = (\mu_t, \beta_t, S_t, S_{t-1}, \dots, S_{t-10})'\)