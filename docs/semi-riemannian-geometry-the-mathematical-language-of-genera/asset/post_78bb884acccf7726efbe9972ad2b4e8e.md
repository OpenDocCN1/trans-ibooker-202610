
**Proof.** Consider the sets $S_1=f(X)\cap(-\infty,c)$ and $S_2=f(X)\cap(c,+\infty)$. It is clear that $S_1$ and $S_2$ are disjoint and nonempty. Since $(-\infty,c)$ and $(c,+\infty)$ are open in $\mathbb{R}$, $S_1$ and $S_2$ are open in $f(X)$. Suppose there is no point $x_0$ in $X$ such that $f(x_0)=c$. Then $f(X)=S_1\cup S_2$. Thus, $f(X)$ is disconnected in $\mathbb{R}$, which contradicts Theorem 9.1.18. ◻

## Theorem 9.1.20.

*If $X$ is a connected topological space and $f:X\to\mathbb{R}$ is a nowhere-vanishing continuous function, then $f$ is either strictly positive or strictly negative.*

**Proof.** Let $x_1$ and $x_2$ be distinct points in $X$. Since $f$ is nowhere-vanishing, $f(x_1),f(x_2)\ne0$. If $f(x_1)>0$, then $f(x_2)>0$; for if not, by Theorem 9.1.19, there is a point $x_0$ in $X$ such that $f(x_0)=0$, which contradicts the nowhere-vanishing assumption on $f$. Similarly, if $f(x_1)<0$, then $f(x_2)<0$. Since $x_1$ and $x_2$ were arbitrary, the result follows. ◻

Let $X$ be a topological space, and let $\mathcal{U}=\{U_\alpha:\alpha\in A\}$ be a collection of open sets in $X$. We say that $\mathcal{U}$ is an **open cover** of $X$ if $X=\bigcup_{\alpha\in A}U_\alpha$. A subcollection of $\mathcal{U}$ that is also an open cover of $X$ is said to be a **subcover of $\mathcal{U}$**. A subcover that is a finite set is called a **finite subcover**. We say that $X$ is a **compact** topological space if every open cover of $X$ has a finite subcover. A subset $S$ of $X$ is said to be a **compact set in $X$** or simply **compact in $X$** if it is compact as a topological subspace.

## Theorem 9.1.21.

*Let $X$ and $Y$ be topological spaces, and let $F:X\to Y$ be a continuous map. If $X$ is compact, then $F(X)$ is compact in $Y$.*

**Proof.** Let $\mathcal{V}$ be an open cover of $F(X)$. Then $\mathcal{V}=\{V_\alpha\cap F(X):\alpha\in A\}$, where each $V_\alpha$ is an open set in $Y$. By Theorem A.1(a),

$$F(X)=\left(\bigcup_{\alpha\in A}V_\alpha\right)\cap F(X)=\bigcup_{\alpha\in A}[V_\alpha\cap F(X)],$$

and by Theorem A.3(b),

$$X=F^{-1}(F(X))=\bigcup_{\alpha\in A}F^{-1}(V_\alpha\cap F(X))=\bigcup_{\alpha\in A}F^{-1}(V_\alpha).$$

It follows from Theorem 9.1.7 that $\{F^{-1}(V_\alpha):\alpha\in A\}$ is an open cover of $X$. Since $X$ is compact, there is a finite subcover $\{F^{-1}(V_i):i=1,\ldots,k\}$, so

$$X=\bigcup_iF^{-1}(V_i).$$

By parts (a) and (d) of Theorem A.3,

$$F(X)=\bigcup_iF(F^{-1}(V_i))=\bigcup_i[V_i\cap F(X)].$$

Thus, $\{V_i\cap F(X):i=1,\ldots,k\}$ is a finite subcover of $\mathcal{V}$. ◻

Let X be a topological space, and let f : X ⟶ ℝ be a function. We say that f is bounded on X or simply bounded if there is a real number c > 0 such that |f(x)| < c for all points x in X. Let S be a nonempty subset of ℝ. The element s1 in S is said to be the smallest element in S if s1 ≤ s for all s in S. Similarly, the element s2 in S is said to be the largest element in S if s ≤ s2 for all s in S. If S has a smallest element s1 and a largest element s2, then s1 ≤ s ≤ s2 for all s in S. Consider the collection 𝒰 = {(-∞, s) ∩ S : s ∈ S} of open sets in S. If S has a largest element s2, then s2 is not in ∪_{s∈S}(-∞, s), so 𝒰 is not an open cover of S in ℝ. On the other hand, if S does not have a largest element, then 𝒰 is an open cover of S in ℝ.

## Theorem 9.1.22 (Extreme Value Theorem). If X is a compact topological space and f : X ⟶ ℝ is a continuous function, then:

- (a) There are points x1 and x2 in X such that f(x1) ≤ f(x) ≤ f(x2) for all x in X.
- (b) f is bounded.

Proof. (a): The assertion is equivalent to: There are points x1 and x2 in X such that f(x1) is the smallest element in f(X), and f(x2) is the largest element in f(X). Suppose, for a contradiction, that f(X) does not have a largest element. In light of above remarks, {(-∞, f(x)) ∩ f(X) : x ∈ X} is an open cover of f(X). Since X is compact and f is continuous, by Theorem 9.1.21, f(X) is compact in ℝ. It follows that there is a finite subcover {(-∞, f(xi)) ∩ f(X) : i = 1, …, m}, where we assume without loss of generality that f(x1) < ⋯ < f(xm). By Theorem A.1(a), f(X) = ⋃_{i=1}^m [(-∞, f(xi)) ∩ f(X)] = [⋃_{i=1}^m (-∞, f(xi))] ∩ f(X) = (-∞, f(xm)) ∩ f(X), hence f(X) ⊆ (-∞, f(xm)). Since f(xm) is in f(X) but not in (-∞, f(xm)), we have a contradiction. Thus, f(X) has a largest element. The proof that f(X) has a smallest element is similar. (b): This follows from part (a).

Let X be a topological space, and let f : X ⟶ ℝ be a function. The support of f is denoted by supp(f) and defined to be the closure in X of the set of points at which f is nonvanishing: supp(f) = cl_X({x ∈ X : f(x) ≠ 0}). Thus, supp(f) is the smallest closed set in X containing those points at which f is nonvanishing. We say that f has compact support if supp(f) is compact in X. Given a subset S of X, we say that f has support in S if supp(f) ⊆ S.

Proof. For open balls, this is true by definition. Let \overline{B}_r(x) be a closed ball in X, and let y be a point in X\setminus\overline{B}_r(x), so that d(x,y) > r. Let s = d(x,y) − r, and let z be a point in B_s(y), which means d(y,z) < s. See Figure 9.2.1. From [D2] and [D3],

```
r + s = d(x,y) ≤ d(x,z) + d(z,y) < d(x,z) + s,
```

hence d(x,z) > r, so z is in X\setminus\overline{B}_r(x). Thus, B_s(y) ⊆ X\setminus\overline{B}_r(x). It follows that X\setminus\overline{B}_r(x) is the union of a collection of basis elements of B_d. By Theorem 9.1.3 and Theorem 9.2.2, X\setminus\overline{B}_r(x) is open with respect to the metric topology induced by d, hence \overline{B}_r(x) = X\setminus[X\setminus\overline{B}_r(x)] is closed with respect to that topology.

![](img/d712710a8eb78c0959204afd5dde4c91_209_0.png)

## Figure 9.2.1. Diagram for Theorem 9.2.3

The next result expresses continuity in metric spaces in terms familiar from the differential calculus of one real variable.

## Theorem 9.2.4 (ε-δ Criterion). Let (X,d) and (Y,e) be metric spaces, each with its induced metric topology, let F : X ⟶ Y be a map, and let x be a point in X. Then F is continuous at x if and only if for every real number ε > 0, there is a real number δ > 0 (possibly depending on ε) such that:

```
d(x,y) < δ    ⇒    e(F(x),F(y)) < ε,
```

or equivalently,

```
y is in B_δ(x)    ⇒    F(y) is in B_ε(F(x)),
```

or equivalently,

```
B_δ(x) ⊆ F^{-1}(B_ε(F(x))).
```

Proof. The equivalence of the three conditions is clear. Let us work with the last one.

(⇒): Since \(F\) is continuous at \(x\) and \(B_{\varepsilon}(F(x))\) is a neighbourhood of \(F(x)\) in \(Y\), by definition, there is a neighbourhood \(U\) of \(x\) in \(X\) such that \(F(U)\subset B_{\varepsilon}(F(x))\). By Theorem 9.1.3 and Theorem 9.2.2, there is an open ball \(B_{\delta}(x)\) such that

\[
B_{\delta}(x)\subset U\subset F^{-1}(B_{\varepsilon}(F(x))).
\]

(⇐): Let \(V\) be a neighbourhood of \(F(x)\) in \(Y\). We have from Theorem 9.1.3 and Theorem 9.2.2 that there is an open ball \(B_{\varepsilon}(F(x))\) such that \(B_{\varepsilon}(F(x))\subset V\). By assumption, there is an open ball \(B_{\delta}(x)\) such that \(B_{\delta}(x)\subset F^{-1}(B_{\varepsilon}(F(x)))\). Thus, \(B_{\delta}(x)\) is a neighbourhood of \(x\) in \(X\) such that \(F(B_{\delta}(x))\subset V\).

## 9.3 Normed Vector Spaces

Let \(V\) be a vector space. A function

\[
\|\cdot\|: V\longrightarrow \mathbb{R}
\]

is said to be a **norm** on \(V\) if for all vectors \(v,w\) in \(V\) and all real numbers \(c\):

- [N1] \(\|v\|\ge 0\), with \(\|v\|=0\) if and only if \(v=0\).
- [N2] \(\|cv\|=|c|\|v\|\).
- [N3] \(\|v+w\|\le \|v\|+\|w\|\). (triangle equality)

A **normed vector space** is a pair \((V,\|\cdot\|)\) consisting of a vector space \(V\) and a norm \(\|\cdot\|\) on \(V\).

**Theorem 9.3.1.** If \((V,g)\) is an inner product space and \(\|\cdot\|\) is the norm defined by \(\|v\|=\sqrt{\langle v,v\rangle}\) for all \(v\) in \(V\), then \((V,\|\cdot\|)\) is a normed vector space.

*Proof.* Using Theorem 4.6.5 and the basic properties of inner products, it is easily shown that [N1]–[N3] are satisfied.

Theorem 9.3.1 justifies our use of the term “norm” in Section 4.6 (and to a lesser extent its use in Section 4.1).

**Theorem 9.3.2.** If \((V,\|\cdot\|)\) is a normed vector space and \(d:V\times V\longrightarrow\mathbb{R}\) is the function defined by \(d(v,w)=\|v-w\|\) for all vectors \(v,w\) in \(V\), then \((V,d)\) is a metric space.

*Proof.* It is easily shown that conditions [D1]–[D3] of Section 9.2 are satisfied.

## 9.4 Euclidean Topology on \(\mathbb{R}^m\)

Let us put the results of Sections 9.1–9.3 to work constructing a series of “spaces” starting with the inner product space \(\mathbb{R}^m=(\mathbb{R}^m,e)\), as defined in Example 4.6.1. We first apply Theorem 9.3.1 to \((\mathbb{R}^m,e)\) and obtain a normed vector space \((\mathbb{R}^m,\|\cdot\|)\), where

\[
\|\cdot\|:\mathbb{R}^m\times\mathbb{R}^m\longrightarrow\mathbb{R}
\]
