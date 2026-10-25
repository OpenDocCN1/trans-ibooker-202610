
Let \((\widetilde{U}, \widetilde{\varphi})\) be another chart in \(\mathfrak{A}\) at \(p\), and let \(\widetilde{\mathcal{H}}\) be the corresponding coordinate frame. Since \(\mathfrak{A}\) is consistent, \((U, \varphi)\) and \((\widetilde{U}, \widetilde{\varphi})\) are consistent, hence \([\mathcal{H}_{\varphi^{-1}(p)}] = [\widetilde{\mathcal{H}}_{\widetilde{\varphi}^{-1}(p)}]\). This shows that the definition of \(\mathfrak{O}(p)\) is independent of the choice of representative chart at \(p\). We call the set of equivalence classes

\[
\mathfrak{O} = \{\mathfrak{O}(p) : p \in M\}
\]

the orientation induced by \(\mathfrak{A}\) and say that \(M\) is oriented by \(\mathfrak{A}\). The notation \((M, \mathfrak{g}, \mathfrak{O})\), and sometimes \((M, \mathfrak{g}, \mathfrak{A}, \mathfrak{O})\), is used as an alternative to \((M, \mathfrak{g}, \mathfrak{A})\).

Consider the map \(\iota : \mathbb{R}^2 \longrightarrow \mathbb{R}^2\) given by \(\iota(r^1, r^2) = (-r^1, r^2)\). Since \(\iota\) is a diffeomorphism and \(\iota^{-1} = \iota\), \((\iota(U), \varphi \circ \iota)\) is a chart on \(M\), where, for brevity, we denote \(\iota|_{\iota(U)}\) by \(\iota\). Because

\[
\varphi \circ \iota(r^1, r^2) = (\varphi^1(-r^1, r^2), \varphi^2(-r^1, r^2), \varphi^3(-r^1, r^2)),
\]

the corresponding coordinate frame and coordinate unit normal vector field are

\[
-\mathcal{H} = (-H_1, H_2)
\]

and \(-G\). It is easily shown using Theorem 11.3.3 that

\[
-\mathfrak{A} = \{(\iota(U), \varphi \circ \iota) : (U, \varphi) \in \mathfrak{A}\}
\]

is a consistent atlas for \(M\). The orientation of \(M\) induced by \(-\mathfrak{A}\) is

\[
-\mathfrak{O} = \{-\mathfrak{O}(p) : p \in M\},
\]

where

\[
-\mathfrak{O}(p) = [-\mathcal{H}_{\varphi^{-1}(p)}] = [(-H_1|_{\varphi^{-1}(p)}, H_2|_{\varphi^{-1}(p)})].
\]

We say that the orientation \(-\mathfrak{O}\) is the opposite of \(\mathfrak{O}\).

**Theorem 12.7.3.** If \((M, \mathfrak{g}, \mathfrak{A})\) is an oriented regular surface in \(\mathbb{R}^3_\nu\), then:

- (a) There is a unit normal vector field \(\mathcal{N}\) in \(\mathfrak{X}_{\mathbb{R}^3_\nu}(M)\), called the Gauss map, such that for every chart \((U, \varphi)\) in \(\mathfrak{A}\), \(\mathcal{N} \circ \varphi\) is the corresponding coordinate unit normal vector field.
- (b) \(\mathcal{N}\) satisfies properties [V1] and [V2] of Section 12.2.

**Remark.** Calling \(\mathcal{N}\) a "map" is reasonable because \(\mathfrak{X}_{\mathbb{R}^3_\nu}(M)\) is, by definition, a set of maps.

**Proof.** (a): Since \(\mathfrak{A}\) is a consistent atlas for \(M\), it follows from Theorem 12.7.2 that whenever two charts in \(\mathfrak{A}\) overlap, the corresponding coordinate unit normal vector fields agree on the overlap. We can therefore assign to each point \(p\) in \(M\) a vector \(\mathcal{N}_p\) that is unit normal at \(p\), and do so in such a way that \(\mathcal{N}_p\) is independent of the choice of chart at \(p\) selected from \(\mathfrak{A}\). More specifically, given the chart \((U, \varphi)\) in \(\mathfrak{A}\) and the corresponding coordinate unit normal vector field \(G\), we define \(\mathcal{N}_p = G_{\varphi^{-1}(p)}\) for all \(p\) in \(\varphi(U)\). Since \(G\) is smooth on \(U\), it follows

Reviewing the proof of Theorem 12.2.12, we see that the preceding example rests on the gradient in question satisfying property [V2] of Section 12.2. More generally, we have the following extension of Theorem 12.2.10(b).

**Theorem 12.7.8 (Level Set of Function).** Let \(U\) be an open set in \(\mathbb{R}^3_\nu\), let \(f\) be a function in \(C^\infty(U)\), let \(c\) be a real number in \(f(U)\), and let \(M = f^{-1}(c)\). If \(\operatorname{Grad}(f)\) satisfies property [V2] of Section 12.2, then \((M, \mathfrak{g})\) is an orientable regular surface in \(\mathbb{R}^3_\nu\).

**Proof.** This follows from Theorem 12.2.10(b) and Theorem 12.7.6.

**Example 12.7.9 (Möbius Band).** The Möbius band is the subset of \(\mathbb{R}^3\) defined by

\[
\text{Möb} = \{\sigma(t, \varphi) \in \mathbb{R}^3 : (t, \varphi) \in U\},
\]

where

\[
\sigma(t, \varphi) = ([1 - t \sin(\varphi/2)] \cos(\varphi), [1 - t \sin(\varphi/2)] \sin(\varphi), t \cos(\varphi/2))
\]

and

\[
U = (-1/2, 1/2) \times (-\pi, \pi).
\]

A model of Möb can be made by giving a strip of paper a half twist and then pasting the ends together. See Figure 12.7.1. It can be shown that Möb is a regular surface in \(\mathbb{R}^3_0\), something that is intuitively clear from the diagram. However, \(\mathfrak{X}_{\mathbb{R}^3_0}(\text{Möb})\) does not contain a unit normal vector field. This can be seen by choosing a starting point on the curve that bisects Möb longitudinally (dotted line in Figure 12.7.1), and then sliding a given unit normal vector (viewed as an arrow) along the curve on its base while keeping the shaft normal to the surface. After making a complete circuit, the unit normal vector is at the starting point, but now projects from the “opposite” side of the band. This shows that any unit normal vector field along Möb is not continuous, let alone smooth. However, completing a second circuit produces the original vector. In a manner of speaking, Möb has only one “side”.

![](img/d712710a8eb78c0959204afd5dde4c91_306_0.png)

**Figure 12.7.1.** Möbius band: Diagram for Example 12.7.9

## 12.8 Gauss Curvature in \(\mathbb{R}^3_\nu\)

In this section, we describe a way of measuring the “curvature” of a regular surface in \(\mathbb{R}^3_\nu\).

As part of the discussion of hyperquadrics \(Q^2\) in \(\mathbb{R}^3_\nu\), in Section 12.2, we observed that \(S^2\) is the set of (spacelike) unit vectors in \(\mathbb{R}^3_0\), \(P^2\) is the set of spacelike unit vectors in \(\mathbb{R}^3_1\), and \(H^2\) is the set of timelike unit vectors in \(\mathbb{R}^3_1\). In fact, more than just being sets, according to Theorem 12.2.12(a), \(S^2\) is a regular surface in \(\mathbb{R}^3_0\), and \(P^2\) and \(H^2\) are regular surfaces in \(\mathbb{R}^3_1\).

Let \((M, \mathfrak{g}, \mathcal{N}, \mathfrak{O})\) be an oriented regular surface in \(\mathbb{R}^3_\nu\), let \(\mathcal{N} : M \longrightarrow \mathbb{R}^3_\nu\) be the Gauss map, and let \(p\) be a point in \(M\). Since \(\mathcal{N}_p\) is a unit normal vector at \(p\), it follows from (12.2.3) that \(q(\mathcal{N}_p) = \langle \mathcal{N}_p, \mathcal{N}_p \rangle = \epsilon_M\), where \(q\) is the quadratic function corresponding to \(\mathfrak{g}\). Thus, \(\mathcal{N}_p\) is in the same hyperquadric for all \(p\) in \(M\). Denoting the hyperquadric by \(Q^2\), we can now say that \(\mathcal{N}_p\) is in \(Q^2\) for all \(p\) in \(M\). Thus, \(\mathcal{N}\) can be expressed more precisely as

\[
\mathcal{N} : M \longrightarrow Q^2.
\]

The situation for \(S^2\) is depicted in Figure 12.8.1, where \(\mathcal{N}_i\) stands for \(\mathcal{N}_{p_i}\), for \(i = 1, 2, 3\).

![](img/d712710a8eb78c0959204afd5dde4c91_307_0.png)

**Figure 12.8.1.** Gauss map

The differential of \(\mathcal{N}\) at \(p\) is \(d_p(\mathcal{N}) : T_p(M) \longrightarrow T_{\mathcal{N}_p}(Q^2)\). By definition, \(\mathcal{N}_p\) is in \(T_p(M)^\perp\). On the other hand, since \(\mathcal{N}_p\) is in \(Q^2\), we have from Theorem 12.2.12(c) that \(\mathcal{N}_p\) is also in \(T_{\mathcal{N}_p}(Q^2)^\perp\). Since \(T_p(M)^\perp\) and \(T_{\mathcal{N}_p}(Q^2)^\perp\) are both 1-dimensional, it follows that \(T_p(M)^\perp = T_{\mathcal{N}_p}(Q^2)^\perp\), and then from Theorem 4.1.2(c) that

\[
T_p(M) = T_{\mathcal{N}_p}(Q^2).
\]

We can therefore express the differential of \(\mathcal{N}\) at \(p\) as

\[
d_p(\mathcal{N}) : T_p(M) \longrightarrow T_p(M).
\]

Thus, \(d_p(\mathcal{N})\) is a linear map from \(T_p(M)\) to itself.

For each point \(p\) in \(M\), the **Weingarten map at \(p\)** is denoted by

\[
\mathcal{W}_p : T_p(M) \longrightarrow T_p(M)
\]

and defined by

\[
\mathcal{W}_p = -d_p(\mathcal{N}). \tag{12.8.1}
\]

For all vectors \(v\) in \(T_p(M)\), we have from (11.6.1) that

\[
\mathcal{W}_p(v) = -\frac{d(\mathcal{N} \circ \lambda)}{dt}(t_0), \tag{12.8.2}
\]

where \(\lambda : (a,b) \longrightarrow M\) is any smooth curve such that \(\lambda(t_0) = p\) and \((d\lambda/dt)(t_0) = v\) for some \(t_0\) in \((a,b)\). The **Weingarten map** is the linear map

\[
\mathcal{W} : \mathfrak{X}(M) \longrightarrow \mathfrak{X}(M)
\]

defined by

\[
\mathcal{W}(X)_p = \mathcal{W}_p(X_p) \tag{12.8.3}
\]

for all vector fields \(X\) in \(\mathfrak{X}(M)\) and all \(p\) in \(M\).

Let \((U, \varphi)\) be a chart in \(\mathfrak{A}\), let \(\mathcal{H} = (H_1, H_2)\) be the corresponding coordinate frame, and let \(q\) be a point in \(U\). The vector \(\mathcal{W}_p(H_j|_q)\) can be expressed as

\[
\mathcal{W}_p(H_j|_q) = \sum_i \mathfrak{w}_j^i(q) H_i|_q, \tag{12.8.4}
\]

where the \(\mathfrak{w}_j^i\) are uniquely determined functions in \(C^\infty(U)\). We then have from (2.2.2) and (2.2.3) that

\[
[\mathcal{W}_p]_{\mathcal{H}_q} = \begin{bmatrix} \mathfrak{w}_1^1(q) & \mathfrak{w}_1^2(q) \\ \mathfrak{w}_2^1(q) & \mathfrak{w}_2^2(q) \end{bmatrix}. \tag{12.8.5}
\]

**Theorem 12.8.1.** *If \((M, \mathfrak{g}, \mathfrak{A}, \mathfrak{O})\) is an oriented regular surface in \(\mathbb{R}^3_\nu\) and \(X\) is a vector field in \(\mathfrak{X}(M)\), then*

\[
\mathcal{W}(X) = -D_X(\mathcal{N}).
\]

**Proof.** Let \(p\) be a point in \(M\), and let \(\lambda : (a,b) \longrightarrow M\) be a smooth curve such that \(\lambda(t_0) = p\) and \((d\lambda/dt)(t_0) = X_p\) for some \(t_0\) in \((a,b)\). Then

\[
\begin{aligned}
-D_X(\mathcal{N})_p
&= -\frac{d(\mathcal{N} \circ \lambda)}{dt}(t_0) \quad [(12.3.4)] \\
&= \mathcal{W}_p(X_p) \quad [(12.8.2)] \\
&= \mathcal{W}(X)_p. \quad [(12.8.3)]
\end{aligned}
\]

Since \(p\) was arbitrary, the result follows.
