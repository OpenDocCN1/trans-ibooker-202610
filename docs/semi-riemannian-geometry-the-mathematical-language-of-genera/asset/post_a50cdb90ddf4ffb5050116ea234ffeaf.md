
By Theorem 8.2.2,

\[
\varpi_{\alpha}\big|_{p}\left(\left.\frac{\partial}{\partial x_{\beta}^{1}}\right|_{p},\ldots,\left.\frac{\partial}{\partial x_{\beta}^{m}}\right|_{p}\right)>0
\]

for all \(\alpha\), hence

\[
\Omega_{p}\left(\left.\frac{\partial}{\partial x_{\beta}^{1}}\right|_{p},\ldots,\left.\frac{\partial}{\partial x_{\beta}^{m}}\right|_{p}\right)>0.
\]

Since \(p\) was arbitrary, we have from the preceding inequality that \(\Omega\) is nowhere-vanishing and is therefore an orientation form. Since \((U_{\beta},x_{\beta}^{i})\) was arbitrary, it also follows from the preceding inequality and Theorem 16.6.3 that \(\Omega\) induces \(\mathfrak{D}\).

### Theorem 16.6.5 (Orientability Criterion for Smooth Manifolds).

A smooth manifold is orientable if and only if it has an orientation form.

**Proof.** (⇒): This follows from Theorem 16.6.4.  
(⇐): This follows from Theorem 16.6.3.

### Theorem 16.6.6.

Let \((r^{1},\ldots,r^{m})\) be standard coordinates on \(\mathbb{R}^{m}\), and let \((d(r^{1}),\ldots,d(r^{m}))\) be the corresponding standard dual coordinate frame. Then \(d(r^{1})\wedge\cdots\wedge d(r^{m})\) induces the standard orientation of \(\mathbb{R}^{m}\).

**Proof.** According to Example 15.1.6, we have the identification \((\partial/\partial r^{1},\ldots,\partial/\partial r^{m})=(e_{1},\ldots,e_{m})\), where \((e_{1},\ldots,e_{m})\) is the standard basis for \(\mathbb{R}^{m}\). By Theorem 15.8.4, \(d(r^{1})\wedge\cdots\wedge d(r^{m})(e_{1},\ldots,e_{m})=1\). Thus, \(d(r^{1})\wedge\cdots\wedge d(r^{m})\) is an orientation form on \(\mathbb{R}^{m}\). By Theorem 16.6.3, \([(e_{1},\ldots,e_{m})]\) is in the orientation of \(\mathbb{R}^{m}\) induced by \(d(r^{1})\wedge\cdots\wedge d(r^{m})\).

### Theorem 16.6.7.

Let \(M\) be an orientable smooth manifold, let \(\{[\varpi]\}\) be the set of equivalence classes of orientation forms on \(M\), and let \(\{\mathfrak{D}\}\) be the set of orientations of \(M\). Then the map \(\iota:\{[\varpi]\}\longrightarrow\{\mathfrak{D}\}\) defined by assigning \([\varpi]\) to the orientation induced by \(\varpi\) is bijective.

It was remarked in the introduction to this section that if a smooth manifold has an orientation, then it has at least two of them. The next result gives a condition under which there are two orientations, and no more.

### Theorem 16.6.8.

A connected orientable smooth manifold has precisely two orientations.

**Proof.** Let \(M\) be a connected orientable smooth manifold. By Theorem 16.6.5, \(M\) has an orientation form \(\varpi\). Each orientation form in \([\varpi]\) induces the same orientation, and each orientation form in \([-\varpi]\) induces its opposite. Let \(\vartheta\) be another orientation form on \(M\). We have from Theorem 16.6.1(a) that there is a uniquely determined nowhere-vanishing function \(f\in C^{\infty}(M)\) such that \(\vartheta=f\varpi\), and then from Theorem 16.6.1(b) that \(\vartheta\) is either in \([\varpi]\) or \([-\varpi]\).

It follows from Theorem 14.1.3 that a smooth manifold has countably many connected components, each of which is an open set in the smooth manifold. Let \(M\) be a smooth manifold that has a finite number of connected components \(C_{1},\ldots,C_{k}\). Suppose \(C_{i}\), viewed as an open submanifold of \(M\), is orientable for \(i=1,\ldots,k\). Then \(C_{i}\) can be given an orientation \(\mathfrak{D}_{i}\) independently of the other connected components. Taken together, the \(\mathfrak{D}_{i}\) give an orientation of \(M\), which we denote by \((\mathfrak{D}_{1},\ldots,\mathfrak{D}_{k})\). It follows from Theorem 16.6.8 that by taking all combinations of signs in \((\pm\mathfrak{D}_{1},\ldots,\pm\mathfrak{D}_{k})\) we obtain the \(2^{k}\) possible orientations of \(M\).

Let \((\overline{M},\overline{\mathfrak{D}})\) be an oriented smooth \(\bar{m}\)-manifold, where \(\bar{m}\ge 2\), and let \(M\) be a submanifold of \(\overline{M}\). In general, there is no mechanism for constructing an orientation of \(M\) from \(\overline{\mathfrak{D}}\). However, when \(M\) is a hypersurface of \(\overline{M}\), this may be possible, as the next result shows.

### Theorem 16.6.9.

Let \((\overline{M},\overline{\mathfrak{D}})\) be an oriented smooth \(\bar{m}\)-manifold, where \(\bar{m}\ge 2\), let \(M\) be a hypersurface of \(\overline{M}\), and suppose there is a nowhere-tangent vector field \(V\) in \(\widetilde{\mathfrak X}_{M}(M)\). Then:

- (a) There is a unique orientation \(\mathfrak{D}_{M}\) of \(M\), called the orientation induced by \(V\), such that for all points \(p\in M\), \((v_{1},\ldots,v_{\bar{m}-1})\) is a basis for \(T_{p}(M)\) that is positively oriented with respect to \(\mathfrak{D}_{M}(p)\) if and only if \((V_{p},v_{1},\ldots,v_{\bar{m}-1})\) is a basis for \(T_{p}(\overline{M})\) that is positively oriented with respect to \(\overline{\mathfrak{D}}(p)\).

- (b) If \(\varpi\) is an orientation form on \(\overline{M}\) that induces \(\overline{\mathfrak{D}}\), then \(i_{V}(\varpi)|_{M}\) is an orientation form on \(M\) that induces \(\mathfrak{D}_{M}\).

- (c) \(\mathfrak{D}_{M}\) is independent of the choice of orientation form on \(\overline{M}\) that induces \(\overline{\mathfrak{D}}\).

**Proof.** This is the manifold version of Theorem 8.2.5, but we provide a detailed proof.

(a), (b): Let \(\varpi\) be an orientation form on \(\overline{M}\) that induces \(\overline{\mathfrak{D}}\). According to Theorem 16.6.3, by definition, \(\varpi_{q}\) is an orientation multicovector on \(T_{q}(\overline{M})\) that induces \(\overline{\mathfrak{D}}(q)\) for all \(q\in\overline{M}\). It follows from Theorem 8.2.5(b) that \((i_{V}(\varpi)|_{M})|_{p}=i_{V_{p}}(\varpi_{p})|_{T_{p}(M)}\) is an orientation multicovector on \(T_{p}(M)\) for all \(p\in M\). Thus, \(i_{V}(\varpi)|_{M}\) is a nowhere-vanishing form on \(M\). For vector fields \(X_{1},\ldots,X_{\bar{m}-1}\in\mathfrak X(M)\), we have

\[
i_{V}(\varpi)|_{M}(X_{1},\ldots,X_{\bar{m}-1})=\varpi(V,X_{1},\ldots,X_{\bar{m}-1}).
\]

Since \(\varpi\) is smooth and \(X_{1},\ldots,X_{\bar{m}-1}\) were arbitrary, by definition, \(i_{V}(\varpi)|_{M}\) is smooth. This shows that \(i_{V}(\varpi)|_{M}\) is an orientation form on \(M\). It follows from Theorem 8.2.5(a) and Theorem 16.6.3 that the orientation induced by \(i_{V}(\varpi)|_{M}\) is the one described in the statement of part (a).

(c): This follows from part (a).

A couple of remarks on Theorem 16.6.9 are in order. First, although \(\mathfrak{D}_{M}\) is independent of the choice of orientation form \(\varpi\) on \(\overline{M}\) that induces \(\overline{\mathfrak{D}}\), the same cannot be said for the choice of nowhere-tangent vector field \(V\) in \(\widetilde{\mathfrak X}_{M}(M)\). In particular, the orientation induced by \(-V\) is \(-\mathfrak{D}_{M}\). Second, although \(M\) satisfies the hypotheses above, there is no guarantee that a nowhere-tangent vector field in \(\widetilde{\mathfrak X}_{M}(M)\) exists in the first place, an issue we return to in Theorem 19.11.1.

Let \((M,\mathfrak{D})\) and \((\overline{M},\overline{\mathfrak{D}})\) be oriented smooth \(m\)-manifolds, and let \(F:M\longrightarrow\overline{M}\) be a diffeomorphism. We say that \(F\) is orientation-preserving if \(d_{p}(F):T_{p}(M)\longrightarrow T_{F(p)}(\overline{M})\) is orientation-preserving as a linear map for all \(p\in M\).

### Theorem 16.6.10.

Let \(U\) and \(V\) be open sets in \(\mathbb{R}^{m}\), and let \(F:U\longrightarrow V\) be a diffeomorphism. Then \(F\) is orientation-preserving if and only if \(\det(J_{F}(p))>0\) for all \(p\in U\), where \(J_{F}(p)\) is the Jacobian matrix of \(F\) at \(p\).

**Proof.** Let \(\mathcal E\) be the standard basis for \(\mathbb{R}^{m}\). From (10.1.3), we have \([d_{p}(F)]_{\mathcal E}^{\mathcal E}=J_{F}(p)\), hence \(\det(d_{p}(F))=\det(J_{F}(p))\). The result now follows from Theorem 8.2.8.

## 16.7 Integration of Differential Forms

In this section, we introduce the theory of integration on smooth manifolds. It may come as a surprise to learn that in this setting the objects to be integrated are not functions but rather differential forms. In order for integration of functions to be possible, we need a type of smooth manifold that has structure beyond what has been considered up to now. This topic is covered in Section 19.10.

Let \(M\) be a smooth \(m\)-manifold, and let \(\omega\) be a form in \(\Lambda^{m}(M)\). The **support of \(\omega\)** is denoted by \(\operatorname{supp}(\omega)\) and defined to be the closure in \(M\) of the set of points at which \(\omega\) is nonvanishing:

\[
\operatorname{supp}(\omega)=\operatorname{cl}_{M}(\{p\in M:\omega_{p}\ne 0\}).
\]

We say that \(\omega\) has **compact support** if \(\operatorname{supp}(\omega)\) is compact in \(M\). Let \(U\) be an open set in \(M\). We say that \(\omega\) has **support in \(U\)** if \(\operatorname{supp}(\omega)\subseteq U\), and that \(\omega\) has **compact support in \(U\)** if \(\operatorname{supp}(\omega)\subseteq U\) and \(\operatorname{supp}(\omega)\) is compact in \(M\).

We begin by defining the integral of differential forms in the setting of Euclidean spaces and then generalize to smooth manifolds.

Let \(V\) be an open set in \(\mathbb{R}^{m}\), and let \(\omega\) be a form in \(\Lambda^{m}(V)\) that has compact support. By Theorem 15.8.5, \(\omega\) can be expressed as

\[
\omega=f\,d(r^{1})\wedge\cdots\wedge d(r^{m}), \tag{16.7.1}
\]

It can be shown that \(\int_{M}\omega\) does not depend on the choice of open cover or the choice of partition of unity.

Integration of differential forms has the properties desired of an integral.

### Theorem 16.7.2.

Let \((M,\mathcal D)\) be an oriented smooth \(m\)-manifold, let \(\omega,\xi\) be forms in \(\Lambda^{m}(M)\) that have compact support, and let \(c\) be a real number. Then:

- (a) \(\displaystyle\int_{M}(c\omega+\xi)=c\int_{M}\omega+\int_{M}\xi.\)

- (b) \(\displaystyle\int_{-M}\omega=-\int_{M}\omega,\) where \(-M\) denotes \((M,-\mathcal D)\).

## 16.8 Line Integrals

Due to its endpoints, a closed interval \([a,b]\) in \(\mathbb{R}\) is not a smooth 1-manifold. However, as discussed in Section 17.1, \([a,b]\) is what we will later refer to as a smooth 1-manifold with boundary. For present purposes, it is convenient to ignore the distinction and treat \([a,b]\) as a smooth 1-manifold.

Let \(M\) be a smooth manifold, and let \(\lambda(t):[a,b]\longrightarrow M\) be a (not necessarily smooth) curve. We say that \(\lambda\) is piecewise smooth if there is a finite sequence of real numbers \(a=a_{0}<a_{1}<a_{2}<\cdots<a_{k}=b\) such that the restriction of \(\lambda\) to \([a_{i},a_{i+1}]\) is a smooth curve for \(i=0,\ldots,k-1\). Suppose \(\lambda\) is in fact piecewise smooth, and let \(\omega\) be a covector field in \(\mathfrak X^{*}(M)\). Motivated by (16.7.3), the line integral of \(\omega\) over \(\lambda\) is denoted by \(\int_{\lambda}\omega\) and defined by

\[
\int_{\lambda}\omega=\sum_{i=0}^{k-1}\int_{[a_{i},a_{i+1}]}\lambda^{*}(\omega), \tag{16.8.1}
\]

where the \(i\)th integrand on the right-hand side is interpreted as the restriction of \(\lambda^{*}(\omega)\) to \([a_{i},a_{i+1}]\). When \(\lambda\) is smooth, (16.8.1) simplifies to

\[
\int_{\lambda}\omega=\int_{[a,b]}\lambda^{*}(\omega). \tag{16.8.2}
\]

Clearly, \([a_{i},a_{i+1}]\) is a domain of integration in \(\mathbb{R}\), and by Theorem 9.4.2, it is compact for \(i=0,\ldots,k-1\). It follows from Theorem 15.11.8, (16.7.2), and (16.8.1) that

\[
\int_{\lambda}\omega=\sum_{i=0}^{k-1}\int_{a_{i}}^{a_{i+1}}\omega_{\lambda(t)}\left(\frac{d\lambda}{dt}(t)\right)\,dt. \tag{16.8.3}
\]

When \(\lambda\) is smooth, (16.8.3) simplifies to

\[
\int_{\lambda}\omega=\int_{a}^{b}\omega_{\lambda(t)}\left(\frac{d\lambda}{dt}(t)\right)\,dt. \tag{16.8.4}
\]
