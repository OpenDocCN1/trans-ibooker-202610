**Proof.** Let $(\xi^1,\ldots,\xi^m)$ and $(\varphi^1,\ldots,\varphi^m)$ be the dual bases corresponding to $\mathcal E$ and $\mathcal F$, respectively. We have

$$\varpi(e_1,\ldots,e_m)\,\xi^1\wedge\cdots\wedge\xi^m$$

$$=\varpi(f_1,\ldots,f_m)\,\varphi^1\wedge\cdots\wedge\varphi^m \qquad [\mathrm{Thm\ 7.2.13(b)}]$$

$$=\pm\varpi(f_1,\ldots,f_m)\,\xi^1\wedge\cdots\wedge\xi^m, \qquad [\mathrm{Thm\ 8.3.1(c)}]$$

where the positive (negative) sign is chosen if $\mathcal E$ and $\mathcal F$ are consistent (not consistent). Then Theorem 7.2.9 gives the result.

# **Theorem 8.3.3.**

*Let $(V,g)$ be a scalar product space, let $\mathcal E=(e_1,\ldots,e_m)$ and $\mathcal F$ be orthonormal bases for $V$, and let $(\varphi^1,\ldots,\varphi^m)$ be the dual basis corresponding to $\mathcal F$. Then:*

$$\det\left([\operatorname{id}_V]_{\mathcal E}^{\mathcal F}\right)=\varphi^1\wedge\cdots\wedge\varphi^m(e_1,\ldots,e_m)=\pm1,$$

where the positive (negative) sign is chosen if $\mathcal E$ and $\mathcal F$ are consistent (not consistent).

**Proof.** The first equality follows from Theorem 7.2.11. Setting $\varpi=\varphi^1\wedge\cdots\wedge\varphi^m$ in Theorem 8.3.2 and using Theorem 7.2.9 gives the second equality.

# **Theorem 8.3.4 (Existence of Volume Multicovector).**

*If $(V,g,\mathcal D)$ is an oriented scalar product space, then there is a unique orientation multicovector $\Omega_V$ on $V$, called the* **volume multicovector**, *such that: (i) $\Omega_V$ induces $\mathcal D$, and (ii) if $(e_1,\ldots,e_m)$ is an orthonormal basis for $V$ that is positively oriented with respect to $\mathcal D$, then $\Omega_V(e_1,\ldots,e_m)=1$. In fact, if $(f_1,\ldots,f_m)$ is any orthonormal basis for $V$ that is positively oriented with respect to $\mathcal D$ and $(\varphi^1,\ldots,\varphi^m)$ is its dual basis, then $\Omega_V=\varphi^1\wedge\cdots\wedge\varphi^m$.*

**Proof.** *Existence.* By Theorem 7.2.9, $\varphi^1\wedge\cdots\wedge\varphi^m(f_1,\ldots,f_m)=1$, so $\varphi^1\wedge\cdots\wedge\varphi^m$ is an orientation multicovector on $V$. Then $\varphi^1\wedge\cdots\wedge\varphi^m$ induces an orientation of $V$, of which there are two possibilities, namely, $\mathcal D$ and $-\mathcal D$. Since $(f_1,\ldots,f_m)$ is positively oriented with respect to $\mathcal D$ and $\varphi^1\wedge\cdots\wedge\varphi^m(f_1,\ldots,f_m)>0$, by Theorem 8.2.3, $\varphi^1\wedge\cdots\wedge\varphi^m$ induces $\mathcal D$. Since $(e_1,\ldots,e_m)$ are positively oriented with respect to $\mathcal D$, they are consistent. It follows from Theorem 8.3.3 that $\varphi^1\wedge\cdots\wedge\varphi^m(e_1,\ldots,e_m)=1$.

*Uniqueness.* Suppose $\varpi$ is an orientation multicovector on $V$ such that $\varpi(f_1,\ldots,f_m)=1$. It follows from Theorem 7.2.13(b) that

$$\varpi=\varpi(f_1,\ldots,f_m)\,\varphi^1\wedge\cdots\wedge\varphi^m=\varphi^1\wedge\cdots\wedge\varphi^m.$$

# **Theorem 8.3.5 (Expression for Volume Multicovector).**

*Let $(V,g,\mathcal D)$ be an oriented scalar product space, let $\mathcal H$ be a basis for $V$ that is positively oriented with respect to $\mathcal D$, and let $(\theta^1,\ldots,\theta^m)$ be its dual basis. Then*

$$\Omega_V=\sqrt{|\det(g_{\mathcal H})|}\,\theta^1\wedge\cdots\wedge\theta^m.$$