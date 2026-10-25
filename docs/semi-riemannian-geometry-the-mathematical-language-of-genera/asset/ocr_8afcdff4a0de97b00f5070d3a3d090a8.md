Let $(M,\mathfrak{g},\mathfrak{A},\mathfrak{O})$ be an oriented regular surface in $\mathbb{R}^3_{\nu}$. The Gauss curvature is the smooth function

$\mathcal{K}: M \longrightarrow \mathbb{R}$

defined by

$\mathcal{K}(p)=\epsilon_M \det(\mathcal{W}_p)\qquad(12.8.12)$

for all $p$ in $M$. An intuitively appealing justification for this definition is provided below. For the moment, we simply observe that from $(12.8.1)$, $\mathcal{W}_p$ is defined in terms of $d_p(\mathcal{N})$, which is related to the “rate of change” of the unit normal vector field $\mathcal{N}$ at $p$. In geometric terms, the greater the rate of change of $\mathcal{N}$, the greater the “curvature” we expect $M$ to have at $p$.

It follows from Theorem 4.7.4 and Theorem 12.8.2(b) that $\mathcal{W}_p$ has two (not necessarily distinct) real eigenvalues, which we denote by $\kappa_1(p)$ and $\kappa_2(p)$.

# Theorem 12.8.4. Let $(M,\mathfrak{g},\mathfrak{A},\mathfrak{O})$ be an oriented regular surface in $\mathbb{R}^3_{\nu}$, and let $p$ be a point in $M$. Let $(U,\varphi)$ be a chart in $\mathfrak{A}$ at $p$, let $\mathcal{H}$ be the corresponding coordinate frame, and let $q=\varphi^{-1}(p)$. Then

$\mathcal{K}(p)=\epsilon_M\frac{\det(\mathfrak{h}_{\mathcal{H}}(q))}{\det(\mathfrak{g}_{\mathcal{H}}(q))}=\epsilon_M\,\kappa_1(p)\kappa_2(p).$

Proof. By Theorem 4.7.5,

$\det(\mathcal{W}_p)=\kappa_1(p)\kappa_2(p),$

and by Theorem 12.8.3(b),

$\det(\mathcal{W}_p)=\frac{\det(\mathfrak{h}_{\mathcal{H}}(q))}{\det(\mathfrak{g}_{\mathcal{H}}(q))}.$

The result now follows from (12.8.12).

The next result uses material on “local diffeomorphisms” from Section 14.6 and “area” from Section 19.10. It is included here because it provides a rationale for the definition of Gauss curvature when $\nu=0$.

# Theorem 12.8.5. Let $(M,\mathfrak{g},\mathfrak{A},\mathfrak{O})$ be an oriented regular surface in $\mathbb{R}^3_{0}$, let $\mathcal{N}$ be the Gauss map, and let $p$ be a point in $M$. Let $(U,\varphi)$ be a chart in $\mathfrak{A}$ at $p$, let $q=\varphi^{-1}(p)$, and let $\varepsilon>0$ be a real number small enough that the open disk $B_{\varepsilon}=B_{\varepsilon}(q)$ is contained in $U$. If $\mathcal{K}(p)\neq0$, then

$|\mathcal{K}(p)|=\lim_{\varepsilon\to0}\frac{\operatorname{area}(\mathcal{N}\circ\varphi(B_{\varepsilon}))}{\operatorname{area}(\varphi(B_{\varepsilon}))}.$

Proof. By Theorem 11.2.10, $(B_{\varepsilon},\varphi|_{B_{\varepsilon}})$ is a chart on $M$. Let $(H_1,H_2)$ be the coordinate frame corresponding to $(U,\varphi)$. Since $\mathcal{K}(p)\neq0$, we have from (12.8.1) and (12.8.12) that $\det(d_p(\mathcal{N}))\neq0$, and then from Theorem 2.5.3 that $d_p(\mathcal{N})$ is a linear isomorphism. Thus, $\mathcal{N}$ is an immersion at $p$. Since $M$ and $\mathcal{S}^2$ are