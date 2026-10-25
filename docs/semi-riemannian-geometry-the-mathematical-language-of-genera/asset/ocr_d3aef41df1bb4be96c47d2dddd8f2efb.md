be a **subspace** of $V$, and an $F$-module homomorphism (isomorphism) is called an **$F$-linear map ($F$-linear isomorphism)**.

Let $W$ be a vector space over $F$. In the notation of Section B.5, we have the $F$-vector spaces $\mathrm{Mult}_F(V_1 \times \cdots \times V_m, W)$, $\mathrm{Mult}_F(V^m, W)$, and $\mathrm{Lin}_F(V, W)$. For brevity, when $F = \mathbb{R}$, we drop the subscript $\mathbb{R}$ from the notation and write $\mathrm{Mult}(V_1 \times \cdots \times V_m, W)$, $\mathrm{Mult}(V^m, W)$, and $\mathrm{Lin}(V, W)$. In particular, when $F = W = \mathbb{R}$, we have $\mathrm{Mult}(V_1 \times \cdots \times V_m, \mathbb{R})$, $\mathrm{Mult}(V^m, \mathbb{R})$, and $\mathrm{Lin}(V, \mathbb{R})$.

# B.7 Lie Algebras

Let $R$ be a commutative ring with identity. A **Lie algebra over $R$** is a 4-tuple $(V, +, \circ, [\cdot, \cdot])$, where $(V, +, \circ)$ is an $R$-module and $[\cdot, \cdot] : V \times V \longrightarrow V$ is a map, called **bracket**, such that for all $u, v, w$ in $V$ and all $c$ in $R$:

+   [L1] Bracket is **$R$-bilinear**:
+   
+   $[cu + v, w] = c[u, w] + [v, w]$
+   
+   $[u, cv + w] = c[u, v] + [u, w]$.

+   [L2] Bracket is **alternating**:
+   
+   $[v, w] = -[w, v]$.

+   [L3] Bracket satisfies **Jacobi's identity**:
+   
+   $[u, [v, w]] + [v, [w, u]] + [w, [u, v]] = 0$.

As an example, let $R$ be a (not necessarily commutative) ring with identity and define a map $[\cdot, \cdot] : R \times R \longrightarrow R$ by $[a, b] = ab - ba$ for all $a, b$ in $R$. Then $(R, +, \circ, [\cdot, \cdot])$ is a Lie algebra over $R$.