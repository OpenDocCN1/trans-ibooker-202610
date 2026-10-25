is said to be a derivation [on $C^{\infty}(M)$] if it satisfies the following product rule:

```
$\mathcal{D}(fg) = f\mathcal{D}(g) + g\mathcal{D}(f)$
```

for all functions $f, g$ in $C^{\infty}(M)$; that is,

```
$\mathcal{D}(fg)(p) = f(p)\,\mathcal{D}(g)(p) + g(p)\,\mathcal{D}(f)(p)$
```

for all $p$ in $M$. The set of derivations on $C^{\infty}(M)$ is denoted by $\operatorname{Der}(M)$. The zero derivation in $\operatorname{Der}(M)$, denoted by $0$, is the derivation that sends all functions in $C^{\infty}(M)$ to the zero function in $C^{\infty}(M)$. We make $\operatorname{Der}(M)$ into both a vector space over $\mathbb{R}$ and a module over $C^{\infty}(M)$ by defining operations as follows: for all derivations $\mathcal{D}, \mathcal{E}$ in $\operatorname{Der}(M)$, all functions $f, g$ in $C^{\infty}(M)$, and all real numbers $c$, let

```
$(\mathcal{D} + \mathcal{E})(f)(p) = \mathcal{D}(f)(p) + \mathcal{E}(f)(p)$
```

```
$(f\mathcal{D})(g)(p) = f(p)\,\mathcal{D}(g)(p)$
```

and

```
$(c\mathcal{D})(f)(p) = c\,\mathcal{D}(f)(p)$
```

for all $p$ in $M$.

We see from (15.1.3) that a vector field on $M$ can be thought of as derivation on $C^{\infty}(M)$. Pursuing this line of reasoning, let us consider the map

```
$\mathfrak{F}: \mathfrak{X}(M) \longrightarrow \operatorname{Der}(M)$
```

defined by

```
$\mathfrak{F}(X)(f) = X(f)$
```

for all vector fields $X$ in $\mathfrak{X}(M)$ and all functions $f$ in $C^{\infty}(M)$; that is,

```
$\mathfrak{F}(X)(f)(p) = X(f)(p)$
```

for all $p$ in $M$, where the right-hand side of the above identity is given by (15.1.2).

# **Theorem 15.2.1 (Representation of Vector Fields).** *If $M$ is a smooth manifold, then $\mathfrak{F}$ is a $C^{\infty}(M)$-module isomorphism:*

```
$\mathfrak{X}(M) \approx \operatorname{Der}(M)$.
```

*Proof.* It is easily shown that $\mathfrak{F}$ is a $C^{\infty}(M)$-module homomorphism. It remains to show that $\mathfrak{F}$ is bijective.

*Injectivity.* Suppose $X$ is a vector field in $\mathfrak{X}(M)$ such that $\mathfrak{F}(X)=0$; that is, $X(f)=0$ for all functions $f$ in $C^{\infty}(M)$. By definition, $X=0$, so $\ker(\mathfrak{F}) = \{0\}$. It follows from Theorem B.5.3 that $\mathfrak{F}$ is injective.

*Surjectivity.* Let $\mathcal{D}$ be a derivation on $C^{\infty}(M)$. For each point $p$ in $M$, define a function

```
$X_{\mathcal{D}|p}: C^{\infty}(M) \longrightarrow \mathbb{R}$
```