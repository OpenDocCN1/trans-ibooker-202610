
Let \((M,\mathfrak{g},\mathfrak{A},\mathfrak{D})\) be an oriented regular surface in \(\mathbb{R}^3_{\nu}\), and let \(p\) be a point in \(M\). Since \(\mathfrak{g}_p\) is bilinear and \(\mathcal{W}_p\) is linear, we have the tensor \(\mathfrak{h}_p\) in \(\mathcal{T}_2^0(T_p(M))\) defined by

\[
\mathfrak{h}_p(v,w)=\langle\mathcal{W}_p(v),w\rangle\tag{12.8.6}
\]

for all vectors \(v,w\) in \(T_p(M)\). The **second fundamental form on \(M\)** is the map denoted by \(\mathfrak{h}\) and defined by the assignment \(p\longmapsto\mathfrak{h}_p\) for all \(p\) in \(M\). In the literature, \(\mathfrak{h}\) is often denoted by \(II\).

For vector fields \(X,Y\) in \(\mathfrak{X}(M)\), we define a function

\[
\mathfrak{h}(X,Y)=\langle\mathcal{W}(X),Y\rangle: M\longrightarrow\mathbb{R}
\]

in \(C^{\infty}(M)\) by the assignment

\[
p\longmapsto\mathfrak{h}_p(X_p,Y_p)=\langle\mathcal{W}(X)_p,Y_p\rangle\tag{12.8.7}
\]

for all \(p\) in \(M\).

Let \((U,\varphi)\) be a chart in \(\mathfrak{A}\), and let \(\mathcal{H}=(H_1,H_2)\) be the corresponding coordinate frame. We define functions \(\mathfrak{h}_{ij}\) in \(C^{\infty}(U)\) by

\[
\mathfrak{h}_{ij}(q)=\mathfrak{h}_p(H_i|_q,H_j|_q)\tag{12.8.8}
\]

for all \(q\) in \(U\) for \(i,j=1,2\), where \(p=\varphi(q)\). The **matrix of \(\mathfrak{h}\)** with respect to \(\mathcal{H}\) is denoted by \(\mathfrak{h}_{\mathcal{H}}\) and defined by

\[
\mathfrak{h}_{\mathcal{H}}(q)=
\begin{bmatrix}
\mathfrak{h}_{11}(q)&\mathfrak{h}_{12}(q)\\
\mathfrak{h}_{21}(q)&\mathfrak{h}_{22}(q)
\end{bmatrix}
\]

for all \(q\) in \(U\).

## Theorem 12.8.2.

Let \((M,\mathfrak{g},\mathfrak{A},\mathfrak{D})\) be an oriented regular surface in \(\mathbb{R}^3_{\nu}\), let \(\mathcal{N}\) be the Gauss map, and let \(p\) be a point in \(M\). Let \((U,\varphi)\) be a chart in \(\mathfrak{A}\) at \(p\), let \((H_1,H_2)\) be the corresponding coordinate frame, and let \(q=\varphi^{-1}(p)\). Then:

- (a) \(\mathfrak{h}_{ij}(q)=\left\langle\left.\frac{\partial H_i}{\partial r^j}\right|_q,\mathcal{N}_p\right\rangle=\epsilon_M\vartheta_{ij}(q)\), where \(\vartheta_{ij}\) is given by (12.3.6).
- (b) \(\mathcal{W}_p\) is self-adjoint with respect to \(\mathfrak{g}_p\); that is, \(\langle\mathcal{W}_p(v),w\rangle=\langle v,\mathcal{W}_p(w)\rangle\) for all vectors \(v,w\) in \(T_p(M)\).
- (c) \(\mathfrak{h}_p\) is a symmetric bilinear function.

**Proof.** (a): Let \((e_1,e_2)\) be the standard basis for \(\mathbb{R}^2\). For given \(1\le i\le2\), we define a smooth map

\[
\zeta=(\zeta^1,\zeta^2):(-\varepsilon,\varepsilon)\longrightarrow U
\]

by \(\zeta(t)=q+t e_i\), where \(\varepsilon>0\) is chosen small enough that \((q-\varepsilon e_i,q+\varepsilon e_i)\subset U\). Consider the smooth curve \(\lambda(t):(-\varepsilon,\varepsilon)\longrightarrow M\) defined by \(\lambda=\varphi\circ\zeta\), and observe that \(\lambda(0)=\varphi(q)=p\). By Theorem 10.1.10,

\[
\left.\frac{d\lambda}{dt}\right|_0
=\left.\frac{d(\varphi\circ\zeta)}{dt}\right|_0
=\sum_j\left.\frac{d\zeta^j}{dt}\right|_0 H_j|_q
=H_i|_q.
\]

It also follows from Theorem 10.1.10 that

\[
\left.\frac{d(\mathcal{N}\circ\lambda)}{dt}\right|_0
=\left.\frac{d(\mathcal{N}\circ\varphi\circ\zeta)}{dt}\right|_0
=\sum_j\left.\frac{d\zeta^j}{dt}\right|_0 \left.\frac{\partial(\mathcal{N}\circ\varphi)}{\partial r^j}\right|_q
=\left.\frac{\partial(\mathcal{N}\circ\varphi)}{\partial r^i}\right|_q,
\]

and then from (12.8.2) and (12.8.9) that

\[
\mathcal{W}_p(H_i|_q)=-\left.\frac{d(\mathcal{N}\circ\lambda)}{dt}\right|_0
=-\left.\frac{\partial(\mathcal{N}\circ\varphi)}{\partial r^i}\right|_q.
\]

Taking the partial derivative with respect to \(r^i\) of both sides of \(\langle\mathcal{N}_p,H_j|_q\rangle=0\) yields

\[
0=
\left\langle\left.\frac{\partial(\mathcal{N}\circ\varphi)}{\partial r^i}\right|_q,H_j|_q\right\rangle
+
\left\langle\mathcal{N}_p,\left.\frac{\partial H_j}{\partial r^i}\right|_q\right\rangle.
\]

We have from Theorem 10.1.6 and (11.2.1) that

\[
\frac{\partial H_i}{\partial r^j}=\frac{\partial H_j}{\partial r^i},
\]

hence

\[
\left\langle-\left.\frac{\partial(\mathcal{N}\circ\varphi)}{\partial r^i}\right|_q,H_j|_q\right\rangle
=
\left\langle\mathcal{N}_p,\left.\frac{\partial H_i}{\partial r^j}\right|_q\right\rangle.
\]

Then

\[
\begin{aligned}
\mathfrak{h}_{ij}(q)
&=\langle\mathcal{W}_p(H_i|_q),H_j|_q\rangle & &[(12.8.6),\ (12.8.8)]\\
&=\left\langle-\left.\frac{\partial(\mathcal{N}\circ\varphi)}{\partial r^i}\right|_q,H_j|_q\right\rangle & &[(12.8.10)]\\
&=\left\langle\left.\frac{\partial H_i}{\partial r^j}\right|_q,\mathcal{N}_p\right\rangle, & &[(12.8.11)]
\end{aligned}
\]

which proves the first equality. We also have

\[
\begin{aligned}
\left\langle\left.\frac{\partial H_i}{\partial r^j}\right|_q,\mathcal{N}_p\right\rangle
&=\left\langle\left.\frac{\partial H_i}{\partial r^j}\right|_q,G_q\right\rangle & &[\text{Th 12.7.3(a)}]\\
&=\epsilon_M\vartheta_{ij}(q), & &[\text{Th 12.3.3(c)}]
\end{aligned}
\]

which proves the second equality.

**(b):** We have

\[
\begin{aligned}
\langle\mathcal{W}_p(H_i|_q),H_j|_q\rangle
&=\mathfrak{h}_p(H_i|_q,H_j|_q) & &[(12.8.6)]\\
&=\mathfrak{h}_{ij}(q) & &[(12.8.8)]\\
&=\epsilon_M\vartheta_{ij}(q) & &[\text{part (a)}]\\
&=\epsilon_M\vartheta_{ji}(q) & &[\text{Th 12.3.3(d)}]\\
&=\mathfrak{h}_{ji}(q) & &[\text{part (a)}]\\
&=\mathfrak{h}_p(H_j|_q,H_i|_q) & &[(12.8.8)]\\
&=\langle\mathcal{W}_p(H_j|_q),H_i|_q\rangle & &[(12.8.6)]\\
&=\langle H_i|_q,\mathcal{W}_p(H_j|_q)\rangle.
\end{aligned}
\]

The result now follows from the observations that \((H_1|_q,H_2|_q)\) is a basis for \(T_p(M)\), \(\mathfrak{g}_p\) is bilinear, and \(\mathcal{W}_p\) is linear.

**(c):** It was established in connection with (12.8.6) that \(\mathfrak{h}_p\) is bilinear. For vectors \(v,w\) in \(T_p(M)\), we have

\[
\begin{aligned}
\mathfrak{h}_p(v,w)
&=\langle\mathcal{W}_p(v),w\rangle & &[(12.8.6)]\\
&=\langle v,\mathcal{W}_p(w)\rangle & &[\text{part (b)}]\\
&=\langle\mathcal{W}_p(w),v\rangle\\
&=\mathfrak{h}_p(w,v). & &[(12.8.6)]
\end{aligned}
\]

Since \(v\) and \(w\) were arbitrary, the result follows.

## Theorem 12.8.3.

Let \((M,\mathfrak{g},\mathfrak{A},\mathfrak{D})\) be an oriented regular surface in \(\mathbb{R}^3_{\nu}\), and let \(p\) be a point in \(M\). Let \((U,\varphi)\) be a chart in \(\mathfrak{A}\) at \(p\), let \(\mathcal{H}\) be the corresponding coordinate frame, and let \(q=\varphi^{-1}(p)\). Then:

- (a)
\[
\begin{bmatrix}
\mathfrak{h}_{11}(q)&\mathfrak{h}_{12}(q)\\
\mathfrak{h}_{21}(q)&\mathfrak{h}_{22}(q)
\end{bmatrix}
=
\begin{bmatrix}
\mathfrak{g}_{11}(q)&\mathfrak{g}_{12}(q)\\
\mathfrak{g}_{21}(q)&\mathfrak{g}_{22}(q)
\end{bmatrix}
\begin{bmatrix}
\mathfrak{w}_1^1(q)&\mathfrak{w}_2^1(q)\\
\mathfrak{w}_1^2(q)&\mathfrak{w}_2^2(q)
\end{bmatrix}.
\]

- (b)
\[
[\mathcal{W}_p]_{\mathcal{H}_q}^{\mathcal{H}_q}
=
\begin{bmatrix}
\mathfrak{g}^{11}(q)&\mathfrak{g}^{12}(q)\\
\mathfrak{g}^{21}(q)&\mathfrak{g}^{22}(q)
\end{bmatrix}
\begin{bmatrix}
\mathfrak{h}_{11}(q)&\mathfrak{h}_{12}(q)\\
\mathfrak{h}_{21}(q)&\mathfrak{h}_{22}(q)
\end{bmatrix}.
\]

**Proof.** (a): Let \(\mathcal{H}=(H_1,H_2)\). We have

\[
\begin{aligned}
\mathfrak{h}_{ij}(q)
&=\langle\mathcal{W}_p(H_i|_q),H_j|_q\rangle & &[(12.8.6),\ (12.8.8)]\\
&=\langle H_i|_q,\mathcal{W}_p(H_j|_q)\rangle & &[\text{Th 12.8.2(b)}]\\
&=\left\langle H_i|_q,\sum_k\mathfrak{w}_j^k(q)H_k|_q\right\rangle & &[(12.8.4)]\\
&=\sum_k\mathfrak{g}_{ik}(q)\mathfrak{w}_j^k(q),
\end{aligned}
\]

from which the result follows.

**(b):** This follows from (12.8.5) and part (a).

Let \((M,\mathfrak{g},\mathfrak{A},\mathfrak{D})\) be an oriented regular surface in \(\mathbb{R}^3_{\nu}\). The Gauss curvature is the smooth function

\[
\mathcal{K}: M\longrightarrow\mathbb{R}
\]

defined by

\[
\mathcal{K}(p)=\epsilon_M\det(\mathcal{W}_p)\qquad(12.8.12)
\]

for all \(p\) in \(M\). An intuitively appealing justification for this definition is provided below. For the moment, we simply observe that from (12.8.1), \(\mathcal{W}_p\) is defined in terms of \(d_p(\mathcal{N})\), which is related to the “rate of change” of the unit normal vector field \(\mathcal{N}\) at \(p\). In geometric terms, the greater the rate of change of \(\mathcal{N}\), the greater the “curvature” we expect \(M\) to have at \(p\).

It follows from Theorem 4.7.4 and Theorem 12.8.2(b) that \(\mathcal{W}_p\) has two (not necessarily distinct) real eigenvalues, which we denote by \(\kappa_1(p)\) and \(\kappa_2(p)\).

## Theorem 12.8.4.

Let \((M,\mathfrak{g},\mathfrak{A},\mathfrak{D})\) be an oriented regular surface in \(\mathbb{R}^3_{\nu}\), and let \(p\) be a point in \(M\). Let \((U,\varphi)\) be a chart in \(\mathfrak{A}\) at \(p\), let \(\mathcal{H}\) be the corresponding coordinate frame, and let \(q=\varphi^{-1}(p)\). Then

\[
\mathcal{K}(p)=\epsilon_M\frac{\det(\mathfrak{h}_{\mathcal{H}}(q))}{\det(\mathfrak{g}_{\mathcal{H}}(q))}=\epsilon_M\,\kappa_1(p)\kappa_2(p).
\]

**Proof.** By Theorem 4.7.5,

\[
\det(\mathcal{W}_p)=\kappa_1(p)\kappa_2(p),
\]

and by Theorem 12.8.3(b),

\[
\det(\mathcal{W}_p)=\frac{\det(\mathfrak{h}_{\mathcal{H}}(q))}{\det(\mathfrak{g}_{\mathcal{H}}(q))}.
\]

The result now follows from (12.8.12).

The next result uses material on “local diffeomorphisms” from Section 14.6 and “area” from Section 19.10. It is included here because it provides a rationale for the definition of Gauss curvature when \(\nu=0\).

## Theorem 12.8.5.

Let \((M,\mathfrak{g},\mathfrak{A},\mathfrak{D})\) be an oriented regular surface in \(\mathbb{R}^3_{0}\), let \(\mathcal{N}\) be the Gauss map, and let \(p\) be a point in \(M\). Let \((U,\varphi)\) be a chart in \(\mathfrak{A}\) at \(p\), let \(q=\varphi^{-1}(p)\), and let \(\varepsilon>0\) be a real number small enough that the open disk \(B_{\varepsilon}=B_{\varepsilon}(q)\) is contained in \(U\). If \(\mathcal{K}(p)\neq0\), then

\[
|\mathcal{K}(p)|=\lim_{\varepsilon\to0}\frac{\operatorname{area}(\mathcal{N}\circ\varphi(B_{\varepsilon}))}{\operatorname{area}(\varphi(B_{\varepsilon}))}.
\]

**Proof.** By Theorem 11.2.10, \((B_{\varepsilon},\varphi|_{B_{\varepsilon}})\) is a chart on \(M\). Let \((H_1,H_2)\) be the coordinate frame corresponding to \((U,\varphi)\). Since \(\mathcal{K}(p)\neq0\), we have from (12.8.1) and (12.8.12) that \(\det(d_p(\mathcal{N}))\neq0\), and then from Theorem 2.5.3 that \(d_p(\mathcal{N})\) is a linear isomorphism. Thus, \(\mathcal{N}\) is an immersion at \(p\). Since \(M\) and \(\mathcal{S}^2\) are 2-dimensional, by Theorem 14.6.2, \(\mathcal{N}\) is a local diffeomorphism at \(p\). Taking \(\varepsilon\) to be smaller if necessary, it follows that \((B_{\varepsilon},\mathcal{N}\circ\varphi|_{B_{\varepsilon}})\) is a chart on \(\mathcal{S}^2\). See Figure 12.8.2. We have from Example 19.10.3 that

\[
\operatorname{area}(\varphi(B_{\varepsilon}))=\iint_{B_{\varepsilon}}\|H_1\times H_2\|\,dr^1dr^2.
\]

and

\[
\begin{aligned}
\operatorname{area}(\mathcal{N}\circ\varphi(B_{\varepsilon}))
&=\iint_{B_{\varepsilon}}\left\|\frac{\partial(\mathcal{N}\circ\varphi)}{\partial r^1}\times\frac{\partial(\mathcal{N}\circ\varphi)}{\partial r^2}\right\|\,dr^1dr^2\\
&=\iint_{B_{\varepsilon}}\|\mathcal{W}(H_1)\times\mathcal{W}(H_2)\|\,dr^1dr^2 & &[(12.8.10)]\\
&=\iint_{B_{\varepsilon}}|\det(\mathcal{W})|\,\|H_1\times H_2\|\,dr^1dr^2. & &[\text{Th 8.4.7}]
\end{aligned}
\]

It can be shown using Theorem 10.5.6 that

\[
\lim_{\varepsilon\to0}\frac{\operatorname{area}(\mathcal{N}\circ\varphi(B_{\varepsilon}))}{\operatorname{area}(\varphi(B_{\varepsilon}))}
=\lim_{\varepsilon\to0}\frac{\iint_{B_{\varepsilon}}|\det(\mathcal{W})|\,\|H_1\times H_2\|\,dr^1dr^2}{\iint_{B_{\varepsilon}}\|H_1\times H_2\|\,dr^1dr^2}
=|\det(\mathcal{W}_p)|=|\mathcal{K}(p)|.
\]

![](img/d712710a8eb78c0959204afd5dde4c91_313_0.png)

**Figure 12.8.2.** Diagram for Theorem 12.8.5

Figure 12.8.2 provides the geometric intuition for Theorem 12.8.5. Since \(M\) as depicted is highly curved at \(p\), the area of \(\mathcal{N}\circ\varphi(B_{\varepsilon})\) is correspondingly greater than the area of \(\varphi(B_{\varepsilon})\), leading to a larger value of \(|\mathcal{K}(p)|\).

## Example 12.8.6.

The tables below present a summary of the Gauss curvatures of the regular surfaces in \(\mathbb{R}^3_0\) and \(\mathbb{R}^3_1\) detailed in Chapter 13. The notation is explained in the relevant sections of that chapter. A few observations are in order.

In \(\mathbb{R}^3_0\), the plane, cylinder, and cone all have a constant Gauss curvature of \(0\). This is not surprising for the plane, but is perhaps counterintuitive for the
