
![](img/d712710a8eb78c0959204afd5dde4c91_547_0.png)

**Figure 19.5.1.** Commutative diagram for Theorem 19.5.3

Our efforts in this section, and much of what we previously developed in the area of “representations”, have ultimately been directed at obtaining the following result. It provides the long-awaited justification for using the term “tensor” when referring to the torsion tensor and curvature tensor.

**Theorem 19.5.5 (Representation of $T$ and $R$).** Let $(M,g)$ be a semi-Riemannian manifold, and let $T$ and $R$ be the torsion tensor and curvature tensor, respectively. Then:

- **(a)** $T$ is a map in $\mathrm{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^2,\mathfrak{X}(M))$ and can be identified with a tensor field in $\mathcal{T}^1_2(M)$.

- **(b)** $R$ is a map in $\mathrm{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^3,\mathfrak{X}(M))$ and can be identified with a tensor field in $\mathcal{T}^1_3(M)$.

*Proof.* (a): This follows from Theorem 18.6.2(b) and Theorem 19.5.4.

(b): This follows from Theorem 18.7.1(b) and Theorem 19.5.4. □

## 19.6 Contraction of Tensor Fields

The contraction of tensor fields on smooth manifolds was considered in Section 15.14. We now return to this topic armed with the properties of semi-Riemannian manifolds.

**Theorem 19.6.1 (Orthonormal Frame Expression for Ordinary Contraction).** Let $(M,g)$ be a semi-Riemannian $m$-manifold with signature $(\varepsilon_1,\ldots,\varepsilon_m)$, let $(E_1,\ldots,E_m)$ be an orthonormal frame on an open set $U$ in $M$, and let $\mathcal{A}$ be a tensor field in $\mathcal{T}^1_s(M)$. Then:

- **(a)** For integers $s \ge 2$ and $1 \le l \le s-1$, the tensor field $\mathrm{C}_l^1(\mathcal{A})$ in $\mathcal{T}_{s-1}^0(M)$ is given on $U$ by
  $$
  \mathrm{C}_l^1(\mathcal{A})(X_1,\ldots,X_{s-1})
  = \sum_i \varepsilon_i \left\langle \mathfrak{R}_s^{-1}\circ \mathrm{C}_s^1(\mathcal{A})(X_1,\ldots,X_{l-1},E_i,X_l,\ldots,X_{s-1}), E_i\right\rangle,
  $$
  for all vector fields $X_1,\ldots,X_{s-1}$ in $\mathfrak{X}(M)$, where $\mathfrak{R}_s$ and $\mathrm{C}_s^1$ are given by (19.5.1) and (15.7.1), respectively.

- **(b)** For $s = 1$, the function $\mathrm{C}_1^1(\mathcal{A})$ in $C^\infty(M)$ is given by
  $$
  \mathrm{C}_1^1(\mathcal{A}) = \sum_i \varepsilon_i \left\langle \mathfrak{R}^{-1}\circ \mathrm{C}_1^1(\mathcal{A})(E_i), E_i\right\rangle,
  $$
  where $\mathfrak{R}$ is given by (19.5.2).

**Remark.** According to Figure 19.5.1, $\mathfrak{R}_s^{-1}\circ \mathrm{C}_s^1(\mathcal{A})$ is in $\mathrm{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))$, so the scalar product in part (a) makes sense.

*Proof.* This is the manifold version of Theorem 6.1.1.

Let $(M,g)$ be a semi-Riemannian manifold. Corresponding to the $(k,l)$-flat map $F_l^k$ and the $(k,l)$-sharp map $S_l^k$ introduced in Section 19.4, we now define manifold versions of the metric contraction maps described in Section 6.5.

For integers $r \ge 2$, $s \ge 0$, and $1 \le k < l \le r$, and motivated by (6.5.1), the $(k,l)$-contravariant metric contraction is the map

$$
C^{kl} : \mathcal{T}_s^r(M) \longrightarrow \mathcal{T}_s^{r-2}(M)
$$

defined by

$$
C^{kl} = \mathrm{C}_1^k \circ F_1^l. \tag{19.6.1}
$$

Corresponding to (6.5.2), we have

$$
C^{k,k+1} = \mathrm{C}_1^k \circ F_1^k.
$$

For integers $r \ge 0$, $s \ge 2$, and $1 \le k < l \le s$, and motivated by (6.5.3), the $(k,l)$-covariant metric contraction is the map

$$
C_{kl} : \mathcal{T}_s^r(M) \longrightarrow \mathcal{T}_{s-2}^r(M)
$$

defined by

$$
C_{kl} = \mathrm{C}_k^1 \circ S_l^1. \tag{19.6.2}
$$

**Theorem 19.6.2 (Orthonormal Frame Expression for Metric Contraction).** Let $(M,\mathfrak{g})$ be a semi-Riemannian manifold with signature $(\varepsilon_1,\dots,\varepsilon_m)$, let $(E_1,\dots,E_m)$ be an orthonormal frame on an open set $U$ in $M$, and let $\mathcal{A}$ be a tensor field in $\mathcal{T}^0_s(M)$. Then:

- **(a)** For integers $s \ge 3$ and $1 \le k < l \le s$, the tensor field $\mathrm{C}_{kl}(\mathcal{A})$ in $\mathcal{T}^0_{s-2}(M)$ is given on $U$ by
  $$
  \mathrm{C}_{kl}(\mathcal{A})(X_1,\dots,X_{s-2})
  = \sum_i \varepsilon_i \mathcal{A}(X_1,\dots,X_{k-1},E_i,X_k,\dots,X_{l-2},E_i,X_{l-1},\dots,X_{s-2})
  $$
  for all vector fields $X_1,\dots,X_{s-2}$ in $\mathfrak{X}(M)$.

- **(b)** For $s = 2$, the function $\mathrm{C}_{12}(\mathcal{A})$ in $C^\infty(M)$ is given by
  $$
  \mathrm{C}_{12}(\mathcal{A}) = \sum_i \varepsilon_i \mathcal{A}(E_i,E_i).
  $$

**Remark.** Observe that in part (a), the $E_i$ appear in the $k$th and $l$th positions.

*Proof.* This is the manifold version of Theorem 6.5.1.

**Theorem 19.6.3.** If $(M,\mathfrak{g})$ is a semi-Riemannian manifold, then covariant derivatives and total covariant derivatives commute with flat maps, sharp maps, ordinary contractions, and metric contractions.

*Proof.* We illustrate the proof with a few representative cases. Let $X$ be a vector field in $\mathfrak{X}(M)$, and let $\mathcal{A}$ be a tensor field in $\mathcal{T}^r_s(M)$. Since $\nabla_X$ is a tensor derivation, property [D2] in Section 16.2 gives

$$
\nabla_X \circ \mathrm{C}^k_l = \mathrm{C}^k_l \circ \nabla_X. \tag{19.6.4}
$$

We have

$$
\begin{aligned}
\nabla_X \circ \mathrm{F}^k_1(\mathcal{A})
&= \nabla_X\left(\mathrm{C}^k_1(\mathfrak{g}\otimes\mathcal{A})\right) && \text{[mv Th 6.2.6]}\\
&= \mathrm{C}^k_1\left(\nabla_X(\mathfrak{g}\otimes\mathcal{A})\right) && \text{[(19.6.4)]}\\
&= \mathrm{C}^k_1\left(\mathfrak{g}\otimes\nabla_X(\mathcal{A})\right) && \text{[Th 19.3.8]}\\
&= \mathrm{F}^k_1\left(\nabla_X(\mathcal{A})\right) && \text{[mv Th 6.2.6]}\\
&= \mathrm{F}^k_1 \circ \nabla_X(\mathcal{A}),
\end{aligned}
$$

## 19.7 Isometries

Linear isometries on scalar product spaces were introduced in Section 4.4. We now describe their counterpart for semi-Riemannian manifolds.

Let $(M,\mathfrak{g})$ and $(\widetilde{M},\widetilde{\mathfrak{g}})$ be semi-Riemannian manifolds with the same dimension, and let $F,G : M \longrightarrow \widetilde{M}$ be smooth maps. We say that $F$ is an **isometry**, and that $M$ and $\widetilde{M}$ are **isometric**, if $F$ is a diffeomorphism and $F^*(\widetilde{\mathfrak{g}})=\mathfrak{g}$, where, by definition, the latter condition is equivalent to $F^*(\widetilde{\mathfrak{g}})_p = \mathfrak{g}_p$ for all $p$ in $M$. We say that $G$ is a **local isometry**, and that $M$ is **locally isometric** to $\widetilde{M}$, if for every point $p$ in $M$, there is a neighborhood $U$ of $p$ in $M$ and a neighborhood $\widetilde{U}$ of $G(p)$ in $\widetilde{M}$ such that $G|_U : U \longrightarrow \widetilde{U}$ is an isometry. We have from Theorem 19.1.2 that $(U,\mathfrak{g}|_U)$ and $(\widetilde{U},\widetilde{\mathfrak{g}}|_{\widetilde{U}})$ are semi-Riemannian manifolds with the same dimension and index, so the preceding definition makes sense. Since every diffeomorphism is a local diffeomorphism, it follows that every isometry is a local isometry.

**Theorem 19.7.1.** Let $(M,\mathfrak{g})$ and $(\widetilde{M},\widetilde{\mathfrak{g}})$ be semi-Riemannian manifolds with the same dimension, and let $F : M \longrightarrow \widetilde{M}$ be a smooth map. Then the following are equivalent:

- **(a)** $F$ is a local isometry.
- **(b)** $F^*(\widetilde{\mathfrak{g}})=\mathfrak{g}$.
- **(c)** The differential map $d_p(F) : T_p(M) \longrightarrow T_{F(p)}(\widetilde{M})$ is a linear isometry for all $p$ in $M$.

*Proof.* (b) $\Leftrightarrow$ (c): For a point $p$ in $M$,

$$
\begin{aligned}
F^*(\widetilde{\mathfrak{g}})_p = \mathfrak{g}_p
&\Leftrightarrow\quad F^*(\widetilde{\mathfrak{g}})_p(v,w) = \mathfrak{g}_p(v,w) \text{ for all } v,w \text{ in } T_p(M)\\
&\Leftrightarrow\quad \widetilde{\mathfrak{g}}_{F(p)}(d_p(F)(v), d_p(F)(w)) = \mathfrak{g}_p(v,w) \text{ for all } v,w \text{ in } T_p(M)\\
&\Leftrightarrow\quad d_p(F) \text{ is a linear isometry,}
\end{aligned}
$$

where the second equivalence follows from (15.12.2). Since $p$ was arbitrary, the result follows.

(a) $\Rightarrow$ (b): Straightforward.

(b) $\Rightarrow$ (a): We have

$$
\begin{aligned}
F^*(\widetilde{\mathfrak{g}})=\mathfrak{g}
&\Rightarrow\quad d_p(F) \text{ is a linear isometry for all } p \text{ in } M \qquad [(b)\Rightarrow(c)]\\
&\Rightarrow\quad d_p(F) \text{ is a linear isomorphism for all } p \text{ in } M \qquad [\text{Th 4.4.1(a)}]\\
&\Rightarrow\quad d_p(F) \text{ is injective for all } p \text{ in } M \qquad [\text{Th 1.1.14}] \quad (19.7.1)\\
&\Rightarrow\quad F \text{ is an immersion} \qquad [\text{by definition}]\\
&\Rightarrow\quad F \text{ is a local diffeomorphism,} \qquad [\text{Th 14.6.2}]
\end{aligned}
$$

from which the result follows.

**Theorem 19.7.2.** Let $(M,\mathfrak{g})$ and $(\widetilde{M},\widetilde{\mathfrak{g}})$ be semi-Riemannian manifolds with the same dimension, and let $F : M \longrightarrow \widetilde{M}$ be a smooth map. Then $F$ is an isometry if and only if it is a bijective local isometry.

*Proof.* ($\Rightarrow$): By definition, a diffeomorphism (hence an isometry) is bijective. As remarked above, an isometry is a local isometry.

($\Leftarrow$): Since $F$ is a local isometry, by Theorem 19.7.1, $F^*(\widetilde{\mathfrak{g}}) = \mathfrak{g}$. It follows from (19.7.1) that $F$ is a local diffeomorphism. Since a bijective local diffeomorphism is a diffeomorphism, $F$ is a diffeomorphism.

Let $(M,\mathfrak{g})$ be a semi-Riemannian $m$-manifold of index $\nu$, let $p$ be a point in $M$, and let $(\widetilde{e}_1,\dots,\widetilde{e}_m)$ be an orthonormal basis for $T_p(M)$. Replacing $\mathbb{R}^m$ with $\mathbb{R}_\nu^m$ in (18.10.1), we obtain the linear isomorphism

$$
\ell_p : \mathbb{R}_\nu^m \longrightarrow T_p(M)
$$

defined by

$$
\ell_p(r^1,\dots,r^m) = \sum_i r^i \widetilde{e}_i. \tag{19.7.2}
$$

Using Theorem 14.1.6 and Theorem 14.2.5, let us now view $\mathbb{R}_\nu^m$ and $(T_p(M),\mathfrak{g}_p)$ as semi-Riemannian $m$-manifolds of index $\nu$, and $\ell_p$ as a diffeomorphism.

**Theorem 19.7.3.** With the above setup, $\ell_p$ is an isometry (between semi-Riemannian $m$-manifolds of index $\nu$).

*Proof.* Let $q$ be a point in $\mathbb{R}_\nu^m$, and make the identifications $T_q(\mathbb{R}_\nu^m)=\mathbb{R}_\nu^m$ and $T_q(T_p(M))=T_p(M)$. Since $\ell_p$ is linear, we have from Theorem 14.4.4 that $d_q(\ell_p)=\ell_p$. Let $(e_1,\dots,e_m)$ be the standard basis for $\mathbb{R}_\nu^m$. Because $\mathbb{R}_\nu^m$ and $T_p(M)$ have the same index, hence the same signature,

$$
\begin{aligned}
\|d_q(\ell_p)(r^1,\dots,r^m)\| &= \|\ell_p(r^1,\dots,r^m)\| = \sqrt{\left|\left\langle\sum_i r^i\widetilde{e}_i,\sum_j r^j\widetilde{e}_j\right\rangle\right|}\\
&= \sqrt{\left|\sum_i \langle\widetilde{e}_i,\widetilde{e}_i\rangle (r^i)^2\right|} = \sqrt{\left|\sum_i \langle e_i,e_i\rangle (r^i)^2\right|}\\
&= \sqrt{\left|\left\langle\sum_i r^i e_i,\sum_j r^j e_j\right\rangle\right|} = \|(r^1,\dots,r^m)\|,
\end{aligned}
$$

so $d_q(\ell_p)$ is a linear isometry for all $q$ in $\mathbb{R}_\nu^m$. By Theorem 19.7.1, $\ell_p$ is a local isometry. Since $\ell_p$ is a diffeomorphism, and therefore bijective, it follows from Theorem 19.7.2 that $\ell_p$ is an isometry.

**Theorem 19.7.4.** Let $(M,\mathfrak{g})$ be a semi-Riemannian manifold, let $p$ be a point in $M$, and let $\psi : (-\varepsilon,\varepsilon) \longrightarrow T_p(M)$ be a smooth curve. Consider the parametrized surface $\sigma : [0,1] \times (-\varepsilon,\varepsilon) \longrightarrow M$ given by

$$
\sigma(r,s) = \exp_p\bigl(r\psi(s)\bigr),
$$
