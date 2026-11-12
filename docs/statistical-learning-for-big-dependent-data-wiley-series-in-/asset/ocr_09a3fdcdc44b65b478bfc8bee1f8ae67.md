(AR) representation of the series \(\pi(B)y_t = a_t\) or the moving-average (MA) representation of the series \(y_t = \psi(B)a_t\), where it is understood that \(\pi(B) = \phi(B)(1-B)^d/\theta(B)\) and \(\psi(B) = \theta(B)/[\phi(B)(1-B)^d]\) and \(\pi(B)\psi(B) = 1\). In practice, both \(\pi_i\) and \(\psi_j\) can be obtained by long division of two polynomials in \(B\).

We assume that an unusual event of size \(w_0\) happened at time \(t = h\). In this case, by using the indicator variable in Eq. (4.1), we can write the observed time series as

\(z_t = w_0 I_t^{(h)} + y_t = w_0 I_t^{(h)} + \psi(B)a_t. \quad (4.3)\)

In other words, we have

\(z_t = \left\{ \begin{array}{ll} y_t, & \text{if } t \neq h, \\ y_h + w_0, & \text{if } t = h. \end{array} \right.\)

Representation (4.3) is a special case of the transfer functions presented in Chapter 3 with input variable \(I_t^{(h)}\). It is also possible that an unusual event at time \(t = h\) can affect the time series under study over several time periods. For instance, a big winter snow storm in Chicago is likely to interrupt the air traffic at the O'Hare International Airport for several days. In this case, we can write the observed time series as

\(z_t = \sum_{i=0}^m w_i I_t^{(h+i)} + \psi(B)a_t, \quad (4.4)\)

where \(m\) is a finite positive integer and \(w_i\) is the impact of the event on time period \(t = h + i, i = 0, ..., m\). Here we use a parameter \(w_i\) to denote the impact of the event on the series at \(t = h + i\). In practice, it might be hard to know a *priori* the value of \(m\). Also, it is likely that the impacts will decay smoothly as time elapses. It is then logical to postulate the following model for \(z_t\):

\(z_t = \frac{w_0}{1 - \delta B} I_t^{(h)} + \psi(B)a_t = \frac{w_0}{1 - \delta B} I_t^{(h)} + y_t, \quad (4.5)\)

where \(w_0\) is the initial impact of the unusual event and \(0 < \delta < 1\) denotes the discounting rate of the impact. This is so because \(w_0/(1 - \delta B) = w_0(1 + \delta B + \delta^2 B^2 + \cdots)\). More specifically, Eq. (4.5) shows that

\(z_t = \left\{ \begin{array}{ll} y_t, & \text{if } t < h, \\ y_h + w_0, & \text{if } t = h, \\ y_{h+1} + w_0\delta, & \text{if } t = h+1, \\ y_{h+2} + w_0\delta^2, & \text{if } t = h+2, \\ \vdots & \vdots \end{array} \right.\)

where \(y_t\) is the unobserved uncontaminated time series.

In general, a flexible model to describe the impact of an unusual event on a time series at time index \(t = h\) is

\(z_t = \frac{w(B)}{\delta(B)} I_t^{(h)} + y_t, \quad (4.6)\)

where \(w(B) = w_0 + w_1 B + \cdots + w_m B^m\) and \(\delta(B) = 1 - \delta_1 B - \cdots - \delta_r B^r\) are two polynomials in \(B\) with finite orders \(m\) and \(r\), and \(y_t = \psi(B)a_t\) is the uncontaminated