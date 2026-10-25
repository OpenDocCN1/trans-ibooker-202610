Reversing the roles of $r$ and $s$ in the preceding identity and using Theorem 10.1.6 yields

$$\frac{\nabla}{\partial s}\left(\frac{\partial\sigma}{\partial r}\right)=\sum_k\left[\frac{\partial^2(x^k\circ\sigma)}{\partial s\partial r}\right]+\sum_{ij}\left[\frac{\partial(x^j\circ\sigma)}{\partial s}\frac{\partial(x^i\circ\sigma)}{\partial r}(\Gamma^k_{ji}\circ\sigma)\right]\left(\frac{\partial}{\partial x^k}\circ\sigma\right)$$

$$=\sum_k\left[\frac{\partial^2(x^k\circ\sigma)}{\partial r\partial s}\right]+\sum_{ij}\left[\frac{\partial(x^j\circ\sigma)}{\partial r}\frac{\partial(x^i\circ\sigma)}{\partial s}(\Gamma^k_{ij}\circ\sigma)\right]\left(\frac{\partial}{\partial x^k}\circ\sigma\right).$$

Comparing the last two identities and invoking Theorem 18.6.3(b) gives the result.

# 18.7  Curvature Tensors

In this section, we generalize to smooth manifolds with a connection the Riemann curvature tensor on regular surfaces in $\mathbb{R}^3$.

Let $(M,\nabla)$ be a smooth manifold with a connection. The **curvature tensor (field)** is the map

$$R:\mathfrak{X}(M)^3\longrightarrow\mathfrak{X}(M)$$

defined by

$$R(X,Y)Z=\nabla_X(\nabla_Y(Z))-\nabla_Y(\nabla_X(Z))-\nabla_{[X,Y]}(Z) \tag{18.7.1}$$

for all vector fields $X,Y,Z$ in $\mathfrak{X}(M)$. Since $R$ takes values in $\mathfrak{X}(M)$, referring to $R$ as a “tensor field”, although ubiquitous in the literature, presents the same issues of terminology encountered with the torsion tensor in Section 18.6. This concern will be addressed in Section 19.5.

**Theorem 18.7.1.** If $(M,\nabla)$ is a smooth manifold with a connection and $X,Y,Z$ are vector fields in $\mathfrak{X}(M)$, then:

(a) $R(Y,X)Z=-R(X,Y)Z$.

(b) $R$ is $C^\infty(M)$-multilinear.

**Proof.** (a): This follows from Theorem 15.3.2(a).

(b): Using Theorem 15.3.2 and Theorem 18.1.2, it is easily shown that $R$ is additive in each argument. For a function $f$ in $C^\infty(M)$, we have from Theorem