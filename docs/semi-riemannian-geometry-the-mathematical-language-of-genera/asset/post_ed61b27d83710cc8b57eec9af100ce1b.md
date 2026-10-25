
## 22.3 Einstein Tensor

Proof. This follows from Theorem 19.6.2(b) and (22.3.2).

**Theorem 22.3.5 (Orthonormal Frame Expression for Divergence of Ricci Curvature).** Let \((M,\mathfrak{g})\) be a semi-Riemannian \(m\)-manifold with signature \((\varepsilon_1,\dots,\varepsilon_m)\), let \((E_1,\dots,E_m)\) be an orthonormal frame on an open set \(U\) in \(M\), and let \(X\) be a vector field in \(\mathfrak{X}(M)\). Then

\[
\operatorname{Div}(\mathrm{R})(X)=\sum_i \varepsilon_i \nabla_{E_i}(\mathrm{R})(X,E_i)=\sum_i \varepsilon_i \nabla_{E_i}(\mathrm{R})(E_i,X)
\]

on \(U\).

Proof. This follows from Theorem 20.9.1 and Theorem 22.3.2(b).

**Theorem 22.3.6.** If \((M,\mathfrak{g})\) is a semi-Riemannian \(m\)-manifold, then:

- (a) \[
\operatorname{Div}(\mathrm{R})=\frac12 d(\mathrm{S})=\frac12 \operatorname{Div}(\mathrm{S}\mathfrak{g}).
\]

- (b) \(\operatorname{Div}(\mathbf{G})=0.\)

Proof. (a): For a vector field \(X\) in \(\mathfrak{X}(M)\), we have

\[
\begin{aligned}
d(\mathrm{S})(X)&=X(\mathrm{S}) && [\text{Th 15.4.5(b)}]\\
&=\nabla_X(\mathrm{S}) && [(18.1.1)]\\
&=\nabla_X\circ\mathrm{C}_{12}\circ\mathrm{C}_{14}(\mathcal{R}) && [(22.3.1),\ (22.3.2)]\\
&=\mathrm{C}_{12}\circ\mathrm{C}_{14}\circ\nabla_X(\mathcal{R}). && [\text{Th 19.6.3}]
\end{aligned}\tag{22.3.5}
\]

Since \(x\) and \(X\) were arbitrary, this proves the first equality. By Theorem 20.9.2, \(\operatorname{Div}(\mathrm{S}\mathfrak{g})=d(\mathrm{S})\), so the second equality follows from the first.

(b): This follows from part (a).

Adopting widely-accepted units, **Einstein’s field equations** for a Lorentz 4-manifold are given by the tensor identity

\[
\mathbf{G}=\frac{8\pi G}{c^4}\mathbf{T},
\]

where \(G\) is Newton’s gravitational constant, \(c\) is the speed of light, and \(\mathbf{T}\) is the so-called **stress–energy tensor**. These 10 partial differential equations, which form the foundation of the general theory of relativity, describe the way gravity results from spacetime being curved by mass and energy. It is remarkable that the “left-hand sides” of Einstein’s equations are expressed entirely in geometric terms, in the sense that the Einstein tensor is completely determined by the metric of a Lorentz 4-manifold.

## Part IV
## Appendices

## Appendix A
### Notation and Set Theory

In this appendix, we recall some of the elements of set theory.

Let \(X\) be a set, and let \(S_1\) and \(S_2\) be subsets of \(X\). We say that \(S_1\) and \(S_2\) **intersect** or **overlap** if \(S_1 \cap S_2 \neq \varnothing\), and that they **do not intersect**, **do not overlap**, or are **disjoint** if \(S_1 \cap S_2 = \varnothing\), where \(\varnothing\) denotes the **empty set**. The **difference** between \(S_1\) and \(S_2\) (in that order) is the set

\[
S_1 \setminus S_2 = \{x \in S_1 : x \notin S_2\}.
\]

Let \(\{S_\alpha : \alpha \in A\}\) be a collection of subsets of \(X\), where \(A\) is some indexing set. The union and intersection of the \(S_\alpha\) are denoted by

\[
\bigcup_{\alpha \in A} S_\alpha \quad \text{and} \quad \bigcap_{\alpha \in A} S_\alpha,
\]

respectively. The (**Cartesian**) product of sets \(X_1, \dots, X_n\) is defined by

\[
X_1 \times \cdots \times X_n = \{(x_1, \dots, x_n) : x_i \in X_i \text{ for } i = 1, \dots, n\},
\]

and each \((x_1, \dots, x_n)\) is called an \(n\)-tuple. For a set \(X\), we denote \(X \times \cdots \times X\) [\(n\) copies] by \(X^n\).

**Theorem A.1.** If \(X\) is a set and \(\{S_\alpha : \alpha \in A\} \cup \{T\}\) is a collection of subsets of \(X\), then

- (a) \[
\left( \bigcup_{\alpha \in A} S_\alpha \right) \cap T = \bigcup_{\alpha \in A} (S_\alpha \cap T).
\]

- (b) \[
\left( \bigcap_{\alpha \in A} S_\alpha \right) \cup T = \bigcap_{\alpha \in A} (S_\alpha \cup T).
\]

- (c) \(X\setminus\left(\bigcup_{\alpha\in A}S_\alpha\right)=\bigcap_{\alpha\in A}(X\setminus S_\alpha).\)

- (d) \(X\setminus\left(\bigcap_{\alpha\in A}S_\alpha\right)=\bigcup_{\alpha\in A}(X\setminus S_\alpha).\)

**Theorem A.2.** If \(X_i\) is a set, and \(S_i\) and \(T_i\) are subsets of \(X_i\) for \(i=1,\ldots,n\), then

\[
(S_1\times\cdots\times S_n)\cap(T_1\times\cdots\times T_n)=(S_1\cap T_1)\times\cdots\times(S_n\cap T_n).
\]

Let \(F:X\longrightarrow Y\) be a map. (The term “function” is reserved for a special type of map, as discussed below.) We refer to \(X\) and \(Y\) as the **domain of \(F\)** and **codomain of \(F\)**, respectively. The **image of \(F\)** is denoted by \(\operatorname{im}(F)\) or \(F(X)\) and defined by

\[
\operatorname{im}(F)=F(X)=\{F(x):x\in X\}.
\]

It is implicit in the notation \(F:X\longrightarrow Y\) that \(F(x)\) is defined for all \(x\) in \(X\). On the other hand, \(F(X)\) may be a proper subset of \(Y\). Let \(T\) be a subset of \(Y\), and define

\[
F^{-1}(T)=\{x\in X:F(x)\in T\}.
\]

Observe that we do not require \(T\) to be a subset of \(F(X)\). Thus,

\[
F^{-1}(T)=F^{-1}(T\cap F(X)).
\]

**Theorem A.3.** Let \(F:X\longrightarrow Y\) be a map, let \(\{S_\alpha:\alpha\in A\}\) be a collection of subsets of \(X\), and let \(\{T_\beta:\beta\in B\}\cup\{T_1,T_2\}\) be a collection of subsets of \(Y\). Then:

- (a) \[
F\left(\bigcup_{\alpha\in A}S_\alpha\right)=\bigcup_{\alpha\in A}F(S_\alpha).
\]

- (b) \[
F^{-1}\left(\bigcup_{\beta\in B}T_\beta\right)=\bigcup_{\beta\in B}F^{-1}(T_\beta).
\]

- (c) \[
F^{-1}\left(\bigcap_{\beta\in B}T_\beta\right)=\bigcap_{\beta\in B}F^{-1}(T_\beta).
\]

- (d) \[
F(F^{-1}(T))=T\cap F(X).
\]

- (e) \[
F^{-1}(T_1\setminus T_2)=F^{-1}(T_1)\setminus F^{-1}(T_2).
\]

If there is an element \(y_0\) in \(Y\) such that \(F(x)=y_0\) for all \(x\) in \(X\), then \(F\) is said to be **constant** or to have **constant value**. This is sometimes expressed as \(F=y_0\). We say that \(F\) is **injective** if distinct elements of \(X\) are mapped to distinct elements of \(Y\); that is, if \(x_1\) and \(x_2\) are elements of \(X\) such that \(F(x_1)=F(x_2)\), then \(x_1=x_2\). We say that \(F\) is **surjective** or **onto** if every element of \(Y\) is the image of some element of \(X\); that is, \(F(X)=Y\). A map that is both injective and surjective is called **bijective**.

The restriction of \(F:X\longrightarrow Y\) to a subset \(S\) of \(X\) is the map denoted by

\[
F|_S:S\longrightarrow Y
\]

and defined by \(F|_S(x)=F(x)\) for all \(x\) in \(S\). It is sometimes convenient to use the notation \(F|S\) instead of \(F|_S\), especially when \(S\) is a complicated expression.

Given a map \(G:Y\longrightarrow Z\), the **composite** of \(G\) and \(F\) is the map denoted by

\[
G\circ F:X\longrightarrow Z
\]

and defined by

\[
(G\circ F)(x)=G(F(x))
\]

for all \(x\) in \(X\). Observe that we do not require \(F\) to be surjective, so that \(G\circ F(X)\) might be a proper subset of \(G(Y)\).

The **identity map**

\[
\operatorname{id}_X:X\longrightarrow X
\]

is defined by \(\operatorname{id}_X(x)=x\) for all \(x\) in \(X\).

**Theorem A.4.** *The map* \(F:X\longrightarrow Y\) *is bijective if and only if there is a map*

\[
F^{-1}:Y\longrightarrow X,
\]

*called the* **inverse of \(F\)**, *such that* \(F^{-1}\circ F=\operatorname{id}_X\) *and* \(F\circ F^{-1}=\operatorname{id}_Y\).

Let \(F:X\longrightarrow Y_1\times\cdots\times Y_n\) be a map, and for each \(x\) in \(X\), let us express \(F(x)\) as

\[
F(x)=(F_1(x),\ldots,F_n(x)).
\]

This defines maps \(F_i:X\longrightarrow Y_i\) for \(i=1,\ldots,n\), called the **component maps of \(F\)** or simply the **components of \(F\)**. We often write

\[
F=(F_1,\ldots,F_n).
\]

**Theorem A.5.** Let \(F=(F_1,\ldots,F_n):X\longrightarrow Y_1\times\cdots\times Y_n\) be a map, and let \(T_i\) be a subset of \(Y_i\) for \(i=1,\ldots,n\). Then

\[
F^{-1}(T_1\times\cdots\times T_n)=\bigcap_{i=1}^{n}F_i^{-1}(T_i).
\]

The set of real numbers is denoted by \(\mathbb{R}\). We adopt the following convention.

**Throughout, a map with the codomain \(\mathbb{R}\) is called a function.**

Let \(f : X \longrightarrow \mathbb{R}\) be a function, and let \(x\) be an element of \(X\). We say that \(f\) vanishes at \(x\) if \(f(x)=0\), is nonvanishing at \(x\) if \(f(x)\neq 0\), and is nowhere-vanishing (on \(X\)) if it is nonvanishing at every \(x\) in \(X\). We say that \(f\) is strictly positive if \(f(x)>0\) for all \(x\) in \(X\), and strictly negative if \(f(x)<0\) for all \(x\) in \(X\). The function \(f : \mathbb{R} \longrightarrow \mathbb{R}\) is said to be increasing if \(f(x_1)\leq f(x_2)\) for all \(x_1 < x_2\), and strictly increasing if \(f(x_1) < f(x_2)\) for all \(x_1 < x_2\). Analogous definitions are given for decreasing and strictly decreasing.

Given a real number \(c\), the constant function on \(X\) with value \(c\), denoted by \(c_X\) or simply \(c\), is the function that sends all elements of \(X\) to \(c\). In particular, we have \(0_X\), called the zero function, and \(1_X\).

The cardinality of a set \(X\) is denoted by \(\operatorname{card}(X)\) and defined to be the number of elements in the set, which is either finite or infinite. Two sets are said to have the same cardinality if there is a bijective map between them. It is known that there are different types of “infinity”. For instance, we say that a set is countable if it has the same cardinality as the positive integers. It can be shown that the rational numbers are countable, but the real numbers are not.

A binary relation on a set \(X\) is a subset \(\sim\) of \(X \times X\). If \((x_1,x_2)\) is in \(\sim\), then \(x_1\) is said to be related to \(x_2\), written \(x_1 \sim x_2\). We say that \(\sim\) is an equivalence relation on \(X\) if:

- [E1] \(x \sim x\) for all \(x\) in \(X\). (reflexivity)
- [E2] If \(x_1 \sim x_2\), then \(x_2 \sim x_1\) for all \(x_1,x_2\) in \(X\). (symmetry)
- [E3] If \(x_1 \sim x_2\) and \(x_2 \sim x_3\), then \(x_1 \sim x_3\) for all \(x_1,x_2,x_3\) in \(X\). (transitivity)

If \(\sim\) is an equivalence relation on \(X\) and \(x\) is an element of \(X\), the equivalence class determined by \(x\) is denoted by \([x]\) and defined to be the set of elements of \(X\) that are related to \(x\):

\[
[x] = \{ y \in X : y \sim x \}.
\]

In particular, we have from [E1] that \([x]\) contains \(x\).

A partition of \(X\) is a collection of disjoint nonempty subsets of \(X\) whose union equals \(X\).

**Theorem A.6.** Given an equivalence relation on a set \(X\), the collection of distinct equivalence classes forms a partition of \(X\).

The sign of a nonzero real number \(x\) is defined by

\[
\operatorname{sgn}(x) = \dfrac{x}{|x|}.
\]

The sign of \(0\) is not defined.

Kronecker’s delta is the function

\[
\delta : \{1,\dots,n\} \times \{1,\dots,n\} \longrightarrow \{0,1\}
\]
