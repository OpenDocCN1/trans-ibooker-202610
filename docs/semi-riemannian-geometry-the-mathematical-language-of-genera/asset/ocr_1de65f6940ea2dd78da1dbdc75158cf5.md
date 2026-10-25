# 19.7 Isometries

where it is assumed that $r\psi(s)$ is in $\mathcal{E}_p$ for all $(r,s)$ in $[0,1]\times(-\varepsilon,\varepsilon)$. Then, for all $(r,s)$ in $[0,1]\times(-\varepsilon,\varepsilon)$:

+   (a)

$$\left\langle \frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial r}(r,s)\right\rangle=\langle\psi(s),\psi(s)\rangle.$$

+   (b)

$$\left\langle\frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial s}(r,s)\right\rangle=r\left\langle\frac{d\psi}{ds}(s),\psi(s)\right\rangle.$$

**Proof.** (a): Recall from Section 14.9 that for a given point $s$ in $(-\varepsilon,\varepsilon)$, the longitude curve $\sigma_s(r):[0,1]\longrightarrow M$ is defined by $\sigma_s(r)=\sigma(r,s)$. By Theorem 18.11.4(a), $\sigma_s$ is a geodesic with initial velocity $\psi(s)$; that is, $(d\sigma_s/dr)(0)=\psi(s)$. It follows from Theorem 19.9.1(a) that the function $\langle d\sigma_s/dr,d\sigma_s/dr\rangle:[0,1]\longrightarrow M$ is constant, hence

$$\left\langle\frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial r}(r,s)\right\rangle=\left\langle\frac{d\sigma_s}{dr}(r),\frac{d\sigma_s}{dr}(r)\right\rangle=\left\langle\frac{d\sigma_s}{dr}(0),\frac{d\sigma_s}{dr}(0)\right\rangle=\langle\psi(s),\psi(s)\rangle.$$

(b): By Theorem 19.3.1,

$$\frac{\partial}{\partial r}\left(\left\langle\frac{\partial\sigma}{\partial r},\frac{\partial\sigma}{\partial s}\right\rangle\right)=\left\langle\nabla_{\frac{\partial}{\partial r}}\left(\frac{\partial\sigma}{\partial r}\right),\frac{\partial\sigma}{\partial s}\right\rangle+\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial r}}\left(\frac{\partial\sigma}{\partial s}\right)\right\rangle.$$

Since $\sigma_s$ is a geodesic,

$$\nabla_{\frac{\partial}{\partial r}}\left(\frac{\partial\sigma}{\partial r}\right)=0,$$

and according to Theorem 18.6.4,

$$\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial r}}\left(\frac{\partial\sigma}{\partial s}\right)\right\rangle=\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial s}}\left(\frac{\partial\sigma}{\partial r}\right)\right\rangle.$$

Combining the preceding three identities gives

$$\frac{\partial}{\partial r}\left(\left\langle\frac{\partial\sigma}{\partial r},\frac{\partial\sigma}{\partial s}\right\rangle\right)=\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial s}}\left(\frac{\partial\sigma}{\partial r}\right)\right\rangle.$$

We have

$$2\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial s}}\left(\frac{\partial\sigma}{\partial r}\right)\right\rangle=\frac{\partial}{\partial s}\left(\left\langle\frac{\partial\sigma}{\partial r},\frac{\partial\sigma}{\partial r}\right\rangle\right)\qquad[\text{Th 19.3.1}]$$

$$=\frac{d}{ds}(\langle\psi,\psi\rangle)\qquad[\text{part (a)}]$$

$$=2\left\langle\frac{d\psi}{ds},\psi\right\rangle.\qquad[\text{Th 19.3.1}]$$