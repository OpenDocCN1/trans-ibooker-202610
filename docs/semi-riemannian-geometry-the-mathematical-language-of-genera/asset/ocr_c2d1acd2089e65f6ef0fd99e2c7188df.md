Since $X^\mathsf{F}|_{\partial M}$ is a covector field in $\mathfrak{X}^*(\partial M)=\Lambda^1(\partial M)$, it follows from Theorem 3.3.3, Theorem 15.8.5, and Theorem 19.10.2 that there is a uniquely determined function $f$ in $C^\infty(U)$ such that

$$X^\mathsf{F}|_{\partial M}=f\,\Omega_{\partial M}.\tag{21.2.4}$$

We have

$$\begin{aligned}f&=f\,\Omega_{\partial M}(U) &&[(21.2.3)]\\&=X^\mathsf{F}|_{\partial M}(U) &&[(21.2.4)]\\&=\langle X|_{\partial M},U\rangle. &&[(19.4.1)]\end{aligned}\tag{21.2.5}$$

Then (21.2.4) and (21.2.5) give

$$X^\mathsf{F}|_{\partial M}=\langle X|_{\partial M},U\rangle\,\Omega_{\partial M}.\tag{21.2.6}$$

Substituting (21.2.2) and (21.2.6) into (21.2.1) gives the result.

# **Theorem 21.2.5 (Classical Divergence Theorem).** *Let $(M,\mathfrak{e}|_M,\mathfrak{O}_M)$ be a compact oriented Riemannian 3-submanifold with boundary of $\mathbb{R}^3_0=(\mathbb{R}^3,\mathfrak{e})$, so that $(\partial M,\mathfrak{e}|_{\partial M})$ is a Riemannian hypersurface of $(M,\mathfrak{e}|_M,\mathfrak{O}_M)$, and let $X$ be a vector field in $\mathfrak{X}(M)$. Let $V$ be the unique outward-pointing unit normal vector field in $\mathfrak{X}_M(\partial M)$ (as given by Theorem 19.11.3), and let $\partial M$ have the orientation $\mathfrak{O}_{\partial M}$ induced by $V$, so that $(\partial M,\mathfrak{e}|_{\partial M},\mathfrak{O}_{\partial M})$ is an oriented Riemannian hypersurface of $(M,\mathfrak{e}|_M,\mathfrak{O}_M)$. Let $\Omega_{\partial M}$ be its volume form. Then*

$$\int_M \operatorname{Div}(X)\,\Omega_M = \int_{\partial M} \langle X,V\rangle\,\Omega_{\partial M}.$$

*Proof.* We have

$$\begin{aligned}\int_M \operatorname{Div}(X)\,\Omega_M &= \int_{\partial M} i_X(\Omega_M) &&[\text{Th 20.4.5}]\\&= \int_{\partial M} \langle X,V\rangle\,\Omega_{\partial M}. &&[\text{Th 19.11.2(b)}]\end{aligned}$$