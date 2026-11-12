mean and finite variance \(\sigma^2\). Using the lag operator \(B\), the equation of an AR(1) model is

\((1 - \phi_1 B)\tilde{z}_t = a_t.\)

By either repeated substitutions or inverting the operator \((1 - \phi_1 B)\) with

\((1 - \phi_1 B)^{-1} = 1 + \phi_1 B + \phi_1^2 B^2 + \cdots , \quad (2.11)\)

the process can be written as

\(\tilde{z}_t = a_t + \sum_{i=1}^{\infty} \phi_1^i a_{t-i}. \quad (2.12)\)

In the water reservoir example, if the amount of water is determined by a policy that depends on the deviations of the previous \(p\) periods, we have an AR(\(p\)) model with the general equation

\(\tilde{z}_t = \phi_1 \tilde{z}_{t-1} + \cdots + \phi_p \tilde{z}_{t-p} + a_t. \quad (2.13)\)

The coefficients \(\phi_i\) must satisfy certain conditions, explained next, for the process to be stationary. Using the lag operator, \(B\), the equation for an AR(\(p\)) model is

\((1 - \phi_1 B - \cdots - \phi_p B^p)\tilde{z}_t = a_t, \quad (2.14)\)

and letting \(\phi_p(B) = 1 - \phi_1 B - \cdots - \phi_p B^p\) be the polynomial of degree \(p\) in the lag operator, we have

\(\phi_p(B)\tilde{z}_t = a_t, \quad (2.15)\)

which is the general expression of an autoregressive process. The representation in Eq. (2.8) is called the AR(\(\infty\)) form of a linear stationary process.

The *characteristic equation* of the AR(\(p\)) process is defined as

\(\phi_p(B) = 0, \quad (2.16)\)

considered as a function of \(B\). This characteristic equation has \(p\) roots \(G_1^{-1}, \dots, G_p^{-1}\), which can be complex and are generally distinct in practice (see Appendix 2.A), and we have

\(\phi_p(B) = \prod_{i=1}^p (1 - G_i B),\)

where the components \((1 - G_i B)\) are the factors of the characteristic equation. It can be proved that the process is stationary if \(|G_i| < 1\) for all \(i\). In particular, for an AR(1) model, if \(|G_1| = |\phi_1| < 1\), then the process is stationary.

**2.2.1.1 Autocorrelation Functions** Consider an AR(\(p\)) process. Multiplying (2.13) by \(\tilde{z}_{t-h}\) (\(h > 0\)), taking expectations, and dividing the result by the variance of the process, we find that the autocorrelation coefficients of an AR(\(p\)) verify the following difference equation:

\(\rho_h = \phi_1 \rho_{h-1} + \cdots + \phi_p \rho_{h-p}, \quad h > 0.\)