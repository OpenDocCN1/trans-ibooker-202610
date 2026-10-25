# Theorem 8.5.2. Let $(V,\mathfrak{g},\mathfrak{O})$ be an oriented scalar product space, let $(e_1,\dots,e_m)$ be an orthonormal basis for $V$ that is positively oriented with respect to $\mathfrak{O}$, let $(\xi^1,\dots,\xi^m)$ be its dual basis, and let $I$ be a multi-index in $\mathcal{I}_{s,m}$. Then

$\star(\xi^I) = \operatorname{sgn}(\sigma_{(I,I^c)})\langle\xi^{I^c},\xi^{I^c}\rangle^{\Lambda}\xi^{I^c}$,

where $\sigma_{(I,I^c)}$ is the permutation defined in Section B.2.

Proof. We have from Theorem 7.5.3(b) that $(\xi^I : I \in \mathcal{I}_{s,m})$ is an orthonormal basis for $(\Lambda^s(V),\mathfrak{g}^{\Lambda})$, and then from Theorem 4.2.7 that

$\star(\xi^I) = \sum_{J \in \mathcal{I}_{m-s,m}} \langle\xi^J,\xi^J\rangle^{\Lambda}\langle\star(\xi^I),\xi^J\rangle^{\Lambda}\xi^J. \quad (8.5.3)$

According to Theorem 8.5.1,

$\langle\star(\xi^I),\xi^J\rangle^{\Lambda}\Omega_V = \xi^I \wedge \xi^J.$

For $J = I^c$, we have

$\xi^I \wedge \xi^{I^c} = \operatorname{sgn}(\sigma_{(I,I^c)})\xi^1 \wedge \cdots \wedge \xi^m \quad [\text{Th 7.2.10}]$

$= \operatorname{sgn}(\sigma_{(I,I^c)})\Omega_V, \quad [\text{Th 8.3.4}]$

hence $\langle\star(\xi^I),\xi^{I^c}\rangle^{\Lambda}\Omega_V = \operatorname{sgn}(\sigma_{(I,I^c)})\Omega_V.$

Since $(\Omega_V)$ is a basis for $\Lambda^m(V)$, $\langle\star(\xi^I),\xi^{I^c}\rangle^{\Lambda} = \operatorname{sgn}(\sigma_{(I,I^c)}).$

On the other hand, for $J \neq I^c$, we have from Theorem 7.2.6 that $\xi^I \wedge \xi^J = 0.$ In summary, $\langle\star(\xi^I),\xi^J\rangle^{\Lambda} = \begin{cases}\operatorname{sgn}(\sigma_{(I,I^c)}) & \text{if } J = I^c \\ 0 & \text{if } J \neq I^c.\end{cases}$

Substituting into (8.5.3) gives the result.

Theorem 8.5.2 provides a way to compute with $\star$ on an orthonormal basis for $\Lambda^s(V)$. Computations are then extended to all of $\Lambda^s(V)$ by the linearity of $\star$.

# Theorem 8.5.3. Let $(V,\mathfrak{g},\mathfrak{O})$ be an oriented scalar product space of dimension $m$ and index $\nu$, let $\Omega_V$ be its volume multicovector, and let $\eta,\zeta$ be multicovectors in $\Lambda^s(V)$. Then:

+   (a) $\star(1) = (-1)^{\nu}\Omega_V$.
+   (b) $\star(\Omega_V) = 1$.
+   (c) $\star^2(\eta) = (-1)^{s(m-s)+\nu}\eta$.
+   (d) $\eta \wedge \star(\zeta) = (-1)^{\nu}\langle\eta,\zeta\rangle^{\Lambda}\Omega_V.$