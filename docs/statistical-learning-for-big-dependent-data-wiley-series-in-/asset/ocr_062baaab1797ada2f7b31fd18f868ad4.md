# 2.2.3 The ARMA Process

Combining properties of AR and MA processes together, we obtain an ARMA process that has proven to be widely applicable and useful in real applications. The simplest ARMA process is the ARMA(1,1) model given by

$(1 - \phi_1 B)\widetilde{z}_t = (1 - \theta_1 B)a_t,$

where, again, $\widetilde{z}_t = z_t - E(z_t)$ is the deviation of $z_t$ from its mean and $\{a_t\}$ is a white noise process with mean zero and variance $\sigma^2 < \infty$. The ARMA(1,1) process is stationary if $|\phi_1| < 1$, and it is invertible if $|\theta_1| < 1$. Also, we assume $\phi_1 \neq \theta_1$ to avoid the cancellation of the AR and MA polynomials.

In general, the ARMA(p,q) process is defined as

$(1 - \phi_1 B - \cdots - \phi_p B^p)\widetilde{z}_t = (1 - \theta_1 B - \cdots - \theta_q B^q)a_t, \quad (2.22)$

or, in the compact notation with back-shift operator,

$\phi_p(B)\widetilde{z}_t = \theta_q(B)a_t,$

where we assume that no common roots exist between the AR operator $\phi(B)$ and the MA operator $\theta(B)$. The process $z_t$ is stationary if all of the roots of $\phi_p(B) = 0$ are outside the unit circle, and invertible if all of the roots of $\theta_q(B) = 0$ are outside the unit circle.

The MA representation of $z_t$ can be obtained using

$\widetilde{z}_t = \phi_p(B)^{-1}\theta_q(B)a_t = \psi(B)a_t,$

where $\psi_t$ can be obtained by equating the coefficients of $B^i$ in the identity $\psi(B)\phi_p(B) = \theta_q(B)$. For instance, an ARMA(1,1) has $\psi(B) = 1 + (\phi_1 - \theta_1)B + \phi_1(\phi_1 - \theta_1)B^2 + \phi_1^2(\phi_1 - \theta_1)B^3 + \cdots$ . Analogously, we can obtain the AR representation of an ARMA(p,q) process via

$\pi(B)\widetilde{z}_t = \theta_q^{-1}(B)\phi_p(B)\widetilde{z}_t = a_t,$

where $\pi(B) = 1 - \sum_{i=1}^\infty \pi_i B^i$ with $\pi_i$ being obtained by equating the coefficients of $B^i$ in the identity $\phi_p(B) = \theta_q(B)\pi(B)$.

To calculate the autocovariances of $z_t$, we multiply (2.22) by $\widetilde{z}_{t-h}$ and take expectations, resulting in

$\gamma_h - \phi_1 \gamma_{h-1} - \cdots - \phi_p \gamma_{h-p} = E[a_t\widetilde{z}_{t-h}] - \sum_{i=1}^q \theta_i E[a_{t-i}\widetilde{z}_{t-h}]. \quad (2.23)$

For $h = q$, we have $\gamma_q - \phi_1 \gamma_{q-1} - \cdots - \phi_p \gamma_{q-p} = -\theta_q \sigma^2 \neq 0$, but, for $h > q$, all of the expectations on the right side of Eq. (2.23) are zero and $\gamma_h - \phi_1 \gamma_{h-1} - \cdots - \phi_p \gamma_{h-p} = 0$. Dividing by $\gamma_0$, we obtain

$\rho_h - \phi_1 \rho_{h-1} - \cdots - \phi_p \rho_{h-p} = 0, \quad h > q. \quad (2.24)$