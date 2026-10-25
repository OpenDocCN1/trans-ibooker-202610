# 18.6  Torsion Tensors

**Proof.** Let \((U,(x^i))\) be a chart on \(M\) that intersects \(\sigma((a,b)\times(-\epsilon,\epsilon))\). By Theorem 14.9.2,

\[
\frac{\partial\sigma}{\partial r}=\sum_i\frac{\partial(x^i\circ\sigma)}{\partial r}\left(\frac{\partial}{\partial x^i}\circ\sigma\right).
\tag{18.6.2}
\]

and

\[
\frac{\partial\sigma}{\partial s}=\sum_i\frac{\partial(x^i\circ\sigma)}{\partial s}\left(\frac{\partial}{\partial x^i}\circ\sigma\right).
\tag{18.6.3}
\]

We have from Theorem 18.3.1(c) and (18.6.3) that

\[
\nabla_{\partial r}\left(\frac{\partial\sigma}{\partial s}\right)
=\sum_i\nabla_{\partial r}\left(\frac{\partial(x^i\circ\sigma)}{\partial s}\left(\frac{\partial}{\partial x^i}\circ\sigma\right)\right)
\]

\[
=\sum_i\frac{\nabla}{\partial r}\left(\frac{\partial(x^i\circ\sigma)}{\partial s}\right)\left(\frac{\partial}{\partial x^i}\circ\sigma\right)+\sum_i\frac{\partial(x^i\circ\sigma)}{\partial s}\frac{\nabla}{\partial r}\left(\frac{\partial}{\partial x^i}\right)
\]

\[
=\sum_k\frac{\partial^2(x^k\circ\sigma)}{\partial r\partial s}\left(\frac{\partial}{\partial x^k}\circ\sigma\right)+\sum_i\frac{\partial(x^i\circ\sigma)}{\partial s}\nabla_{\partial\sigma/\partial r}\left(\frac{\partial}{\partial x^i}\right).
\]

and from (18.2.1) and (18.6.2) that

\[
\sum_i\frac{\partial(x^i\circ\sigma)}{\partial s}\nabla_{\partial\sigma/\partial r}\left(\frac{\partial}{\partial x^i}\right)
\]

\[
=\sum_i\frac{\partial(x^i\circ\sigma)}{\partial s}\left[\sum_j\frac{\partial(x^j\circ\sigma)}{\partial r}\nabla_{(\partial/\partial x^j)\circ\sigma}\left(\frac{\partial}{\partial x^i}\right)\right]
\]

\[
=\sum_{ij}\left[\frac{\partial(x^j\circ\sigma)}{\partial r}\frac{\partial(x^i\circ\sigma)}{\partial s}\nabla_{(\partial/\partial x^j)\circ\sigma}\left(\frac{\partial}{\partial x^i}\right)\right]
\]

\[
=\sum_{ij}\left\{\frac{\partial(x^j\circ\sigma)}{\partial r}\frac{\partial(x^i\circ\sigma)}{\partial s}\left[\sum_k(\Gamma^k_{ji}\circ\sigma)\left(\frac{\partial}{\partial x^k}\circ\sigma\right)\right]\right\}
\]

\[
=\sum_k\left[\sum_{ij}\frac{\partial(x^j\circ\sigma)}{\partial r}\frac{\partial(x^i\circ\sigma)}{\partial s}(\Gamma^k_{ji}\circ\sigma)\right]\left(\frac{\partial}{\partial x^k}\circ\sigma\right),
\]

hence

\[
\nabla_{\partial r}\left(\frac{\partial\sigma}{\partial s}\right)
=\sum_k\left[\frac{\partial^2(x^k\circ\sigma)}{\partial r\partial s}
+\sum_{ij}\left(\frac{\partial(x^j\circ\sigma)}{\partial r}\frac{\partial(x^i\circ\sigma)}{\partial s}(\Gamma^k_{ji}\circ\sigma)\right)\right]\left(\frac{\partial}{\partial x^k}\circ\sigma\right).
\]