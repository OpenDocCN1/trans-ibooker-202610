Because $p$ and $v$ were arbitrary, it follows that $V$ is a normal vector field along $M$ and $T_p(M)\subseteq (\mathbb{R}V_p)^\perp$ for all $p$ in $M$. We have from parts (a) and (c) of Theorem 12.2.1 that $V$ is nowhere-vanishing on $M$ and $W=V/\|V\|$ is a unit normal vector field along $M$. Since $f$ is smooth, so is $V$, and therefore, by Theorem 12.2.1(c), so is $W$; that is, $W$ is a vector field in $\mathfrak{X}_{\mathbb{R}^3_\nu}(M)$. Clearly, $W$ satisfies [V2]. Since $T_p(M)\subseteq (\mathbb{R}V_p)^\perp=(\mathbb{R}W_p)^\perp$, and by Theorem 11.3.1(a), $T_p(M)$ is 2-dimensional, it follows that $(\mathbb{R}W_p)^\perp$ is either 2-dimensional or 3-dimensional. The latter possibility is excluded because we would then have $(\mathbb{R}W_p)^\perp=\mathbb{R}^3_\nu$, hence $\langle W_p,W_p\rangle=0$, which contradicts the fact that $W$ satisfies [V2]. Thus, $(\mathbb{R}W_p)^\perp$ is 2-dimensional, so $T_p(M)=(\mathbb{R}W_p)^\perp$. By Theorem 4.1.2(c), $T_p(M)=(\mathbb{R}V_p)^\perp$ for all $p$ in $M$; that is, $W$ satisfies [V1]. It follows from part (a) and Theorem 12.2.5 that $(M,g)$ is a regular surface in $\mathbb{R}^3_\nu$.

Let $(M,g)$ be a regular surface in $\mathbb{R}^3_\nu$. We have from Theorem 12.2.5 that there is a (not necessarily smooth) unit normal vector field $V$ along $M$ satisfying [V1] and [V2]. As pointed out in the proof of Theorem 12.2.4, since $V$ is a unit vector field, [V2] is equivalent to: $\langle V_p,V_p\rangle=1$ for all $p\in M$, or $\langle V_p,V_p\rangle=-1$ for all $p\in M$. The common value of the $\langle V_p,V_p\rangle$ is denoted by $\epsilon_M$ and called the sign of $M$. Thus,

$$\epsilon_M=\langle V_p,V_p\rangle$$

for all $p\in M$. By Theorem 12.2.3(b), $\epsilon_M$ is independent of the choice of unit normal vector field along $M$ satisfying [V1] and [V2]. We have from Theorem 12.2.2(b) that

$$\epsilon_M=(-1)^{\nu-\operatorname{ind}(g)}.$$

A convenient way to determine $\operatorname{ind}(g)$ that avoids having to construct an orthonormal basis is to find $\epsilon_M$ using (12.2.3) and then compute $\operatorname{ind}(g)$ using (12.2.4). The values of $\nu$, $\operatorname{ind}(g)$, and $\epsilon_M$ are related to each other as follows:

| $\nu$ | $\operatorname{ind}(g)$ | $\epsilon_M$ |
|---:|---:|---:|
| 0 | 0 | 1 |
| 1 | 1 | 1 |
| 1 | 0 | $-1$ |

# **Theorem 12.2.11.**

*Let $(M,g)$ be a regular surface in $\mathbb{R}^3_\nu$, let $(U,\varphi)$ be a chart on $M$, let $(H_1,H_2)$ be the corresponding coordinate frame, and, using Example 11.2.5, view $\varphi(U)$ as a regular surface. Define a map $G:U\longrightarrow\mathbb{R}^3_\nu$, called the coordinate unit normal vector field corresponding to $(U,\varphi)$, by*

$$G_q=\epsilon_M\frac{H_1|_q\times H_2|_q}{\|H_1|_q\times H_2|_q\|}$$

for all $q\in U$, where the vector product is computed with respect to the standard basis for $\mathbb{R}^3_\nu$. Then: