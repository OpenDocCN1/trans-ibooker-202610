(b)

$\left[\mathrm{id}_{T_p(M)}\right]_{\tilde{\mathcal{X}}_p}^{\tilde{\mathcal{X}}_p} = \begin{bmatrix} \frac{\partial \tilde{x}^1}{\partial x^1}(p) & \cdots & \frac{\partial \tilde{x}^1}{\partial x^m}(p) \\ \vdots & \ddots & \vdots \\ \frac{\partial \tilde{x}^m}{\partial x^1}(p) & \cdots & \frac{\partial \tilde{x}^m}{\partial x^m}(p) \end{bmatrix} = \begin{bmatrix} \frac{\partial (\tilde{x}^1 \circ \varphi^{-1})}{\partial r^1}(\varphi(p)) & \cdots & \frac{\partial (\tilde{x}^1 \circ \varphi^{-1})}{\partial r^m}(\varphi(p)) \\ \vdots & \ddots & \vdots \\ \frac{\partial (\tilde{x}^m \circ \varphi^{-1})}{\partial r^1}(\varphi(p)) & \cdots & \frac{\partial (\tilde{x}^m \circ \varphi^{-1})}{\partial r^m}(\varphi(p)) \end{bmatrix}.$

*Remark.* The second equalities in parts (a) and (b), both of which follow from (14.3.2), are included as a reminder that partial derivatives are ultimately computed using Euclidean methods.

*Proof.* (a): Let $[a_j^i] = [\mathrm{id}_{T_p(M)}]_{\mathcal{X}_p}^{\tilde{\mathcal{X}}_p}$, so that from (2.2.6) and (2.2.7),

$\left.\frac{\partial}{\partial x^j}\right|_p = \sum_k a_j^k \left.\frac{\partial}{\partial \tilde{x}^k}\right|_p.$

Applying both sides of the preceding identity to $\tilde{x}^i$ and using Theorem 14.3.1(b) and (14.3.3) yields

$\frac{\partial \tilde{x}^i}{\partial x^j}(p) = \sum_k a_j^k \frac{\partial \tilde{x}^i}{\partial \tilde{x}^k}(p) = a_j^i.$

(b): This follows from part (a).

Let $M$ be a smooth $m$-manifold, let $p$ be a point in $M$, and let $(U, \varphi = (x^i))$ be a chart at $p$. The **second order partial derivative with respect $x^i$ and $x^j$ at $p$** is the map

$\left.\frac{\partial^2}{\partial x^i \partial x^j}\right|_p : C^\infty(M) \longrightarrow \mathbb{R}$

defined by

$\frac{\partial^2 f}{\partial x^i \partial x^j}(p) = \frac{\partial}{\partial x^i}\left(\frac{\partial f}{\partial x^j}\right)(p)$

for all functions $f$ in $C^\infty(M)$, where we denote

$\left.\frac{\partial^2}{\partial x^i \partial x^j}\right|_p (f)\quad\text{by}\quad \frac{\partial^2 f}{\partial x^i \partial x^j}(p).$

# **Theorem 14.3.6 (Equality of Mixed Partial Derivatives).** *With the above setup,*

$\frac{\partial^2 f}{\partial x^i \partial x^j}(p) = \frac{\partial^2 f}{\partial x^j \partial x^i}(p)$

*for $i, j = 1, \ldots, m$.*

*Proof.* This follows from Theorem 10.1.6.