
Adjoints behave well with respect to basic algebraic structure.

### Theorem 4.3.2

If \((V,\mathfrak{g})\) is a scalar product space and \(A, B : V \longrightarrow V\) are linear maps, then:

- (a) \((A + B)^{\dagger} = A^{\dagger} + B^{\dagger}\).
- (b) \((A \circ B)^{\dagger} = B^{\dagger} \circ A^{\dagger}\).
- (c) \(A^{\dagger\dagger} = A\).
- (d) If \(A\) is a linear isomorphism, then \((A^{-1})^{\dagger} = (A^{\dagger})^{-1}\).
- (e) \(A\) is a linear isomorphism if and only if \(A^{\dagger}\) is a linear isomorphism.

*Proof.* We have \(A^{\dagger} = \mathsf{S} \circ A^{*} \circ \mathsf{F}\) and \(B^{\dagger} = \mathsf{S} \circ B^{*} \circ \mathsf{F}\).

(a): By Theorem 1.3.1(a),

\[
(A + B)^{\dagger} = \mathsf{S} \circ (A + B)^{*} \circ \mathsf{F} = \mathsf{S} \circ (A^{*} + B^{*}) \circ \mathsf{F}
= (\mathsf{S} \circ A^{*} \circ \mathsf{F}) + (\mathsf{S} \circ B^{*} \circ \mathsf{F}) = A^{\dagger} + B^{\dagger}.
\]

(b): By Theorem 1.3.1(b),

\[
(A \circ B)^{\dagger} = \mathsf{S} \circ (A \circ B)^{*} \circ \mathsf{F} = \mathsf{S} \circ (B^{*} \circ A^{*}) \circ \mathsf{F}
= (\mathsf{S} \circ B^{*} \circ \mathsf{F}) \circ (\mathsf{S} \circ A^{*} \circ \mathsf{F}) = B^{\dagger} \circ A^{\dagger}.
\]

(c): By Theorem 1.3.1(c) and Theorem 3.3.5,

\[
A^{\dagger\dagger} = \mathsf{S} \circ (A^{\dagger})^{*} \circ \mathsf{F} = \mathsf{S} \circ (\mathsf{S} \circ A^{*} \circ \mathsf{F})^{*} \circ \mathsf{F}
= \mathsf{S} \circ (\mathsf{F}^{*} \circ A^{**} \circ \mathsf{S}^{*}) \circ \mathsf{F} = A.
\]

(d): By Theorem 1.3.1(d),

\[
(A^{-1})^{\dagger} = \mathsf{S} \circ (A^{-1})^{*} \circ \mathsf{F} = \mathsf{F}^{-1} \circ (A^{*})^{-1} \circ \mathsf{S}^{-1}
= (\mathsf{S} \circ A^{*} \circ \mathsf{F})^{-1} = (A^{\dagger})^{-1}.
\]

(e): Since \(A^{\dagger} = \mathsf{S} \circ A^{*} \circ \mathsf{F}\), and \(\mathsf{F}\) and \(\mathsf{S}\) are isomorphisms, the result follows.

For a scalar product space \((V,\mathfrak{g})\), we say that a linear map \(A : V \longrightarrow V\) is self-adjoint if \(A = A^{\dagger}\). In view of Theorem 4.3.1(a), this condition is equivalent to

\[
\langle A(v), w \rangle = \langle v, A(w) \rangle
\]

for all vectors \(v, w\) in \(V\).

### Theorem 4.3.3

Let \((V,\mathfrak{g})\) be a scalar product space, let \(A : V \longrightarrow V\) be a linear map, and let \(\mathcal{H}\) be a basis for \(V\). Then

\[
[A^{\dagger}]_{\mathcal{H}} = \mathfrak{g}_{\mathcal{H}}^{-1} [A]_{\mathcal{H}}^{\mathsf{T}} \mathfrak{g}_{\mathcal{H}}.
\]

*Remark.* It follows from Theorem 3.3.3 that the inverse of \(\mathfrak{g}_{\mathcal{H}}\) exists, so the assertion makes sense.

*Proof.* Let \(\mathcal{H} = (h_1, \dots, h_m)\), and let \([A] = [A]_{\mathcal{H}}^{\mathcal{H}} = [a_i^j]\) and \([A^{\dagger}] = [A^{\dagger}]_{\mathcal{H}}^{\mathcal{H}} = [b_j^i]\). We have

\[
[\langle A(h_i), h_j \rangle] = [\langle h_i, A^{\dagger}(h_j) \rangle].
\]

By Theorem 2.1.6(b), (2.2.2), and (2.2.3),

\[
\begin{aligned}
\langle A(h_i), h_j \rangle
&= \left\langle \sum_k a_i^k h_k, h_j \right\rangle
= \sum_k a_i^k \langle h_k, h_j \rangle
= \sum_k a_i^k g_{kj} \\
&= ([A]_{(i)})^{\mathsf{T}} (\mathfrak{g}_{\mathcal{H}})_{(j)}
= ([A]^{\mathsf{T}})^{(i)} (\mathfrak{g}_{\mathcal{H}})_{(j)},
\end{aligned}
\]

hence

\[
[\langle A(h_i), h_j \rangle] = [A]^{\mathsf{T}} \mathfrak{g}_{\mathcal{H}}.
\]

On the other hand,

\[
\begin{aligned}
\langle h_i, A^{\dagger}(h_j) \rangle
&= \left\langle h_i, \sum_k b_j^k h_k \right\rangle
= \sum_k b_j^k \langle h_i, h_k \rangle
= \sum_k g_{ik} b_j^k \\
&= (\mathfrak{g}_{\mathcal{H}})^{(i)} [A^{\dagger}]_{(j)},
\end{aligned}
\]

so

\[
[\langle h_i, A^{\dagger}(h_j) \rangle] = \mathfrak{g}_{\mathcal{H}} [A^{\dagger}].
\]

Combining the above identities gives \([A]^{\mathsf{T}} \mathfrak{g}_{\mathcal{H}} = \mathfrak{g}_{\mathcal{H}} [A^{\dagger}]\), from which the result follows.

### Theorem 4.3.4

Let \((V, \mathfrak{g})\) and \((W, \mathfrak{h})\) be scalar product spaces, and let \(A : V \longrightarrow W\) be a linear map. Then:

- (a) \(\operatorname{rank}(A^{\dagger}) = \operatorname{rank}(A)\).
- (b) If \(\dim(V) = \dim(W)\), then \(\operatorname{null}(A^{\dagger}) = \operatorname{null}(A)\).
- (c) \(\ker(A^{\dagger}) = \operatorname{im}(A)^{\perp}\).
- (d) \(\operatorname{im}(A^{\dagger}) = \ker(A)^{\perp}\).

*Proof.* The proof emulates that of Theorem 1.4.3.

(c): We have

\[
\begin{aligned}
w \in \ker(A^{\dagger})
&\iff A^{\dagger}(w) = 0 \\
&\iff \langle A^{\dagger}(w), v \rangle = 0 \text{ for all } v \in V
&& [\mathfrak{h} \text{ is nondegenerate}] \\
&\iff \langle w, A(v) \rangle = 0 \text{ for all } v \in V
&& [\text{Th 4.3.1(a)}] \\
&\iff w \in \operatorname{im}(A)^{\perp}.
\end{aligned}
\]

(d): It follows from Theorem 4.3.2(c) and part (c) that

\[
\ker(A) = \ker(A^{\dagger\dagger}) = \operatorname{im}(A^{\dagger})^{\perp},
\]

and then from Theorem 4.1.2(c) that

\[
\ker(A)^{\perp} = \operatorname{im}(A^{\dagger})^{\perp\perp} = \operatorname{im}(A^{\dagger}).
\]

(a): We have

\[
\begin{aligned}
\operatorname{rank}(A^{\dagger})
&= \dim(\operatorname{im}(A^{\dagger})) \\
&= \dim(\ker(A)^{\perp}) && [\text{part (d)}] \\
&= \dim(V) - \dim(\ker(A)) && [\text{Th 4.1.2(b)}] \\
&= \operatorname{rank}(A). && [\text{Th 1.1.11}]
\end{aligned}
\]

(b): We have

\[
\begin{aligned}
\operatorname{rank}(A^{\dagger}) + \operatorname{null}(A^{\dagger})
&= \dim(W) && [\text{Th 1.1.11}] \\
&= \dim(V) && [\text{assumption}] \\
&= \operatorname{rank}(A) + \operatorname{null}(A). && [\text{Th 1.1.11}]
\end{aligned}
\]

The result now follows from part (a).

Let \((V,\mathfrak{g})\) be a scalar product space, let \(A : V \longrightarrow V\) be a linear map, and let \(U\) be a subspace of \(V\) on which \(\mathfrak{g}\) is nondegenerate. The left-hand side of the following table summarizes selected results on annihilators and pullbacks, while the right-hand side of the table does the same for perps and adjoints. The correspondence between annihilators and perps on the one hand, and pullbacks and adjoints on the other, is evident and demonstrates the similar roles played by \(U^0\) and \(U^{\perp}\).

| Annihilators and pullbacks | Perps and adjoints |
|---|---|
| \(\dim(V) = \dim(U) + \dim(U^0)\) | \(\dim(V) = \dim(U) + \dim(U^{\perp})\) |
| \(U^{00} = U\) | \(U^{\perp\perp} = U\) |
| \(A^{**} = A\) | \(A^{\dagger\dagger} = A\) |
| \(\operatorname{rank}(A^*) = \operatorname{rank}(A)\) | \(\operatorname{rank}(A^{\dagger}) = \operatorname{rank}(A)\) |
| \(\operatorname{null}(A^*) = \operatorname{null}(A)\) | \(\operatorname{null}(A^{\dagger}) = \operatorname{null}(A)\) |
| \(\ker(A^*) = \operatorname{im}(A)^0\) | \(\ker(A^{\dagger}) = \operatorname{im}(A)^{\perp}\) |
| \(\operatorname{im}(A^*) = \ker(A)^0\) | \(\operatorname{im}(A^{\dagger}) = \ker(A)^{\perp}\) |

## 4.4 Linear Isometries

A linear map is one that respects the linear structure of a vector space. In a scalar product space, there is the additional feature of a scalar product. This section introduces a type of linear map that also respects scalar products.

Let \((V,\mathfrak{g})\) and \((W,\mathfrak{h})\) be scalar product spaces, and let \(A : V \longrightarrow W\) be a linear map. We say that \(A\) is a linear isometry (or orthogonal transformation) if

\[
\langle A(v), A(w) \rangle = \langle v, w \rangle
\]

for all vectors \(v, w\) in \(V\).

### Theorem 4.4.1

If \((V,\mathfrak{g})\) and \((W,\mathfrak{h})\) are scalar product spaces with the same dimension and \(A : V \longrightarrow W\) is a linear isometry, then:

- (a) \(A\) is a linear isomorphism.
- (b) \(A\) maps every orthonormal basis for \(V\) to an orthonormal basis for \(W\).

*Proof.* (a): Let \(v\) be a vector in \(\ker(A)\). Then \(\langle v,w \rangle = \langle A(v), A(w) \rangle = 0\) for all vectors \(w\) in \(V\). Since \(\mathfrak{g}\) is nondegenerate, \(v = 0\). Thus, \(\ker(A) = \{0\}\). By Theorem 1.1.12 and Theorem 1.1.14, \(A\) is a linear isomorphism.

(b): Let \(\mathcal{E} = (e_1, \ldots, e_m)\) be an orthonormal basis for \(V\). By Theorem 1.1.13(a) and part (a), \(A(\mathcal{E})\) is a basis for \(W\). Since \(A\) is a linear isometry, \(\langle A(e_i), A(e_j) \rangle = \langle e_i, e_j \rangle\) for \(i,j = 1, \ldots, m\). Thus, \(A(\mathcal{E})\) is an orthonormal basis for \(W\).

### Theorem 4.4.2

Let \((V,\mathfrak{g})\) be a scalar product space, and let \(A : V \longrightarrow V\) be a linear map. Then the following are equivalent:

- (a) \(A\) is a linear isometry.
- (b) \(A\) is a linear isomorphism and \(A^{\dagger} = A^{-1}\).
- (c) For some (hence every) basis \(\mathcal{H}\) for \(V\),

\[
\mathfrak{g}_{\mathcal{H}} = [A]_{\mathcal{H}}^{\mathsf{T}} \mathfrak{g}_{\mathcal{H}} [A]_{\mathcal{H}}.
\]

*Proof.* (a) \(\Rightarrow\) (b): By Theorem 4.4.1(a), \(A\) is a linear isomorphism. Let \(v,w\) be vectors in \(V\). Then

\[
\langle v,w \rangle = \langle A(v), A(w) \rangle = \langle A^{\dagger}(A(v)), w \rangle,
\]

hence

\[
0 = \langle A^{\dagger}(A(v)) - v, w \rangle = \langle ((A^{\dagger} \circ A) - \operatorname{id}_V)(v), w \rangle.
\]

Since \(\mathfrak{g}\) is nondegenerate and \(w\) was arbitrary, \(((A^{\dagger} \circ A) - \operatorname{id}_V)(v) = 0\) for all \(v\) in \(V\). That is, \(A^{\dagger} \circ A = \operatorname{id}_V\), hence \(A^{\dagger} = A^{-1}\).

(b) \(\Rightarrow\) (c): Since \(A^{\dagger} = A^{-1}\), by Theorem 2.2.5(b),

\[
[A^{\dagger}]_{\mathcal{H}} = [A^{-1}]_{\mathcal{H}} = ([A]_{\mathcal{H}})^{-1}.
\]

The result now follows from Theorem 4.3.3.

(c) \(\Rightarrow\) (a): For vectors \(v,w\) in \(V\), we have

\[
\begin{aligned}
\langle A(v), A(w) \rangle
&= ([A(v)]_{\mathcal{H}})^{\mathsf{T}} \mathfrak{g}_{\mathcal{H}} [A(w)]_{\mathcal{H}}
&& [\mathrm{Th\ 3.1.2(a)}] \\
&= ([A]_{\mathcal{H}} [v]_{\mathcal{H}})^{\mathsf{T}} \mathfrak{g}_{\mathcal{H}} [A]_{\mathcal{H}} [w]_{\mathcal{H}}
&& [\mathrm{Th\ 2.2.4}] \\
&= ([v]_{\mathcal{H}})^{\mathsf{T}} [A]_{\mathcal{H}}^{\mathsf{T}} \mathfrak{g}_{\mathcal{H}} [A]_{\mathcal{H}} [w]_{\mathcal{H}} \\
&= ([v]_{\mathcal{H}})^{\mathsf{T}} \mathfrak{g}_{\mathcal{H}} [w]_{\mathcal{H}} \\
&= \langle v,w \rangle
&& [\mathrm{Th\ 3.1.2(a)}].
\end{aligned}
\]

### Theorem 4.4.3

If \((V,\mathfrak{g})\) is a scalar product space and \(A : V \longrightarrow V\) is a linear isometry, then \(\det(A) = \pm 1\).
