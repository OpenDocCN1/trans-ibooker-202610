# **Theorem 16.4.4 (Global Expression for Lie Derivative).**

Let $M$ be a smooth manifold, and let $X$ be a vector field in $\mathfrak{X}(M)$. Let $\mathcal{A}$ be a tensor field in $T^r_s(M)$, let $\omega^1,\ldots,\omega^r$ be covector fields in $\mathfrak{X}^*(M)$, and let $Y_1,\ldots,Y_s$ be vector fields in $\mathfrak{X}(M)$. Then

$$\mathcal{L}_X(\mathcal{A}(\omega^1,\ldots,\omega^r,Y_1,\ldots,Y_s))$$
$$=X(\mathcal{A}(\omega^1,\ldots,\omega^r,Y_1,\ldots,Y_s))$$
$$-\sum_{i=1}^{r}\mathcal{A}(\omega^1,\ldots,\omega^{i-1},\mathcal{L}_X(\omega^i),\omega^{i+1},\ldots,\omega^r,Y_1,\ldots,Y_s)$$
$$-\sum_{j=1}^{s}\mathcal{A}(\omega^1,\ldots,\omega^r,Y_1,\ldots,Y_{j-1},[X,Y_j],Y_{j+1},\ldots,Y_s).$$

**Remark.** We have from (16.4.1) that $X(\mathcal{A}(\omega^1,\ldots,\omega^r,Y_1,\ldots,Y_s))$ in the above identity can be replaced with $\mathcal{L}_X(\mathcal{A}(\omega^1,\ldots,\omega^r,Y_1,\ldots,Y_s))$.

**Proof.** Since $\mathcal{L}_X(Y_j)=[X,Y_j]$, the result follows from Theorem 16.2.4.

# **Example 16.4.5.**

For a covector field $\omega$ in $\mathfrak{X}^*(M)=T^0_1(M)$ and vector fields $X,Y$ in $\mathfrak{X}(M)$, setting $r=0$ and $s=1$ in Theorem 16.4.4 gives

$$\mathcal{L}_X(\omega)(Y)=X(\omega(Y))-\omega([X,Y]).$$

# **Theorem 16.4.6 (Commutativity of Lie Derivative and Exterior Derivative of Functions).**

If $M$ is a smooth manifold and $X$ is a vector field in $\mathfrak{X}(M)$, then

$$\mathcal{L}_X\circ d=d\circ\mathcal{L}_X;$$

that is,

$$\mathcal{L}_X(d(f))=d(\mathcal{L}_X(f))$$

for all functions $f$ in $C^\infty(M)$.

**Proof.** For a vector field $Y$ in $\mathfrak{X}(M)$, we have

$$\mathcal{L}_X(d(f))(Y)=X(d(f)(Y))-d(f)([X,Y]) \qquad [\mathrm{Ex\ 16.4.5}]$$
$$=X(Y(f)-[X,Y](f) \qquad [\mathrm{Th\ 15.4.5(b)}]$$
$$=Y(X(f)) \qquad [(15.3.1)]$$
$$=d(X(f))(Y) \qquad [\mathrm{Th\ 15.4.5(b)}]$$
$$=d(\mathcal{L}_X(f))(Y). \qquad [(16.4.1)]$$

Since $f$ and $Y$ were arbitrary, the result follows.

# **Theorem 16.4.7.**

Let $M$ be a smooth $m$-manifold, let $X$ be a vector field in $\mathfrak{X}(M)$, and, in local coordinates, let

$$X=\sum_i \alpha^i\frac{\partial}{\partial x^i}.$$