where \(z_t^*\) is used to denote the complete data set and \((1-B)y_t = a_t\) under the random walk model. Taking the first difference of the prior equation, we have

\(\nabla z_t^* = w_0 x_t + a_t,\)

where \(x_t = (1-B)I_t^{(h)}\), so that \(x_h = 1\), \(x_{h+1} = -1\), and \(x_t = 0\), otherwise. The least squares estimate of \(w_0\) in this regression equation is given by

\(\hat{w}_0 = \frac{\sum_t x_t \nabla z_t^*}{\sum (x_t)^2} = \frac{\nabla z_h^* - \nabla z_{h+1}^*}{2} = \frac{z_h^* - z_{h-1} - z_{h+1} + z_h^*}{2} = z_h^* - \frac{z_{h-1} + z_{h+1}}{2},\)

and the estimate of the missing value, via Eq. (4.12), is

\(\hat{z}_h = \frac{z_{h-1} + z_{h+1}}{2},\)

which does not depend on \(z_h^*\). Note that this equation is a particular case of (4.11) because the inverse process of a random walk is an MA(1) process with unit parameter and, therefore, its first-order autocorrelation coefficient is well defined and equal to \(-0.5\). Thus, the missing value is simply estimated by the average of the observations \(z_{h-1}\) and \(z_{h+1}\). The fact that \(\hat{z}_h\) does not depend on \(z_h^*\) continues to hold when the model parameters are estimated simultaneously using the aforementioned scalar filter algorithm.

This filtering approach can easily be generalized if a block of \(g\) observations is missing, say from \(h\) and \(h+g-1\). Let \(\mathbf{z}_{1[-h:(h+g-1)]} = (z_1, \dots, z_{h-1}, z_{h+g}, \dots, z_T)'\) be the available data. Putting zeros in these \(g\) missing values to construct the complete data set \(\mathbf{z}^*\), and defining \(g\) indicator variables \(I_t^{(h+i-1)}\) for \(i=1, \dots, g\), we estimate the intervention model

\(z_t^* = \sum_{i=1}^g w_i I_t^{(h+i-1)} + z_t, \quad (4.14)\)

and the missing values are obtained by

\(\hat{z}_{h+i} = z_{*+h+i}^* - \hat{w}_i, \quad i=0, \dots, h+g-1.\)

This idea can be generalized to any combination of missing values. Of course, the efficacy of the algorithm depends on the random patterns and numbers of missing values. Note that if the series is stationary with a non-zero mean, this average value should be subtracted from all the observations and the interpolation obtained, as explained before, will be added to the sample mean to obtain the original value. With integrated time series the intervention analysis approach can always be used.

# 4.2.2 Multivariate Interpolation

Given a set of related time series \(\mathbf{z}_t\), missing values may occur at \(\mathbf{z}_h\) for the whole vector or for some of its components. One can apply the scalar filter algorithm of the previous section to individual time series separately, but such an approach fails to make use of the interdependence between the series. A better procedure is to use