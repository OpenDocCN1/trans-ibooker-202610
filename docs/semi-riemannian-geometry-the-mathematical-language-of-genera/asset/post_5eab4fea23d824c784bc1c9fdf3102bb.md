
(c) If $v$ is a vector in $\mathcal{E}_p$, then $\gamma_{p,v}$ is defined on $[0,1]$.

**Proof.** We consider only parts (b) and (c).

(b): Let $v$ be a vector in $T_p(M)$, and let $0<\epsilon\leq 1$. If $\gamma_{p,v}$ has domain $(a,b)$, then, by Theorem 18.8.6, $\gamma_{p,\epsilon v}$ has domain $(a/\epsilon,b/\epsilon)$, which contains $(a,b)$. It follows that if $\gamma_{p,v}(1)$ is defined, then so is $\gamma_{p,\epsilon v}(1)$; that is, if $v$ is in $\mathcal{E}_p$, then so is $\epsilon v$.

(c): We have from part (b) that $\gamma_{p,tv}(1)$ is defined for all $t$ in $[0,1]$, and from Theorem 18.8.6 that $\gamma_{p,v}(t)=\gamma_{p,tv}(1)$. The result follows.

Continuing with the above setup, let $v$ be a vector in $\mathcal{E}_p$. Using Theorem 18.9.1(c), we restrict $\gamma_{p,v}$ to $[0,1]$ and obtain the geodesic

$\rho_{p,v}(t)=\gamma_{p,v}|_{[0,1]}:[0,1]\longrightarrow M,$  \hfill (18.9.1)

called the **radial geodesic with starting point $p$ and initial velocity $v$**. It follows from Theorem 18.8.2(a) that $\rho_{p,v}(0)=p$. By varying $v$, we obtain a family of radial geodesics with starting point $p$.

## Example 18.9.2 (Pln and $S^2$). Let $p$ be a point in Pln. We have from Example 18.8.8 that the radial geodesics with starting point $p$ are line segments in Pln radiating from $p$. It follows that $\mathcal{E}_p=T_p(\mathrm{Pln})$. Let $q$ be another point in Pln. As is clear from the underlying Euclidean geometry, there is a unique radial geodesic starting at $p$ and ending at $q$.

Now let $p$ be a point in $S^2$. We have from Example 18.8.10 that the radial geodesics with starting point $p$ are portions (or all) of great circles on $S^2$ emanating from $p$. It follows that $\mathcal{E}_p=T_p(S^2)$. Let $q$ be another point in $S^2$. There is a unique great circle passing through $p$ and $q$, obtained by intersecting $S^2$ with the plane passing through $p$, $q$, and $(0,0,0)$. This gives two radial geodesics that start at $p$ and end at $q$.

For the remainder of this section, we work toward a condition that ensures a uniqueness condition for radial geodesics.

Let $(M,\nabla)$ be a smooth $m$-manifold with a connection, and let $p$ be a point in $M$. The **exponential map at $p$** is the smooth map

$\exp_p:\mathcal{E}_p\longrightarrow M$

defined by

$\exp_p(v)=\gamma_{p,v}(1)$  \hfill (18.9.2)

for all vectors $v$ in $\mathcal{E}_p$.

## Theorem 18.9.3. Let $(M,\nabla)$ be a smooth manifold with a connection, let $p$ be a point in $M$, and let $v$ be a vector in $\mathcal{E}_p$. Let $\gamma_{p,v}(t):(a,b)\to M$ be the maximal geodesic with starting point $p$ and initial velocity $v$, and let $\rho_{p,v}(t):[0,1]\to M$ be the radial geodesic with starting point $p$ and initial velocity $v$. Then

$\gamma_{p,v}(t)=\exp_p(tv)\qquad\text{for all }t\text{ in }(a,b)$

and

$\rho_{p,v}(t)=\exp_p(tv)\qquad\text{for all }t\text{ in }[0,1].$  \hfill (18.9.3)

Proof. We have

t is in (a,b)
⇔ γ_{p,v}(t) is defined
⇔ γ_{p,tv}(1) is defined      [Th 18.8.6]
⇔ exp_p(tv) is defined.      [(18.9.2)]

Thus, when any one of γ_{p,v}(t), γ_{p,tv}(1), or exp_p(tv) is defined, they all are, in which case equality holds:

γ_{p,v}(t) = γ_{p,tv}(1) = exp_p(tv).

This proves the first equality. The second equality follows from (18.9.1).

Roughly speaking, Theorem 18.9.3 says that the exponential function sends lines (or portions of lines) passing through the zero vector of the tangent space at a point to geodesics in the smooth manifold that pass through the point. See Figure 18.9.1.

![](img/d712710a8eb78c0959204afd5dde4c91_519_0.png)

# Figure 18.9.1. Exponential map

By Theorem 14.8.4 and Theorem 18.9.1(a), ℰ_p is an open m-submanifold of T_p(M), and from Theorem 14.3.3(b), we have the identification T_v(ℰ_p) = T_v(T_p(M)) for each vector v in ℰ_p. The differential map of exp_p : ℰ_p ⟶ M at v can therefore be expressed as

d_v(exp_p) : T_v(T_p(M)) ⟶ T_{exp_p(v)}(M).      (18.9.4)

See Figure 18.9.2, and, for context, also see Figure 11.6.1. Let w be a vector in T_p(M), and consider the smooth curve μ_v(t) : (−ε, ε) ⟶ T_p(M) defined by μ_v(t) = v + t w. According to (14.7.4), (dμ_v/dt)(t) is a vector in T_{μ_v(t)}(T_p(M)). In the literature, (dμ_v/dt)(0) is sometimes denoted by w_v. Thus, w_v is a vector in T_v(T_p(M)), and it follows from (18.9.4) that d_v(exp_p)(w_v) is a vector in T_{exp_p(v)}(M). In geometric terms, we can think of T_v(T_p(M)) as the translation of T_p(M) to v, with w_v the corresponding translation of w. It is sometimes convenient to denote w_v simply by w and allow the underlying translation to be understood. As a further simplification, when v = 0, we use (14.3.4) to identify T_0(T_p(M)) with T_p(M), and write

T_0(T_p(M)) = T_p(M).

Since exp_p(0) = p, we have from (18.9.4) that for v = 0 the differential map can be expressed as

d₀(exp_p) : T_p(M) ⟶ T_p(M).

![](img/d712710a8eb78c0959204afd5dde4c91_520_0.png)

# Figure 18.9.2. Differential map of exponential map

## Theorem 18.9.4. If (M, ∇) is a smooth manifold with a connection and p is a point in M, then d₀(exp_p) is the identity map on T_p(M).

Proof. Let v be a vector in T_p(M), and let γ_{p,v}(t) : (a, b) ⟶ M be the maximal geodesic with starting point p and initial velocity v. Consider the smooth curve λ(t) : (a, b) ⟶ E_p defined by λ(t) = t v. By Theorem 18.9.3, γ_{p,v} = exp_p ∘ λ. Since λ(0) = 0 and (dλ/dt)(0) = v, we have from Theorem 14.7.4(a) and Theorem 18.8.2(b) that

d₀(exp_p)(v) = d/dt (exp_p ∘ λ)(0) = dγ_{p,v}/dt (0) = v.

Let (M, ∇) be a smooth manifold with a connection, and let p be a point in M. We say that a neighborhood V of p in M is normal if there is a star-shaped neighborhood \tilde{V} ⊆ E_p of the zero vector in T_p(M) such that exp_p|_{\tilde{V}} : \tilde{V} ⟶ V is a diffeomorphism. For brevity, we often denote

exp_p|_{\tilde{V}} : \tilde{V} ⟶ V   by   exp_p : \tilde{V} ⟶ V.

## Theorem 18.9.5 (Existence of Normal Neighborhood). Every point in a smooth manifold with a connection has a normal neighborhood.

**Proof.** Let $(M,\nabla)$ be a smooth manifold with a connection, and let $p$ be a point in $M$. By Theorem 14.6.2 and Theorem 18.9.4, there is a neighborhood $\widetilde{W}\subset\mathcal{E}_p$ of the zero vector in $T_p(M)$ and a neighborhood $W$ of $p$ in $M$ such that $\exp_p|_{\widetilde{W}}:\widetilde{W}\longrightarrow W$ is a diffeomorphism. Suppose $T_p(M)$ has the standard smooth structure, and let $(T_p(M),A)$ be a (covering) chart on $T_p(M)$, where $A:T_p(M)\longrightarrow\mathbb{R}^m$ is any linear isomorphism. By Theorem 9.1.14, $A(\widetilde{W})$ is an open set in $\mathbb{R}^m$. Let $B\subset A(\widetilde{W})$ be an open ball in $\mathbb{R}^m$ centered at $(0,\ldots,0)$, and observe that $B$ is star-shaped about $(0,\ldots,0)$. Let $v$ be a vector in $A^{-1}(B)$. Then $A(tv)=tA(v)$ is in $B$ for all $t$ in $[0,1]$, hence $tv$ is in $A^{-1}(B)$ for all $t$ in $[0,1]$. Thus, $A^{-1}(B)\subset\mathcal{E}_p$ is star-shaped about the zero vector in $T_p(M)$. Setting $\widetilde{V}=A^{-1}(B)$, we find that $V=\exp_p(A^{-1}(B))$ is a normal neighborhood of $p$ in $M$.

As we now show, radial geodesics on a normal neighborhood have a uniqueness property.

## Theorem 18.9.6 (Uniqueness of Radial Geodesic).

Let $(M,\nabla)$ be a smooth manifold with a connection, let $p$ be a point in $M$, let $V$ be a normal neighborhood of $p$ in $M$, and let $\exp_p:\widetilde{V}\longrightarrow V$ be the corresponding diffeomorphism. Let $q$ be a point in $V$, let $v=\exp_p^{-1}(q)$, and let $\rho_{p,v}:[0,1]\longrightarrow M$ be the radial geodesic with starting point $p$ and initial velocity $v$. Then $\rho_{p,v}$ is the unique radial geodesic such that $\rho_{p,v}(0)=p$, $\rho_{p,v}(1)=q$, and $\rho_{p,v}([0,1])\subset V$.

**Remark.** Since $q$ is in $V$ and $\exp_p^{-1}(V)=\widetilde{V}\subset\mathcal{E}_p$, we see that $v$ is in $\mathcal{E}_p$, hence $\rho_{p,v}$ is defined, so assertion makes sense.

**Proof.** As was observed in connection with (18.9.1), $\rho_{p,v}(0)=p$, and we have from (18.9.3) that $\rho_{p,v}(1)=\exp_p(v)=q$. By assumption, $\widetilde{V}$ is star-shaped about the zero vector in $T_p(M)$ and $\exp_p(\widetilde{V})=V$, so it also follows from (18.9.3) that $\rho_{p,v}(t)$ is in $V$ for all $t$ in $[0,1]$. Thus, $\rho_{p,v}$ satisfies the three specified properties. To show uniqueness, suppose $\rho_{r,w}$ is another radial geodesic satisfying these properties; that is, (i) $\rho_{r,w}(0)=p$, (ii) $\rho_{r,w}(1)=q$, and (iii) $\rho_{r,w}([0,1])\subset V$. We need to show that $r=p$ and $v=w$. It follows from Theorem 18.8.2(a), (18.9.1), and (i) that $r=\rho_{r,w}(0)=p$. We have from $r=p$ and (iii) that $\rho_{p,w}(t)\in V$, hence $\rho_{p,w}$ is defined, and then from (18.9.3) that $\rho_{p,w}(1)=\exp_p(w)$. Then (18.9.3), $r=p$ and (ii) give

$$\exp_p(v)=\rho_{p,v}(1)=q=\rho_{p,w}(1)=\rho_{p,w}(1)=\exp_p(w).$$

Since $\exp_p|_{\widetilde{V}}$ is a diffeomorphism, $v=w$. Thus, $\rho_{r,w}=\rho_{p,v}$.

## Example 18.9.7 (Pln and $S^2$).

We continue with Example 18.9.2. It is clear that $\mathrm{Pln}$ is a normal neighborhood of any of its points. However, the same is not true of $S^2$, because $S^2$ is not homeomorphic (let alone diffeomorphic) to an open set in the Euclidean plane. However, an appropriate subset of $S^2$, such as a hemisphere, is a normal neighborhood of any of its points.
