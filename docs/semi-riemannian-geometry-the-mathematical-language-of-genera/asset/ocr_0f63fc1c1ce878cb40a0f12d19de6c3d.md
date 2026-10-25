Theorem 8.4.8, and Theorem 11.4.1(b) that $H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)} = c_p W_p$ for some nonzero real number $c_p$, hence

$$\langle H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)}, H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)} \rangle = c_p^2 \langle W_p, W_p \rangle$$

for all $p$ in $\varphi(U)$. Since $W$ satisfies [V2], property (*) is satisfied.

(⇐): We have from property (*) that $||H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)}|| \neq 0$ for all $p$ in $\varphi(U)$. Setting $(V,\mathfrak{g}) = \mathbb{R}^3_{\nu}$, $U = T_p(M)$, and $(u_1,u_2) = (H_1|_{\varphi^{-1}(p)}, H_2|_{\varphi^{-1}(p)})$ in Theorem 8.4.9, and using Theorem 11.4.1(b), shows that for all $p$ in $\varphi(U)$, $(\mathfrak{g}|_{\varphi(U)})|_p$ is nondegenerate on $T_p(\varphi(U))$ if and only if $||H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)}|| \neq 0$. Thus, $\mathfrak{g}|_{\varphi(U)}$ satisfies [G1]. For a given point $p$ in $\varphi(U)$, consider the vector

$$V_p = \frac{H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)}}{||H_1|_{\varphi^{-1}(p)} \times H_2|_{\varphi^{-1}(p)}||}.$$

We have from Theorem 8.4.8 that $V_p \neq (0,0,0)$, and from Theorem 8.4.4 and Theorem 11.4.1(b) that $V_p$ is in $T_p(\varphi(U))^{\perp}$. Thus, the assignment $p \longmapsto V_p$ defines a unit normal vector field $V$ along $\varphi(U)$. It follows from property (*) that $V$ satisfies [V2]. By Theorem 12.2.4, $\mathfrak{g}|_{\varphi(U)}$ satisfies [G2].

# Theorem 12.2.7 (Open Set). If $(M,\mathfrak{g})$ is a regular surface in $\mathbb{R}^3_{\nu}$ and $W$ is an open set in $M$, then $(W,\mathfrak{g}|_W)$ is a regular surface in $\mathbb{R}^3_{\nu}$. In particular, if $(U,\varphi)$ is a chart on $M$, then $(\varphi(U),\mathfrak{g}|_{\varphi(U)})$ is a regular surface in $\mathbb{R}^3_{\nu}$.

Proof. By Theorem 11.4.1, $W$ is a regular surface and $T_p(W) = T_p(M)$ for all $p$ in $W$. It follows that $(\mathfrak{g}|_W)|_p$ is nondegenerate on $T_p(W)$ and $\operatorname{ind}(\mathfrak{g}_p) = \operatorname{ind}((\mathfrak{g}|_W)_p)$ for all $p$ in $W$. Thus, $\mathfrak{g}|_W$ satisfies [G1] and [G2], which proves the first assertion. According to [C2] of Section 11.2, $\varphi(U)$ is an open set in $M$, so the second assertion follows from the first.

# Theorem 12.2.8 (Graph of Function). In the notation of Theorem 11.4.2, graph$(f)$ is a regular surface in $\mathbb{R}^3_{\nu}$ if and only if

$$\left(\frac{\partial f}{\partial x}(x,y)\right)^2 + \left(\frac{\partial f}{\partial y}(x,y)\right)^2 - 1$$

is positive for all $(x,y)$ in $U$, or negative for all $(x,y)$ in $U$.

Proof. As remarked in connection with Theorem 12.2.6, the case $\nu = 0$ is straightforward, so assume $\nu = 1$. We have from Example 11.2.5 that graph$(f)$ is a regular surface, from Theorem 11.4.2 that $(U,\varphi)$ is a covering chart, and from the proof of part (b) of Theorem 12.10.2 that

$$\langle H_1|_{(x,y)} \times H_2|_{(x,y)}, H_1|_{(x,y)} \times H_2|_{(x,y)} \rangle = \left(\frac{\partial f}{\partial x}(x,y)\right)^2 + \left(\frac{\partial f}{\partial y}(x,y)\right)^2 - 1.$$

The result now follows from Theorem 12.2.6.