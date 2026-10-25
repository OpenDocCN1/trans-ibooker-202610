for all $(t,p)$ in $D$. For each $t$ in $\mathbb{R}$, let

$M_t = \{p \in M : t \in (a_p, b_p)\}$,

and consider the map

$\Phi_t : M_t \longrightarrow M$

defined by

$\Phi_t(p) = \Phi(t,p)$

for all $p$ in $M_t$. We note that for sufficiently large $t$, $M_t$ might be empty.

In physical terms, $M_t$ is the set of points $p$ in $M$ for which a “particle” starting at $p$ is able to “flow” for $t$ units under the “action” of $\Phi$. It is helpful to visualize $\Phi_t$ as a map that causes a portion of the vector field to “flow” bodily across the underlying smooth manifold for a “distance” of $t$ units. We observe that with the above definitions,

$\Phi_p(t) = \Phi(t,p) = \Phi_t(p).$ \quad (16.10.3)

# Theorem 16.10.3 (Fundamental Theorem on Flows). Let $M$ be a smooth manifold, let $X$ be a vector field in $\mathfrak{X}(M)$, and let $\Phi(t,p) : D \longrightarrow M$ be the maximal flow of $X$. Then:

+   (a) $D$ is an open set in $\mathbb{R} \times M$.
+   (b) $\Phi$ is smooth.
+   (c) $\Phi$ satisfies the group laws: for all $p$ in $M$, $\Phi_p(0) = p$, and for all $s$ in $(a_p, b_p)$ and $t$ in $(a_{\Phi_p(s)}, b_{\Phi_p(s)})$ such that $s+t$ is in $(a_p, b_p)$, $\Phi_{\Phi_p(s)}(t) = \Phi_p(s+t)$.
+   (d) For each $t$ in $\mathbb{R}$ such that $M_t$ is nonempty, $M_t$ is an open set in $M$ and $\Phi_t : M_t \longrightarrow M_{-t}$ is a diffeomorphism, with inverse $(\Phi_t)^{-1} = \Phi_{-t}$.

Theorem 16.10.3 says that the maximal integral curves corresponding to a vector field combine to form a smooth map defined on an open set, and that the “flow” of particles referred to above takes place in a “diffeomorphic” fashion.

We close this section with a pair of results that demonstrate the surprisingly close connection between maximal flows and Lie derivatives.

# Theorem 16.10.4 (Lie Derivative of Function). Let $M$ be a smooth manifold, let $X$ be a vector field in $\mathfrak{X}(M)$, let $\Phi : D \longrightarrow M$ be the maximal flow of $X$, and let $f$ be a function in $C^{\infty}(M)$. Then

$\mathcal{L}_X(f)(p) = \lim_{t \to 0} \frac{(\Phi_t)^\bullet(f)(p) - f(p)}{t}$

for all $p$ in $M$.