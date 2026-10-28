# ### 8.5.2 Model-Based Identification: Linear Chirp Signals

Thus, the model-based linear *chirp estimation* problem can simply be stated as

**GIVEN** a set of noisy measurement data \(\{y(t_k)\}\), \(k=0,1,\ldots,K\) contaminated with broadcast disturbances \(\{d(t_k)\}\), extraneous audio signals \(\{e(t_k)\}\) and Gaussian noise \(\{v(t_k)\}\), **FIND** the best estimate of the chirp signal \(\hat{s}(t_k)\) or equivalently the parameters \(\{\hat{\alpha},\hat{f}_o,\hat{f}_f\}\).

The estimated chirp is obtained directly from its parameter estimates as

$$\hat{s}(t_k)=\hat{\alpha}\cos\left(\hat{f}_o\cdot t+2\pi\left(\frac{\hat{\beta}}{2}\cdot t^2\right)\right)$$  (8.141)

for

$$\hat{\beta}=\frac{\hat{f}_f-\hat{f}_o}{t_f}$$  (8.142)

Thus, the solution to estimating the linear chirp in the midst of these disturbances reduces to a problem of MBID.

# ### 8.5.2.1 Gauss–Markov State-Space Model: Linear Chirp

We choose to apply the UKF of Section 4.3 as a MBID-technique to solve this nonlinear problem by modeling the parameter variations as *random walks*, that is, in state-space form we have that

$$\mathbf{f}(t_k)=\mathbf{f}(t_{k-1})+\mathbf{w}_f(t_{k-1})$$
$$\alpha(t_k)=\alpha(t_{k-1})+w_\alpha(t_{k-1})$$  (8.143)

where \(\mathbf{f}\in\mathbb{R}^{N_f\times1}\) is the vector of frequency parameters, \(\alpha\) is the scalar amplitude and \(\mathbf{w}\in\mathbb{R}^{N_w\times1}\) a zero-mean, multivariate Gaussian vector process with covariance \(\mathbf{R}_{ww}(t_k)\). Here \(\mathbf{f}=[f_o\ f_f]^T\) and \(\mathbf{w}_f=[w_o\ w_f]^T\). The corresponding measurement model is given in Eq. (8.141).

Generally, the UKF algorithm provides the updated signal (parameter) estimates as²

$$\hat{\mathbf{f}}(t_k|t_k)=\hat{\mathbf{f}}(t_k|t_{k-1})+\mathbf{K}_f(t_k)e(t_k|t_{k-1})$$
$$\hat{\alpha}(t_k|t_k)=\hat{\alpha}(t_k|t_{k-1})+\mathbf{K}_\alpha(t_k)e(t_k|t_{k-1})$$
$$e(t_k|t_{k-1})=y(t_k)-\hat{y}(t_k|t_{k-1})-d(t_k)-e(t_k)$$
$$\hat{y}(t_k|t_{k-1})=\hat{s}(t_k|t_k)=\hat{\alpha}(t_k|t_{k-1})$$
$$\qquad\times\cos\left(2\pi\left(\hat{f}_o(t_k|t_{k-1})\cdot t_k+\frac{\hat{\beta}(t_k|t_{k-1})}{2}\cdot t_k^2\right)\right)$$
$$\mathbf{K}(t_k)=\mathbf{R}_{fe}(t_k|t_{k-1})\mathbf{R}_{ee}^{-1}(t_k|t_{k-1})$$  (8.144)

² The notation \(\hat{\theta}(t_k|t_{k-1})\) means the estimate of \(\theta\) at time \(t_k\) based on all the data up to time \(t_{k-1}\), \(Y_{k-1}=\{y(t_j)\};\ k=0,\ldots,K\).