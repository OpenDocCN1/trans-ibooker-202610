# ## Theorem 19.4.2.

With the above setup, F is a $C^\infty(M)$-module isomorphism:

$$\mathfrak{X}(M) \cong \mathfrak{X}^*(M).$$

**Proof.** It is easily shown that F is a $C^\infty(M)$-module homomorphism. We need to show that F is bijective.

**Injectivity.** Since $\mathbf g$ is nondegenerate, $\ker(F)=0$. It follows from Theorem B.5.3 that F is injective.

**Surjectivity.** Let $\omega$ be a covector field in $\mathfrak{X}^*(M)$, let $(U,(x^i))$ be a chart on $M$, and, in local coordinates, let $\omega|_U=\sum_i\alpha_i d(x^i)$. Define a vector field $X|_U$ in $\mathfrak{X}(U)$ by

$$X|_U=\sum_{ij}g^{ij}\alpha_i\frac{\partial}{\partial x^j}.$$

Then

$$(X|_U)^F\left(\frac{\partial}{\partial x^i}\right)=\left\langle X|_U,\frac{\partial}{\partial x^i}\right\rangle=\left\langle\sum_{jk}g^{jk}\alpha_j\frac{\partial}{\partial x^k},\frac{\partial}{\partial x^i}\right\rangle=\sum_{jk}\alpha_jg^{jk}g_{ki}$$

$$=\sum_j\alpha_j\left(\sum_kg^{jk}g_{ki}\right)=\sum_j\delta_i^j\alpha_j=\alpha_i$$

for $i=1,\ldots,m$, hence

$$\omega|_U=\sum_i(X|_U)^F\left(\frac{\partial}{\partial x^i}\right)d(x^i)$$

It follows from the manifold version of Theorem 1.2.1(e) that $(X|_U)^F=\omega|_U$.

Let $(\widetilde U,\widetilde\varphi)$ be a chart on $M$ that overlaps with $(U,\varphi)$, and, in local coordinates, let $\omega|_{\widetilde U}=\sum_i\widetilde\alpha_i d(\widetilde x^i)$. We have

$$\frac{\partial}{\partial\widetilde x^i}=\sum_q\frac{\partial x^q}{\partial\widetilde x^i}\frac{\partial}{\partial x^q},\qquad \widetilde\alpha_j=\sum_p\frac{\partial x^p}{\partial\widetilde x^j}\alpha_p,\qquad \widetilde g^{ij}=\sum_{kl}\frac{\partial\widetilde x^i}{\partial x^k}g^{kl}\frac{\partial\widetilde x^j}{\partial x^l},$$

where the first identity follows from Theorem 15.1.8, the second identity from Theorem 15.6.3, and the third identity from the manifold version of Theorem 2.2.5(b), Theorem 14.3.5(b), and Theorem 19.1.3. Then

$$X|_{\widetilde U}=\sum_{ij}\widetilde g^{ij}\widetilde\alpha_i\frac{\partial}{\partial\widetilde x^j}=\sum_{ijkl p q}\left(\frac{\partial\widetilde x^i}{\partial x^k}g^{kl}\frac{\partial\widetilde x^j}{\partial x^l}\right)\left(\frac{\partial\widetilde x^j}{\partial x^p}\alpha_p\right)\left(\frac{\partial x^q}{\partial\widetilde x^i}\frac{\partial}{\partial x^q}\right)$$

$$=\sum_{klp q}\left[g^{kl}\alpha_p\left(\sum_i\frac{\partial\widetilde x^i}{\partial x^k}\frac{\partial x^q}{\partial\widetilde x^i}\right)\left(\sum_j\frac{\partial\widetilde x^j}{\partial x^l}\frac{\partial x^p}{\partial\widetilde x^j}\right)\frac{\partial}{\partial x^q}\right]$$

$$=\sum_{klpq}\delta_k^q\delta_l^p g^{kl}\alpha_p\frac{\partial}{\partial x^q}$$

$$=\sum_{kl}g^{kl}\alpha_k\frac{\partial}{\partial x^l}=X|_U,$$