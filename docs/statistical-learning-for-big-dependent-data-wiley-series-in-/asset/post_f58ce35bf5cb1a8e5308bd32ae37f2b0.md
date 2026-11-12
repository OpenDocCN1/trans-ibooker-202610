
method and it can be shown that the universal-kriging predictor is

\[
\widehat{Y}(s_0) = x(s_0)'\widehat{\beta}_{gls} + c_y(s_0)'\Sigma_z^{-1}(Z - X\widehat{\beta}_{gls}), \tag{9.45}
\]

where \(\widehat{\beta}_{gls} = (X'\Sigma_z^{-1}X)^{-1}X'\Sigma_z^{-1}Z\) is the generalized least squares estimator of \(\beta\). The universal-kriging variance is

\[
\sigma^2_{y,uk}(s_0) = c_y(s_0)'\Sigma_z^{-1}c_y(s_0)
+ [x(s_0) - X\Sigma_z^{-1}c_y(s_0)]'(X'\Sigma_z^{-1}X)^{-1}[x(s_0) - X\Sigma_z^{-1}c_y(s_0)], \tag{9.46}
\]

where the subscript _uk_ signifies universal kriging.

In real applications, one uses the universal kriging when some covariates are available. The ordinary kriging can be used in the absence of covariates. Indeed, most spatial packages use universal kriging.

## 9.4 LATTICE PROCESSES

Lattice processes \(\{Y(s)|s \in D\}\) are defined on a finite or countable subset \(D \subset R^d\). They are used to study properties of and to model lattice data **Z** = {_Z_(**_s_**<sub>1</sub>)_,...,Z_(**_s_**_<sub>m</sub>_)}, where {**_s_**<sub>1</sub>_,...,**s**<sub>m</sub>_} are data locations defined on discrete spatial features such as grid nodes or pixels. Often an \(m \times m\) spatial neighborhood matrix **W** = [_w<sub>ij</sub>_] is defined with zero diagonal elements and _w<sub>ij</sub>_ = 1 if location **_s_**_<sub>i</sub>_ is considered a neighbor of location **_s_**_<sub>j</sub>_. The word _neighbor_ is defined by the data analyst based on her domain knowledge. For instance, one may define **_s_**_<sub>i</sub>_ and **_s_**_<sub>j</sub>_ as neighbors if \(\|s_i - s_j\| \le r\) for some positive real number _r_.

Moran (1950) considered the simple case of real line with _s<sub>i</sub>_ = _i_ and _i_ = 1_,...,m_, and defined _w<sub>ij</sub>_ = _w<sub>ji</sub>_ = 1 if \(0 < |s_i - s_j| \le 1\). For detrended data **Z** with _E_(**Z**) = **0**, he proposed the statistic

\[
I = \frac{Z'WZ}{Z'Z}, \tag{9.47}
\]

for testing the null hypothesis _H_<sub>0</sub>:_Z_(**_s_**<sub>1</sub>)_,...,Z_(**_s_**_<sub>m</sub>_) are _iid_. Since the data are mean-adjusted, the numerator and denominator of Eq. (9.47) are the neighboring covariance and variance of the process, respectively. Thus, the test belongs to a correlation test. Cliff and Ord (1981) proposed to use more general **W**, where **W** may not be symmetric and its off-diagonal elements do not have to be 1, and coined the test statistic _Moran’s I_. This statistic has been widely used in the literature for testing the null hypothesis of no spatial autocorrelation in **Z**. Similarly to the _F_-statistic of empirical variogram in Eq. (9.23), permutation-based reference distribution can be used for the Moran’s I test.

### 9.4.1 Markov-Type Models

A key difference between spatial and time-series processes is that there is no natural ordering in _R<sup>d</sup>_ (with _d_ > 1) for the spatial processes. In the literature, various statistical models have been developed for spatial processes in _R<sup>d</sup>_. Many of them make use of the Markov property with certain neighboring system. See, for instance, Besag (1974). In this section, we briefly introduce some Markov-type models for spatial data.

The simplest Markov model for lattice processes is the first-order unilateral spatial process in \(R^1\) with \(D = \{1, 2, \dots, n\}\), where \(n\) is the sample size. The model is

\[
Y(s) = \phi Y(s-1) + \epsilon(s), \quad s = 2, \dots, n, \tag{9.48}
\]

where \(|\phi| < 1\), \(\{\epsilon(s)\} \sim^{iid} N(0, \sigma^2)\) and \(\epsilon(s)\) is independent of \(Y(s-1)\), which is similar to the autoregressive model of order 1, i.e. AR(1) model of Chapter 2. Since the sample size \(n\) is finite, one needs an initial condition for the spatial process to be a stationary Gaussian process. The initial condition is

\[
Y(1) \sim N[0, \sigma^2/(1 - \phi^2)]. \tag{9.49}
\]

It is easy to see that \(\text{cor}(Y(s), Y(s-1)) = \phi\) and, for \(\mathbf{Y} = [Y(1), \dots, Y(n)]'\), we have \(E(\mathbf{Y}) = \mathbf{0}\) and

\[
\text{Var}(\mathbf{Y}) = \frac{\sigma^2}{1 - \phi^2} \left[ \begin{array}{cccc} 1 & \phi & \cdots & \phi^{n-1} \\ \phi & 1 & \cdots & \phi^{n-2} \\ \vdots & \vdots & \ddots & \vdots \\ \phi^{n-1} & \phi^{n-2} & \cdots & 1 \end{array} \right]_{n \times n}. \tag{9.50}
\]

The joint distribution of \(\mathbf{Y}\) is Gaussian and it is easy to see that

\[
[\text{Var}(\mathbf{Y})]^{-1} = \frac{1}{\sigma^2} \left[ \begin{array}{cccccccc} 1 & -\phi & 0 & 0 & \cdots & 0 & 0 & 0 \\ -\phi & 1 + \phi^2 & -\phi & 0 & \cdots & 0 & 0 & 0 \\ 0 & -\phi & 1 + \phi^2 & -\phi & \cdots & 0 & 0 & 0 \\ \vdots & \vdots & \vdots & \vdots & & \vdots & \vdots & \vdots \\ 0 & 0 & 0 & 0 & \cdots & -\phi & 1 + \phi^2 & -\phi \\ 0 & 0 & 0 & 0 & \cdots & 0 & -\phi & 1 \end{array} \right].
\]

In spatial statistics, this model belongs to the class of conditional autoregressive (CAR) models.

In general, suppose that \(D = \{s_1, \dots, s_n\} \subset R^d\). Let \(\mathbf{Y} = [Y(s_1), \dots, Y(s_n)]'\) and \(\mathbf{Y}_{-i} = [Y(s_1), \dots, Y(s_{i-1}), Y(s_{i+1}), \dots, Y(s_n)]'\) for \(i = 1, \dots, n\). That is, \(\mathbf{Y}_{-i}\) consists of all data in \(\mathbf{Y}\) but \(Y(s_i)\). In addition, let \(N(s_i)\) be a set of prespecified locations that define the _neighborhood_ of \(s_i\), \(i = 1, \dots, n\). Then, a CAR model on \(D\) can be specified as follows: the sequence of conditional distributions \(\{[Y(s_i) | \mathbf{Y}_{-i}] | i = 1, \dots, n\}\) each of which follows a Gaussian distribution with mean and variance

\[
E[Y(s_i) | \mathbf{Y}_{-i}] = \sum_{s_j \in N(s_i)} c_{ij} Y(s_j), \tag{9.51}
\]

\[
\text{Var}[Y(s_i) | \mathbf{Y}_{-i}] = \tau_i > 0, \tag{9.52}
\]

where the coefficients \(c_{ij}\) must satisfy certain conditions to guarantee the existence of the joint distribution of \(\mathbf{Y}\). See, for instance, the condition below.

Since \( s_i \ni N(s_i) \), we define \( c_{ii} = 0 \). Here we use the notation \(\ni\) to denote *not in*. Furthermore, let \( c_{ij} = 0 \) if \( s_j \ni N(s_i) \). Then, Eq. (9.51) can be written as

\[
E[Y(s_i)|\boldsymbol{Y}_{-i}] = \sum_{j=1}^n c_{ij} Y(s_j), \quad i=1, \dots, n.
\]

Let \(\boldsymbol{C} = [c_{ij}]\) be the \( n \times n \) coefficient matrix and \(\boldsymbol{M} = \text{diag}\{\tau_1^2, \dots, \tau_n^2\}\). Besag (1974) showed that if \(\boldsymbol{M}^{-1}(\boldsymbol{I} - \boldsymbol{C})\) is positive-definite, then the joint distribution of the CAR model is

\[
\boldsymbol{Y} \sim N[\boldsymbol{0}, (\boldsymbol{I} - \boldsymbol{C})^{-1}\boldsymbol{M}].
\]

An example of this type of Markov model is given below under the HM framework:

- Data model: For \( i=1, \dots, n \),
  \( Z(s_i)|Y(s_i) \sim_{\text{ind}} \text{Po}[\exp\{Y(s_i)\}] \),
  where \(\text{Po}(u)\) denotes a Poisson distribution with mean \( u \).

- Process model: Conditional on \( \boldsymbol{\beta}, \tau^2, \phi \) and for \( \boldsymbol{Y} = [Y(s_1), \dots, Y(s_n)]' \),
  \( \boldsymbol{Y} | \boldsymbol{\beta}, \tau^2, \phi \sim N[\boldsymbol{X}\boldsymbol{\beta}, \tau^2(\boldsymbol{I} - \phi\boldsymbol{H})^{-1}] \),
  where \(\boldsymbol{X} = [x_j(s_i)]\) is an \( n \times k \) matrix obtained from the \( k \)-dimensional covariate \( x(s_i) \), \(\boldsymbol{H}\) is a known neighborhood matrix with zero diagonal elements and \( h_{ij} = 1 \) if \( s_j \in N(s_i) \), and the parameter space of \( \phi \) ensures that the matrix inverse exists.

The neighborhood \( N(s_i) \) of a lattice process \( Y(s_i) \) is often defined as the collection of all other locations \( s_j \ (j \neq i) \) such that

\[
[Y(s_i)|\boldsymbol{Y}_{-i}] = [Y(s_i)|\boldsymbol{Y}[N(s_i)]], \quad i=1, \dots, n,
\]

where \( \boldsymbol{Y}[N(s_i)] = [Y(s_j) : s_j \in N(s_i)] \). If the conditional distributions of Eq. (9.55) define the joint distribution of \( \boldsymbol{Y} \), then the process \( \{Y(s)|s \in D\} \) is called a *Markov random field* (MRF). If one further assumes that

\[
Y(s_i)|Y[N(s_i)] \sim \text{Po} \left( \exp \left\{ \alpha_i + \sum_{s_j \in N(s_i)} \theta_{ij} Y(s_j) \right\} \right), \quad i=1, \dots, n,
\]

where \( \theta_{ij} = \theta_{ji} \), then the model is referred to as the *auto Poisson model*; see Besag (1974). In the literature, graphs are used to represent the conditional dependence of Eq. (9.55). Interested readers are referred to Cressie and Wikle (2011, chapter 4). If the data are binary, Besag (1974) shows that, under certain conditions,

\[
[Y(s_i)|\boldsymbol{Y}[N(s_i)]] = \frac{\exp\{\alpha_i Y(s_i) + \sum_{s_j \in N(s_i)} \theta_{ij} Y(s_i) Y(s_j)\}}{1 + \exp\{\alpha_i Y(s_i) + \sum_{s_j \in N(s_i)} \theta_{ij} Y(s_j)\}}, \quad Y(s_i) \in \{0, 1\},
\]

\( \theta_{ij} = \theta_{ji} \). This is called an *auto logistic model*.
