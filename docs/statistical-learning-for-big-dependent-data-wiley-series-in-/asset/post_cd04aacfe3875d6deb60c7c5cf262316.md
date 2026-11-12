
![](img/d4c84ae3c2178f4c506af3dd2ed13122_27_0.png)

Figure 1.8 Time plots of daily sales in natural logarithms of a clothing brand in 25 provinces in China from 1 January 2008 to 9 December 2012.

series analysis. To gain further insight, Figure 1.9 shows the same time plots for the first eight provinces. They are Beijing, Fujian, Guangdong, Guangxi, Hainan, Hebei, Henan, and Hubei. The levels of the plots are adjusted so that there is no overlapping in the figure. A special characteristic of the plots is that local peaks occur irregularly in the early part of each year, followed by certain drops in sales. This is caused by the Chinese New Year holidays that vary from year to year. In addition, the peaks do not occur for all provinces; see the second plot (from the top) of Figure 1.9. Analyzing these series jointly requires modeling the common features as well as the variations from one province to another.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_27_1.png)

Figure 1.9 Time plots of daily sales in natural logarithms of a clothing brand in eight provinces in China from 1 January 2008 to 9 December 2012.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_28_0.png)

Figure 1.10 Time plots of hourly observations of the particulate matter PM$_{2.5}$ from two monitoring stations in southern Taiwan from 1 January 2006 to 31 December 2015.

Figure 1.10 shows the time plots of hourly observations of the particulate matter PM$_{2.5}$ from two monitoring stations in southern Taiwan from 1 January 2006 to 31 December 2015. We drop the data of February 29 for simplicity, resulting in a sample size 87 600. The data are given in the file `TaiwanPM25.csv`. The two series are parts of many monitoring stations throughout the island. Also available at each station are measurements of temperature, dew points, and other air pollution indexes. Thus, this is a simple example of big dependent data. The time plots exhibit certain strong annual patterns and seem to indicate a minor decreasing trend. Figure 1.11 shows the first 30 000 sample autocorrelations of the PM$_{2.5}$ series of Station 1. The annual cycle with periodicity $s = 24 \times 365 = 8760$ is clearly seen from the autocorrelations. Figure 1.12, on the other hand, shows the first 120 sample autocorrelations of the same time series. The plot shows that there also exists a daily cyclic pattern in the

![](img/d4c84ae3c2178f4c506af3dd2ed13122_28_1.png)

Figure 1.11 Sample autocorrelations of PM$_{2.5}$ measurement of Station 1 in southern Taiwan. The first 30 000 autocorrelations are shown. The annual cycle with periodicity 8760 is clearly seen.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_29_0.png)

Figure 1.12 Sample autocorrelations of PM2.5 measurement of Station 1 in southern Taiwan. The first 120 autocorrelations are shown. The daily cyclical pattern with periodicity 24 is clearly seen.

PM2.5 series with a periodicity λ = 24. Consequently, analysis of such a big dependent data should consider not only the overall trend, but also various cyclic patterns with different frequencies.

Finally, Figure 1.13 plots the monitoring stations (in circle) and the ozone levels (in color-coded squares) of US Midwestern states on 20 June 1987. The data set is available from the R package fields. The states are added to the plot so that readers can understand the geographical features of ozone levels in the Midwest. Clearly, on this particular day, high ozone readings appeared in the southwest of Illinois and south of Indiana. Also, ozone levels of neighboring monitoring stations are similar. This is an example of spatial process discussed in Chapter 9.

The six data sets presented are examples of multivariate spatio-temporal series, where we observe the values of several variables of interest over time and space.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_29_1.png)

Figure 1.13 Locations and ozone levels at various monitoring stations in US Midwestern states. The measurements are 8-hour averages (9 a.m. to 4 p.m.) on 20 June 1987.

The time sequence introduces dynamic dependency in a given series, which may be time varying. The data also show cross dependence between series and the strength of cross dependence may, in turn, depend on geographical locations or economic relations among countries. One of the main focuses of the book is to investigate both the dynamic and cross dependence between multiple time series.

The objectives of analyzing big dependent data include (a) to study the relationships between the series, (b) to explore the dynamic dependence within a series and across series, and (c) to predict the future values of the series. For spatial processes, one might be interested in predicting the variable of interest at a location where no observations are available. The first two objectives are referred to as data analysis and modeling, whereas the third objective involves statistical inference. Both modeling and inference require not only methods for handling data, but also the theory on which proper inference can be drawn. To this end, we need a framework for understanding the characteristics of the time series under study and for making sound predictions. We discuss this framework by starting with stochastic processes in the next section.

## 1.2 STOCHASTIC PROCESSES

Theory and properties of stochastic processes form the foundation for statistical analysis and inference of dependent data.

### 1.2.1 Scalar Processes

A scalar stochastic process is a sequence of random variables {z_t}, where the subscript t takes values in a certain set C. In most cases, the elements of C are ordered and often correspond to certain calendar time (days, months, years, etc.). The resulting stochastic process z_t is called a time series. Unless specifically mentioned, we assume that the time index t is equally spaced in the book. For a spatial process, C consists of locations of certain geographical regions. The locations may denote the latitude and longitude of an observation station or a city. For each value t in C (e.g. each point in time), z_t is a real-valued random variable defined on a common measurable space. The observed values, also denoted by {z_t} for simplicity, form a realization of the stochastic process. We shall denote the realization by z_1^T = (z_1, ..., z_t, ..., z_T)', where T is the sample size and A' denotes the transpose of the matrix or vector A. The sample size T denotes the number of observations for a time series or the number of locations for a pure spatial process.

Statistical properties of a stochastic process z_t are characterized by its probability distribution. More specifically, for a given T (finite and fixed), properties of z_1^T are determined by the joint probability distribution of its elements {z_t | t = 1, ..., T}. The marginal distribution of any non-empty subset of z_1^T can be obtained from the joint distribution by integrating out elements not in the subset. In particular, the probability distribution of the individual element z_t is called the marginal distribution of z_t. Thus, similar to the general finite-dimensional random variable, we say that the probabilistic structure of z_1^T is known when we know its joint probability distribution.

In this book, we assume that a probability distribution has a well-defined density function. Let f_t(z) be the marginal probability density function of z_t. Define the

expected value or mean of $z_t$ as

$$\mu_t = E(z_t) = \int_R z_t f_t(z) dz$$

provided that the integral exists, where $R$ denotes the real line. The sequence $\mu_1^T = (\mu_1, \mu_2, \dots, \mu_T)$ is the mean function of $\mathbf{z}_1^T$. Define the variance of $z_t$ as

$$\sigma_t^2 = E(z_t - \mu_t)^2 = \int_R (z - \mu_t)^2 f_t(z) dz$$

provided that the integral exists. The sequence $\{\sigma_1^2\}_1^T = (\sigma_1^2, \dots, \sigma_T^2)$ is the variance function of $\mathbf{z}_1^T$. Higher order moments can be defined similarly, assuming that they exist.

A special case of interest is that all elements of $\mathbf{z}_1^T$ have the same mean. In this case, $\mu_1^T$ is a constant function. If all elements of $\mathbf{z}_1^T$ have the same variance, then $\{\sigma_t^2\}_1^T$ is a constant function. If both $\mu_1^T$ and $\{\sigma_t^2\}_1^T$ are constants for all finite $T$, then the stochastic process $z_t$ is stable and we say that it is a homogeneous process. In some applications, $\mu_1^T$ or $\{\sigma_t^2\}_1^T$ may not exist.

In what follows, we assume that $\sigma_t^2$ exists for all $t$. The linear dynamic dependence of a stochastic process $z_t$ is determined by its autocovariance or autocorrelation function (ACF). For $z_t$ and $z_{t-j}$, where $j$ is a given integer, we define their autocovariance as

$$\gamma(t, t-j) = \text{Cov}(z_t, z_{t-j}) = E[(z_t - \mu_t)(z_{t-j} - \mu_{t-j})]. \quad (1.1)$$

When $j=0$, $\gamma(t, t) = \text{Var}(z_t) = \sigma_t^2$. Also, from the definition, it is clear that $\gamma(t, t-j) = \gamma(t-j, t)$. The autocorrelation between $z_t$ and $z_{t-j}$ is defined as

$$\rho(t, t-j) = \frac{\gamma(t, t-j)}{\sigma_t \sigma_{t-j}}. \quad (1.2)$$

Of particular importance in real applications is the case in which the autocovariance and, hence, the autocorrelation between $z_t$ and $z_{t-j}$ is a function of the lag $j$, and not a function of $t$. In this case, the autocovariance and autocorrelation are time-invariant and we write $\gamma(t, t-j) = \gamma_j$ and $\rho(t, t-j) = \rho_j$.

### 1.2.1.1 Stationarity

A stochastic process $z_t$ is strictly stationary if the joint distribution of the $m$-dimensional random vectors $(z_{t_1}, z_{t_2}, \dots, z_{t_m})$ and $(z_{t_1+h}, z_{t_2+h}, \dots, z_{t_m+h})$ are the same, where $m$ is an arbitrary positive integer, $\{t_1, \dots, t_m\}$ are arbitrary $m$ time indexes, and $h$ is an arbitrary integer. In other words, the joint distribution of arbitrary $m$ random variables is time-invariant. In particular, the marginal probability density $f_t(z)$ of a strictly stationary process $z_t$ does not depend on $t$, and we simply denote it by $f(z)$. A stochastic process $z_t$ is weakly stationary if its mean and autocovariance functions exist and are time-invariant. Specifically, a process $z_t$ is stationary in the weak sense if (1) $\mu_t = \mu$ for all $t$; (2) $\sigma_t^2 = \sigma^2$ for all $t$; and (3) $\gamma(t, t-j) = \gamma_j$ for all $t$, where $j$ is a given integer. The first two conditions say that the mean and variance are time-invariant. The third condition states that the autocovariance and, hence, the autocorrelation between two variables depend only on their time separation $j$. Unless specifically stated, we
