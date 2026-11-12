method and it can be shown that the universal-kriging predictor is

```
\(\widehat{Y}(s_0) = x(s_0)'\widehat{\beta}_{gls} + c_y(s_0)'\Sigma_z^{-1}(Z - X\widehat{\beta}_{gls}),\) (9.45)
```

where \(\widehat{\beta}_{gls} = (X'\Sigma_z^{-1}X)^{-1}X'\Sigma_z^{-1}Z\) is the generalized least squares estimator of \(\beta\). The universal-kriging variance is

```
\(\sigma^2_{y,uk}(s_0) = c_y(s_0)'\Sigma_z^{-1}c_y(s_0)\)
\(+ [x(s_0) - X\Sigma_z^{-1}c_y(s_0)]'(X'\Sigma_z^{-1}X)^{-1}[x(s_0) - X\Sigma_z^{-1}c_y(s_0)],\) (9.46)
```

where the subscript _uk_ signifies universal kriging.

In real applications, one uses the universal kriging when some covariates are available. The ordinary kriging can be used in the absence of covariates. Indeed, most spatial packages use universal kriging.

# 9.4 LATTICE PROCESSES

Lattice processes \(\{Y(s)|s \in D\}\) are defined on a finite or countable subset \(D \subset R^d\). They are used to study properties of and to model lattice data **Z** = {_Z_(**_s_**<sub>1</sub>)_,...,Z_(**_s_**_<sub>m</sub>_)}, where {**_s_**<sub>1</sub>_,...,**s**<sub>m</sub>_} are data locations defined on discrete spatial features such as grid nodes or pixels. Often an \(m \times m\) spatial neighborhood matrix **W** = [_w<sub>ij</sub>_] is defined with zero diagonal elements and _w<sub>ij</sub>_ = 1 if location **_s_**_<sub>i</sub>_ is considered a neighbor of location **_s_**_<sub>j</sub>_. The word _neighbor_ is defined by the data analyst based on her domain knowledge. For instance, one may define **_s_**_<sub>i</sub>_ and **_s_**_<sub>j</sub>_ as neighbors if \(\|s_i - s_j\| \le r\) for some positive real number _r_.

Moran (1950) considered the simple case of real line with _s<sub>i</sub>_ = _i_ and _i_ = 1_,...,m_, and defined _w<sub>ij</sub>_ = _w<sub>ji</sub>_ = 1 if \(0 < |s_i - s_j| \le 1\). For detrended data **Z** with _E_(**Z**) = **0**, he proposed the statistic

```
\(I = \frac{Z'WZ}{Z'Z},\) (9.47)
```

for testing the null hypothesis _H_<sub>0</sub>:_Z_(**_s_**<sub>1</sub>)_,...,Z_(**_s_**_<sub>m</sub>_) are _iid_. Since the data are mean-adjusted, the numerator and denominator of Eq. (9.47) are the neighboring covariance and variance of the process, respectively. Thus, the test belongs to a correlation test. Cliff and Ord (1981) proposed to use more general **W**, where **W** may not be symmetric and its off-diagonal elements do not have to be 1, and coined the test statistic _Moran’s I_. This statistic has been widely used in the literature for testing the null hypothesis of no spatial autocorrelation in **Z**. Similarly to the _F_-statistic of empirical variogram in Eq. (9.23), permutation-based reference distribution can be used for the Moran’s I test.

# 9.4.1 Markov-Type Models

A key difference between spatial and time-series processes is that there is no natural ordering in _R<sup>d</sup>_ (with _d_ > 1) for the spatial processes. In the literature, various statistical models have been developed for spatial processes in _R<sup>d</sup>_. Many of them make use of the Markov property with certain neighboring system. See, for instance,