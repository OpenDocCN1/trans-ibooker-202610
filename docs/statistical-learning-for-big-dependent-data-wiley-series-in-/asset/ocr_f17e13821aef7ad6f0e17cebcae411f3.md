+   2a. Define $t_2 = t_{\max}$ of step 1, and obtain new values of $D_L$ and $(i_{\max}, t_{\max})$ of (4.50) and (4.51), respectively. If $D_L > D_{L,\alpha}$, and $t_2 - t_{\max} > H$, then we redefine $t_2 = t_{\max}$ and repeat step 2a until $D_L < D_{L,\alpha}$ or $t_2 - t_{\max} \le H$. Define $t_{\text{first}} = t_2$ where $t_2$ is the last time index that attains the maximum of the cusum statistic that is larger than $D_{L,\alpha}$ and satisfies $t_2 - t_{\max} > H$. The point $t_{\text{first}} + 1$ is the first time point with a possible level shift.

+   2b. Define $t_1 = t_{\max}$ of step 1 and $t_2 = T$, and obtain new values of $D_L$ and $(i_{\max}, t_{\max})$ of (4.50) and (4.51). If $D_L > D_{L,\alpha}$, and $t_{\max} - t_1 > H$, then we redefine $t_1 = t_{\max}$ and repeat step 2b until $D_L < D_{L,\alpha}$ or $t_{\max} - t_1 \le H$. Define $t_{\text{last}} = t_1$ where $t_1$ is the last time index that attains the maximum of the cusum statistics which is larger than $D_{L,\alpha}$ and satisfies $t_{\max} - t_1 > H$. The point $t_{\text{last}} + 1$ is the last time point with a possible level shift.

+   2c. If $t_{\text{last}} - t_{\text{first}} < H$, there is just a level shift and the algorithm stops. If not, keep both values as possible change points and repeat steps 2a and 2b for $t_1 = t_{\text{first}}$ and $t_2 = t_{\text{last}}$ until no more possible change points are detected. Then, go to step 3.

+   3. Define a vector $h^L = (h^L_0, \dots, h^L_{r_L+1})$, where $h^L_0 = 1, h^L_{r_L+1} = T$ and $h^L_1 < \cdots < h^L_{r_L}$ are the change points detected in step 2. Obtain the statistic $D_L$ in each sub-intervals $(h^L_i, h^L_{i+2})$ and check its statistical significance. If it is not significant, eliminate the corresponding possible change point. Repeat step 3 until the number of possible change points remains unchanged and the time indexes of change points are the same between iterations. Removing $h^L_0 = 1$ and $h^L_{r_L+1} = T$ from the final vector of time indexes, we obtain $r_L$ level shifts in the series at time indexes $h^L_i + 1$ for $i = 1, \dots, L$.

+   4. Let $\{h^L_1, \dots, h^L_{r_L}\}$ be the time indexes of $r_L$ detected level shifts. To remove the impacts of level shifts, fit the model:

\((\mathbf{I} - \Pi_1 B - \cdots - \Pi_{\hat{p}} B^{\hat{p}}) z^*_t = \mathbf{a}^*_t, \quad (4.52)\)

where \(z^*_t = z_t - \sum_{i=1}^{r_L} w_i S_t^{(h^L_i)}\), and the order \(\hat{p}\) is chosen such that

\(\hat{p} = \arg \min_{0 \le p \le p_{\max}} AIC(p) = \arg \min_{0 \le p \le p_{\max}} \left\{ \log |\hat{\Sigma}_p| + 2 \frac{k^2 p}{T} \right\},\)

where \(\hat{\Sigma}_p = \frac{1}{T - 2p - 1} \sum_{t=p+1}^T \mathbf{a}^*_t \mathbf{a}^{*\prime}_t\) and \(p_{\max}\) is a prespecified upper bound. If some of the effects of level shifts are not significant, the least significant one is removed from the model in (4.52) and the effects of the remaining \(r_L - 1\) level shifts are re-estimated. This process is repeated until all the level shifts are significant.

Critical values of the statistic $D_L$, which is the maximum of dependent random variables, can be obtained by simulation (see Galeano et al., 2006). Note that the test statistics (4.49) are highly correlated for close observations and consecutive large values of $C_t$ might be caused by a single level shift. To avoid over detection two level shifts should not be closer than the number $H$ in steps 2 and 3; s $H = 10$ seems to works well in practice.