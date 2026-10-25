
Proof. By Theorem 14.5.2, \(d_p(f)=\sum_j a_j d_p(x^j)\) for some real numbers \(a_j\). We have
\[
\begin{aligned}
\frac{\partial f}{\partial x^i}(p)
&= d_p(f)\left(\frac{\partial}{\partial x^i}\bigg|_p\right) \qquad [(14.5.4)] \\
&= \left(\sum_j a_j d_p(x^j)\right)\left(\frac{\partial}{\partial x^i}\bigg|_p\right) \\
&= \sum_j a_j d_p(x^j)\left(\frac{\partial}{\partial x^i}\bigg|_p\right) \\
&= a_i \qquad [\text{Th }14.5.2]
\end{aligned}
\]
for \(i=1,\dots,m\).

### Theorem 14.5.4 (Change of Dual Coordinate Basis)

Let \(M\) be a smooth \(m\)-manifold, and let \(p\) be a point in \(M\). Let \((U,(x^i))\) and \((\tilde U,(\tilde x^i))\) be charts at \(p\), let \(\mathcal X_p=((\partial/\partial x^1)|_p,\dots,(\partial/\partial x^m)|_p)\) and \(\tilde{\mathcal X}_p=((\partial/\partial \tilde x^1)|_p,\dots,(\partial/\partial \tilde x^m)|_p)\) be the corresponding coordinate bases at \(p\), and let \(\mathcal X_p^*=(d_p(x^1),\dots,d_p(x^m))\) and \(\tilde{\mathcal X}_p^*=(d_p(\tilde x^1),\dots,d_p(\tilde x^m))\) be the corresponding dual coordinate bases at \(p\). Then:

- (a) \[
  d_p(x^j)=\sum_i \frac{\partial x^j}{\partial \tilde x^i}(p)\,d_p(\tilde x^i)
  \]
  for \(j=1,\dots,m\).

- (b) \[
  [\mathrm{id}_{T_p(M)}]_{\mathcal X_p^*}^{\tilde{\mathcal X}_p^*}
  =
  \begin{bmatrix}
  \frac{\partial x^1}{\partial \tilde x^1}(p) & \dots & \frac{\partial x^m}{\partial \tilde x^1}(p) \\
  \vdots & \ddots & \vdots \\
  \frac{\partial x^1}{\partial \tilde x^m}(p) & \dots & \frac{\partial x^m}{\partial \tilde x^m}(p)
  \end{bmatrix}
  =
  \left( [\mathrm{id}_{T_p(M)}]_{\tilde{\mathcal X}_p}^{\mathcal X_p} \right)^{\mathrm T}.
  \]

**Proof.** (a): Setting \(f=x^j\) and \(x^i=\tilde x^i\) in Theorem 14.5.3 gives the result.  
(b): The first equality follows from (2.2.6), (2.2.7), and part (a). The second equality follows from Theorem 14.3.5(b).

### Theorem 14.5.5

Let \(M\) be a smooth manifold, let \(f\) be a function in \(C^\infty(M)\), let \(p\) be a point in \(M\), and let \(v\) be a vector in \(T_p(M)\). Then
\[
d_p(f)(v)=v(f).
\]

**Proof.** By Theorem 1.2.1(d) and Theorem 14.5.2,
\[
v=\sum_i d_p(x^i)(v)\,\frac{\partial}{\partial x^i}\bigg|_p,
\]
hence
\[
\begin{aligned}
v(f)
&= \left(\sum_i d_p(x^i)(v)\frac{\partial}{\partial x^i}\bigg|_p\right)(f) \\
&= \sum_i d_p(x^i)(v)\frac{\partial f}{\partial x^i}(p) \qquad [(14.3.3)] \\
&= \left(\sum_i \frac{\partial f}{\partial x^i}(p)\,d_p(x^i)\right)(v) \\
&= d_p(f)(v). \qquad [\text{Th }14.5.3]
\end{aligned}
\]

### Theorem 14.5.6

Let \(M\) be a smooth manifold, let \(p\) be a point in \(M\), let \(f,g\) be functions in \(C^\infty(M)\), and let \(c\) be a real number. Then:

- (a) \(d_p(cf+g)=c\,d_p(f)+d_p(g)\).
- (b) \(d_p(fg)=f(p)d_p(g)+g(p)d_p(f)\).

**Proof.** For a vector \(v\) in \(T_p(M)\), we have
\[
\begin{aligned}
d_p(cf+g)(v)
&= v(cf+g) \qquad [\text{Th }14.5.5] \\
&= c\,v(f)+v(g) \\
&= c\,d_p(f)(v)+d_p(g)(v) \qquad [\text{Th }14.5.5] \\
&= (c\,d_p(f)+d_p(g))(v)
\end{aligned}
\]
and
\[
\begin{aligned}
d_p(fg)(v)
&= v(fg) \qquad [\text{Th }14.5.5] \\
&= f(p)v(g)+g(p)v(f) \qquad [(14.3.1)] \\
&= f(p)d_p(g)(v)+g(p)d_p(f)(v) \qquad [\text{Th }14.5.5] \\
&= (f(p)d_p(g)+g(p)d_p(f))(v).
\end{aligned}
\]
Since \(v\) was arbitrary, the result follows.

### Theorem 14.5.7

Let \((e_1,\dots,e_m)\) be the standard basis for \(\mathbb{R}^m\), and let \((\xi^1,\dots,\xi^m)\) be its dual basis. Let \((r^1,\dots,r^m)\) be standard coordinates on \(\mathbb{R}^m\), let \(p\) be a point in \(\mathbb{R}^m\), let \(((\partial/\partial r^1)|_p,\dots,(\partial/\partial r^m)|_p)\) be the corresponding coordinate basis at \(p\), called the standard coordinate basis at \(p\), and let \((d_p(r^1),\dots,d_p(r^m))\) be the corresponding dual coordinate basis at \(p\), called the standard dual coordinate basis at \(p\). Then \(((\partial/\partial r^1)|_p,\dots,(\partial/\partial r^m)|_p)\) can be identified with \((e_1,\dots,e_m)\), and \((d_p(r^1),\dots,d_p(r^m))\) can be identified with \((\xi^1,\dots,\xi^m)\), written
\[
\left(\frac{\partial}{\partial r^1}\bigg|_p,\dots,\frac{\partial}{\partial r^m}\bigg|_p\right)=(e_1,\dots,e_m)
\]
and
\[
(d_p(r^1),\dots,d_p(r^m))=(\xi^1,\dots,\xi^m).
\]

**Proof.** It is sufficient to prove either of the above identities, the other following by taking the dual. In keeping with (14.3.4), we make the identification
\[
T_p(\mathbb{R}^m)=\mathbb{R}^m.
\]
By definition, \((r^1,\dots,r^m)\) is the coordinate map of the chart \((\mathbb{R}^m,\mathrm{id}_{\mathbb{R}^m}=(r^1,\dots,r^m))\). Since \(r^i\) is linear, by Theorem 14.4.4, \(d_p(r^i)=r^i\) for \(i=1,\dots,m\). For a point \(q\in\mathbb{R}^m\),
\[
\begin{aligned}
\sum_i d_p(r^i)(q)e_i
&= \sum_i r^i(q)e_i \\
&= (r^1(q),\dots,r^m(q)) \\
&= (r^1,\dots,r^m)(q) \\
&= \mathrm{id}_{\mathbb{R}^m}(q) \\
&= q \\
&= \sum_i \xi^i(q)e_i,
\end{aligned}
\]
where the last equality follows from Theorem 1.2.1(d). Since \(q\) was arbitrary, \(d_p(r^i)=\xi^i\) for \(i=1,\dots,m\).

## 14.6 Immersions and Diffeomorphisms

In this brief section, we generalize the discussion of immersions and diffeomorphisms in Section 10.2 to the setting of smooth manifolds.

Let \(M\) and \(N\) be smooth manifolds, where \(\dim(M)\leq\dim(N)\), let \(F:M\longrightarrow N\) be a smooth map, and let \(p\) be a point in \(M\). We say that \(F\) is an **immersion at \(p\)** if the differential map \(d_p(F):T_p(M)\longrightarrow T_{F(p)}(N)\) is injective, and that \(F\) is an **immersion (on \(M\))** if it is an immersion at every \(p\) in \(M\).

Now suppose \(M\) and \(N\) have the same dimension, and let \(G,H:M\longrightarrow N\) be smooth maps. We say that \(G\) is a **diffeomorphism**, and that \(M\) and \(N\) are **diffeomorphic**, if \(G\) is bijective and \(G^{-1}:N\longrightarrow M\) is smooth. We say that \(H\) is a **local diffeomorphism at \(p\)** if there is a neighborhood \(U\) of \(p\) in \(M\) and a neighborhood \(V\) of \(H(p)\) in \(N\) such that \(H|_U:U\longrightarrow V\) is a diffeomorphism. Then \(H\) is said to be a **local diffeomorphism (on \(M\))** if it is a local diffeomorphism at every \(p\) in \(M\).

### Theorem 14.6.1

Let \(M\) and \(N\) be smooth manifolds with the same dimension, let \(F:M\longrightarrow N\) be a diffeomorphism, and let \(p\) be a point in \(M\). Then the differential map \(d_p(F):T_p(M)\longrightarrow T_{F(p)}(N)\) is a linear isomorphism, with inverse \(d_p(F)^{-1}=d_{F(p)}(F^{-1})\).

**Proof.** By Theorem 14.4.2, \(d_p(F)\) is a linear map. We have from Theorem 14.4.3 and Theorem 14.4.5 that
\[
\mathrm{id}_{T_p(M)}
=d_p(\mathrm{id}_M)
=d_p(F^{-1}\circ F)
=d_{F(p)}(F^{-1})\circ d_p(F).
\]
Similarly, \(\mathrm{id}_{T_{F(p)}(N)}=d_p(F)\circ d_{F(p)}(F^{-1})\). By Theorem A.4, \(d_p(F)\) is bijective.

### Theorem 14.6.2 (Inverse Map Theorem)

Let \(M\) and \(N\) be smooth manifolds with the same dimension, let \(F:M\longrightarrow N\) be a smooth map, and let \(p\) be a point in \(M\). Then \(F\) is a local diffeomorphism at \(p\) if and only if it is an immersion at \(p\). Thus, \(F\) is a local diffeomorphism if and only if it is an immersion.

## 14.7 Curves

The following definitions are borrowed more or less directly from Section 10.1 and Section 11.1.

A (parametrized) curve on a smooth manifold \(M\) is a map \(\lambda:I\longrightarrow M\), where \(I\) is an interval in \(\mathbb{R}\) that is either open, closed, half-open, or half-closed, and where the possibility that \(I\) is infinite is not excluded. Our focus will be on the case where \(I\) is a finite open interval, usually denoted by \((a,b)\). Rather than provide a separate statement identifying the independent variable for the curve, most often denoted by \(t\), and sometimes by \(u\), it is helpful to incorporate this into the notation for \(\lambda\), as in \(\lambda(t):I\longrightarrow M\). When \(I\) is a closed interval \([a,b]\) and \(\lambda\) is continuous, we say that \(\lambda\) joins \(\lambda(a)\) to \(\lambda(b)\). It is convenient to adopt the following convention.

Henceforth, when required by the context, the interval \(I\) is assumed to contain \(0\).

Consider the curve \(\lambda(t):(a,b)\longrightarrow M\). Viewing \((a,b)\) as a smooth \(1\)-manifold, we say that \(\lambda\) is smooth [on \((a,b)\)] if it is smooth as a map between smooth manifolds. Suppose \(\lambda\) is in fact smooth. For a given point \(t\) in \((a,b)\), we have the differential map
\[
d_t(\lambda):T_t((a,b))\longrightarrow T_{\lambda(t)}(M).
\]
A covering chart for \((a,b)\) is \(((a,b),\mathrm{id}_{(a,b)}=r)\), where \(r\) is the standard coordinate on \((a,b)\). The corresponding coordinate basis at \(t\) is \(((d/dr)|_t)\). It follows that
\[
d_t(\lambda)\left(\frac{d}{dr}\bigg|_t\right)\in T_{\lambda(t)}(M) \tag{14.7.1}
\]
for all \(t\in(a,b)\). Thus, for all functions \(f\in C^\infty((a,b))\),
\[
d_t(\lambda)\left(\frac{d}{dr}\bigg|_t\right)(f)
= \frac{d}{dr}\bigg|_t (f\circ\lambda)
\quad [(14.4.1)]
= \frac{d(f\circ\lambda)}{dr}(t).
\quad [(14.3.3)]
\tag{14.7.2}
\]
