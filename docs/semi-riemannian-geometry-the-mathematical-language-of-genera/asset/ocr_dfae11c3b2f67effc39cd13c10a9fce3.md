Then (22.1.5) gives

$$P_1^1[A]_2^1=-\beta\gamma\begin{bmatrix}(P_1)^\mathrm{T}P_{m-1}\\ \vdots\\ (P_{m-1})^\mathrm{T}P_{m-1}\end{bmatrix}=[0\ \cdots\ 0\ -\beta\gamma]^\mathrm{T}.$$

Let

$$P_1^1[A]_1^1=[Q_1\ \cdots\ Q_{m-1}]^\mathrm{T},$$

where $Q_1,\ldots,Q_{m-1}$ are matrices in $\operatorname{Mat}_{(m-1)\times 1}$. We have

$$\begin{bmatrix}(Q_1)^\mathrm{T}\\ \vdots\\ (Q_{m-1})^\mathrm{T}\end{bmatrix}[Q_1\ \cdots\ Q_{m-1}]$$

$$=(P_1^1[A]_1^1)(P_1^1[A]_1^1)^\mathrm{T}$$
$$=P_1^1\big([A]_1^1([A]_1^1)^\mathrm{T}\big)(P_1^1)^\mathrm{T}$$
$$=P_1^1\big(I_{m-1}+[A]_2^1([A]_2^1)^\mathrm{T}\big)(P_1^1)^\mathrm{T}\quad[\text{Th 22.1.2(a)}]$$
$$=I_{m-1}+(P_1^1[A]_2^1)(P_1^1[A]_2^1)^\mathrm{T}\quad[\text{Th 22.1.10(a)}]$$
$$=I_{m-1}+\operatorname{diag}(0,\ldots,0,\beta^2\gamma^2)\quad[(22.1.8)]$$
$$=\begin{bmatrix}I_{m-2}&O_{(m-2)\times1}\\O_{1\times(m-2)}&\gamma^2\end{bmatrix}\quad[(22.1.3)].$$

which shows that $(Q_1,\ldots,Q_{m-1})$ is “close” to being an orthonormal basis for $\operatorname{Mat}_{(m-1)\times1}$. Since $A$ is in $L_m^+$, we have, by definition, that $\gamma\geq1$. Let $\varepsilon=\pm1$, and let

$$R_{m-1}=\frac{\varepsilon}{\gamma}Q_{m-1}.$$

We see from (22.1.10) that $R_{m-1}$ is a unit vector and

$$(Q_{m-1})^\mathrm{T}R_{m-1}=\varepsilon\gamma.$$

For an appropriate choice of $\varepsilon$, $(Q_1,\ldots,Q_{m-2},R_{m-1})$ is a positively oriented orthonormal basis for $\operatorname{Mat}_{(m-1)\times1}$. (We will see shortly that, in fact, $\varepsilon=1$.) Let

$$R_1^1=[Q_1\ \cdots\ Q_{m-2}\ R_{m-1}]^\mathrm{T}$$

and

$$R=\begin{bmatrix}R_1^1&O_{(m-1)\times1}\\O_{1\times(m-1)}&1\end{bmatrix}.$$

By Theorem 2.2.3(a), there are maps $F$ and $G$ in $\operatorname{Lin}(V,V)$ such that

$$[F]=P\quad\text{and}\quad[G]=R^\mathrm{T}.$$