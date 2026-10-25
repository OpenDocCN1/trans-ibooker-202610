# Theorem 19.7.2. Let $(M,\mathfrak{g})$ and $(\widetilde{M},\widetilde{\mathfrak{g}})$ be semi-Riemannian manifolds with the same dimension, and let $F : M \longrightarrow \widetilde{M}$ be a smooth map. Then $F$ is an isometry if and only if it is a bijective local isometry.

Proof. $(\Rightarrow)$: By definition, a diffeomorphism (hence an isometry) is bijective. As remarked above, an isometry is a local isometry.

$(\Leftarrow)$: Since $F$ is a local isometry, by Theorem 19.7.1, $F^*(\widetilde{\mathfrak{g}}) = \mathfrak{g}$. It follows from (19.7.1) that $F$ is a local diffeomorphism. Since a bijective local diffeomorphism is a diffeomorphism, $F$ is a diffeomorphism.

Let $(M,\mathfrak{g})$ be a semi-Riemannian $m$-manifold of index $\nu$, let $p$ be a point in $M$, and let $(\widetilde{e}_1,\dots,\widetilde{e}_m)$ be an orthonormal basis for $T_p(M)$. Replacing $\mathbb{R}^m$ with $\mathbb{R}_\nu^m$ in (18.10.1), we obtain the linear isomorphism

$\ell_p : \mathbb{R}_\nu^m \longrightarrow T_p(M)$

defined by

$\ell_p(r^1,\dots,r^m) = \sum_i r^i \widetilde{e}_i. \qquad (19.7.2)$

Using Theorem 14.1.6 and Theorem 14.2.5, let us now view $\mathbb{R}_\nu^m$ and $(T_p(M),\mathfrak{g}_p)$ as semi-Riemannian $m$-manifolds of index $\nu$, and $\ell_p$ as a diffeomorphism.

# Theorem 19.7.3. With the above setup, $\ell_p$ is an isometry (between semi-Riemannian $m$-manifolds of index $\nu$).

Proof. Let $q$ be a point in $\mathbb{R}_\nu^m$, and make the identifications $T_q(\mathbb{R}_\nu^m)=\mathbb{R}_\nu^m$ and $T_q(T_p(M))=T_p(M)$. Since $\ell_p$ is linear, we have from Theorem 14.4.4 that $d_q(\ell_p)=\ell_p$. Let $(e_1,\dots,e_m)$ be the standard basis for $\mathbb{R}_\nu^m$. Because $\mathbb{R}_\nu^m$ and $T_p(M)$ have the same index, hence the same signature,

$\|d_q(\ell_p)(r^1,\dots,r^m)\| = \|\ell_p(r^1,\dots,r^m)\| = \sqrt{\left|\left\langle\sum_i r^i\widetilde{e}_i,\sum_j r^j\widetilde{e}_j\right\rangle\right|}$

$= \sqrt{\left|\sum_i \langle\widetilde{e}_i,\widetilde{e}_i\rangle (r^i)^2\right|} = \sqrt{\left|\sum_i \langle e_i,e_i\rangle (r^i)^2\right|}$

$= \sqrt{\left|\left\langle\sum_i r^i e_i,\sum_j r^j e_j\right\rangle\right|} = \|(r^1,\dots,r^m)\|,$

so $d_q(\ell_p)$ is a linear isometry for all $q$ in $\mathbb{R}_\nu^m$. By Theorem 19.7.1, $\ell_p$ is a local isometry. Since $\ell_p$ is a diffeomorphism, and therefore bijective, it follows from Theorem 19.7.2 that $\ell_p$ is an isometry.

# Theorem 19.7.4. Let $(M,\mathfrak{g})$ be a semi-Riemannian manifold, let $p$ be a point in $M$, and let $\psi(s) : (-\varepsilon,\varepsilon) \longrightarrow T_p(M)$ be a smooth curve. Consider the parametrized surface $\sigma(r,s) : [0,1] \times (-\varepsilon,\varepsilon) \longrightarrow M$ given by

$\sigma(r,s) = \exp_p\bigl(r\psi(s)\bigr),$