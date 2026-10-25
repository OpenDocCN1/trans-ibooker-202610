Let $(M, \nabla)$ be a smooth manifold with a connection, and let

$\sigma(r,s): (a,b) \times (-\varepsilon,\varepsilon) \longrightarrow M$

be a parametrized surface. Recall from Section 14.9 that for a given point $s$ in $(-\varepsilon,\varepsilon)$, the corresponding longitude curve $\sigma_s(r): (a,b) \longrightarrow M$ is defined by $\sigma_s(r) = \sigma(r,s)$. Let $\gamma(r): (a,b) \longrightarrow M$ be a geodesic. We say that $\sigma$ is a **geodesic variation** of $\gamma$ if $\sigma_0 = \gamma$ and each $\sigma_s$ is a geodesic.

Suppose $\sigma$ is in fact a geodesic variation of $\gamma$. Then

$\frac{d\gamma}{dr}(r) = \frac{\partial \sigma}{\partial r}(r,0)$

(18.11.2)

for all $r$ in $(a,b)$. The **variation field** of $\sigma$ is the vector field $J$ in $\mathfrak{X}_M(\gamma)$ defined by

$J(r) = \frac{\partial \sigma}{\partial s}(r,0)$

(18.11.3)

for all $r$ in $(a,b)$. Since $\sigma_s$ is a geodesic and $\partial \sigma / \partial r$ is its velocity, we have

$\frac{\nabla}{\partial r}\left(\frac{\partial \sigma}{\partial r}\right)(r,s) = 0$

(18.11.4)

for all $(r,s)$ in $(a,b) \times (-\varepsilon,\varepsilon)$. See Figure 18.11.1.

![](bbox=[0.22, 0.55, 0.8, 0.85])

**Figure 18.11.1. Geodesic variation**

# **Example 18.11.2 ($\mathcal{S}^2$).** In the notation of Section 13.4, consider the parametrized surface

$\varphi(\theta,\phi): (0,\pi) \times (-\pi,\pi) \longrightarrow \mathcal{S}^2$