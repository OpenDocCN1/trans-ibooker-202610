(ii) estimate the model parameters from the data or correlation lags; and (iii) obtain the spectral estimate by using the (estimated) model parameters. The major advantage of these parametric methods is that higher frequency resolution is achievable, since the data are not windowed [17]. It can be shown that these methods imply an infinite extension of the autocorrelation sequence [18]. We also note that this approach is indirect, since we must first find the appropriate model and then use it to estimate the PSD. Let us recall the basic spectral relations developed previously for linear systems with random inputs. The measurement of the output spectrum of a process is related to the input spectrum by the factorization relations:

```
S_{yy}(z) = H(z)H^*(z)S_{xx}(z) = |H(z)|^2 S_{xx}(z) \tag{2.38}
```

where

+   - x is the input process
+   - H is the linear system transfer function
+   - y is the measurement.

Since we are taking the signal processing viewpoint, then we will assume that only the measured data, y, are available and not the excitation, x. In fact, if both x and y are available, then the problem is called the system identification problem (see Ljung and Soderstrom [19] for details) and the ARMAX model will be used. However, in this section, we restrict ourselves to purely signal representations and select the AR and ARMA model sets as representative for spectral estimation.

In summary, the modern method of parametric spectral estimation is given by:

+   - Select a representative model set (AR, ARMA).
+   - Estimate the model parameters from the data, that is given {y(t)}, find
+     \hat{\Theta} = {\hat{A}(q^{-1}), \hat{C}(q^{-1})} and \hat{R}_{ee}.
+   - Estimate the PSD using these parameters, that is,
+     S_{yy}(z) \approx \hat{S}_{yy}(z, \hat{\Theta}) = \hat{H}(z)\hat{H}^*(z)S_{ee}(z)

# 2.5.2.1 Autoregressive (All-Pole) Spectral Estimation

The AR or all-pole model is characterized by the difference equation

```
A(q^{-1})y(t) = \sqrt{R_{ee}} e(t) \tag{2.39}
```

where

+   - y is the measured data
+   - e is a zero-mean, white noise sequence with variance, R_{ee} and
+   - A is an N_a-th-order polynomial in backward shift operator q^{-1}.