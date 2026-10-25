
## Theorem 8.2.7. If $(V,\mathfrak{O})$ and $(\widetilde{V},\widetilde{\mathfrak{O}})$ are oriented vector spaces and $A : V \longrightarrow \widetilde{V}$ is a linear isomorphism, then the following are equivalent:

- (a) $A$ is orientation-preserving.
- (b) $\det\left([A]_{\mathcal{H}}^{\widetilde{\mathcal{H}}}\right) > 0$ for some (hence every) basis $\mathcal{H}$ in $\mathfrak{O}$ and some (hence every) basis $\widetilde{\mathcal{H}}$ in $\widetilde{\mathfrak{O}}$.
- (c) $A^*(\widetilde{\varpi})$ is an orientation multicovector on $V$ that induces $\mathfrak{O}$ for some (hence every) orientation multicovector $\widetilde{\varpi}$ on $\widetilde{V}$ that induces $\widetilde{\mathfrak{O}}$.

Remark. It is easily shown that since $A$ is a linear isomorphism, $A^*(\widetilde{\varpi})$ is an orientation multicovector on $V$, so the assertion in part (c) makes sense.

Proof. (a) $\Leftrightarrow$ (b): We have

$A$ is orientation-preserving
$\Leftrightarrow$ $A(\mathcal{H})$ and $\widetilde{\mathcal{H}}$ are consistent for some (hence every) $\mathcal{H}$ in $\mathfrak{O}$ and $\widetilde{\mathcal{H}}$ in $\widetilde{\mathfrak{O}}$
$\Leftrightarrow$ $\det\left([\mathrm{id}_{\widetilde{V}}]_{A(\mathcal{H})}^{\widetilde{\mathcal{H}}}\right) > 0$ for some (hence every) $\mathcal{H}$ in $\mathfrak{O}$ and $\widetilde{\mathcal{H}}$ in $\widetilde{\mathfrak{O}}$
$\Leftrightarrow$ $\det\left([A]_{\mathcal{H}}^{\widetilde{\mathcal{H}}}\right) > 0$ for some (hence every) $\mathcal{H}$ in $\mathfrak{O}$ and $\widetilde{\mathcal{H}}$ in $\widetilde{\mathfrak{O}}$,

where the first equivalence follows from Theorem 8.2.6, and the last equivalence from Theorem 2.2.5(a).

(b) $\Leftrightarrow$ (c): Let $\mathcal{H} = (h_1,\dots,h_m)$ and $\widetilde{\mathcal{H}} = (\widetilde{h}_1,\dots,\widetilde{h}_m)$ be bases in $\mathfrak{O}$ and $\widetilde{\mathfrak{O}}$, respectively, and let $\widetilde{\varpi}$ be an orientation multicovector that induces $\widetilde{\mathfrak{O}}$. We have from Theorem 7.3.3(b) that

$A^*(\widetilde{\varpi})(h_1,\dots,h_m) = \det\left([A]_{\mathcal{H}}^{\widetilde{\mathcal{H}}}\right)\,\widetilde{\varpi}(\widetilde{h}_1,\dots,\widetilde{h}_m).$

By Theorem 8.2.3, $\widetilde{\varpi}(\widetilde{h}_1,\dots,\widetilde{h}_m) > 0$, hence

$A^*(\widetilde{\varpi})$ induces $\mathfrak{O}$
$\Leftrightarrow$ $A^*(\widetilde{\varpi})(h_1,\dots,h_m) > 0$ for some (hence every) $\mathcal{H}$ in $\mathfrak{O}$
$\Leftrightarrow$ $\det\left([A]_{\mathcal{H}}^{\widetilde{\mathcal{H}}}\right)\,\widetilde{\varpi}(\widetilde{h}_1,\dots,\widetilde{h}_m) > 0$ for some (hence every) $\mathcal{H}$ in $\mathfrak{O}$ and $\widetilde{\mathcal{H}}$ in $\widetilde{\mathfrak{O}}$
$\Leftrightarrow$ $\det\left([A]_{\mathcal{H}}^{\widetilde{\mathcal{H}}}\right) > 0$ for some (hence every) $\mathcal{H}$ in $\mathfrak{O}$ and $\widetilde{\mathcal{H}}$ in $\widetilde{\mathfrak{O}}$,

where the first equivalence follows from Theorem 8.2.3.

## Theorem 8.2.8. Let $(V,\mathfrak{O})$ be an oriented vector space, and let $A : V \longrightarrow V$ be a linear isomorphism. Then $A$ is orientation-preserving if and only if $\det(A) > 0$.

Proof. Setting $(V,\mathfrak{O}) = (\widetilde{V},\widetilde{\mathfrak{O}})$ and $\mathcal{H} = \widetilde{\mathcal{H}}$ in Theorem 8.2.7 gives the result.

## 8.3 Orientation of Scalar Product Spaces

In Section 8.2, we considered orientation of vector spaces. We now expand our coverage to include scalar product spaces.

## Theorem 8.3.1. Let $(V,\mathfrak{g})$ be a scalar product space of index $\nu$, let $\mathcal{E} = (e_1,\dots,e_m)$ and $\mathcal{H} = (h_1,\dots,h_m)$ be bases for $V$, with $\mathcal{E}$ orthonormal, and let $(\xi^1,\dots,\xi^m)$ and $(\theta^1,\dots,\theta^m)$ be the corresponding dual bases. Then:

(a)

$\det(\mathfrak{g}_{\mathcal{H}}) = (-1)^\nu (\det([id_V]_{\mathcal{H}}^{\mathcal{E}}))^2$.

(b)

$\xi^1 \wedge \cdots \wedge \xi^m = \pm\sqrt{|\det(\mathfrak{g}_{\mathcal{H}})|}\ \theta^1 \wedge \cdots \wedge \theta^m,$

where the positive (negative) sign is chosen if $\mathcal{E}$ and $\mathcal{H}$ are consistent (not consistent).

(c) If $\mathcal{H}$ is orthonormal, then

$\xi^1 \wedge \cdots \wedge \xi^m = \pm\theta^1 \wedge \cdots \wedge \theta^m,$

where the signs are chosen as in part (b).

Proof. (a): We have from Theorem 3.1.3 that

$\mathfrak{g}_{\mathcal{H}} = ([id_V]_{\mathcal{H}}^{\mathcal{E}})^T \mathfrak{g}_{\mathcal{E}} [id_V]_{\mathcal{H}}^{\mathcal{E}},$

and from (4.2.3) that $\det(\mathfrak{g}_{\mathcal{E}}) = (-1)^\nu$. The result follows.

(b): By Theorem 7.3.3(a),

$\xi^1 \wedge \cdots \wedge \xi^m = \det([id_V]_{\mathcal{H}}^{\mathcal{E}})\ \theta^1 \wedge \cdots \wedge \theta^m,$

and by part (a), $|\det(\mathfrak{g}_{\mathcal{H}})| = (\det([id_V]_{\mathcal{H}}^{\mathcal{E}}))^2$, hence

$\det([id_V]_{\mathcal{H}}^{\mathcal{E}}) = \pm\sqrt{|\det(\mathfrak{g}_{\mathcal{H}})|},$

where the positive (negative) sign is chosen if $\mathcal{H}$ and $\mathcal{E}$ are consistent (not consistent). The result follows.

(c): This follows from (4.2.3) and part (b).

## Theorem 8.3.2. Let $(V,\mathfrak{g})$ be a scalar product space, let $\mathcal{E} = (e_1,\dots,e_m)$ and $\mathcal{F} = (f_1,\dots,f_m)$ be orthonormal bases for $V$, and let $\varpi$ be an orientation multicovector on $V$. Then

$\varpi(e_1,\dots,e_m) = \pm\varpi(f_1,\dots,f_m),$

where the positive (negative) sign is chosen if $\mathcal{E}$ and $\mathcal{F}$ are consistent (not consistent).

**Proof.** Let $(\xi^1,\ldots,\xi^m)$ and $(\varphi^1,\ldots,\varphi^m)$ be the dual bases corresponding to $\mathcal E$ and $\mathcal F$, respectively. We have

$$\varpi(e_1,\ldots,e_m)\,\xi^1\wedge\cdots\wedge\xi^m$$

$$=\varpi(f_1,\ldots,f_m)\,\varphi^1\wedge\cdots\wedge\varphi^m \qquad [\mathrm{Thm\ 7.2.13(b)}]$$

$$=\pm\varpi(f_1,\ldots,f_m)\,\xi^1\wedge\cdots\wedge\xi^m, \qquad [\mathrm{Thm\ 8.3.1(c)}]$$

where the positive (negative) sign is chosen if $\mathcal E$ and $\mathcal F$ are consistent (not consistent). Then Theorem 7.2.9 gives the result.

## Theorem 8.3.3.

*Let $(V,g)$ be a scalar product space, let $\mathcal E=(e_1,\ldots,e_m)$ and $\mathcal F$ be orthonormal bases for $V$, and let $(\varphi^1,\ldots,\varphi^m)$ be the dual basis corresponding to $\mathcal F$. Then:*

$$\det\left([\operatorname{id}_V]_{\mathcal E}^{\mathcal F}\right)=\varphi^1\wedge\cdots\wedge\varphi^m(e_1,\ldots,e_m)=\pm1,$$

where the positive (negative) sign is chosen if $\mathcal E$ and $\mathcal F$ are consistent (not consistent).

**Proof.** The first equality follows from Theorem 7.2.11. Setting $\varpi=\varphi^1\wedge\cdots\wedge\varphi^m$ in Theorem 8.3.2 and using Theorem 7.2.9 gives the second equality.

## Theorem 8.3.4 (Existence of Volume Multicovector).

*If $(V,g,\mathcal D)$ is an oriented scalar product space, then there is a unique orientation multicovector $\Omega_V$ on $V$, called the* **volume multicovector**, *such that: (i) $\Omega_V$ induces $\mathcal D$, and (ii) if $(e_1,\ldots,e_m)$ is an orthonormal basis for $V$ that is positively oriented with respect to $\mathcal D$, then $\Omega_V(e_1,\ldots,e_m)=1$. In fact, if $(f_1,\ldots,f_m)$ is any orthonormal basis for $V$ that is positively oriented with respect to $\mathcal D$ and $(\varphi^1,\ldots,\varphi^m)$ is its dual basis, then $\Omega_V=\varphi^1\wedge\cdots\wedge\varphi^m$.*

**Proof.** *Existence.* By Theorem 7.2.9, $\varphi^1\wedge\cdots\wedge\varphi^m(f_1,\ldots,f_m)=1$, so $\varphi^1\wedge\cdots\wedge\varphi^m$ is an orientation multicovector on $V$. Then $\varphi^1\wedge\cdots\wedge\varphi^m$ induces an orientation of $V$, of which there are two possibilities, namely, $\mathcal D$ and $-\mathcal D$. Since $(f_1,\ldots,f_m)$ is positively oriented with respect to $\mathcal D$ and $\varphi^1\wedge\cdots\wedge\varphi^m(f_1,\ldots,f_m)>0$, by Theorem 8.2.3, $\varphi^1\wedge\cdots\wedge\varphi^m$ induces $\mathcal D$. Since $(e_1,\ldots,e_m)$ are positively oriented with respect to $\mathcal D$, they are consistent. It follows from Theorem 8.3.3 that $\varphi^1\wedge\cdots\wedge\varphi^m(e_1,\ldots,e_m)=1$.

*Uniqueness.* Suppose $\varpi$ is an orientation multicovector on $V$ such that $\varpi(f_1,\ldots,f_m)=1$. It follows from Theorem 7.2.13(b) that

$$\varpi=\varpi(f_1,\ldots,f_m)\,\varphi^1\wedge\cdots\wedge\varphi^m=\varphi^1\wedge\cdots\wedge\varphi^m.$$

## Theorem 8.3.5 (Expression for Volume Multicovector).

*Let $(V,g,\mathcal D)$ be an oriented scalar product space, let $\mathcal H$ be a basis for $V$ that is positively oriented with respect to $\mathcal D$, and let $(\theta^1,\ldots,\theta^m)$ be its dual basis. Then*

$$\Omega_V=\sqrt{|\det(g_{\mathcal H})|}\,\theta^1\wedge\cdots\wedge\theta^m.$$
