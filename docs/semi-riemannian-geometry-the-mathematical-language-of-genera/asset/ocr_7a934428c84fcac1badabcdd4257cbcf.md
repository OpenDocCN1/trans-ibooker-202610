where we note that, by definition, $\gamma$ and $\beta$ have the same sign. Then

$$-1<\beta<1 \qquad\text{and}\qquad \gamma^2(1-\beta^2)=1. \tag{22.1.3}$$

When $\gamma\ge 1$, which is the case of greatest interest,

$$0\le\beta<1 \qquad\text{and}\qquad \gamma=\frac{1}{\sqrt{1-\beta^2}}.$$

Given a map $A$ in $L_m$, we have from Theorem 4.4.3 that $\det(A)=\pm1$, and from (22.1.1) that $\gamma\ge1$ or $\gamma\le-1$. Consider the sets

$$L_m^+ = \{A\in L_m:\det(A)=1\},$$

$$L_m^{\uparrow}=\{A\in L_m:\gamma\ge1\},$$

and

$$L_m^{+\uparrow}=L_m^+\cap L_m^{\uparrow}.$$

In the present context, an orientation-preserving (orientation-reversing) map is said to be **proper (improper)**.

**Theorem 22.1.3.** *Let $A$ be a map in $L_m$. Then $A$ is in $L_m^{+\uparrow}$ if and only if $A$ is proper.*

*Proof.* Since $\det(A)=+1$, the result follows from Theorem 8.2.8.

Let us recall the convention adopted in Section 4.9 that for a given orthonormal basis $(e_1,\ldots,e_m)$ for $V$, the future time cone $\mathcal T^+$ is defined to be the one containing the timelike unit vector $e_m$.

**Theorem 22.1.4.** *If $A$ is a map in $L_m$, then the following are equivalent:*  
(a) $A$ is in $L_m^{\uparrow}$.  
(b) $A(e_m)$ is in $\mathcal T^+$.  
(c) $A$ is orthochronous.

*Proof.* (a) $\Longleftrightarrow$ (b): According to the above convention on $e_m$, we have from (4.9.8) that $A(e_m)$ is in $\mathcal T^+$ if and only if $\langle A(e_m),e_m\rangle<0$. Since $[e_m]=[0\ \cdots\ 0\ 1]^{\mathsf T}$, by Theorem 2.2.4,

$$[A(e_m)]=\begin{bmatrix}[A^1_m]_2\\ \gamma\end{bmatrix},$$

and because $g_{\mathcal E}=\operatorname{diag}(1,\ldots,1,-1)$, by Theorem 3.1.2(a),

$$\langle A(e_m),e_m\rangle=[A(e_m)]^{\mathsf T}g_{\mathcal E}[e_m]=-\gamma.$$

It follows that $\langle A(e_m),e_m\rangle<0$ if and only if $\gamma>0$. Since $\gamma^2\ge1$, we have $\gamma>0$ if and only if $\gamma\ge1$. Thus, $A(e_m)$ is in $\mathcal T^+$ if and only if $\gamma\ge1$.

(b) $\Longleftrightarrow$ (c): Given the above convention on $e_m$, this follows from Theorem 4.9.6.