
Substituting (12.9.6) and (12.9.7) into (12.9.5) gives

$$
\begin{aligned}
D_X(D_Y(Z)) &= \nabla_X(\nabla_Y(Z)) - \epsilon_M\langle\mathcal{W}(Y), Z\rangle\mathcal{W}(X) \\
&\quad + \epsilon_M\big[\langle D_X(\mathcal{W}(Y)), Z\rangle + \langle\mathcal{W}(Y), D_X(Z)\rangle \\
&\quad + \langle\mathcal{W}(X), \nabla_Y(Z)\rangle\big]\mathcal{N}.
\end{aligned} \tag{12.9.8}
$$

Also by Theorem 12.9.6(b),

$$
\begin{aligned}
\langle D_X(\mathcal{W}(Y)), Z\rangle
&= \langle\nabla_X(\mathcal{W}(Y)) + \epsilon_M\langle\mathcal{W}(X), \mathcal{W}(Y)\rangle\mathcal{N}, Z\rangle \\
&= \langle\nabla_X(\mathcal{W}(Y)), Z\rangle + \epsilon_M\langle\mathcal{W}(X), \mathcal{W}(Y)\rangle\langle\mathcal{N}, Z\rangle \\
&= \langle\nabla_X(\mathcal{W}(Y)), Z\rangle.
\end{aligned} \tag{12.9.9}
$$

and

$$
\begin{aligned}
\langle\mathcal{W}(Y), D_X(Z)\rangle
&= \langle\mathcal{W}(Y), \nabla_X(Z) + \epsilon_M\langle\mathcal{W}(X), Z\rangle\mathcal{N}\rangle \\
&= \langle\mathcal{W}(Y), \nabla_X(Z)\rangle + \epsilon_M\langle\mathcal{W}(X), Z\rangle\langle\mathcal{W}(Y), \mathcal{N}\rangle \\
&= \langle\mathcal{W}(Y), \nabla_X(Z)\rangle.
\end{aligned} \tag{12.9.10}
$$

Substituting (12.9.9) and (12.9.10) into (12.9.8) yields

$$
\begin{aligned}
D_X(D_Y(Z)) &= \nabla_X(\nabla_Y(Z)) - \epsilon_M\langle\mathcal{W}(Y), Z\rangle\mathcal{W}(X) \\
&\quad + \epsilon_M\big[\langle\nabla_X(\mathcal{W}(Y)), Z\rangle + \langle\mathcal{W}(Y), \nabla_X(Z)\rangle \\
&\quad + \langle\mathcal{W}(X), \nabla_Y(Z)\rangle\big]\mathcal{N}.
\end{aligned} \tag{12.9.11}
$$

Interchanging the roles of \(X\) and \(Y\) in (12.9.11) gives

$$
\begin{aligned}
D_Y(D_X(Z)) &= \nabla_Y(\nabla_X(Z)) - \epsilon_M\langle\mathcal{W}(X), Z\rangle\mathcal{W}(Y) \\
&\quad + \epsilon_M\big[\langle\nabla_Y(\mathcal{W}(X)), Z\rangle + \langle\mathcal{W}(X), \nabla_Y(Z)\rangle \\
&\quad + \langle\mathcal{W}(Y), \nabla_X(Z)\rangle\big]\mathcal{N}.
\end{aligned} \tag{12.9.12}
$$

Yet again by Theorem 12.9.6(b),

$$
D_{[X,Y]}(Z) = \nabla_{[X,Y]}(Z) + \epsilon_M\langle\mathcal{W}([X,Y]), Z\rangle\mathcal{N}. \tag{12.9.13}
$$

We note that in the present context, \([X,Y]\) denotes \(\nabla_X(Y) - \nabla_Y(X)\), not \(D_X(Y) - D_Y(X)\). But according to Theorem 12.6.2, the latter two vector fields are equal. By Theorem 10.1.15, there is an open set \(U\) in \(\mathbb{R}^3\) containing \(M\), and vector fields \(\tilde{X}, \tilde{Y}, \tilde{Z}\) in \(\mathfrak{X}(U)\) that restrict to \(X, Y, Z\) on \(M\), respectively. By Theorem 10.4.3,

$$
\tilde{D}_{\tilde{X}}(\tilde{D}_{\tilde{Y}}(\tilde{Z}))
- \tilde{D}_{\tilde{Y}}(\tilde{D}_{\tilde{X}}(\tilde{Z}))
- \tilde{D}_{\tilde{D}_{\tilde{X}}(\tilde{Y}) - \tilde{D}_{\tilde{Y}}(\tilde{X})}(\tilde{Z})
= (0,0,0)
$$

on \(U\), where \(\tilde{D}\) denotes the Euclidean derivative (Section 10.3). It follows that

$$
\begin{aligned}
(0,0,0) &= D_X(D_Y(Z)) - D_Y(D_X(Z)) - D_{D_X(Y)-D_Y(X)}(Z) \\
&= D_X(D_Y(Z)) - D_Y(D_X(Z)) - D_{[X,Y]}(Z).
\end{aligned} \tag{12.9.14}
$$

on \(M\). Substituting (12.9.11)–(12.9.13) into (12.9.14) and using (12.9.1) gives

$$
R(X,Y)Z - \epsilon_M[\langle\mathcal{W}(Y),Z\rangle\mathcal{W}(X) - \langle\mathcal{W}(X),Z\rangle\mathcal{W}(Y)]
+ \epsilon_M\langle\nabla_X(\mathcal{W}(Y)) - \nabla_Y(\mathcal{W}(X)) - \mathcal{W}([X,Y]), Z\rangle\mathcal{N} = (0,0,0).
$$

It follows that both the tangential and normal components of the left-hand side of the preceding expression equal \((0,0,0)\), hence

$$
R(X,Y)Z = \epsilon_M[\langle\mathcal{W}(Y),Z\rangle\mathcal{W}(X) - \langle\mathcal{W}(X),Z\rangle\mathcal{W}(Y)] \tag{12.9.15}
$$

and

$$
\langle\nabla_X(\mathcal{W}(Y)) - \nabla_Y(\mathcal{W}(X)) - \mathcal{W}([X,Y]), Z\rangle = 0. \tag{12.9.16}
$$

From (12.8.7) and (12.9.15), we obtain

$$
R(X,Y)Z = \epsilon_M[\mathfrak{h}(Y,Z)\mathcal{W}(X) - \mathfrak{h}(X,Z)\mathcal{W}(Y)],
$$

which proves part (a). Since \(\mathfrak{g}\) is nondegenerate and (12.9.16) holds for all vector fields \(Z\) in \(\mathfrak{X}(M)\), we have

$$
\nabla_X(\mathcal{W}(Y)) - \nabla_Y(\mathcal{W}(X)) - \mathcal{W}([X,Y]) = (0,0,0),
$$

which proves part (b).

Let \((M,\mathfrak{g})\) be a regular surface in \(\mathbb{R}^3_\nu\), and define a map

$$
\mathcal{R}\colon \mathfrak{X}(M)^4 \longrightarrow C^\infty(M),
$$

also called the Riemann curvature tensor, by

$$
\mathcal{R}(X,Y,Z,W) = \langle R(X,Y)Z, W\rangle
$$

for all vector fields \(X, Y, Z, W\) in \(\mathfrak{X}(M)\); that is,

$$
\mathcal{R}(X,Y,Z,W)(p) = \langle (R(X,Y)Z)_p, W_p\rangle \tag{12.9.17}
$$

for all \(p\) in \(M\). By definition, \(\mathcal{R}(X,Y,Z,W)\) is a function in \(C^\infty(M)\). Since \(\mathcal{R}(X,Y,Z,W)(p)\) is a real number, calling \(\mathcal{R}\) a “tensor” is perhaps justified. We return to this issue below.

### Theorem 12.9.8.

Let \((M,\mathfrak{g})\) be a regular surface in \(\mathbb{R}^3_\nu\), let \(X, Y, Z, W\) be vector fields in \(\mathfrak{X}(M)\), and, in local coordinates, let

$$
X\circ\varphi=\sum_i \alpha^i H_i,\qquad Y\circ\varphi=\sum_j \beta^j H_j,\qquad Z\circ\varphi=\sum_k \gamma^k H_k,\qquad W\circ\varphi=\sum_l \delta^l H_l.
$$

Then

$$
\mathcal{R}(X,Y,Z,W)\circ\varphi = \sum_{i j k l n} \alpha^i \beta^j \gamma^k \delta^l g_{ln} R^n_{ijk}. \tag{12.9.18}
$$

**Proof.** We have from (12.9.2) that

$$
\begin{aligned}
\mathcal{R}(X,Y,Z,W)\circ\varphi
&= \langle R(X,Y)Z\circ\varphi, W\circ\varphi\rangle \\
&= \left\langle\sum_n\left(\sum_{ijk}\alpha^i\beta^j\gamma^kR^n_{ijk}\right)H_n,\sum_l\delta^lH_l\right\rangle \\
&= \sum_{ijkln}\alpha^i\beta^j\gamma^k\delta^l g_{ln}R^n_{ijk}.
\end{aligned}
$$

We noted in conjunction with (12.9.2) and (12.9.3) that the Riemann curvature tensor \(R\) is intrinsic. In view of (12.9.18), the same can be said of the Riemann curvature tensor \(\mathcal{R}\). Just as was the case for (12.9.2), there are no partial derivatives of the component functions in (12.9.18). This observation underlies the next two results, which are counterparts of Theorem 12.9.4 and Theorem 12.9.5, and are proved similarly.

### Theorem 12.9.9.

If \((M,g)\) is a regular surface in \(\mathbb{R}^3_\nu\), then \(\mathcal{R}\) is determined pointwise in the following sense: for all points \(p\) in \(M\), if \(X,Y,Z,W\) and \(\widetilde X,\widetilde Y,\widetilde Z,\widetilde W\) are vector fields in \(\mathfrak{X}(M)\) such that

$$
(X_p,Y_p,Z_p,W_p)=(\widetilde X_p,\widetilde Y_p,\widetilde Z_p,\widetilde W_p),
$$

then

$$
\mathcal{R}(X,Y,Z,W)(p)=\mathcal{R}(\widetilde X,\widetilde Y,\widetilde Z,\widetilde W)(p).
$$

### Theorem 12.9.10.

If \((M,g)\) is a regular surface in \(\mathbb{R}^3_\nu\), then \(\mathcal{R}\) is \(C^\infty(M)\)-multilinear in the following sense: for all vector fields \(X,Y,Z,V,W\) in \(\mathfrak{X}(M)\) and all functions \(f\) in \(C^\infty(M)\),

$$
\mathcal{R}(fX+W,Y,Z,V)=f\mathcal{R}(X,Y,Z,V)+\mathcal{R}(W,Y,Z,V),
$$

with corresponding identities for the other three arguments of \(\mathcal{R}\).

Let \((M,g)\) be a regular surface in \(\mathbb{R}^3_\nu\), let \(p\) be a point in \(M\), and let \(v\) be a vector in \(T_p(M)\). According to Theorem 15.1.22, there is a vector field \(X\) in \(\mathfrak{X}(M)\) such that \(X_p=v\). Taken in conjunction with Theorem 12.9.4, Theorem 12.9.5, Theorem 12.9.9, and Theorem 12.9.10, this allows us to give \(R\) and \(\mathcal{R}\) interesting interpretations. We define a map

$$
R_p\colon T_p(M)^3\longrightarrow T_p(M)
$$

by

$$
R_p(v_1,v_2)v_3=(R(X_1,X_2)X_3)_p, \tag{12.9.19}
$$

and a map

$$
\mathcal{R}_p\colon T_p(M)^4\longrightarrow\mathbb{R}
$$

by

$$
\mathcal{R}_p(v_1,v_2,v_3,v_4)=\mathcal{R}(X_1,X_2,X_3,X_4)(p)
$$

for all vectors \(v_1,v_2,v_3,v_4\) in \(T_p(M)\), where \(X_1,X_2,X_3,X_4\) are any vector fields in \(\mathfrak{X}(M)\) such that

$$
(X_1|_p,X_2|_p,X_3|_p,X_4|_p)=(v_1,v_2,v_3,v_4).
$$

By Theorem 12.9.4 and Theorem 12.9.9, respectively, \(R_p\) and \(\mathcal{R}_p\) are independent of the choice of vector fields, so the definitions make sense. It follows from (12.9.17) and the above identities that

$$
\mathcal{R}_p(v_1,v_2,v_3,v_4)
= \mathcal{R}(X_1,X_2,X_3,X_4)(p)
= \langle (R(X_1,X_2)X_3)_p, X_4|_p\rangle
= \langle R_p(v_1,v_2)v_3, v_4\rangle.
$$

By Theorem 12.9.10, \(\mathcal{R}_p\) is in \(\mathcal{T}_4^0(T_p(M))\), and by Theorem 12.9.5, \(R_p\) is in \(\operatorname{Mult}(T_p(M)^3,T_p(M))\). This provides a justification for calling \(\mathcal{R}\) a “tensor”, and to a lesser extent a rationale for doing the same with \(R\). Another tensor of interest in \(\mathcal{T}_4^0(T_p(M))\) is \(\mathcal{D}_p\), as defined by (6.6.7):

$$
\mathcal{D}_p(v_1,v_2,v_3,v_4) = \det\left(\begin{bmatrix}\langle v_1,v_3\rangle & \langle v_1,v_4\rangle \\ \langle v_2,v_3\rangle & \langle v_2,v_4\rangle\end{bmatrix}\right). \tag{12.9.20}
$$

### Theorem 12.9.11 (Gauss’s Equation).

If \((M,g,\mathfrak{O})\) is an oriented regular surface in \(\mathbb{R}^3_\nu\) and \(X,Y,Z,W\) are vector fields in \(\mathfrak{X}(M)\), then

$$
\mathcal{R}(X,Y,Z,W) = -\epsilon_M \det\left(\begin{bmatrix}\mathfrak{h}(X,Z) & \mathfrak{h}(X,W) \\ \mathfrak{h}(Y,Z) & \mathfrak{h}(Y,W)\end{bmatrix}\right).
$$

**Proof.** It follows from (12.8.7) and Theorem 12.9.7(a) that

$$
\begin{aligned}
\mathcal{R}(X,Y,Z,W)
&= \epsilon_M \langle \mathfrak{h}(Y,Z)\mathcal{W}(X) - \mathfrak{h}(X,Z)\mathcal{W}(Y), W\rangle \\
&= \epsilon_M[\mathfrak{h}(Y,Z)\langle\mathcal{W}(X),W\rangle - \mathfrak{h}(X,Z)\langle\mathcal{W}(Y),W\rangle] \\
&= \epsilon_M[\mathfrak{h}(Y,Z)\mathfrak{h}(X,W) - \mathfrak{h}(X,Z)\mathfrak{h}(Y,W)] \\
&= -\epsilon_M \det\left(\begin{bmatrix}\mathfrak{h}(X,Z) & \mathfrak{h}(X,W) \\ \mathfrak{h}(Y,Z) & \mathfrak{h}(Y,W)\end{bmatrix}\right).
\end{aligned}
$$

### Theorem 12.9.12 (Symmetries of Riemann Curvature Tensor).

If \((M,g,\mathfrak{O})\) is an oriented regular surface in \(\mathbb{R}^3_\nu\) and \(X,Y,Z,W\) are vector fields in \(\mathfrak{X}(M)\), then \(\mathcal{R}\) satisfies the following symmetries:

- [S1] \(\mathcal{R}(X,Y,Z,W) = -\mathcal{R}(Y,X,Z,W).\)
- [S2] \(\mathcal{R}(X,Y,Z,W) = -\mathcal{R}(X,Y,W,Z).\)
- [S3] \(\mathcal{R}(X,Y,Z,W) = \mathcal{R}(Z,W,X,Y).\)

**Proof.** This follows from Theorem 12.9.11.
