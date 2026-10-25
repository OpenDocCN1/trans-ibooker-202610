Then $(\mathbb{R}^3,(s^1,s^2,s^3))$ is a chart on $\mathbb{R}^3$. Let $\mathcal{Y}_p$ be the corresponding coordinate basis at $p=\iota(p)$. It follow from Theorem 14.4.6(b) that

$$
[d_p(\iota)]^{\mathcal{Y}_p}_{\mathcal{X}_p}=\left[\frac{\partial(s^i\circ\iota\circ\varphi)}{\partial r^j}(\varphi^{-1}(p))\right]=\left[\frac{\partial s^i}{\partial r^j}(\varphi^{-1}(p))\right],
$$

and then from property [C1] of Section 11.2 and Theorem 11.2.2 that $d_p(\iota)$ is injective.

Reflecting on the above definition of a submanifold, [S1] seems like an obvious requirement, but the same cannot be said of [S2]. Theorem 14.8.2 gives some insight into [S2], but its rationale is far from transparent. Rather than search for a deeper understanding of [S2], we change course and provide an alternative perspective on submanifolds.

For a given integer $1\le k\le m$, we define a type of projection map $P_k:\mathbb{R}^m\longrightarrow\mathbb{R}^k\times\{0\}^{m-k}$ by

$$
P_k(r^1,\ldots,r^k,r^{k+1},\ldots,r^m)=(r^1,\ldots,r^k,0,\ldots,0).
$$

Let $U$ be an open set in $\mathbb{R}^m$, and let $S$ be a subset of $U$. We say that $S$ is a **$k$-slice** of $U$ if $S\subseteq P_k(U)$.

# **Theorem 14.8.3 (Slice Criterion for Submanifolds).**

Let $\overline{M}$ be a smooth $\overline{m}$-manifold, let $M$ be a subset of $\overline{M}$ with the subspace topology, and let $1\le m\le\overline{m}$. Then:

+   (a) $M$ is an $m$-submanifold of $\overline{M}$ if and only if for every point $p$ in $M$, there is a chart $(U,\varphi=(x^1,\ldots,x^{\overline{m}}))$ on $\overline{M}$ at $p$, called a **slice chart for $M$ in $\overline{M}$**, such that $\varphi(M\cap U)$ is an $m$-slice of $\varphi(U)$.
+   
+   (b) If $M$ is an $m$-submanifold of $\overline{M}$, then corresponding to the slice chart $(U,\varphi=(x^1,\ldots,x^{\overline{m}}))$ on $\overline{M}$ at $p$, there is the chart $(M\cap U,(x^1|_{M\cap U},\ldots,x^m|_{M\cap U}))$ on $M$ at $p$ such that
+   
+   $$
+   \left(\left.\frac{\partial}{\partial(x^1|_{M\cap U})}\right|_q,\ldots,\left.\frac{\partial}{\partial(x^m|_{M\cap U})}\right|_q\right)
+   $$
+   
+   can be identified with
+   
+   $$
+   \left(\left.\frac{\partial}{\partial x^1}\right|_q,\ldots,\left.\frac{\partial}{\partial x^m}\right|_q\right)
+   $$
+   
+   for all $q$ in $M\cap U$. Thus, $T_q(M)$ can be identified with the subspace of $T_q(\overline{M})$ spanned by $(\partial/\partial x^1)|_q,\ldots,(\partial/\partial x^m)|_q$ for all $q$ in $M\cap U$.

The above notation is rather cumbersome. Henceforth, $x^i|_{M\cap U}$ will be abbreviated to $x^i$ for $i=1,\ldots,m$. In this revised notation, we denote

$$
(M\cap U,(x^1|_{M\cap U},\ldots,x^m|_{M\cap U}))\quad\text{by}\quad(M\cap U,(x^1,\ldots,x^m)),
$$

and

$$
\left(\left.\frac{\partial}{\partial(x^1|_{M\cap U})}\right|_q,\ldots,\left.\frac{\partial}{\partial(x^m|_{M\cap U})}\right|_q\right)\quad\text{by}\quad\left(\left.\frac{\partial}{\partial x^1}\right|_q,\ldots,\left.\frac{\partial}{\partial x^m}\right|_q\right).
$$