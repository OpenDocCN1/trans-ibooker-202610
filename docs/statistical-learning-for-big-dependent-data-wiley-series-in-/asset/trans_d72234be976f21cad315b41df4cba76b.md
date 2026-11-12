

## APPENDIX 4.A: CUSUM ALGORITHMS

### 4.A.1 Detecting Univariate LS

Bai (1994) proposed to identify LS by the cusum statistic,

$$ C_t = \frac{t}{\sqrt{T \psi(1)\sigma_e}} \left( \frac{1}{t} \sum_{i=1}^t z_i - \bar{z} \right), \quad (4.49) $$

to test for a level shift at $t = h + 1$ in a linear process, and showed that the statistic converges weakly to a standard Brownian Bridge on $[0, 1]$. In practice, the quantity $\psi(1)\sigma_e$ is replaced by a consistent estimator,

$$ \widehat{\psi(1)}\sigma_e = \left[ \widehat{\gamma(0)} + 2 \sum_{i=1}^K \left( 1 - \frac{|i|}{K} \right) \widehat{\gamma(i)} \right]^{\frac{1}{2}}, $$

where $\widehat{\gamma(i)} = \text{Cov}(y_t, y_{t-i})$ and $K$ is a quantity such that $K \to \infty$ and $K/T \to 0$ as $T \to \infty$. Under the assumption of no level shifts in the sample, the statistic $\max_{1 \le t \le T} |C_t|$ is asymptotically distributed as the supremum of the absolute value of a Brownian Bridge with cumulative distribution function, $F(x) = 1 + 2 \sum_{i=1}^\infty (-1)^i e^{-2i^2 x^2}$, for $x > 0$. The main advantages of the cusum statistic (4.49) over the LRT statistic for detecting level shifts include that (1) the ARMA model is not needed, (2) the cusum statistic has been shown to be more powerful than the LRT in several Monte Carlo studies, and (3) the statistic (4.49) seems to be robust to the presence of other outliers whereas the LRT statistic is not.

### 4.A.2 Detecting Multivariate Level Shift

Galeano et al. (2006) proposed an iterative procedure to identify multivariate level shifts based on the algorithm proposed in Inclán and Tiao (1994) for detecting several variance changes. For a vector of $k$ series compute projected univariate series, $z_{it} = v_i' z_t$, for $i = 1, ..., N$, where $N \ge k$, and let $H$ be the prespecified minimum distance between two level shifts. The proposed algorithm divides the series into pieces after detecting a level shift and proceeds as follows:

- 1. Let $t_1 = 1$ and $t_2 = T$. Obtain

$$ D_L = \max_{1 \le i \le N} \max_{t_1 \le t \le t_2} |C_t^i|, \quad (4.50) $$

where $C_t^i$ is the cusum statistic (4.49) applied to the $i$th projected series for $i = 1, ..., k$. Let

$$ (i_{\max}, t_{\max}) = \arg \max_{1 \le i \le N} \arg \max_{t_1 \le t \le t_2} |C_t^i|. \quad (4.51) $$

If $D_L > D_{L,\alpha}$, where $D_{L,\alpha}$ is the critical value for the significance level $\alpha$, then there is a possible level shift at $t = t_{\max} + 1$, and we go to 2a. If $D_L < D_{L,\alpha}$, then there is no level shift in the series and the algorithm stops.

- 2a. Define $t_2 = t_{\max}$ of step 1, and obtain new values of $D_L$ and $(i_{\max}, t_{\max})$ of (4.50) and (4.51), respectively. If $D_L > D_{L,\alpha}$, and $t_2 - t_{\max} > H$, then we redefine $t_2 = t_{\max}$ and repeat step 2a until $D_L < D_{L,\alpha}$ or $t_2 - t_{\max} \le H$. Define $t_{\text{first}} = t_2$ where $t_2$ is the last time index that attains the maximum of the cusum statistic that is larger than $D_{L,\alpha}$ and satisfies $t_2 - t_{\max} > H$. The point $t_{\text{first}} + 1$ is the first time point with a possible level shift.

- 2b. Define $t_1 = t_{\max}$ of step 1 and $t_2 = T$, and obtain new values of $D_L$ and $(i_{\max}, t_{\max})$ of (4.50) and (4.51). If $D_L > D_{L,\alpha}$, and $t_{\max} - t_1 > H$, then we redefine $t_1 = t_{\max}$ and repeat step 2b until $D_L < D_{L,\alpha}$ or $t_{\max} - t_1 \le H$. Define $t_{\text{last}} = t_1$ where $t_1$ is the last time index that attains the maximum of the cusum statistics which is larger than $D_{L,\alpha}$ and satisfies $t_{\max} - t_1 > H$. The point $t_{\text{last}} + 1$ is the last time point with a possible level shift.

- 2c. If $t_{\text{last}} - t_{\text{first}} < H$, there is just a level shift and the algorithm stops. If not, keep both values as possible change points and repeat steps 2a and 2b for $t_1 = t_{\text{first}}$ and $t_2 = t_{\text{last}}$ until no more possible change points are detected. Then, go to step 3.

- 3. Define a vector $h^L = (h^L_0, \dots, h^L_{r_L+1})$, where $h^L_0 = 1, h^L_{r_L+1} = T$ and $h^L_1 < \cdots < h^L_{r_L}$ are the change points detected in step 2. Obtain the statistic $D_L$ in each sub-intervals $(h^L_i, h^L_{i+2})$ and check its statistical significance. If it is not significant, eliminate the corresponding possible change point. Repeat step 3 until the number of possible change points remains unchanged and the time indexes of change points are the same between iterations. Removing $h^L_0 = 1$ and $h^L_{r_L+1} = T$ from the final vector of time indexes, we obtain $r_L$ level shifts in the series at time indexes $h^L_i + 1$ for $i = 1, \dots, r_L$.

- 4. Let $\{h^L_1, \dots, h^L_{r_L}\}$ be the time indexes of $r_L$ detected level shifts. To remove the impacts of level shifts, fit the model:

\((\mathbf{I} - \Pi_1 B - \cdots - \Pi_{\hat{p}} B^{\hat{p}}) z^*_t = \mathbf{a}^*_t, \quad (4.52)\)

where \(z^*_t = z_t - \sum_{i=1}^{r_L} w_i S_t^{(h^L_i)}\), and the order \(\hat{p}\) is chosen such that

\(\hat{p} = \arg \min_{0 \le p \le p_{\max}} AIC(p) = \arg \min_{0 \le p \le p_{\max}} \left\{ \log |\hat{\Sigma}_p| + 2 \frac{k^2 p}{T} \right\},\)

where \(\hat{\Sigma}_p = \frac{1}{T - 2p - 1} \sum_{t=p+1}^T \mathbf{a}^*_t \mathbf{a}^{*\prime}_t\) and \(p_{\max}\) is a prespecified upper bound. If some of the effects of level shifts are not significant, the least significant one is removed from the model in (4.52) and the effects of the remaining \(r_L - 1\) level shifts are re-estimated. This process is repeated until all the level shifts are significant.

Critical values of the statistic $D_L$, which is the maximum of dependent random variables, can be obtained by simulation (see Galeano et al., 2006). Note that the test statistics (4.49) are highly correlated for close observations and consecutive large values of $C_t$ might be caused by a single level shift. To avoid over detection two level shifts should not be closer than the number $H$ in steps 2 and 3; $H = 10$ seems to work well in practice.

### 4.A.3 Detecting Multiple Covariance Changes

The algorithm is a generalization of Inclán and Tiao (1994) and proceeds as follows:

- 1. Assuming no covariance changes, a vector ARMA model is specified for the observed series \(\boldsymbol{z}_t\). The maximum likelihood estimates of the model are obtained as well as the residual series that we denote by \(\widehat{\boldsymbol{e}}_t\). Let \(t_1 = 1\).

- 2. Obtain \(\Gamma_{\max}(h_{\max}^C)\) in (4.47) for \(t = 1, \dots, T\). If \(\Gamma_{\max}(h_{\max}^C) > C\), where \(C\) is a specified critical value for a given critical level, go to step 3. If \(\Gamma_{\max}(h_{\max}^C) < C$, it is assumed that there is no covariance change in the series and the procedure ends.

- 3. Step 3 contains three substeps:
  - a. Obtain \(\Gamma_{\max}(h_{\max}^C)\) for \(t = 1, \dots, t_2$, where \(t_2 = h_{\max}^C$. If \(\Gamma_{\max}(h_{\max}^C) > C$, redefine \(t_2 = h_{\max}^C$ and repeat step 3(a) until \(\Gamma_{\max}(h_{\max}^C) < C$. When this happens, define \(h_{\text{first}} = t_2\), where \(t_2\) is the last value such that \(\Gamma_{\max}(h_{\max}^C) > C$.
  - b. Repeat a similar search in the interval \(t_2 \le t \le T$, where \(t_2\) is the point \(h_{\max}^C$ obtained in step 2. For that, define \(t_1 = h_{\max}^C + 1$, where \(h_{\max}^C = \arg \max \{ C_t : t = t_1, \dots, T \}$ and repeat it until \(\Gamma_{\max}(h_{\max}^C) < C$. Define \(h_{\text{last}} = t_1 - 1$, where \(t_1\) is the last value such that \(\Gamma_{\max}(h_{\max}^C) > C$.
  - c. If \(|h_{\text{last}} - h_{\text{first}}| < d$, there is just one change point and the algorithm ends. Otherwise, keep both values as possible change points and repeat steps 2 and 3 for \(t_1 = h_{\text{first}}\) and \(T = h_{\text{last}}$, until no more possible change points are detected. Then, go to step 4.

- 4. Define a vector \(\boldsymbol{\ell} = (\ell_1, \dots, \ell_s)$, where \(\ell_1 = 1$, \(\ell_s = T$ and \(\ell_2, \dots, \ell_{s-1}$ are the points detected in steps 2 and 3 in increasing order. Obtain the statistic \(C_t$ in each one of the intervals \((\ell_i, \ell_{i+2})\) and check if its maximum is still significant. If it is not, eliminate the corresponding point. Repeat step 4 until the number of possible change points does not change, and the points found in previous iterations do not differ from those in the last one. The vector \((\ell_2 + 1, \dots, \ell_{s-1} + 1)$ are the points of covariance change.

- 5. Finally, estimate the parameters of the model and the covariance changes detected in the previous steps jointly by using (4.48).

## EXERCISES

- 1. Compute the optimal interpolation for the univariate ARMA process \((1 - 0.6B - 0.3B^2)z_t = 5 + a_t\) at time \(h\) as a function of the observations before and after \(t = h\). How many values are used? How are they weighted?

- 2. Prove that the optimal interpolation of the vector process \((\boldsymbol{I} - \boldsymbol{\Phi}B)\boldsymbol{z}_t = \boldsymbol{a}_t\) at time \(t = h\) is given by \(\widehat{\boldsymbol{z}}_h = (\boldsymbol{I} + \boldsymbol{\Phi}'\boldsymbol{\Phi})^{-1}\boldsymbol{\Phi}(\boldsymbol{z}_{h-1} + \boldsymbol{z}_{h+1})\).

- 3. Use the package **tsoutliers** to detect outliers in the 9th and 10th series of the data set `TaiwanAirBox032017.csv`. Then, use the Lasso approach to detect level shifts and AOs in the same two time series. Compare the detection results.

- 4. Compare the results of Exercise 3 with those obtained via the program **arima.rob**.

- 5. With the three series of world temperature (`Temperatures.csv`) find outliers using the programs **tso**, **arima.rob** and **outlierLasso**.

