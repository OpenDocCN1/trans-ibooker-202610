
对于 \(i=1,\ldots,r\)，并且

\[
\mathcal{F}(\omega^1,\ldots,\omega^r,X_1,\ldots,fX_j,\ldots,X_s)=f\mathcal{F}(\omega^1,\ldots,\omega^r,X_1,\ldots,X_j,\ldots,X_s)
\]

对于 \(j=1,\ldots,s\)。在后续章节中，我们会遇到此类计算的若干例子。

在本节结束时，让我们就“表示”作几点说明。在 15.2 节中，我们说明了 \(\mathfrak{X}(M)\) 中的一个向量场等价于从 \(C^\infty(M)\) 到 \(C^\infty(M)\) 的一类映射。在 15.5 节中，我们证明了 \(\mathfrak{X}^*(M)\) 中的一个向量场等价于从 \(\mathfrak{X}(M)\) 到 \(C^\infty(M)\) 的一类映射。在本节中，我们说明了（或至少断言）\(\mathcal{T}^r_s(M)\) 中的一个张量场等价于从 \(\mathfrak{X}^*(M)^r\times\mathfrak{X}(M)^s\) 到 \(C^\infty(M)\) 的一类映射。粗略地说，我们一直在进行一项将“场”表示为生成“函数”的映射的工作。

## 15.8 微分形式

设 \(M\) 是一个光滑 \(m\)-流形，令 \(0\le s\le m\) 是一个整数。**\(M\) 上的微分 \(s\)-形式**是一个映射 \(\omega\)，它给 \(M\) 中的每个点 \(p\) 指派 \(\Lambda^s(T_p(M))\) 中的一个 \(s\)-余向量 \(\omega_p\)。在文献中，微分 \(s\)-形式通常被称为 **\(s\)-形式** 或简称为 **形式**。注意，1-形式与余向量场是同一个概念。设 \(X_1,\ldots,X_s\) 是 \(\mathfrak{X}(M)\) 中的向量场，并定义一个函数

\[
\omega(X_1,\ldots,X_s):M\longrightarrow\mathbb{R}
\]

如下：

\[
\omega(X_1,\ldots,X_s)(p)=\omega_p(X_1|_p,\ldots,X_s|_p)
\]

其中 \(p\) 取遍 \(M\) 中的所有点。我们说 \(\omega\) 是 **在 \(M\) 上光滑** 的，如果对于 \(\mathfrak{X}(M)\) 中的所有向量场 \(X_1,\ldots,X_s\)，函数 \(\omega(X_1,\ldots,X_s)\) 都在 \(C^\infty(M)\) 中。\(M\) 上光滑 \(s\)-形式的集合记为 \(\Lambda^s(M)\)。显然，\(\Lambda^s(M)\) 是 \(\mathcal{T}^0_s(M)\) 的一个 \(\mathbb{R}\)-子空间和 \(C^\infty(M)\)-子模，并且

\[
\Lambda^1(M)=\mathfrak{X}^*(M)=\mathcal{T}^0_1(M). \tag{15.8.1}
\]

为完整起见，并且为了与 (15.6.3) 保持一致，我们定义

\[
\Lambda^0(M)=C^\infty(M). \tag{15.8.2}
\]

根据定理 7.2.12(b)，当 \(s>m\) 时，我们令 \(\Lambda^s(M)=\{0\}\)。

设 \(\omega\) 和 \(\xi\) 分别是 \(\Lambda^s(M)\) 和 \(\Lambda^{s'}(M)\) 中的形式。我们定义一个 \(\Lambda^{s+s'}(M)\) 中的形式 \(\omega\wedge\xi\)，称为 \(\omega\) 和 \(\xi\) 的**楔积**，定义为

\[
(\omega\wedge\xi)_p=\omega_p\wedge\xi_p
\]

其中 \(p\) 取遍 \(M\) 中的所有点。特别地，对于 \(C^\infty(M)=\Lambda^0(M)\) 中的函数 \(f\)，有

\[
f\wedge\omega=f\omega. \tag{15.8.3}
\]

设 \((U,(x^i))\) 是 \(M\) 上的一个坐标卡，令 \(1\le i_1<\cdots<i_s\le m\) 是整数，并令

\[
d(x^{i_1})\wedge\cdots\wedge d(x^{i_s}):U\longrightarrow \Lambda^s(U)
\]

为根据对应关系

\[
p\longmapsto d_p(x^{i_1})\wedge\cdots\wedge d_p(x^{i_s})
\]

定义的映射，其中 \(p\) 取遍 \(U\) 中的所有点。设 \(\omega\) 是 \(\Lambda^s(M)\) 中的一个形式。那么 \(\omega|_U\) 可以表示为

\[
\omega|_U=\sum_{1\le i_1<\cdots<i_s\le m}\alpha_{i_1,\ldots,i_s}\,d(x^{i_1})\wedge\cdots\wedge d(x^{i_s}),\tag{15.8.4}
\]

其中 \(\alpha_{i_1,\ldots,i_s}\) 是 \(C^\infty(U)\) 中唯一确定的函数，称为 \(\omega\) 关于 \((U,(x^i))\) 的**分量**。为简洁起见，我们用 \(\omega\) 表示 \(\omega|_U\)。

(15.8.4) 的右端称为在 **局部坐标** 下关于 \((U,(x^i))\) 表达 \(\omega\)。

**定理 15.8.1.** 设 \(M\) 是一个光滑流形，设 \(\omega,\omega^1,\omega^2\) 和 \(\xi,\xi^1,\xi^2\) 分别是 \(\Lambda^s(M)\) 和 \(\Lambda^{s'}(M)\) 中的形式，并设 \(f\) 是 \(C^\infty(M)\) 中的一个函数。则：

- (a) \((\omega^1+\omega^2)\wedge\xi=\omega^1\wedge\xi+\omega^2\wedge\xi.\)
- (b) \(\omega\wedge(\xi^1+\xi^2)=\omega\wedge\xi^1+\omega\wedge\xi^2.\)
- (c) \((f\omega)\wedge\xi=f(\omega\wedge\xi)=\omega\wedge(f\xi)\)。

*证明.* 这是定理 7.2.2 的流形版本。

**定理 15.8.2.** 设 \(M\) 是一个光滑流形，设 \(\omega\) 和 \(\xi\) 分别是 \(\Lambda^s(M)\) 和 \(\Lambda^{s'}(M)\) 中的形式。则：

- (a) \(\omega\wedge\xi=(-1)^{ss'}\xi\wedge\omega.\)
- (b) 若 \(s=s'=1\)，则 \(\omega\wedge\xi=-\xi\wedge\omega.\)
- (c) 若 \(s=1\)，则 \(\omega\wedge\omega=0.\)

*证明.* 这是定理 7.2.3 的流形版本。

**定理 15.8.3.** 设 \(M\) 是一个光滑流形，设 \(\omega^1,\ldots,\omega^s\) 是 \(\mathfrak{X}^*(M)\) 中的余向量场，并设 \(X_1,\ldots,X_s\) 是 \(\mathfrak{X}(M)\) 中的向量场。则：

- (a)
\[
\omega^1\wedge\cdots\wedge\omega^s(X_1,\ldots,X_s)=
\det\begin{pmatrix}
\omega^1(X_1)&\cdots&\omega^1(X_s)\\
\vdots&\ddots&\vdots\\
\omega^s(X_1)&\cdots&\omega^s(X_s)
\end{pmatrix}.
\]

- (b) 若对于某个 \(1\le i<j\le s\) 有 \(\omega^i=\omega^j\)，则 \(\omega^1\wedge\cdots\wedge\omega^s=0\)。

*证明.* 这是定理 7.2.6 的流形版本与定理 7.2.8 的流形版本的结合。

**定理 15.8.4.** 设 \(M\) 是一个光滑 \(m\)-流形，并设 \(U\) 是 \(M\) 中的一个开集。若 \((X_1,\ldots,X_m)\) 是 \(U\) 上的一个标架，且 \((\omega^1,\ldots,\omega^m)\) 是其对偶标架，则

\[
\omega^{j_1}\wedge\cdots\wedge\omega^{j_s}(X_{i_1},\ldots,X_{i_s})=
\begin{cases}
1 & \text{if } (i_1,\ldots,i_s)=(j_1,\ldots,j_s),\\
0 & \text{if } (i_1,\ldots,i_s)\neq(j_1,\ldots,j_s).
\end{cases}
\]

对一切 \(1\le i_1<\cdots<i_s\le m\) 和 \(1\le j_1<\cdots<j_s\le m\) 成立。特别地，若 \((U,(x^i))\) 是 \(M\) 上的一个坐标卡，且 \((\partial/\partial x^1,\ldots,\partial/\partial x^m)\) 和 \((d(x^1),\ldots,d(x^m))\) 分别是相应的坐标标架和对偶坐标标架，则

\[
d(x^{j_1})\wedge\cdots\wedge d(x^{j_s})(\partial/\partial x^{i_1},\ldots,\partial/\partial x^{i_s})=
\begin{cases}
1 & \text{if } (i_1,\ldots,i_s)=(j_1,\ldots,j_s),\\
0 & \text{if } (i_1,\ldots,i_s)\neq(j_1,\ldots,j_s).
\end{cases}
\]

对一切 \(1\le i_1<\cdots<i_s\le m\) 和 \(1\le j_1<\cdots<j_s\le m\) 成立。

*证明.* 这是定理 7.2.9 的流形版本。

**定理 15.8.5.** 设 \(M\) 是一个光滑 \(m\)-流形，设 \((U,(x^i))\) 是 \(M\) 上的一个坐标卡，并设 \((d(x^1),\ldots,d(x^m))\) 是相应的对偶坐标标架。若 \(\omega\) 是 \(\Lambda^m(U)\) 中的一个形式，则存在唯一确定的函数 \(f\in C^\infty(U)\)，使得 \(\omega = f\, d(x^1)\wedge\cdots\wedge d(x^m)\)。

*证明.* 设 \((\partial/\partial x^1,\ldots,\partial/\partial x^m)\) 是与 \((U,(x^i))\) 对应的坐标标架，为简洁起见，令 \(\Omega = d(x^1)\wedge\cdots\wedge d(x^m)\)。对于 \(U\) 中的每个点 \(p\)，根据定义，\(\omega_p\) 是 \(T_p^*(U)\) 中的一个余向量。由定理 7.2.12(d) 可知，存在某个实数 \(f(p)\) 使得 \(\omega_p = f(p)\Omega_p\)。对应关系 \(p\longmapsto f(p)\) 定义了函数 \(f:U\longrightarrow\mathbb{R}\)，使得 \(\omega = f\Omega\)。由于 \(\omega\) 属于 \(\Lambda^m(U)\)，根据定义，对 \(\mathfrak{X}(U)\) 中的所有向量场 \(X_1,\ldots,X_m\)，\(\omega(X_1,\ldots,X_m)\) 都是 \(C^\infty(U)\) 中的函数。特别地，对 \(\partial/\partial x^1,\ldots,\partial/\partial x^m\) 也如此。由定理 15.8.4，

\[
f = f\,\Omega\left(\frac{\partial}{\partial x^1},\ldots,\frac{\partial}{\partial x^m}\right) = \omega\left(\frac{\partial}{\partial x^1},\ldots,\frac{\partial}{\partial x^m}\right),
\]

因此 \(f\in C^\infty(U)\)。\(f\) 的唯一性由前面的计算得出。

## 15.9 函数的前推与拉回

设 \(M\) 是一个光滑流形，并设 \(F : M \longrightarrow N\) 是一个微分同胚。由 \(F\) 诱导的前推（对于函数）是如下映射

\[
F_* : C^\infty(M) \longrightarrow C^\infty(N)
\]

定义为

\[
F_*(f) = f \circ F^{-1} \tag{15.9.1}
\]

其中 \(f\) 取遍 \(C^\infty(M)\) 中的所有函数。

**定理 15.9.1.** 若 \(M, N\) 和 \(P\) 都是光滑流形，且 \(F : M \longrightarrow N\) 和 \(G : N \longrightarrow P\) 都是微分同胚，则

\[
(G \circ F)_* = G_* \circ F_*.
\]

*证明.* 对于 \(C^\infty(M)\) 中的一个函数 \(f\)，我们有

\[
(G \circ F)_*(f) = f \circ (G \circ F)^{-1} = (f \circ F^{-1}) \circ G^{-1} = F_*(f) \circ G^{-1} = G_*(F_*(f)) = (G_* \circ F_*)(f).
\]

由于 \(f\) 是任意的，结论得证。

设 \(M\) 和 \(N\) 是光滑流形，并设 \(F : M \longrightarrow N\) 是一个光滑映射（但不一定是微分同胚）。由 \(F\) 诱导的拉回（对于函数）是如下映射

\[
F^* : C^\infty(N) \longrightarrow C^\infty(M)
\]

定义为

\[
F^*(g) = g \circ F \tag{15.9.2}
\]

其中 \(g\) 取遍 \(C^\infty(N)\) 中的所有函数。

**定理 15.9.2.** 若 \(M, N\) 和 \(P\) 都是光滑流形，且 \(F : M \longrightarrow N\) 和 \(G : N \longrightarrow P\) 都是光滑映射，则

\[
(G \circ F)^* = F^* \circ G^*.
\]

*证明.* 对于 \(C^\infty(P)\) 中的一个函数 \(g\)，我们有

\[
(G \circ F)^*(g) = (g \circ G) \circ F = G^*(g) \circ F = F^*(G^*(g)) = (F^* \circ G^*)(g).
\]

由于 \(g\) 是任意的，结论得证。

**定理 15.9.3.** 若 \(M\) 是一个光滑流形且 \(F : M \longrightarrow N\) 是一个微分同胚，则：

- (a) \(F^*\) 是双射，其逆为 \((F^*)^{-1} = (F^{-1})^* = F_*\)。
- (b) \(F_*\) 是双射，其逆为 \((F_*)^{-1} = (F^{-1})_* = F^*\)。

*证明.* (a)：由定理 15.9.2，

\[
\mathrm{id}_{C^\infty(M)} = (\mathrm{id}_M)^* = (F^{-1} \circ F)^* = F^* \circ (F^{-1})^*
\]

且

\[
\mathrm{id}_{C^\infty(N)} = (\mathrm{id}_N)^* = (F \circ F^{-1})^* = (F^{-1})^* \circ F^*.
\]

由定理 A.4 可知 \(F^*\) 是双射。前面的恒等式给出第一个等式。对于 \(C^\infty(M)\) 中的一个函数 \(f\)，我们有

\[
(F^{-1})^*(f) = f \circ F^{-1} = F_*(f).
\]

由于 \(f\) 是任意的，第二个等式成立。

(b)：由 (a) 直接得出。

