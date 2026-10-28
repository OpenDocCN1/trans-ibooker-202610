We must also partition the state and measurement predictions as equations, that is,

$$\hat{X}(t|t-1)=\begin{bmatrix}\hat{x}(t|t-1)\\---\\\hat{\theta}(t|t-1)\end{bmatrix}$$

$$=\begin{bmatrix}\mathbf{a}[\hat{x}(t-1|t-1),\hat{\theta}(t-1|t-1)]\\+\mathbf{b}[\hat{x}(t-1|t-1),\hat{\theta}(t-1|t-1),u(t-1)]\\---\\\hat{\theta}(t-1|t-1)\end{bmatrix} \tag{5.16}$$

where the corresponding predicted measurement equation becomes

$$\hat{y}(t|t-1)=\mathbf{c}[\hat{x}(t|t-1),\hat{\theta}(t|t-1)] \tag{5.17}$$

Here we have “implicitly” assumed that the parameters can be considered *piecewise constant*, $\Theta(t)=\Theta(t-1)$ or follow a random walk if we add process noise to this representation in the Gauss–Markov sense. However, if we *do* have process dynamics with linear or nonlinear models characterizing the parameters, then they will replace the random walk and the associated Jacobian.

Next, we consider the predicted error covariance

$$\tilde{P}(t|t-1)=A[\hat{x}(t|t-1),\hat{\theta}(t|t-1)]\tilde{P}(t|t-1)A'[\hat{x}(t|t-1),\hat{\theta}(t|t-1)] + R_{ww}(t-1) \tag{5.18}$$

where $R_{ww}$ is a block diagonal matrix consisting of the state and parameter covariance matrices: $R_{ww}=\operatorname{diag}[R_{w_xw_x}\ R_{w_\theta w_\theta}]$. The error covariance can be written in partitioned form using Eq. (5.14) and the following Jacobian process matrix:

$$A[\hat{x},\hat{\theta}]=\begin{bmatrix}A_x[\hat{x}(t|t-1),\hat{\theta}(t|t-1)] & A_\theta[\hat{x}(t|t-1),\hat{\theta}(t|t-1)]\\--- & ---\\O & I_{N_\theta}\end{bmatrix} \tag{5.19}$$

where

$$A_x[\hat{x},\hat{\theta}] := \frac{\partial a[\hat{x},\hat{\theta}]}{\partial x}+\frac{\partial b[\hat{x},\hat{\theta}]}{\partial x} \tag{5.20}$$

$$A_\theta[\hat{x},\hat{\theta}] := \frac{\partial a[\hat{x},\hat{\theta}]}{\partial \theta}+\frac{\partial b[\hat{x},\hat{\theta}]}{\partial \theta}$$

with $A\in R^{(N_x+N_\theta)\times(N_x+N_\theta)}$, $A_x\in R^{N_x\times N_x}$, $A_\theta\in R^{N_x\times N_\theta}$.

Here is where the underlying random walk model enters the structure. The lower block rows of $A_x$ could be replaced by $[A_{\theta x}[\hat{x},\hat{\theta}]\ A_{\theta\theta}[\hat{x},\hat{\theta}]]$, which enables a linear or nonlinear dynamic model to be embedded directly.

Using these partitions, we can develop the parametrically adaptive processor directly from the EKF processor in this joint state and “parameter estimation”