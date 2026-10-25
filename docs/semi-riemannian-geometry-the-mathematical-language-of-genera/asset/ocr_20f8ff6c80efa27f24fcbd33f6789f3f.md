defined by

\[
\mathcal{A}(\omega^1,\ldots,\omega^r,X_1,\ldots,X_s)(p)=\mathcal{A}_p(\omega^1|_p,\ldots,\omega^r|_p,X_1|_p,\ldots,X_s|_p)
\tag{15.6.1}
\]

for all $p$ in $M$. We say that $\mathcal{A}$ is **smooth (on $M$)** if the function $\mathcal{A}(\omega^1,\ldots,\omega^r,X_1,\ldots,X_s)$ is in $C^\infty(M)$ for all covector fields $\omega^1,\ldots,\omega^r$ in $\mathfrak{X}^*(M)$ and all vector fields $X_1,\ldots,X_s$ in $\mathfrak{X}(M)$. The set of smooth $(r,s)$-tensor fields on $M$ is denoted by $\mathcal{T}^r_s(M)$. In particular,

\[
\mathcal{T}^0_1(M)=\mathfrak{X}^*(M) \qquad\text{and}\qquad \mathcal{T}^1_0(M)=\mathfrak{X}(M).
\tag{15.6.2}
\]

For completeness, we define

\[
\mathcal{T}^0_0(M)=C^\infty(M).
\tag{15.6.3}
\]

It is instructive to compare identities (15.6.2) and (15.6.3) to identities (5.1.1) and (5.1.2). From now on, we avoid the following trivial case.

> **Throughout, unless stated otherwise, $(r,s)\ne(0,0)$.**

Defining operations on $\mathcal{T}^r_s(M)$ in a manner analogous to that described for vector fields and covector fields, we make $\mathcal{T}^r_s(M)$ into both a vector space over $\mathbb{R}$ and a module over $C^\infty(M)$.

Many of the definitions presented for smooth manifolds are expressed in a pointwise fashion (not to be confused with “determined pointwise”) and ultimately rest on earlier definitions given in the context of vector spaces. For example, a tensor field on a smooth manifold is essentially a collection of tensors, one for each point in the smooth manifold. An important consequence of the pointwise approach is that earlier theorems presented for vector spaces generalize immediately to smooth manifolds. We will say that the resulting smooth manifold theorem is the **manifold version** (abbreviated **mv**) of the earlier vector space theorem. Here is an example.

# **Theorem 15.6.1.**

*Let $M$ be a smooth manifold, let $\mathcal{A},\mathcal{A}_1,\mathcal{A}_2$ and $\mathcal{B},\mathcal{B}_1,\mathcal{B}_2$ and $\mathcal{C}$ be tensor fields in $\mathcal{T}^r_s(M)$ and $\mathcal{T}^{r'}_{s'}(M)$ and $\mathcal{T}^{r''}_{s''}(M)$, respectively, and let $f$ be a function in $C^\infty(M)$. Then:*

(a) $(\mathcal{A}_1+\mathcal{A}_2)\otimes\mathcal{B}=\mathcal{A}_1\otimes\mathcal{B}+\mathcal{A}_2\otimes\mathcal{B}$.

(b) $\mathcal{A}\otimes(\mathcal{B}_1+\mathcal{B}_2)=\mathcal{A}\otimes\mathcal{B}_1+\mathcal{A}\otimes\mathcal{B}_2$.

(c) $(f\mathcal{A})\otimes\mathcal{B}=f(\mathcal{A}\otimes\mathcal{B})=\mathcal{A}\otimes(f\mathcal{B})$.

(d) $(\mathcal{A}\otimes\mathcal{B})\otimes\mathcal{C}=\mathcal{A}\otimes(\mathcal{B}\otimes\mathcal{C})$.

*Proof.* This is the manifold version of Theorem 5.1.2.

Let $M$ be a smooth $m$-manifold, and let $(U,(x^i))$ be a chart on $M$. Let $1\le i_1,\ldots,i_r\le m$ and $1\le j_1,\ldots,j_s\le m$ be integers, and consider the tensor field

\[
\frac{\partial}{\partial x^{i_1}}\otimes\cdots\otimes\frac{\partial}{\partial x^{i_r}}\otimes d(x^{j_1})\otimes\cdots\otimes d(x^{j_s})
\qquad\text{in}\qquad \mathcal{T}^r_s(U)
\]