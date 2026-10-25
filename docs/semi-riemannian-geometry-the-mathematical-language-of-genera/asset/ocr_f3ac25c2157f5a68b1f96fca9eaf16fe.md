# ### Theorem 10.3.2.

Let $U$ be an open set in $\mathbb{R}^m$, let $f$ be a function in $C^\infty(U)$, and let $X,Y$ be vector fields in $\mathfrak{X}(U)$, with
\[
X=(\alpha^1,\ldots,\alpha^m) \qquad\text{and}\qquad Y=(\beta^1,\ldots,\beta^m).
\]
Then:

+   (a)

$$D_X(f)=\sum_i \alpha^i\frac{\partial f}{\partial x^i}.$$

+   (b)

$$D_X(D_Y(f))=\sum_{ij}\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial f}{\partial x^j}+\alpha^i\beta^j\frac{\partial^2f}{\partial x^i\partial x^j}\right).$$

**Proof.** (a): For a point $p$ in $U$, we have
\[
D_X(f)(p)=d_p(f)(X_p)
=d_p(f)(\alpha^1(p),\ldots,\alpha^m(p))
=\sum_i\alpha^i(p)\frac{\partial f}{\partial x^i}(p).
\]
Since $p$ was arbitrary, the result follows.

(b): By part (a),
\[
D_X(D_Y(f))=D_X\left(\sum_j\beta^j\frac{\partial f}{\partial x^j}\right)=\sum_i\alpha^i\frac{\partial}{\partial x^i}\left(\sum_j\beta^j\frac{\partial f}{\partial x^j}\right)
\]
\[
=\sum_{ij}\alpha^i\frac{\partial}{\partial x^i}\left(\beta^j\frac{\partial f}{\partial x^j}\right)=\sum_{ij}\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial f}{\partial x^j}+\alpha^i\beta^j\frac{\partial^2f}{\partial x^i\partial x^j}\right).\quad\square
\]

# ### Theorem 10.3.3.

Let $U$ be an open set in $\mathbb{R}^m$, and let $X,Y,Z$ be vector fields in $\mathfrak{X}(U)$, with
\[
X=(\alpha^1,\ldots,\alpha^m),\qquad Y=(\beta^1,\ldots,\beta^m),\qquad Z=(\gamma^1,\ldots,\gamma^m).
\]
Then:

+   (a)

$$D_X(Y)=\sum_j\left(\sum_i\alpha^i\frac{\partial\beta^j}{\partial x^i}\right)E_j.$$

+   (b)

$$D_X(D_Y(Z))=\sum_k\left[\sum_{ij}\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}+\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}\right)\right]E_k.$$

+   (c)

$$D_{D_X(Y)}(Z)=\sum_k\left(\sum_{ij}\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}\right)E_k.$$