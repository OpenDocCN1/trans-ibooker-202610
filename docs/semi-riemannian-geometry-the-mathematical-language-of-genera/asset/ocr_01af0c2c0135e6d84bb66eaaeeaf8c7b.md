# Theorem 12.6.1. Let $(M,\mathfrak{g})$ be a regular surface in $\mathbb{R}^3_\nu$, let $(U,\varphi)$ be a chart on $M$, let $X,Y$ be vector fields in $\mathfrak{X}(M)$, and, in local coordinates, let

$X \circ \varphi = \sum_i \alpha^i H_i \quad \text{and} \quad Y \circ \varphi = \sum_j \beta^j H_j.$

Then

$[X,Y] \circ \varphi = \sum_j \left\{\sum_i \left(\alpha^i \frac{\partial \beta^j}{\partial r^i} - \beta^i \frac{\partial \alpha^j}{\partial r^i}\right)\right\} H_j$

$= \sum_j \left(\sum_i (\alpha^i \beta^j_{;i} - \beta^i \alpha^j_{;i})\right) H_j.$

Proof. By Theorem 12.4.2(c),

$\nabla_X(Y) \circ \varphi = \sum_j \left(\sum_i \alpha^i \beta^j_{;i}\right) H_j$

and

$\nabla_Y(X) \circ \varphi = \sum_j \left(\sum_i \beta^i \alpha^j_{;i}\right) H_j,$

hence

$[X,Y] \circ \varphi = \nabla_X(Y) \circ \varphi - \nabla_Y(X) \circ \varphi = \sum_j \left(\sum_i (\alpha^i \beta^j_{;i} - \beta^i \alpha^j_{;i})\right) H_j.$

We have from (12.3.9) that

$\sum_i (\alpha^i \beta^j_{;i} - \beta^i \alpha^j_{;i}) = \sum_i \left\{\alpha^i \left(\frac{\partial \beta^j}{\partial r^i} + \sum_k \beta^k \Gamma^j_{ik}\right) - \beta^i \left(\frac{\partial \alpha^j}{\partial r^i} + \sum_k \alpha^k \Gamma^j_{ik}\right)\right\}$

$= \sum_i \left(\alpha^i \frac{\partial \beta^j}{\partial r^i} - \beta^i \frac{\partial \alpha^j}{\partial r^i}\right) + \sum_{ik} \alpha^i \beta^k \Gamma^j_{ik} - \sum_{ik} \alpha^k \beta^i \Gamma^j_{ik}$

$= \sum_i \left(\alpha^i \frac{\partial \beta^j}{\partial r^i} - \beta^i \frac{\partial \alpha^j}{\partial r^i}\right),$

where the last equality relies on Theorem 12.3.3(d). The result follows.

The next result shows that the Lie bracket on a regular surface in $\mathbb{R}^3_\nu$, formulated above in terms of the covariant derivative, can also be expressed in terms of the induced Euclidean derivative.

# Theorem 12.6.2. If $(M,\mathfrak{g})$ is a regular surface in $\mathbb{R}^3_\nu$ and $X,Y$ are vector fields in $\mathfrak{X}(M)$, then

$\nabla_X(Y) - \nabla_Y(X) = D_X(Y) - D_Y(X).$