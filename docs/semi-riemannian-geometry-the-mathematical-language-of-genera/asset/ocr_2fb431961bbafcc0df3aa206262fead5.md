Substituting (15.13.5) and (15.13.6) into (15.13.4) and reversing summations gives the result.
(b): This follows from part (a).

# Example 15.13.4 (Polar Coordinates).

Let $(r,s)$ be standard coordinates on $\mathbb{R}^2$, and let $(\rho,\phi)$ be polar coordinates on

$$U = \{(\rho,\phi) \in \mathbb{R}^2 : \rho > 0, 0 \le \phi < 2\pi\}.$$

Let $F : U \longrightarrow \mathbb{R}^2$ be the smooth map given by

$$F(\rho,\phi) = (\rho \cos(\phi), \rho \sin(\phi)),$$

and consider the form

$$\omega_{(r,s)} = d(r) \wedge d(s)$$

in $\Lambda^2(\mathbb{R}^2)$. Setting $(U,(x^i)) = (U,(\rho,\phi))$ and $(V,(y^j)) = (\mathbb{R}^2,(r,s))$ in Theorem 15.13.3(b) yields

$F^*(d(r) \wedge d(s)) = \det\begin{pmatrix} \dfrac{\partial(r \circ F)}{\partial \rho} & \dfrac{\partial(r \circ F)}{\partial \phi} \\ \dfrac{\partial(s \circ F)}{\partial \rho} & \dfrac{\partial(s \circ F)}{\partial \phi} \end{pmatrix} d(\rho) \wedge d(\phi)$

$= \det\begin{pmatrix} \dfrac{\partial(\rho \cos(\phi))}{\partial \rho} & \dfrac{\partial(\rho \cos(\phi))}{\partial \phi} \\ \dfrac{\partial(\rho \sin(\phi))}{\partial \rho} & \dfrac{\partial(\rho \sin(\phi))}{\partial \phi} \end{pmatrix} d(\rho) \wedge d(\phi)$

$= \det\begin{pmatrix} \cos(\phi) & -\rho \sin(\phi) \\ \sin(\phi) & \rho \cos(\phi) \end{pmatrix} d(\rho) \wedge d(\phi)$

$= \rho \, d(\rho) \wedge d(\phi).$

# 15.14 Contraction of Tensor Fields

This brief section presents the manifold versions of several of the results in Section 5.4.

# Theorem 15.14.1.

If $M$ is a smooth manifold, then there is a unique $C^\infty(M)$-linear map

$$\mathrm{C}_1^1 : \mathcal{T}_1^1(M) \longrightarrow C^\infty(M),$$

called $(1,1)$-contraction, such that

$$\mathrm{C}_1^1(X \otimes \omega) = \omega(X)$$

for all vector fields $X$ in $\mathfrak{X}(M)$ and all covector fields $\omega$ in $\mathfrak{X}^*(M)$.