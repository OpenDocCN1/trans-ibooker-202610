
*证明.* 设 \(\mathcal{H}\) 是 \(V\) 的一组基。由定理4.4.2，我们有 \(\det(g_{\mathcal{H}})=\det(g_{\mathcal{H}})\det(A)^2\)，由定理3.3.3，有 \(\det(g_{\mathcal{H}})\ne 0\)。因此，\(\det(A)^2=1\)，结论得证。

线性等距映射之所以特别重要，是因为它们“保持范数”，如下述结果所示。

**定理4.4.4.**

设 \((V,\mathfrak{q})\) 是一个标量积空间，设 \(A:V\longrightarrow V\) 是一个线性映射。则：

- (a) 若 \(A\) 是线性等距映射，则对所有向量 \(v \in V\)，有 \(\|A(v)\|=\|v\|\)。
- (b) 若对所有向量 \(v \in V\) 都有 \(\|A(v)\|=\|v\|\)，且 \(A\) 将类空（相应地，类时、类光）向量映射到类空（相应地，类时、类光）向量，则 \(A\) 是线性等距映射。

*证明.* (a)：我们有

\[
\|A(v)\|=\sqrt{\langle A(v),A(v)\rangle}=\sqrt{\langle v,v\rangle}=\|v\|.
\]

(b)：由于 \(\|A(v)\|=\|v\|\) 等价于 \(\langle A(v),A(v)\rangle=\langle v,v\rangle\)，关于 \(A\) 映射向量方式的假设给出 \(\langle A(v),A(v)\rangle=\langle v,v\rangle\)。

**定理4.4.5.**

设 \((V,\mathfrak{q})\) 是一个标量积空间，设 \(A:V\longrightarrow V\) 是一个线性映射。则 \(A\) 是线性等距映射当且仅当 \(A^{\dagger}\) 是线性等距映射。

*证明.* (\(\Rightarrow\))：由定理4.4.2，\(A\) 是一个线性同构且 \(A^{\dagger}=A^{-1}\)。对 \(V\) 中的向量 \(v,w\)，我们有

\[
\begin{aligned}
\langle A^{\dagger}(v),A^{\dagger}(w)\rangle &= \langle v,A^{\dagger}(A^{\dagger}(w))\rangle && [\mathrm{Th}\ 4.3.1(a)]\\
&= \langle v,A\circ A^{\dagger}(w)\rangle && [\mathrm{Th}\ 4.3.2(c)]\\
&= \langle v,w\rangle.
\end{aligned}
\]

(\(\Leftarrow\))：这可由定理4.3.2(c)和(\(\Rightarrow\))推出。

设 \((V,\mathfrak{q})\) 是一个维数为 \(m\)、指数为 \(\nu\) 的标量积空间，设 \(\mathcal{E}\) 是 \(V\) 的一个标准正交基，设 \(A:V\longrightarrow V\) 是一个线性等距映射。为简化记号，我们将 \([A]_{\mathcal{E}}^{\mathcal{E}}\) 记为 \([A]\)，并对其他矩阵采用类似的缩写。考虑 \([A]\) 的如下分块：

\[
[A]=\begin{bmatrix}
[A_1^1] &[A_2^1]\\
[A_1^2] &[A_2^2]
\end{bmatrix},\tag{4.4.1}
\]

其中子矩阵的维数如下：

- \([A_1^1]\quad (m-\nu)\times(m-\nu)\)
- \([A_2^1]\quad (m-\nu)\times\nu\)
- \([A_1^2]\quad \nu\times(m-\nu)\)
- \([A_2^2]\quad \nu\times\nu\)。

\[
\begin{bmatrix}
([A^1_1])^\mathsf{T} & -([A^2_1])^\mathsf{T} \\
-([A^1_2])^\mathsf{T} & ([A^2_2])^\mathsf{T}
\end{bmatrix}
\begin{bmatrix}
[A^1_1] & [A^1_2] \\
[A^2_1] & [A^2_2]
\end{bmatrix}
=
\begin{bmatrix}
I_{m-\nu} & O_{(m-\nu)\times\nu} \\
O_{\nu\times(m-\nu)} & I_\nu
\end{bmatrix},
\]
因此

\[
([A^1_1])^\mathsf{T}[A^1_1]-([A^2_1])^\mathsf{T}[A^2_1]=I_{m-\nu}
\]

\[
([A^1_1])^\mathsf{T}[A^1_2]-([A^2_1])^\mathsf{T}[A^2_2]=O_{(m-\nu)\times\nu}
\]

\[
-([A^1_2])^\mathsf{T}[A^1_1]+([A^2_2])^\mathsf{T}[A^2_1]=O_{\nu\times(m-\nu)}
\]

\[
-([A^1_2])^\mathsf{T}[A^1_2]+([A^2_2])^\mathsf{T}[A^2_2]=I_\nu.
\tag{4.4.4}
\]

- (a)：这由(4.4.3)的第一个恒等式得出。
- (b)：这由(4.4.3)的第二个或第三个恒等式得出。
- (c)：这由(4.4.3)的第四个恒等式得出。
- (d)：这由(4.4.4)的第一个恒等式得出。
- (e)：这由(4.4.4)的第二个或第三个恒等式得出。
- (f)：这由(4.4.4)的第四个恒等式得出。

## 4.5 对偶标量积空间

本节中，我们证明向量空间上的标量积在其对偶空间上诱导出相应的标量积。

设 \((V,\mathfrak{g})\) 是一个标量积空间，设 \(V^*\) 是其对偶空间，设 \(F\) 和 \(S\) 是对应于 \(\mathfrak{g}\) 的平坦映射和尖锐映射。我们定义函数

\[
\mathfrak{g}^* = \langle\cdot,\cdot\rangle^* : V^*\times V^*\to\mathbb{R}
\]

如下：

\[
\langle\eta,\zeta\rangle^* = \langle\eta^S,\zeta^S\rangle
\tag{4.5.1}
\]

对 \(V^*\) 中的所有余向量 \(\eta,\zeta\) 成立。根据(3.3.1)，\(\langle\eta^S,\zeta^S\rangle=(\eta^S)^F(\zeta^S)=\eta(\zeta^S)\)，因此

\[
\langle\eta,\zeta\rangle^* = \eta(\zeta^S).
\tag{4.5.2}
\]

**定理4.5.1.** *在上述设定下，\((V^*,\mathfrak{g}^*)\) 是一个标量积空间，称为对应于 \((V,\mathfrak{g})\) 的对偶标量积空间。*

*证明.* 显然，\(\mathfrak{g}^*\) 是双线性的且对称的。设 \(\eta\) 是 \(V^*\) 中的一个余向量，使得对所有余向量 \(\zeta \in V^*\)，都有 \(\langle\eta,\zeta\rangle^*=0\)，即 \(\langle\eta^S,\zeta^S\rangle=0\)。由于 \(S\) 是满射，当 \(\zeta\) 遍历 \(V^*\) 时，\(\zeta^S\) 遍历 \(V\)。因为 \(\mathfrak{g}\) 是非退化的，所以 \(\eta^S=0\)；又由于 \(S\) 是单射，由定理1.1.12，\(\eta=0\)。因此，\(\mathfrak{g}^*\) 是非退化的。

设 \(\mathcal{F}\) 和 \(\mathcal{S}\) 分别为对应于 \(\mathfrak{g}^*\) 的平坦映射和尖锐映射。由等同 \(V^{**}=V\)，我们有

\[
\mathcal{F}: V^* \longrightarrow V \quad \text{and} \quad \mathcal{S}: V \longrightarrow V^*.
\]

也许更明显的记号选择是用 \(\mathrm{F}^*\) 代替 \(\mathcal{F}\)，用 \(\mathrm{S}^*\) 代替 \(\mathcal{S}\)。然而，回顾定理3.3.5，这会把我们现在用 \(\mathcal{F}\) 和 \(\mathcal{S}\) 表示的对象与 \(\mathrm{F}\) 的拉回和 \(\mathrm{S}\) 的拉回混淆。

**定理4.5.2.** 若 \((V,\mathfrak{g})\) 是一个标量积空间，且 \(\mathrm{F}\) 和 \(\mathrm{S}\) 是对应于 \(\mathfrak{g}\) 的平坦映射和尖锐映射，则：

- (a) \(\mathcal{F}=\mathrm{S}\)。
- (b) \(\mathcal{S}=\mathrm{F}\)。

*证明.* (a)：对于 \(V^*\) 中的余向量 \(\eta,\zeta\)，我们有

\[
\begin{aligned}
\eta^{\mathcal{F}}(\zeta)&=\langle\eta,\zeta\rangle^* \quad [(3.3.1)]\\
&=\langle\eta^{\mathrm{S}},\zeta^{\mathrm{S}}\rangle \quad [(4.5.1)]\\
&=\langle\zeta^{\mathrm{S}},\eta^{\mathrm{S}}\rangle\\
&=(\zeta^{\mathrm{S}})^{\mathrm{F}}(\eta^{\mathrm{S}}) \quad [(3.3.1)]\\
&=\zeta(\eta^{\mathrm{S}})\\
&=\eta^{\mathrm{S}}(\zeta). \quad [(1.2.3)]
\end{aligned}
\]

由于 \(\eta\) 和 \(\zeta\) 是任意的，所以 \(\mathcal{F}=\mathrm{S}\)。

(b)：我们有

\[
\begin{aligned}
\mathcal{S}&=\mathcal{F}^{-1} \quad [(3.3.5)]\\
&=\mathrm{S}^{-1} \quad [\text{part (a)}]\\
&=\mathrm{F}. \quad [(3.3.5)]
\end{aligned}
\]

**定理4.5.3.** 设 \((V,\mathfrak{g})\) 是一个标量积空间，设 \(\mathcal{H}=(h_1,\dots,h_m)\) 是 \(V\) 的一组基，设 \(\Theta=(\theta^1,\dots,\theta^m)\) 是其对偶基，设 \(\mathfrak{g}_{\mathcal{H}}^{-1}=[\mathfrak{g}^{ij}]\)。则：

- (a) 对 \(j=1,\dots,m\)，\(\theta^{j\mathrm{S}}=\sum_i \mathfrak{g}^{ij} h_i\)。
- (b) \([\mathrm{S}]_{\Theta}^{\mathcal{H}}=\mathfrak{g}_{\Theta}^*=\mathfrak{g}_{\mathcal{H}}^{-1}\)。

*证明.* (b)：由于标量积是对称的，其关于给定基的矩阵也是对称的。因此，定理3.3.1(b)中的转置可以省略。于是我们有

\[
\begin{aligned}
\mathfrak{g}_{\Theta}^{*} &= [\mathcal{F}]_{\Theta}^{\mathcal{H}} && \text{[Th 3.3.1(b)]}\\
&= [\mathrm{S}]_{\Theta}^{\mathcal{H}} && \text{[Th 4.5.2(a)]}\\
&= [\mathrm{F}^{-1}]_{\Theta}^{\mathcal{H}}\\
&= \left([\mathrm{F}]_{\mathcal{H}}^{\Theta}\right)^{-1} && \text{[Th 2.2.5(b)]}\\
&= \mathfrak{g}_{\mathcal{H}}^{-1}. && \text{[Th 3.3.1(b)]}
\end{aligned}
\]

(a)：这由(2.2.2)、(2.2.3)以及(b)部分得出。

**定理4.5.4（指标提升）。**

设 \((V,g)\) 是一个标量积空间，设 \(\mathcal{H}=(h_1,\ldots,h_m)\) 是 \(V\) 的一组基，设 \(\Theta=(\theta^1,\ldots,\theta^m)\) 是其对偶基。设 \(g_{\mathcal{H}}^{-1}=[g^{ij}]\)，设 \(\eta\) 是 \(V^*\) 中的一个余向量，且

\[
\eta=\sum_i a_i\theta^i.
\tag{4.5.3}
\]

则

\[
\eta^{\mathrm{S}}=\sum_i a^i h_i,
\]

其中

\[
a^i=\sum_j g^{ij}a_j.
\tag{4.5.4}
\]

对 \(i=1,\ldots,m\) 成立。更简洁地表示为：

\[
[\eta^{\mathrm{S}}]_{\mathcal{H}}=g_{\mathcal{H}}^{-1}[\eta]_{\Theta}.
\]

*证明.* 这可由定理3.3.2和定理4.5.2(a)推出，但单独演算细节是有启发性的。设 \(\zeta\) 是 \(V^*\) 中的一个余向量，并注意到 \(g_{\Theta}^{*}=\left[\langle\theta^i,\theta^j\rangle^*\right]\)。则

\[
\begin{aligned}
\eta^{\mathrm{S}}(\zeta) &= \eta^{\mathcal{F}}(\zeta) && \text{[Th 4.5.2(a)]}\\
&=\langle\eta,\zeta\rangle^* && \text{[(3.3.1)]}\\
&=\langle\zeta,\eta\rangle^*\\
&=\left\langle\sum_i\zeta(h_i)\theta^i,\sum_j a_j\theta^j\right\rangle^* && \text{[Th 1.2.1(e)]}\\
&=\sum_{ij}\zeta(h_i)a_j\langle\theta^i,\theta^j\rangle^*\\
&=\sum_{ij}g^{ij}a_jh_i(\zeta) && \text{[(1.2.3), Th 4.5.3(b)]}\\
&=\left(\sum_i\left[\sum_jg^{ij}a_j\right]h_i\right)(\zeta)\\
&=\left(\sum_i a^ih_i\right)(\zeta).
\end{aligned}
\]

