Then the preceding two identities give

$$\frac{\partial}{\partial r}\left\langle\frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial s}(r,s)\right\rangle=\left\langle\frac{d\psi}{ds}(s),\psi(s)\right\rangle,$$

where we observe that the right-hand side is independent of $r$. From the differential calculus of one real variable,

$$\left\langle\frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial s}(r,s)\right\rangle=c+r\left\langle\frac{d\psi}{ds}(s),\psi(s)\right\rangle \tag{19.7.3}$$

for some real number $c$. We have from Theorem 18.9.4 and Theorem 18.11.4(c) that

$$\frac{\partial\sigma}{\partial r}(0,s)=d_0(\exp_p)(\psi(s))=\psi(s),$$

and from Theorem 18.11.4(d) that

$$\frac{\partial\sigma}{\partial s}(0,s)=0.$$

Setting $r=0$ in (19.7.3) and using the preceding two identities yields $c=0$.

Let $(M,\mathfrak g)$ be a semi-Riemannian manifold, and let $p$ be a point in $M$. Let $v$ and $w$ be vectors in $\mathcal E_p$, and recall from (18.9.4) the differential map

$$d_v(\exp_p):T_v(T_p(M))\longrightarrow T_{\exp_p(v)}(M).$$

The next result shows that $d_v(\exp_p)$ can be viewed as a “linear isometry along radial geodesics”.

# **Theorem 19.7.5 (Gauss’s Lemma).** *With the above setup,*

$$\langle v,w\rangle=\left\langle d_v(\exp_p)(v),d_v(\exp_p)(w)\right\rangle.$$

*Proof.* Let $\psi(s):(-\varepsilon,\varepsilon)\longrightarrow T_p(M)$ be a smooth curve such that $\psi(0)=v$ and $(d\psi/ds)(0)=w$, as for example, $\psi(s)=v+sw$. Consider the parametrized surface $\sigma(r,s):[0,1]\times(-\varepsilon,\varepsilon)\longrightarrow M$ defined by $\sigma(r,s)=\exp_p(r\psi(s))$, where we assume that $r\psi(s)$ is in $\mathcal E_p$ for all $(r,s)$ in $[0,1]\times(-\varepsilon,\varepsilon)$. It follows from parts (c) and (d) of Theorem 18.11.4 that

$$\frac{\partial\sigma}{\partial r}(1,0)=d_v(\exp_p)(v)\quad\text{and}\quad\frac{\partial\sigma}{\partial s}(1,0)=d_v(\exp_p)(w),$$

and then from Theorem 19.7.4(b) that

$$\left\langle d_v(\exp_p)(v),d_v(\exp_p)(w)\right\rangle=\left\langle\frac{d\psi}{ds}(0),\psi(0)\right\rangle=\langle w,v\rangle.$$