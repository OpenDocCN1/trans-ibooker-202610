

投影 $\mathcal{T}_k$ 的块 Toeplitz 矩阵为

$\mathcal{T}_k = E\{\mathcal{Y}_p\mathcal{Y}_p'\} = \begin{bmatrix} \Lambda_0 & \Lambda_{-1} & \cdots & \Lambda_{1-k} \\ \Lambda_1 & \Lambda_0 & \cdots & \Lambda_{2-k} \\ \vdots & \vdots & \ddots & \vdots \\ \Lambda_{k-1} & \Lambda_{k-2} & \cdots & \Lambda_0 \end{bmatrix} \in \mathcal{R}^{kN_y \times kN_y}$ (D.5)

利用创新模型，已证明增广状态向量 $\hat{\mathcal{X}}_k := [\hat{x}_k \quad \hat{x}_{k+1} \cdots \hat{x}_{k+k-1}] \in \mathcal{R}^{N_y \times kN_x}$ 满足以下关系 [4, 5] $\hat{\mathcal{X}}_k = \overline{\mathcal{C}}_{AB}^{-}(k) \times \mathcal{T}_k^{-1} \times \mathcal{Y}_p$ (D.6)

因此，将此表达式代入正交投影中，我们得到 $\mathcal{P}_{\mathcal{Y}_i|\mathcal{Y}_p} = \mathbf{T}_k \times \mathcal{T}_k^{-1} \times \mathcal{Y}_p = \mathcal{O}_k \underbrace{\overline{\mathcal{C}}_{AB}^{-}(k) \times \mathcal{T}_k^{-1} \times \mathcal{Y}_p}_{\hat{\mathcal{X}}_k} = \mathcal{O}_k \times \hat{\mathcal{X}}_k$ (D.7)

因此，通过对正交投影矩阵进行奇异值分解，可以提取扩展可观测矩阵和估计状态向量得到 $\mathcal{P}_{\mathcal{Y}_i|\mathcal{Y}_p} = [U_{N_x} \mid U_{\mathcal{N}}] \begin{bmatrix} \Sigma_{N_x} & 0 \\ \hline 0 & \Sigma_{\mathcal{N}} \end{bmatrix} \begin{bmatrix} V'_{N_x} \\ \hline V'_{\mathcal{N}} \end{bmatrix} = U_{N_x} \Sigma_{N_x} V'_{N_x}$ 其中 $\Sigma_{N_x} \gg \Sigma_{\mathcal{N}}$ 且 $\mathcal{P}_{\mathcal{Y}_i|\mathcal{Y}_p} = \mathcal{O}_k \hat{\mathcal{X}}_k = (U_{N_x} \Sigma_{N_x}^{1/2})((\Sigma'_{N_x})^{1/2} V'_{N_x})$ (D.8)

有了奇异值分解，我们利用投影来提取系统和输出测量矩阵，如第 6.2 节所述 $\hat{A} = \mathcal{O}_{N_x}^{\#} \mathcal{O}_{N_x}^{\dagger} = (\Sigma_{N_x}^{-1/2} U'_{N_x} \times \mathcal{O}_{N_x}^{\dagger}); \quad \hat{C} = \mathcal{O}(1 : N_y, 1 : N_x)$ (D.9)

输入传输矩阵 $(\bar{B}_e)$ 可以通过先由式 (D.4) 估计反向可控性矩阵来提取 $\overline{\mathcal{C}}_{AB}^{-}(k) = \mathcal{O}_{N_x}^{\#} \times \mathbf{T}_{k,k} = (\Sigma_{N_x}^{-1/2} U'_{N_x}) \times \mathbf{T}_{k,k}$ (D.10) 并提取最后的 $k$ 列得到 $\hat{B} = \bar{B}_e = \overline{\mathcal{C}}_{AB}^{-}(1 : N_x, (k-1)N_y : kN_y)$ (D.11)

- 5. Katayama, T. (2005). *Subspace Methods for System Identification*. London: Springer.
- 6. Reynders, E. (2012). System identification methods for (operational) modal analysis: review and comparison. *Arch. Comput. Methods Eng.* 19 (1): 51–124.
- 7. Juang, J. (1994). *Applied System Identification*. Upper Saddle River, NJ: Prentice-Hall PTR.

## Index

### a

- a posteriori density 230
- a posteriori particle 192
- a posteriori probability 114, 115
- accelerometer 297, 372
- adaptive innovations processor 214
- adaptive processor 214, 228
- Akaike information criterion 50, 355, 363
- all-pole 67, 95
- all-pole model 48
- all-pole spectral density 49
- all-zero 67
- AM modulator 182
- annealing 192
- antidiagonal 237
- antidiagonal identity matrix 358
- approximate Gauss–Markov process model 106
- approximate Gaussian 149
- approximate Kalman filtering 151
- AR 48
- AR model 49
- area-under-curve 433
- ARMAX 91
- ARMAX model 90, 92, 94
- array 439
- array measurements 222
- array theory 224
- artificial dynamics 191
- augmented data matrix 353
- augmented instrumental variable 336
- augmented state 441
- augmented state equation 442
- augmented state vector 198, 224, 444
- augmented state-space 442
- autocorrelation function 55
- autocorrelation matrix 59
- autocovariance 39
- Autoregressive 43
- autoregressive method 60
- Autoregressive model with exogenous input 44
- autoregressive moving average 44, 51
- autoregressive-moving average model with exogenous inputs 43
- average information 358, 473
- average log-likelihood 359, 360
- average mutual information 474
- average periodogram method 47, 60
- average spectrum 47

### b

- backsubstitution 350
- backward shift operator 43, 49
- balanced deterministic realization 282
- balanced realization 239–241, 275, 305
- balanced stochastic realization 338, 350
- batch 205
- Bayes’ approach 116
- Bayes’ rule 188, 204, 206
- Bayes’ theorem 444
- Bayesian approach 114, 204
- Bayesian decomposition 188, 204
- Bayesian estimation 155
- Bayesian framework 160
- Bayesian information criterion 355
- Bayesian parameter estimator 428
- Bayesian processors 163
- Bayesian recursions 188
- beam 372
- bearing estimate 172, 224
- bearing measurements 170
- bearings-only tracking problem 186
- best rank approximation 289, 357
- bias 360
- biased 360
- binary hypothesis test 230, 231
- black-box 91
- Blackman–Tukey method 45
- block (Hankel) matrices 278
- block Hankel data matrices 282, 344
- block innovations data matrix 333
- block input data matrix 267, 333
- block output data matrix 267, 333
- bootstrap 160
- bootstrap algorithm 158, 432
- bootstrap approach 206
- bootstrap particle filter 446
- bootstrap PF 157, 160, 224
- bootstrap PF algorithm 183
- bootstrap SIR algorithm 159
- Burg’s formula 54

### c

- canonical form 83, 86, 90, 91, 243
- canonical realization 243, 327
- Canonical realization theory 289, 327
- Cartesian coordinates 165
- Cartesian tracking model 182
- Cayley–Hamilton theorem 81, 236
- central difference 229, 439
- chain rule 211
- characteristic polynomial 81
- chi-squared distributed 121
- chirp 455
- chirp signal 455
- Cholesky decomposition 246, 325, 398
- classical approach 100, 234
- classical EKF 224
- classical realization theory 266, 312
- classical statistics 162
- coin tossing 470
- coloring filter 105
- column space 268, 269, 481
- completely controllable 80, 82, 238
- completely observable 78, 82, 238
- complex harmonic model 55
- complex harmonic signal 55
- complex modal case 395
- conditional distribution 475
- conditional expectation 35, 103, 113, 474
- conditional independence 155, 444
- conditional mean 134, 136, 150, 153, 445, 448, 449
- confidence interval 120, 149, 365, 366
- confidence interval estimate 120
- confidence limit 133, 365
- constant velocity model 170
- continuous analog domain 73
- continuous distributions 152
- continuous–discrete 105
- continuous-time 70, 73, 74, 101
- continuous-time process 73
- continuous-time system 69, 73
- controllability 80, 82
- controllability matrices 234, 345
- controllability matrix 279
- convolution relationship 30
- convolution response 291, 364
- coordinate systems 175
- corrected covariance 196
- corrected error covariance 114

### e

- eigenvalues 476
- eigenvectors 59, 476
- EKF 133, 175
- elementary matrices 307
- elementary transformation 243
- empirical 152
- empirical approximation 204
- empirical distribution 205
- enhancement 448
- ensemble 134, 136, 150, 165, 171, 207, 448, 449, 472
- ensemble average 36, 403, 449
- ensemble estimates 141, 207
- ensemble model error 292
- ensemble residual 151
- ensemble statistics 150, 151, 162, 375, 435, 365
- entropy 358, 473, 474
- entropy function 361
- environmental adaptivity 437
- environmentally adaptive 447
- environmentally adaptive processor 452
- ergodic 119
- ergodic process 36, 64
- ergodicity 120
- error covariance 108, 114, 132
- error covariance matrix 198
- estimated distributions 163
- estimated posterior distribution 162
- estimated response 363
- estimated state 199, 203
- estimated state sequences 351
- estimation 361
- estimation error 113, 132
- estimation procedure 7
- estimator 4
- exchange matrix 237
- expectation operator 334
- expected value 473
- expected value operation 29, 33
- exponential window 191
- extended Kalman filter 107, 192
- extended observability matrix 266, 336, 351, 493
- extended states 336
- external 81

### f

- factored power spectrum 104
- factorization techniques 137
- fast Fourier transform 44, 60
- field 469
- filtered estimate 108, 112
- filtered measurement 135, 141, 200
- filtering distribution 205
- filtering posterior 153, 204
- filtering posterior distribution 153
- final prediction error 50, 363
- Finite impulse response 43
- first differences 190, 217, 229
- first-difference 180
- first-difference approximation 75
- fission detection 426
- fission physics 432
- fitting error 291
- forgetting factor 191
- forward propagator 454
- Fourier spectrum 29
- free parameters 360
- frequency modulation 231
- frequency tracking 401
- frequency vector 57
- frequency-shift key 455, 457

### g

- gain 118, 132, 136, 139, 147, 196
- gain matrix 109, 113, 118
- gap 289, 357
- Gauss–Markov 440
- Gauss–Markov equations 84
- Gauss–Markov Measurement Covariance 316

