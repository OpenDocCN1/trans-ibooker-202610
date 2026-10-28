Since $R_{ee} \ge 0$, then the following square-root factorization (Cholesky decomposition) always exists as

$$R_{ee}=R_{ee}^{1/2}(R_{ee}^{1/2})',$$

and therefore Eq. (7.45) admits the following unique spectral factorization (Wiener solution):

```
$$\begin{aligned}
S_{yy}(z)={}&[C(zI-A)^{-1}K_p\mid I_{N_y}]
\begin{bmatrix}R_{ee}^{1/2}\\--\\R_{ee}^{1/2}\end{bmatrix}
\left[(R_{ee}^{1/2})'\mid(R_{ee}^{1/2})'\right]
\begin{bmatrix}K_p'(z^{-1}I-A')C'\\--\\I_{N_y}\end{bmatrix}\\
={}&[C(zI-A)^{-1}K_pR_{ee}^{1/2}+R_{ee}^{1/2}]
\left[(R_{ee}^{1/2})'K_p'(z^{-1}I-A')C'+(R_{ee}^{1/2})'\right]
\end{aligned}$$

$$T_e(z) \qquad\qquad\qquad T_e'(z^{-1})$$
```

Thus, using Eq. (7.46) and then Eq. (7.44), we have

```
$$S_{yy}(z)=T_e(z)\times T_e'(z^{-1}) \qquad\text{(Spectral Factors)}$$
```

which shows that the INV model indeed admits a proper spectral factorization. To demonstrate that $T_e(z)$ is the unique, stable, minimum-phase spectral factor, it is necessary to show that $|T_e(z)|$ has all its poles within the unit circle (stable). It has been shown (e.g. [1, 4, 33]) that $T_e(z)$ does satisfy these constraints and therefore is in fact the *Wiener solution* in the stationary case. Next, we must show how the INV model (Kalman filter) relates to the Markov parameters in order to specify a stochastic realization. That is, we must show the relationship of $\Sigma_{\mathrm{INV}}$ to the corresponding set of KSP parameters.

# 7.4.3  Innovations Measurement Covariance

In our usual model development, we start with the Gauss–Markov representation given by the model set $\Sigma_{\mathrm{GM}} := \{A,B,C,D,R_{ww},R_{vv},R_{wv},x(0),\mathcal{P}(0)\}$, and a variety of state-space problems are defined within this framework (e.g. state estimation). The same problems can be defined in terms of this model. Therefore, the model set for the INV model is defined as $\Sigma_{\mathrm{INV}} := \{A,B,C,D,K_p,R_{ee},x(0),\mathcal{P}(0)\}$. The only problem here is that we are usually “not” given $\Sigma_{\mathrm{INV}}$, but $\Sigma_{\mathrm{GM}}$. From the covariance equivalence of these models, that is, both satisfy the measurement covariance relation, $\Lambda_{\mathrm{GM}}=\Lambda_{\mathrm{INV}}$, it is easy to show that if we are given $\Sigma_{\mathrm{GM}}$ and we want to obtain $\Sigma_{\mathrm{INV}}$, then we must develop the equivalence relations between the parameters of the model sets.

This solution is given by the set of KSP equations in terms of the INV model. The KSP equations can be solved iteratively to obtain $(K_p,R_{ee})$ by directly implementing the INV model for the *time-invariant* case [32]. Of course, another