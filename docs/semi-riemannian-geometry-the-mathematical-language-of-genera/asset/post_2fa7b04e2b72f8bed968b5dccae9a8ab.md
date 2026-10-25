
Theorem 10.1.17 and Theorem 11.2.8 that the map

\[ \mu = (\mu^1, \mu^2) = \varphi^{-1} \circ \lambda : (a,b) \longrightarrow U \]

is smooth. By Theorem 10.1.10,

\[ v = \frac{d\lambda}{dt}(t_0) = \frac{d(\varphi \circ \mu)}{dt}(t_0) = \sum_i \frac{d\mu^i}{dt}(t_0) H_i|_q, \]

so $v$ is in the span of $H_1|_q$ and $H_2|_q$. Thus, $T_p(M) \subseteq \operatorname{span}\{H_1|_q, H_2|_q\}$.

$(\supseteq)$: Let $(e_1,e_2)$ be the standard basis for $\mathbb{R}^2$. For given $1 \le i \le 2$, we define a smooth map

\[ \zeta = (\zeta^1,\zeta^2) : (-\varepsilon,\varepsilon) \longrightarrow U \]

by $\zeta(t)=q+t e_i$, where $\varepsilon>0$ is chosen small enough that $(q-\varepsilon e_i,q+\varepsilon e_i)\subset U$. Consider the smooth curve $\lambda(t):(-\varepsilon,\varepsilon)\longrightarrow M$ defined by $\lambda=\varphi\circ\zeta$, and observe that $\lambda(0)=\varphi(q)$. By Theorem 10.1.10,

\[ \frac{d\lambda}{dt}(0)=\frac{d(\varphi\circ\zeta)}{dt}(0)=\sum_j \frac{d\zeta^j}{dt}(0)H_j|_q = H_i|_q, \qquad (11.3.1) \]

so $H_i|_q$ is in $T_p(M)$ for $i=1,2$. Thus, $\operatorname{span}\{H_1|_q, H_2|_q\}\subseteq T_p(M)$.

This proves the claim. We have from [C1] of Section 11.2 and Theorem 11.2.2 that $H_1|_q$ and $H_2|_q$ are linearly independent. The result follows.

(c): This follows from Theorem 11.2.1(b) and part (b).

(d): We have from part (c) that $d_q(\varphi):\mathbb{R}^2\longrightarrow T_p(M)$ is well-defined, and from [C1] of Section 11.2 that it is injective. The result now follows from part (a) and Theorem 1.1.14.

In the notation of Theorem 11.3.1, let us denote

\[ (H_1, H_2) \quad \text{by} \quad \mathcal{H}, \]

and

\[ (H_1|_q, H_2|_q) \quad \text{by} \quad \mathcal{H}_q. \]

We refer to $\mathcal{H}$ as the coordinate frame corresponding to $(U,\varphi)$. Although there is a tendency to think of $T_p(M)$ as literally “tangent” to $M$ at the point $p$, by Theorem 11.3.1(a), $T_p(M)$ is a subspace of $\mathbb{R}^3$. As such, $T_p(M)$ passes through the origin $(0,0,0)$ of $\mathbb{R}^3$. In geometric terms, it is $T_p(M)+p$, the translation of $T_p(M)$ by $p$, that is tangent to $M$ at $p$. That said, it is convenient in the figures to label tangent planes as $T_p(M)$ rather than $T_p(M)+p$.

## Example 11.3.2 ($\mathcal{S}^2$).

Continuing with Example 11.2.6, consider the chart $(D,\varphi_1)$, where $\varphi_1(x,y)=(x,y,\sqrt{1-x^2-y^2})$. The corresponding coordinate frame is given by

\[ \left(\frac{\partial\varphi_1}{\partial x}(x,y),\frac{\partial\varphi_1}{\partial y}(x,y)\right)=\left(\left(1,0,-\frac{x}{\sqrt{1-x^2-y^2}}\right),\left(0,1,-\frac{y}{\sqrt{1-x^2-y^2}}\right)\right). \]

For example, at $\varphi_1(0,0)=(0,0,1)$, the coordinate basis is $((1,0,0),(0,1,0))$, which spans the $xy$-plane in $\mathbb{R}^3$.

### Theorem 11.3.3 (Change of Coordinate Basis).

Let $M$ be a regular surface, let $p$ be a point in $M$, let $(U,\varphi)$ and $(\widetilde U,\widetilde\varphi)$ be charts at $p$, and let $\mathcal H$ and $\widetilde{\mathcal H}$ be the corresponding coordinate frames. Then

$$[\operatorname{id}_{T_p(M)}]_{\mathcal H_q}^{\widetilde{\mathcal H}_{\widetilde q}}=J_{\widetilde\varphi^{-1}\circ\varphi}(q),$$

where $q=\varphi^{-1}(p)$ and $\widetilde q=\widetilde\varphi^{-1}(p)$.

**Proof.** Let $\mathcal H=(H_1,H_2)$ and $\widetilde{\mathcal H}=(\widetilde H_1,\widetilde H_2)$, and let $(r^1,r^2)$ and $(\widetilde r^1,\widetilde r^2)$ be coordinates on $U$ and $\widetilde U$, respectively. By Theorem 11.2.9, the transition map $F=(F^1,F^2)=\widetilde\varphi^{-1}\circ\varphi$ is a diffeomorphism, hence smooth. It follows from Theorem 10.1.10 that

$$H_{\widetilde j\mid q}=\frac{\partial\varphi}{\partial r^j}(q)=\frac{\partial(\widetilde\varphi\circ F)}{\partial r^j}(q)=\sum_i\frac{\partial F^i}{\partial r^j}(q)\widetilde H_{i\mid\widetilde q},$$

and then from (2.2.6), (2.2.7), and (10.1.2) that

$$[\operatorname{id}_{T_p(M)}]_{\mathcal H_q}^{\widetilde{\mathcal H}_{\widetilde q}}=\left[\frac{\partial F^i}{\partial r^j}(q)\right]=J_{\widetilde\varphi^{-1}\circ\varphi}(q).$$

### Example 11.3.4 (Polar Coordinates).

The $xy$-plane in $\mathbb R^3$, denoted here by $\operatorname{Pln}$, is clearly a regular surface. Consider the open set

$$U=\{(\rho,\phi)\in\mathbb R^2:\rho>0,\ 0<\phi<2\pi\}$$

in $\mathbb R^2$, and define a map $\varphi:U\to\mathbb R^3$ by

$$\varphi(\rho,\phi)=(\rho\cos(\phi),\rho\sin(\phi),0)$$

for all $(\rho,\phi)$ in $U$. The image of $\varphi$ is $\operatorname{Pln}$ with the nonnegative $x$-axis removed. It is easily shown that $(U,\varphi)$ is a chart on $\operatorname{Pln}$, which we call **polar coordinates**.

A covering chart on $\operatorname{Pln}$ is given by $(\widetilde U=\mathbb R^2,\widetilde\varphi)$, where $\widetilde\varphi:\mathbb R^2\to\operatorname{Pln}$ is defined by $\widetilde\varphi(x,y)=(x,y,0)$. The coordinate frames corresponding to $(U,\varphi)$ and $(\widetilde U,\widetilde\varphi)$ are given by

$$\mathcal H_{(\rho,\phi)}=\left(\frac{\partial\varphi}{\partial\rho}(\rho,\phi),\frac{\partial\varphi}{\partial\phi}(\rho,\phi)\right)=\big((\cos(\phi),\sin(\phi),0),(-\rho\sin(\phi),\rho\cos(\phi),0)\big)$$

and

$$\widetilde{\mathcal H}_{(x,y)}=\left(\frac{\partial\widetilde\varphi}{\partial x}(x,y),\frac{\partial\widetilde\varphi}{\partial y}(x,y)\right)=((1,0,0),(0,1,0)),$$

respectively. The transition map $\widetilde\varphi^{-1}\circ\varphi:U\to\widetilde U$ is defined by

$$\widetilde\varphi^{-1}\circ\varphi(\rho,\phi)=(\rho\cos(\phi),\rho\sin(\phi)).$$

Classically, $x$ and $y$ are viewed as functions of $\rho$ and $\phi$, and expressed as

$$x(\rho,\phi)=\rho\cos(\phi)\qquad\text{and}\qquad y(\rho,\phi)=\rho\sin(\phi).$$

## Open set in a regular surface.

As we now show, an open set in a regular surface is itself a regular surface.

## Theorem 11.4.1 (Open Set). Let M be a regular surface, let V be an open set in M, and view V as a topological subspace of ℝ³. Then:

- (a) V is a regular surface.
- (b) For all points p in V, T_p(V) = T_p(M).

Proof. (a): Let p be a point in V, and let (U, φ) be a chart on M at p. We have properties [C1]–[C3] of Section 11.2 at our disposal. According to [C2], φ(U) is open in M, and therefore, so is φ(U) ∩ V. It follows from [C3] and Theorem 9.1.14 that U′ = φ⁻¹(V ∩ φ(U)) is open in U. By Theorem 11.2.10, (U′, φ|U′) is a chart at p such that φ|U′(U′) ⊆ V. Since p was arbitrary, the result follows.
(b): Let v be a vector in T_p(V), and let λ(t) : (a, b) ⟶ V be a smooth curve such that λ(t₀) = p and (dλ/dt)(t₀) = v for some t₀ in (a, b). Since V ⊆ M, the same smooth curve shows that v is also a vector in T_p(M). Thus, T_p(V) ⊆ T_p(M). By Theorem 11.3.1(a), both T_p(V) and T_p(M) are 2-dimensional, and then Theorem 1.1.7(b) gives T_p(V) = T_p(M).

## Throughout, any open set in a regular surface is viewed as a regular surface.

## Graph of a function.

According to Theorem 11.2.11, a regular surface is covered by graphs of functions. We now consider the graph of a function in isolation. Let U be an open set in ℝ², and let f be a function in C^∞(U). Recall from Theorem 11.2.11 that

graph(f) = { (x, y, f(x, y)) ∈ ℝ³ : (x, y) ∈ U },

where we identify ℝ² with the xy-plane in ℝ³. Defining a map φ : U ⟶ ℝ³ by

φ(x, y) = (x, y, f(x, y))

for all (x, y) in U, we see that graph(f) is the image of φ; that is,

graph(f) = φ(U).

## Theorem 11.4.2 (Graph of Function). With the above setup, graph(f), viewed as a topological subspace of ℝ³, is a regular surface, and (U, φ) is a covering chart on graph(f).

Proof. Clearly, φ : U ⟶ ℝ³ is a parametrized surface. We need to prove that:

- [C1] φ : U ⟶ ℝ³ is an immersion.
- [C2] φ(U) is an open set in graph(f).
- [C3] φ : U ⟶ φ(U) is a homeomorphism.

The proofs are as follows:

[C1]: The Jacobian matrix is

| 1 | 0 |
| 0 | 1 |
| ∂f/∂x (x,y) | ∂f/∂y (x,y) |

and the corresponding vector product is

```
(1,0,∂f/∂x(x,y)) × (1,0,∂f/∂y(x,y)) = (-∂f/∂x(x,y), -∂f/∂y(x,y),1)
```

which never equals (0,0,0). By Theorem 11.2.2, φ is an immersion.

[C2]: This follows from φ(U) = graph(f).

[C3]: Clearly, φ is bijective. Since φ is smooth, by Theorem 10.1.7 it is continuous. Let P: ℝ³ ⟶ ℝ² be the projection map defined by P(x,y,z) = (x,y), where we identify ℝ² with the xy-plane in ℝ³. Since P is continuous and φ⁻¹|graph(f) = P|graph(f), the result follows.

Thus, (U,φ) is a chart on graph(f), and since φ(U) = graph(f), it is a covering chart.

## Surface of revolution.

Let ρ(t), h(t) : (a,b) ⟶ ℝ be smooth functions such that:

- [R1] ρ is strictly positive on (a,b).
- [R2] h is strictly increasing or strictly decreasing on (a,b).

We refer to ρ and h as the radius function and height function, respectively. Throughout, it is convenient to denote the derivatives of ρ and h with respect to t by an overdot. Consider the smooth curve σ(t) : (a,b) ⟶ ℝ³ defined by

```
σ(t) = (ρ(t),0,h(t))
```

for all t in (a,b). We observe that [R2] is equivalent to ḣ being strictly positive or strictly negative on (a,b), from which it follows that σ is a regular curve. Let

```
U = (a,b) × (-π,π)
```

and consider the smooth map φ : U ⟶ ℝ³ defined by

```
φ(t,φ) = (ρ(t)cos(φ), ρ(t)sin(φ), h(t))
```

The surface of revolution corresponding to σ is denoted by rev(σ) and defined to be the image of φ:

```
rev(σ) = φ(U)
```

Thus, rev(σ) is obtained by revolving the image of σ around the z-axis. A remark is that (-π,π) was chosen when defining U rather than, for example, [-π,π) or [0,2π), to ensure that U is an open set in ℝ². As a result, a surface of revolution does not quite make a complete circuit around the z-axis.
