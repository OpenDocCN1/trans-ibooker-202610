To give (10.1.4) a more traditional appearance, we continue with the above notation, let $(z^1,\dots,z^k)$ be standard coordinates on $\mathbb{R}^k$, and replace $$G^i \circ F(p) = G^i\left(F^1(p),\dots,F^n(p)\right)$$ with $$z^i = z^i\left(y^1(x^1,\dots,x^m),\dots,y^n(x^1,\dots,x^m)\right).$$ Then (10.1.4) can be expressed as $$\frac{\partial z^i}{\partial x^j} = \sum_{l=1}^n \frac{\partial z^i}{\partial y^l}\frac{\partial y^l}{\partial x^j}.$$

Let $U$ be an open set in $\mathbb{R}^m$, and let $F = (F^1,\dots,F^n) : U \longrightarrow \mathbb{R}^n$ be a map. We say that $F$ is smooth (on $U$) if the function $F^i$ is smooth for $i = 1,\dots,m$. Smooth maps will be our focus for the rest of the book.

The next result, which says that smoothness is ultimately a local phenomenon, will be used frequently, but usually without attribution.

# Theorem 10.1.11. Let $U$ be an open set in $\mathbb{R}^m$, and let $F : U \longrightarrow \mathbb{R}^m$ be a map. Then:

+   (a) If $F$ is smooth and $U'$ is an open set in $U$, then $F|_{U'}$ is smooth.
+   (b) Conversely, if every point $p$ in $U$ has a neighborhood $U'$ in $U$ such that $F|_{U'}$ is smooth, then $F$ is smooth.

# Theorem 10.1.12. Let $U$ and $V$ be open sets in $\mathbb{R}^m$ and $\mathbb{R}^n$, respectively, and let $F : U \longrightarrow \mathbb{R}^n$ and $G : V \longrightarrow \mathbb{R}^k$ be maps such that $F(U) \subseteq V$. If $F$ and $G$ are smooth, then so is $G \circ F$.

A (parametrized) curve in $\mathbb{R}^m$ is a map $\lambda : I \longrightarrow \mathbb{R}^m$, where $I$ is an interval in $\mathbb{R}$ that is either open, closed, half-open, or half-closed, and where the possibility that $I$ is infinite is not excluded. Our focus will be on the case where $I$ is a finite open interval, usually denoted by $(a,b)$. Rather than provide a separate statement identifying the independent variable for the curve, most often denoted by $t$, and sometimes by $u$, it is convenient to incorporate this into the notation for $\lambda$, as in $\lambda(t) : (a,b) \longrightarrow \mathbb{R}^3$. Let $\lambda = (\lambda^1,\dots,\lambda^m)$. By definition, $\lambda$ is smooth [on $(a,b)$] if and only if $\lambda^i$ is smooth for $i = 1,\dots,m$.

Suppose $\lambda$ is in fact smooth. The (Euclidean) velocity of $\lambda$ and the (Euclidean) acceleration of $\lambda$ are the smooth curves $$\frac{d\lambda}{dt}(t) : (a,b) \longrightarrow \mathbb{R}^m \quad \text{and} \quad \frac{d^2\lambda}{dt^2}(t) : (a,b) \longrightarrow \mathbb{R}^m,$$ respectively; that is, $$\frac{d\lambda}{dt}(t) = \left(\frac{d\lambda^1}{dt}(t),\dots,\frac{d\lambda^m}{dt}(t)\right)$$ and $$\frac{d^2\lambda}{dt^2}(t) = \left(\frac{d^2\lambda^1}{dt^2}(t),\dots,\frac{d^2\lambda^m}{dt^2}(t)\right)$$ for all $t$ in $(a,b)$.