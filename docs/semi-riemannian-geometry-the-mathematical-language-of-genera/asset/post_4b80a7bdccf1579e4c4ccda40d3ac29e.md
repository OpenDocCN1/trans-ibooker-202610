
For \(s \ge 2\) and \(l=s\):

\[
S_s^k(A)(\omega^1,\ldots,\omega^{r+1},X_1,\ldots,X_{s-1})
= A(\omega^1,\ldots,\widehat{\omega^k},\ldots,\omega^{r+1},X_1,\ldots,X_{s-1},\omega^k S).
\]

**Theorem 19.4.3.** With the above setup, \(\Gamma_l^k\) and \(S_l^k\) are \(C^\infty(M)\)-module isomorphisms that are inverses.

*Proof.* It is easily shown that \(\Gamma_l^k\) and \(S_l^k\) are \(C^\infty(M)\)-module homomorphisms. That they are \(C^\infty(M)\)-module isomorphisms and inverses follows from the manifold version of Theorem 6.3.7 and Theorem 19.4.2, respectively.

## 19.5 Representation of Tensor Fields

The representation of tensor fields on smooth manifolds was considered in Section 15.7. Let us now return to this topic with the additional structure afforded by semi-Riemannian manifolds. We introduce smooth manifold counterparts of the representation map and scalar product map presented in Section 5.3 and Section 6.4, respectively, and link them using the characterization map defined in Section 15.5 and Section 15.7. The notation is admittedly horrendous, but the ideas are essentially those introduced in the much less complicated setting of scalar product spaces.

Let \((M,g)\) be a semi-Riemannian manifold. Following Section B.5, we denote by

\[
\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))
\]

the \(C^\infty(M)\)-module of \(C^\infty(M)\)-multilinear maps from \(\mathfrak{X}(M)^s\) to \(\mathfrak{X}(M)\), and by

\[
\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}^*(M)^r\times\mathfrak{X}(M)^s,C^\infty(M))
\]

the \(C^\infty(M)\)-module of \(C^\infty(M)\)-multilinear maps from \(\mathfrak{X}^*(M)^r\times\mathfrak{X}(M)^s\) to \(C^\infty(M)\).

Let us define a map

\[
\mathcal{S}_s:\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\longrightarrow \operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^{s+1},C^\infty(M)),
\]

called the **scalar product map**, by

\[
\mathcal{S}_s(\Psi)(X_1,\ldots,X_{s+1})=\langle X_1,\Psi(X_2,\ldots,X_{s+1})\rangle
\]

for all maps \(\Psi\) in \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\) and all vector fields \(X_1,\ldots,X_{s+1}\) in \(\mathfrak{X}(M)\). We observe that \(\mathcal{S}_s\) is the smooth manifold counterpart of the map defined by (6.4.1).

**Theorem 19.5.1.** If \((M,g)\) is a semi-Riemannian manifold, then \(\mathcal{S}_s\) is a \(C^\infty(M)\)-module isomorphism:

\[
\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\cong \operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^{s+1},C^\infty(M)).
\]

**Proof.** It is easily shown that \(\mathcal{S}_s\) is a \(C^\infty(M)\)-module homomorphism. We need to show that \(\mathcal{S}_s\) is bijective.

**Injectivity.** Suppose \(\Psi\) is a map in \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\) such that \(\mathcal{S}_s(\Psi)=0\). Then \(\langle X_1|_p,\Psi(X_2,\ldots,X_{s+1})|_p\rangle=0\) for all vector fields \(X_1,\ldots,X_{s+1}\) in \(\mathfrak{X}(M)\) and all \(p\) in \(M\). Since \(g\) is nondegenerate, we have \(\Psi(X_2,\ldots,X_{s+1})|_p=0\) for all vector fields \(X_2,\ldots,X_{s+1}\) in \(\mathfrak{X}(M)\) and all \(p\) in \(M\). Hence \(\Psi(X_2,\ldots,X_{s+1})=0\) for all vector fields \(X_2,\ldots,X_{s+1}\) in \(\mathfrak{X}(M)\). Thus, \(\Psi=0\), so \(\ker(\mathcal{S}_s)=\{0\}\). By Theorem B.5.3, \(\mathcal{S}_s\) is injective.

**Surjectivity.** Let \(\Upsilon\) be a map in \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^{s+1},C^\infty(M))\). We need to find a map \(\widetilde{\Upsilon}\) in \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\) such that \(\mathcal{S}_s(\widetilde{\Upsilon})=\Upsilon\). According to Theorem 19.4.2, defining a covector field in \(\mathfrak{X}^*(M)\) is equivalent to defining a vector field in \(\mathfrak{X}(M)\). For given vector fields \(X_2,\ldots,X_{s+1}\) in \(\mathfrak{X}(M)\), define a covector field \(\widetilde{\Upsilon}(X_2,\ldots,X_{s+1})^\flat\) in \(\mathfrak{X}^*(M)\) by

\[
\widetilde{\Upsilon}(X_2,\ldots,X_{s+1})^\flat(X_1)=\Upsilon(X_1,\ldots,X_{s+1})
\]

for all vector fields \(X_1\) in \(\mathfrak{X}(M)\). Then

\[
\mathcal{S}_s(\widetilde{\Upsilon})(X_1,\ldots,X_{s+1})
=\langle X_1,\widetilde{\Upsilon}(X_2,\ldots,X_{s+1})\rangle
=\langle\widetilde{\Upsilon}(X_2,\ldots,X_{s+1}),X_1\rangle
=\widetilde{\Upsilon}(X_2,\ldots,X_{s+1})^\flat(X_1)
=\Upsilon(X_1,\ldots,X_{s+1}).
\]

Since \(X_1,\ldots,X_{s+1}\) were arbitrary, \(\mathcal{S}_s(\widetilde{\Upsilon})=\Upsilon\). Thus, \(\mathcal{S}_s\) is surjective. □

Let us now define a map

\[
\mathfrak{R}_s:\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\longrightarrow\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}^*(M)\times\mathfrak{X}(M)^s,C^\infty(M)),
\]

called the **representation map**, by

\[
\mathfrak{R}_s(\Psi)(\omega,X_1,\ldots,X_s)=\omega(\Psi(X_1,\ldots,X_s)) \tag{19.5.1}
\]

for all maps \(\Psi\) in \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\), all covector fields \(\omega\) in \(\mathfrak{X}^*(M)\), and all vector fields \(X_1,\ldots,X_s\) in \(\mathfrak{X}(M)\). We observe that \(\mathfrak{R}_s\) is the smooth manifold counterpart of the map defined by (5.3.1). When \(s=1\), we denote

\[
\mathfrak{R}_1\quad\text{by}\quad\mathfrak{R}
\]

and obtain

\[
\mathfrak{R}:\operatorname{Lin}_{C^\infty(M)}(\mathfrak{X}(M),\mathfrak{X}(M))\longrightarrow\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}^*(M)\times\mathfrak{X}(M),C^\infty(M)).
\]

Then

\[
\mathfrak{R}(\Psi)(\omega,X)=\omega(\Psi(X)) \tag{19.5.2}
\]

for all maps \(\Psi\) in \(\operatorname{Lin}_{C^\infty(M)}(\mathfrak{X}(M),\mathfrak{X}(M))\), all covector fields \(\omega\) in \(\mathfrak{X}^*(M)\), and all vector fields \(X\) in \(\mathfrak{X}(M)\).

**Theorem 19.5.2.** If \((M,g)\) is a semi-Riemannian manifold, then \(\mathfrak{R}_s\) is a \(C^\infty(M)\)-module isomorphism:

\[
\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\cong \operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}^*(M)\times\mathfrak{X}(M)^s,C^\infty(M)).
\]

**Proof.** Recall the characterization map \(\mathcal{C}_s^r\) defined in Section 15.7. Let us define the “sharp” map

\[
\mathfrak{P}_1^1:\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^{s+1},C^\infty(M))\longrightarrow \operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}^*(M)\times\mathfrak{X}(M)^s,C^\infty(M))
\]

by

\[
\mathfrak{P}_1^1=\mathcal{C}_s^1\circ S_1^1\circ(\mathcal{C}_{s+1}^0)^{-1}. \tag{19.5.3}
\]

By Theorem 15.7.2 and Theorem 19.4.3, we have that \(\mathcal{C}_s^1\), \(\mathcal{C}_{s+1}^0\), and \(S_1^1\) are \(C^\infty(M)\)-module isomorphisms, and therefore, so is \(\mathfrak{P}_1^1\).

We claim that

\[
\mathfrak{R}_s=\mathfrak{P}_1^1\circ\mathcal{C}_s, \tag{19.5.4}
\]

which is the manifold version of Theorem 6.4.1(d). Once this is established, the result follows from Theorem 19.5.1. Let \(\Upsilon\) be a map in \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^{s+1},C^\infty(M))\). Making the identifications inherent in Theorem 15.7.2, it can be shown that

\[
\mathfrak{P}_1^1(\Upsilon)(\omega,X_1,\ldots,X_s)=\Upsilon(\omega^\sharp,X_1,\ldots,X_s)
\]

for all covector fields \(\omega\) in \(\mathfrak{X}^*(M)\) and all vector fields \(X_1,\ldots,X_s\) in \(\mathfrak{X}(M)\). For a map \(\Psi\) in \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\), we have

\[
\begin{aligned}
\mathfrak{R}_s(\Psi)(\omega,X_1,\ldots,X_s)
&=\omega(\Psi(X_1,\ldots,X_s))
=(\omega^\sharp)^\flat(\Psi(X_1,\ldots,X_s))\\
&=\langle\omega^\sharp,\Psi(X_1,\ldots,X_s)\rangle
=\mathcal{S}_s(\Psi)(\omega^\sharp,X_1,\ldots,X_s)\\
&=\mathfrak{P}_1^1\circ\mathcal{S}_s(\Psi)(\omega,X_1,\ldots,X_s).
\end{aligned}
\]

Since \(\Psi,\omega\), and \(X_1,\ldots,X_s\) were arbitrary, this proves the claim. ◻

**Theorem 19.5.3.** With the above setup, Figure 19.5.1 is a commutative diagram of \(C^\infty(M)\)-module isomorphisms. That is, any two \(C^\infty(M)\)-modules are connected by the \(C^\infty(M)\)-module isomorphism formed by taking the composite of intervening \(C^\infty(M)\)-module isomorphisms.

**Proof.** It follows from (19.5.3) and (19.5.4) that

\[
\mathfrak{R}_s=\mathcal{C}_s^1\circ S_1^1\circ(\mathcal{C}_{s+1}^0)^{-1}\circ\mathcal{C}_s.
\]
◻

**Theorem 19.5.4 (Representation of Covariant Tensor Fields).** If \((M,g)\) is a semi-Riemannian manifold, then

\[
\mathcal{T}^0_{s+1}(M)\cong\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\cong\mathcal{T}^1_s(M).
\]

**Proof.** This is the manifold version of Theorem 6.4.2 and follows from Theorem 19.5.3. ◻
