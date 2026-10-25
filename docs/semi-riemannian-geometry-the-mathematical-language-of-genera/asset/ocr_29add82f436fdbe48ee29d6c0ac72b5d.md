# 18.7 Curvature Tensors

15.3.2(d) and Theorem 18.1.2 that

$$\begin{aligned}
R(fX,Y)Z &= \nabla_X(\nabla_Y(Z))-\nabla_Y(\nabla_X(Z))-\nabla_{[X,Y]}(Z)\\
&=f\nabla_X(\nabla_Y(Z))-\nabla_Y(f\nabla_X(Z))-\nabla_{[fX,Y]-Y(f)X}(Z)\\
&=f\nabla_X(\nabla_Y(Z))-\{Y(f)\nabla_X(Z)+f\nabla_Y(\nabla_X(Z))\}\\
&\quad-(f\nabla_{[X,Y]}(Z)-Y(f)\nabla_X(Z))\\
&=f(\nabla_X(\nabla_Y(Z))-\nabla_Y(\nabla_X(Z))-\nabla_{[X,Y]}(Z))\\
&=fR(X,Y)Z.
\end{aligned}$$

Likewise,

$$R(X,fY)Z=fR(X,Y)Z.$$

We also have

$$\begin{aligned}
R(X,Y)(fZ)&=\nabla_X(\nabla_Y(fZ))-\nabla_Y(\nabla_X(fZ))-\nabla_{[X,Y]}(fZ),\\
\nabla_X(\nabla_Y(fZ))&=\nabla_X(Y(f)Z+f\nabla_Y(Z))=\nabla_X(Y(f)Z)+\nabla_X(f\nabla_Y(Z))\\
&=X(Y(f))Z+Y(f)\nabla_X(Z)+X(f)\nabla_Y(Z)+f\nabla_X(\nabla_Y(Z)),\\
\nabla_Y(\nabla_X(fZ))&=Y(X(f))Z+X(f)\nabla_Y(Z)+Y(f)\nabla_X(Z)+f\nabla_Y(\nabla_X(Z)),
\end{aligned}$$

and

$$\begin{aligned}
\nabla_{[X,Y]}(fZ)&=[X,Y](f)Z+f\nabla_{[X,Y]}(Z)\\
&=X(Y(f))Z-Y(X(f))Z+f\nabla_{[X,Y]}(Z),
\end{aligned}$$

hence

$$\begin{aligned}
R(X,Y)(fZ)&=f\{\nabla_X(\nabla_Y(Z))-\nabla_Y(\nabla_X(Z))-\nabla_{[X,Y]}(Z)\}\\
&=fR(X,Y)(Z).
\end{aligned}$$

Thus, $R$ is $C^\infty(M)$-multilinear.

Let $(M,\nabla)$ be a smooth $m$-manifold with a connection, and let $(U,(x^i))$ be a chart on $M$. In local coordinates, $R(\partial/\partial x^i,\partial/\partial x^j)\partial/\partial x^k$ can be expressed as

$$R\left(\frac{\partial}{\partial x^i},\frac{\partial}{\partial x^j}\right)\frac{\partial}{\partial x^k}=\sum_l R^l_{ijk}\frac{\partial}{\partial x^l}. \tag{18.7.2}$$

where the $R^l_{ijk}$ are uniquely determined functions in $C^\infty(U)$.

# Theorem 18.7.2 (Local Coordinate Expression for Curvature Tensor).

_With the above setup,_

$$R^l_{ijk}=\frac{\partial\Gamma^l_{jk}}{\partial x^i}-\frac{\partial\Gamma^l_{ik}}{\partial x^j}+\sum_n(\Gamma^l_{in}\Gamma^n_{jk}-\Gamma^l_{jn}\Gamma^n_{ik})$$

for $i,j,k,l=1,\ldots,m$.