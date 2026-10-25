

## 19.3 半黎曼流形的基石定理

**定理 19.3.6（正规坐标）。**

若 $(M,g)$ 是一个签名为 $(\epsilon_1,\ldots,\epsilon_m)$ 的半黎曼 $m$-流形，$p$ 是 $M$ 中的一点，则在 $p$ 处的正规坐标下，

$$g_{ij}(p)=\epsilon_i\delta_{ij}\qquad\text{and}\qquad \Gamma^k_{ij}(p)=0$$

对 $i,j,k=1,\ldots,m$ 成立。

**证明。** 继续采用第 18.10 节开头建立的设定，设 $(\tilde e_1,\ldots,\tilde e_m)$ 是 $T_p(M)$ 的一个正交基。由定理 18.10.1(a) 和 (19.1.1) 可得，

$$g_{ij}(p)=\left\langle\left.\frac{\partial}{\partial x^i}\right|_p,\left.\frac{\partial}{\partial x^j}\right|_p\right\rangle=\langle\tilde e_i,\tilde e_j\rangle.$$

根据定义，$T_p(M)$ 与 $M$ 具有相同的签名，故 $\langle\tilde e_i,\tilde e_j\rangle=\epsilon_i\delta_{ij}$。这证明了第一个恒等式。第二个恒等式是定理 18.10.2 的重复。

设 $(M,g)$ 为半黎曼流形，$U$ 为 $M$ 中的开集，$\mathcal X=(X_1,\ldots,X_m)$ 为 $U$ 上的一个标架。我们称 $\mathcal X$ 为 **正交** 的，如果 $\mathcal X_p=(X_{1|p},\ldots,X_{m|p})$ 对 $U$ 中所有 $p$ 都是 $T_p(M)$ 的正交基。当 $M$ 可定向时，若 $\mathcal X_p$ 对 $U$ 中所有 $p$ 都是正定向的，则称 $\mathcal X$ 为 **正定向** 的。

**定理 19.3.7（正交标架的存在性）。**

半黎曼流形中每一点都存在一个邻域，在该邻域上存在正交标架。若半黎曼流形可定向，则正交标架可以选为正定向的。

**证明。** 设 $(M,g)$ 为半黎曼流形，$p$ 为 $M$ 中的一点，$(e_1,\ldots,e_m)$ 为 $T_p(M)$ 的正交基。设 $V$ 为 $p$ 在 $M$ 中的一个正规邻域，$\exp_p:\widetilde V\longrightarrow V$ 为相应的微分同胚，$\gamma(t):[0,1]\longrightarrow M$ 为起点为 $p$ 的径向测地线。由定理 18.5.7，存在唯一的标架 $(E_{1\gamma},\ldots,E_{m\gamma})$ 在 $\gamma$ 上，由 $\mathcal X_m(\gamma)$ 中的平行向量场组成，使得 $(E_{1\gamma}(0),\ldots,E_{m\gamma}(0))=(e_1,\ldots,e_m)$ 且 $(E_{1\gamma}(t),\ldots,E_{m\gamma}(t))$ 对所有 $t\in[0,1]$ 都是 $T_{\gamma(t)}(M)$ 的基。由定理 19.3.1 可知 $\langle E_{i\gamma}(t),E_{j\gamma}(t)\rangle=\langle e_i,e_j\rangle$ 对所有 $t\in[0,1]$ 及 $i,j=1,\ldots,m$ 成立。因此，每个 $(E_{1\gamma}(t),\ldots,E_{m\gamma}(t))$ 都是正交基。可以证明，将这些向量场在所有以 $p$ 为起点的径向测地线上组合起来，可得到 $V$ 上的正交标架 $(E_1,\ldots,E_m)$。

现在假设 $M$ 可定向。若必要，可将 $e_1$ 替换为 $-e_1$，不失一般性地假设 $(E_{1|p},\ldots,E_{m|p})=(e_1,\ldots,e_m)$ 是 $T_p(M)$ 的正定向正交基，并且上述构造已相应地修改。由定理 16.6.5，存在一个诱导 $M$ 的定向的定向形式 $\varpi$。则 $\varpi_p(E_{1|p},\ldots,E_{m|p})>0$，故 $f=\varpi|_V(E_1,\ldots,E_m)$ 是 $C^\infty(V)$ 中的一个函数且 $f(p)>0$。由于 $f$ 是光滑的，由定理 14.2.2，它是连续的。设 $\epsilon>0$ 为实数，使得开区间 $I=(f(p)-\epsilon,f(p)+\epsilon)$ 由正实数组成。由定理 9.1.5 和定理 9.1.7，$W=f^{-1}(I)$ 是 $p$ 在 $M$ 中的一个邻域，在该邻域上 $f$ 取正值。由此可知 $(E_1,\ldots,E_m)$ 在 $W$ 上是正定向的。

我们经常调用定理 19.3.7，但通常不加引用。

**定理 19.3.8。** 设 $(M,\mathfrak g)$ 为半黎曼流形，$X$ 为 $\mathfrak X(M)$ 中的向量场，$\mathcal A$ 为 $\mathcal T_s^r(M)$ 中的张量场。则

$\nabla_X(\mathfrak g \otimes \mathcal A) = \mathfrak g \otimes \nabla_X(\mathcal A)$。

证明。我们有

$\nabla_X(\mathfrak g \otimes \mathcal A) = \nabla_X(\mathfrak g) \otimes \mathcal A + \mathfrak g \otimes \nabla_X(\mathcal A)$ [Th 18.1.3(b)] = $\mathfrak g \otimes \nabla_X(\mathcal A)$。 [Th 18.4.1, Th 19.3.1]

## 19.4 平坦映射与锐映射

在本节中，我们给出第 3.3 节、第 6.2 节和第 6.3 节中定义的平坦映射和锐映射的流形版本。

设 $(M,\mathfrak g)$ 为半黎曼流形。对 $\mathfrak X(M)$ 中的给定向量场 $X$，我们定义一个映射

$X^f : \mathfrak X(M) \longrightarrow C^\infty(M)$

由

$X^f(Y) = \langle X, Y \rangle$ (19.4.1)

对所有 $\mathfrak X(M)$ 中的向量场 $Y$ 定义。

**定理 19.4.1。** 在上述设定下，$X^f$ 是 $\mathfrak X^*(M)$ 中的一个余向量场。

证明。设 $Y,Z$ 为 $\mathfrak X(M)$ 中的向量场，$f$ 为 $C^\infty(M)$ 中的函数。则

$X^f(Y + Z) = \langle X, Y + Z \rangle = \langle X, Y \rangle + \langle X, Z \rangle = X^f(Y) + X^f(Z)$

且 $(fX)^f(Y) = \langle fX, Y \rangle = f\langle X, Y \rangle = f X^f(Y)$，所以 $X^f$ 是 $\operatorname{Lin}_{C^\infty(M)}(\mathfrak X(M), C^\infty(M))$ 中的映射。结果现在由定理 15.5.3 得出。

$M$ 上的平坦映射是映射

$F : \mathfrak X(M) \longrightarrow \mathfrak X^*(M)$

由赋值

$X \mapsto X^f$

对所有 $\mathfrak X(M)$ 中的向量场 $X$ 定义。因此，作为记号，

$F(X) = X^f$。

## 定理 19.4.2。

在上述设定下，$F$ 是 $C^\infty(M)$-模同构：

$$\mathfrak{X}(M) \cong \mathfrak{X}^*(M).$$

**证明。** 易证 $F$ 是 $C^\infty(M)$-模同态。我们需要证明 $F$ 是双射。

**单射性。** 由于 $\mathbf g$ 非退化，$\ker(F)=0$。由定理 B.5.3 可知 $F$ 是单射。

**满射性。** 设 $\omega$ 为 $\mathfrak{X}^*(M)$ 中的余向量场，$(U,(x^i))$ 为 $M$ 上的一个坐标图，在局部坐标下设 $\omega|_U=\sum_i\alpha_i d(x^i)$。定义 $U$ 上的向量场 $X|_U$ 为

$$X|_U=\sum_{ij}g^{ij}\alpha_i\frac{\partial}{\partial x^j}.$$

则

$(X|_U)^F\left(\frac{\partial}{\partial x^i}\right)=\left\langle X|_U,\frac{\partial}{\partial x^i}\right\rangle=\left\langle\sum_{jk}g^{jk}\alpha_j\frac{\partial}{\partial x^k},\frac{\partial}{\partial x^i}\right\rangle=\sum_{jk}\alpha_jg^{jk}g_{ki}$

$=\sum_j\alpha_j\left(\sum_kg^{jk}g_{ki}\right)=\sum_j\delta_i^j\alpha_j=\alpha_i$

对 $i=1,\ldots,m$，故

$$\omega|_U=\sum_i(X|_U)^F\left(\frac{\partial}{\partial x^i}\right)d(x^i)$$

由流形版本的定理 1.2.1(e) 可知 $(X|_U)^F=\omega|_U$。

设 $(\widetilde U,\widetilde\varphi)$ 为 $M$ 上的一个与 $(U,\varphi)$ 重叠的坐标图，在局部坐标下设 $\omega|_{\widetilde U}=\sum_i\widetilde\alpha_i d(\widetilde x^i)$。我们有

$$\frac{\partial}{\partial\widetilde x^i}=\sum_q\frac{\partial x^q}{\partial\widetilde x^i}\frac{\partial}{\partial x^q},\qquad \widetilde\alpha_j=\sum_p\frac{\partial x^p}{\partial\widetilde x^j}\alpha_p,\qquad \widetilde g^{ij}=\sum_{kl}\frac{\partial\widetilde x^i}{\partial x^k}g^{kl}\frac{\partial\widetilde x^j}{\partial x^l},$$

其中第一个恒等式由定理 15.1.8 得出，第二个恒等式由定理 15.6.3 得出，第三个恒等式由流形版本的定理 2.2.5(b)、定理 14.3.5(b) 和定理 19.1.3 得出。则

$X|_{\widetilde U}=\sum_{ij}\widetilde g^{ij}\widetilde\alpha_i\frac{\partial}{\partial\widetilde x^j}=\sum_{ijkl p q}\left(\frac{\partial\widetilde x^i}{\partial x^k}g^{kl}\frac{\partial\widetilde x^j}{\partial x^l}\right)\left(\frac{\partial\widetilde x^j}{\partial x^p}\alpha_p\right)\left(\frac{\partial x^q}{\partial\widetilde x^i}\frac{\partial}{\partial x^q}\right)$

$=\sum_{klp q}\left[g^{kl}\alpha_p\left(\sum_i\frac{\partial\widetilde x^i}{\partial x^k}\frac{\partial x^q}{\partial\widetilde x^i}\right)\left(\sum_j\frac{\partial\widetilde x^j}{\partial x^l}\frac{\partial x^p}{\partial\widetilde x^j}\right)\frac{\partial}{\partial x^q}\right]$

$=\sum_{klpq}\delta_k^q\delta_l^p g^{kl}\alpha_p\frac{\partial}{\partial x^q}$

$=\sum_{kl}g^{kl}\alpha_k\frac{\partial}{\partial x^l}=X|_U,$

所以在重叠处有相容性。综合起来，对每个点在 $M$ 中的 $X|_U$ 定义一个 $\mathfrak X(M)$ 中的向量场 $X$，使得 $X^F = \omega$。因此，$F$ 是满射。

$M$ 上的锐映射

$S : \mathfrak X^*(M) \longrightarrow \mathfrak X(M)$

定义为 $F$ 的逆；即

$S = F^{-1}$。

设 $1 \le k \le r$ 且 $1 \le l \le s + 1$ 为整数（从而 $r \ge 1$ 且 $s \ge 0$）。$(k,l)$-平坦映射记为

$F_l^k : \mathcal T_s^r(M) \longrightarrow \mathcal T_{s+1}^{r-1}(M)$

并定义（使用定理 15.7.2）对所有 $\mathcal T_s^r(M)$ 中的张量场 $\mathcal A$、所有 $\mathfrak X^*(M)$ 中的余向量场 $\omega^1,\dots,\omega^{r-1}$ 以及所有 $\mathfrak X(M)$ 中的向量场 $X_1,\dots,X_{s+1}$ 如下，其中 $^\wedge$ 表示省略一个表达式：

- 对 $r = 1$（从而 $k = 1$）：

$F_l^1(\mathcal A)(X_1, \dots, X_{s+1}) = \mathcal A(X_l^F, X_1, \dots, \hat X_l, \dots, X_{s+1})$。

- 对 $r \ge 2$ 且 $1 \le k \le r - 1$：

$F_l^k(\mathcal A)(\omega^1, \dots, \omega^{r-1}, X_1, \dots, X_{s+1}) = \mathcal A(\omega^1, \dots, \omega^{k-1}, X_l^F, \omega^k, \dots, \omega^{r-1}, X_1, \dots, \hat X_l, \dots, X_{s+1})$。

- 对 $r \ge 2$ 且 $k = r$：

$F_l^r(\mathcal A)(\omega^1, \dots, \omega^{r-1}, X_1, \dots, X_{s+1}) = \mathcal A(\omega^1, \dots, \omega^{r-1}, X_l^F, X_1, \dots, \hat X_l, \dots, X_{s+1})$。

类似地，设 $1 \le k \le r + 1$ 且 $1 \le l \le s$ 为整数（从而 $r \ge 0$ 且 $s \ge 1$）。$(k,l)$-锐映射记为

$S_l^k : \mathcal T_s^r(M) \longrightarrow \mathcal T_{s-1}^{r+1}(M)$

并定义（使用定理 15.7.2）对所有 $\mathcal T_s^r(M)$ 中的张量场 $\mathcal A$、所有 $\mathfrak X^*(M)$ 中的余向量场 $\omega^1,\dots,\omega^{r+1}$ 以及所有 $\mathfrak X(M)$ 中的向量场 $X_1,\dots,X_{s-1}$ 如下：

- 对 $s = 1$（从而 $l = 1$）：

$S_1^k(\mathcal A)(\omega^1, \dots, \omega^{r+1}) = \mathcal A(\omega^1, \dots, \hat\omega^k, \dots, \omega^{r+1}, \omega^k S)$。

- 对 $s \ge 2$ 且 $1 \le l \le s - 1$：

$S_l^k(\mathcal A)(\omega^1, \dots, \omega^{r+1}, X_1, \dots, X_{s-1}) = \mathcal A(\omega^1, \dots, \hat\omega^k, \dots, \omega^{r+1}, X_1, \dots, X_{l-1}, \omega^k S, X_l, \dots, X_{s-1})$。

