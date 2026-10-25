# Theorem 2.2.1 (Representation of Vectors)

If $V$ is a vector space of dimension $m$ and $\mathcal{H}$ is a basis for $V$, then the map

$$\mathcal{L}_{\mathcal{H}}: V \longrightarrow \operatorname{Mat}_{m\times 1}$$

defined by

$$\mathcal{L}_{\mathcal{H}}(v)=[v]_{\mathcal{H}}$$

for all vectors $v$ in $V$ is a linear isomorphism:

$$V\cong\operatorname{Mat}_{m\times 1}.$$

*Proof.* Straightforward.

With $V$ and $\mathcal{H}$ as above, let $W$ be another vector space, and let $\mathcal{F}=(f_1,\ldots,f_n)$ be a basis for $W$. Let $A:V\longrightarrow W$ be a linear map, with

$$A(h_j)=\sum_i a_j^i f_i,$$

so that

$$[A(h_j)]_{\mathcal{F}}=\begin{bmatrix}a_j^1\\\vdots\\a_j^n\end{bmatrix}$$

for $j=1,\ldots,m$. The matrix representation of $A$ with respect to $\mathcal{H}$ and $\mathcal{F}$ is denoted by $[A]_{\mathcal{H}}^{\mathcal{F}}$ and defined to be the $n\times m$ matrix

$$[A]_{\mathcal{H}}^{\mathcal{F}}=\begin{bmatrix}a_1^1&\cdots&a_m^1\\\vdots&\ddots&\vdots\\a_1^n&\cdots&a_m^n\end{bmatrix}=\left[\,[A(h_1)]_{\mathcal{F}}\ \cdots\ [A(h_m)]_{\mathcal{F}}\,\right].$$

As an example, consider the linear map $A:\mathbb{R}^2\longrightarrow\mathbb{R}^3$ given by $A(x,y)=(y,2x,3x+4y)$, and let $\mathcal{E}$ and $\mathcal{F}$ be the standard bases for $\mathbb{R}^2$ and $\mathbb{R}^3$, respectively. Then

$$[A]_{\mathcal{E}}^{\mathcal{F}}=\begin{bmatrix}0&1\\2&0\\3&4\end{bmatrix}.$$

# Theorem 2.2.2.

Let $U$, $V$, and $W$ be vector spaces, let $\mathcal{H}$, $\mathcal{F}$, and $\mathcal{G}$ be respective bases, let $A,B:U\longrightarrow V$ and $C:V\longrightarrow W$ be linear maps, and let $c$ be a real number. Then:

+   (a) $[A+B]_{\mathcal{H}}^{\mathcal{F}}=[A]_{\mathcal{H}}^{\mathcal{F}}+[B]_{\mathcal{H}}^{\mathcal{F}}$.  
+   (b) $[cA]_{\mathcal{H}}^{\mathcal{F}}=c[A]_{\mathcal{H}}^{\mathcal{F}}$.  
+   (c) $[C\circ A]_{\mathcal{H}}^{\mathcal{G}}=[C]_{\mathcal{F}}^{\mathcal{G}}[A]_{\mathcal{H}}^{\mathcal{F}}$.