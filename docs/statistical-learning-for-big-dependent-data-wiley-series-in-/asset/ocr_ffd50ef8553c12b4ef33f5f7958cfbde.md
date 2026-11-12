of marginal means \(\mu_j = (\mu_{j1}, \dots, \mu_{jT})'\) for \(j = 1, \dots, c\) and covariance matrices that depend on the lag and are denoted by \(\Sigma_j = \{\gamma_j(\ell) : 0 \le \ell \le T - 1\}\), where \(\gamma_j(\ell) = E[(z_t - \mu_{j,t})(z_{t-\ell} - \mu_{j,t-\ell})]\). Suppose that we have prior probabilities \(p(s_i)\) of the system being in the \(i\)th state, where \(\sum_{i=1}^c p(s_i) = 1\). The posterior probability of the system being in state \(s_i\) is, by Bayes’ theorem

```
\(p(s_i|\mathbf{z}) = \frac{p(\mathbf{z}|s_i)p(s_i)}{\sum_{j=1}^c p(\mathbf{z}|s_j)p(s_j)}.\) (5.45)
```

To compute \(p(\mathbf{z}|s_i)\), one typically assumes that the process is Gaussian so that the probability is proportional to the likelihood, and we have

```
\(p(\mathbf{z}|s_j) \propto |\boldsymbol{\Sigma}_j|^{-\frac{1}{2}} \exp \left[ -\frac{1}{2} (\mathbf{z} - \boldsymbol{\mu}_j)' \boldsymbol{\Sigma}_j^{-1} (\mathbf{z} - \boldsymbol{\mu}_j) \right],\) (5.46)
```

and the state is estimated as the one with the highest posterior probability

```
\(\widehat{s}_i = \arg \max_{s_j \in S} [p(\mathbf{z}|s_j)p(s_j)].\)
```

When the prior probabilities are equal, this is equivalent to fitting the series \(\mathbf{z}\) with the \(c\) models and selecting the state that leads to the best fit. As we saw in Chapter 2 (section 2.8.3), by the prediction error decomposition,

```
\(p(\mathbf{z}|s_i) = p(z_1|s_i)p(z_2|z_1, s_i)\dots p(z_T|z_1, \dots, z_{T-1}, s_i).\)
```

Suppose that the time series model in each state is an ARMA \((p_j, q_j)\) model, for \(j = 1, \dots, c\). Calling \(e_{jt} = z_t - \mu_j\), the model for \(e_{jt}\) can be written as \(\pi_j(B)e_{jt} = \epsilon_{jt}\), where \(\pi_j(B) = 1 - \pi_{j1}B - \pi_{j2}B^2 - \cdots\). Thus,

```
\(\left[ \begin{array}{cccccc} 1 & 0 & 0 & \cdots & 0 & \\ -\pi_{j1} & 1 & 0 & \cdots & 0 & \\ -\pi_{j2} & -\pi_{j1} & 1 & \cdots & 0 & \\ \vdots & \vdots & \vdots & \ddots & \vdots & \\ -\pi_{jp} & -\pi_{j,p-1} & \cdots & \cdots & 0 & \\ \vdots & \vdots & \cdots & \cdots & \vdots & \\ -\pi_{j,T-1} & \pi_{j,T-2} & -\pi_{jp} & \cdots & 1 & \end{array} \right] \left[ \begin{array}{c} e_{j1} \\ e_{j2} \\ e_{j3} \\ \vdots \\ \vdots \\ e_{jT} \end{array} \right] = \left[ \begin{array}{c} \epsilon_{j1} \\ \epsilon_{j2} \\ \epsilon_{j3} \\ \vdots \\ \vdots \\ \epsilon_{jT} \end{array} \right].\)
```

or

```
\(\Pi_j e_j = \epsilon_j\)
```

where \(\Pi_j\) is a Toeplitz triangular matrix with the \(j\)th ARMA model parameters. Letting \(\sigma_j^2 I\) be the covariance matrix of the white noise process \(\{\epsilon_{jt}\}\) and calling \(\Sigma_j\) the covariance matrix of \(e_j\), we have,

```
\(\Pi_j \Sigma_j \Pi'_j = \sigma_j^2 I,\)
```

and, therefore,

```
\(\Sigma_j^{-1} = \frac{1}{\sigma_j^2} \Pi' \Pi_j.\)
```