defined by

$$D_X(f)(p)=d_p(f)(X_p)$$  (10.3.2)

for all functions $f$ in $C^\infty(U)$ and all $p$ in $U$. The second is

$$D_X:\mathfrak{X}(U)\longrightarrow\mathfrak{X}(U)$$

defined by

$$D_X(Y)_p=d_p(Y)(X_p)$$  (10.3.3)

for all vector fields $Y$ in $\mathfrak{X}(U)$ and all $p$ in $U$. It follows from Theorem 10.1.14(a) that (10.3.2) and (10.3.3) can be expressed as

$$D_X(f)(p)=\frac{d(f\circ\lambda)}{dt}(t_0)$$

and

$$D_X(Y)_p=\frac{d(Y\circ\lambda)}{dt}(t_0),$$

respectively, where $\lambda(t):(a,b)\longrightarrow U$ is any smooth curve such $\lambda(t_0)=p$ and $(d\lambda/dt)(t_0)=X_p$ for some $t_0$ in $(a,b)$. The existence of such a smooth curve is guaranteed by Theorem 10.1.13.

Evidently, the Euclidean derivatives of $f$ and $Y$ with respect to $X$ evaluated at $p$ have the same mathematical content as the differentials at $p$ of $f$ and $Y$ evaluated at $X_p$. Their difference, such as it is, amounts to a change of notation that emphasizes the role of $X$ in the Euclidean derivative.

For vector fields $X,Y$ in $\mathfrak{X}(U)$, define a function

$$\langle X,Y\rangle:U\longrightarrow\mathbb{R}$$

in $C^\infty(U)$ by the assignment

$$p\longmapsto\langle X_p,Y_p\rangle$$

for all $p$ in $U$.

The Euclidean derivative with respect to $X$ satisfies fundamental algebraic properties, versions of which will reappear later in several settings.

# **Theorem 10.3.1.**

Let $U$ be an open set in $\mathbb{R}^m$, let $X,Y,Z$ be vector fields in $\mathfrak{X}(U)$, and let $f$ be a function in $C^\infty(U)$. Then:

+   (a) $D_{X+Y}(Z)=D_X(Z)+D_Y(Z)$.  
+   (b) $D_{fX}(Y)=fD_X(Y)$.  
+   (c) $D_X(Y+Z)=D_X(Y)+D_X(Z)$.  
+   (d) $D_X(fY)=D_X(f)Y+fD_X(Y)$.  
+   (e) $D_X(\langle Y,Z\rangle)=\langle D_X(Y),Z\rangle+\langle Y,D_X(Z)\rangle$.

> *Proof.* Using Theorem 10.1.3 and Theorem 10.1.8(b) gives the result.