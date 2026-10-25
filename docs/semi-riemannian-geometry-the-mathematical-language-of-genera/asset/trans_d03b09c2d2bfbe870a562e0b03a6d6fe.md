

在本节中，我们提供了积分向量微积分的若干经典定理的现代版本。

设 \(\lambda(t) : [a,b] \longrightarrow \mathbb{R}^3_0\) 为一条光滑曲线，且 \(X = (\alpha^1,\alpha^2,\alpha^3)\) 为 \(\mathfrak{X}(\mathbb{R}^3_0)\) 中的一个向量场。\(X\) 的经典线积分记为 \(\int_C X \cdot ds\)（还有其他记法）并定义为

\[
\int_C X \cdot ds = \int_a^b \langle X_{\lambda(t)}, d\lambda/dt(t) \rangle\, dt,
\]

其中 \(C\) 为 \(\lambda\) 的像。下一个结果表明，除向量场到等价协向量场的转换之外，经典线积分与第 16.8 节中引入的线积分本质上是相同的。

### 定理 21.2.1（经典线积分）

在上述设定下继续，设 \(\omega\) 为 \(\mathfrak{X}^*(\mathbb{R}^3_0)\) 中的协向量场，由

\[
\omega = \alpha^1 d(x) + \alpha^2 d(y) + \alpha^3 d(z).
\]

给出。则

\[
\int_C X \cdot ds = \int_\lambda \omega.
\]

证明。设 \(\lambda = (\lambda^1,\lambda^2,\lambda^3)\)。我们有

\[
\begin{aligned}
\omega_{\lambda(t)}(d\lambda/dt(t)) &= \alpha^1(\lambda(t)) d(x)(d\lambda/dt(t)) + \alpha^2(\lambda(t)) d(y)(d\lambda/dt(t)) + \alpha^3(\lambda(t)) d(z)(d\lambda/dt(t))\\
&= \alpha^1(\lambda(t)) d\lambda^1/dt(t) + \alpha^2(\lambda(t)) d\lambda^2/dt(t) + \alpha^3(\lambda(t)) d\lambda^3/dt(t)\\
&= \langle X_{\lambda(t)}, d\lambda/dt(t) \rangle.
\end{aligned}
\]

结果现在由 (16.8.4) 得出。

设 \(U\) 为 \(\mathbb{R}^2\) 中的一个开集，\(S\) 为 \(\mathbb{R}^3\) 中的一个集合，\(F = (F^1,F^2,F^3) : U \longrightarrow S\) 为具有紧支集的微分同胚，且 \(X = (\alpha^1,\alpha^2,\alpha^3)\) 为 \(\mathfrak{X}(\mathbb{R}^3_0)\) 中的一个向量场。\(X\) 的经典面积分记为 \(\iint_S X \cdot dA\)（还有其他记法）并定义为

\[
\iint_S X \cdot dA = \iint_U \langle X \circ F, \partial F/\partial r \times \partial F/\partial s \rangle\, dr\, ds,
\]

其中我们注意到 \(S\) 为 \(F\) 的像。

### 定理 21.2.2（经典面积分）

在上述设定下继续，设 \(\omega\) 为 \(\Lambda^2(\mathbb{R}^3)\) 中的形式，由

\[
\omega = \alpha^1 d(y) \wedge d(z) + \alpha^2 d(z) \wedge d(x) + \alpha^3 d(x) \wedge d(y).
\]

给出。则

\[
\iint_S X \cdot dA = \int_S \omega.
\]

证明。设 \(\omega\) 为 \(\mathfrak{X}^*(\mathbb{R}^2)\) 中的协向量场，由 \(\omega = \alpha^1 d(r) + \alpha^2 d(s)\) 给出。根据 Stokes 定理，

\[
\int_D d(\omega) = \int_{\partial D} \omega.
\]

与导出 (16.1.2) 类似的计算给出

\[
d(\omega) = \left(\frac{\partial\alpha^2}{\partial r} - \frac{\partial\alpha^1}{\partial s}\right) d(r) \wedge d(s),
\]

然后 (16.7.2) 给出

\[
\int_D d(\omega) = \iint_D \left(\frac{\partial\alpha^2}{\partial r} - \frac{\partial\alpha^1}{\partial s}\right) dr\, ds.
\]

结果由此得出。

下一个结果的设定与定理 19.11.4 的设定相同，只是 \((M,\epsilon|_M)\) 假定为紧致的，这是确保某些积分存在的必要条件。

### 定理 21.2.4（经典 Stokes 定理）

设 \((M,\epsilon|_M)\) 为 \(\mathbb{R}^3_0 = (\mathbb{R}^3,\epsilon)\) 的一个紧致可定向黎曼超曲面带边界，\(V\) 为 \(\mathfrak{X}_\epsilon(M)\) 中的单位法向量场（由定理 19.11.1 给出），且 \(\mathcal{D}_M\) 为由 \(V\) 诱导的 \(M\) 的定向，使得 \((M,\epsilon|_M,\mathcal{D}_M)\) 为 \(\mathbb{R}^3_0\) 的一个紧致定向黎曼超曲面带边界。设 \(\Omega_M\) 为其体积形式。设 \(W\) 为 \(\mathfrak{X}_M(\partial M)\) 中的唯一外指向单位法向量场（由定理 19.11.3 给出），且 \(\partial_M\) 为由 \(W\) 诱导的（Stokes）定向，使得 \((\partial M,\epsilon|_{\partial M},\mathcal{D}_{\partial M})\) 为 \((M,\epsilon|_M,\mathcal{D}_M)\) 的一个定向黎曼超曲面。设 \(\Omega_{\partial M}\) 为其体积形式，且 \(U = V \times W\)。若 \(X\) 为 \(\mathfrak{X}(\mathbb{R}^3_0)\) 中的一个向量场，则

\[
\int_M \langle \operatorname{Curl}(X), V \rangle\, \Omega_M = \int_{\partial M} \langle X, U \rangle\, \Omega_{\partial M}.
\]

证明。根据 Stokes 定理，

\[
\int_M d(X^\flat) = \int_{\partial M} X^\flat. \tag{21.2.1}
\]

我们寻求 (21.2.1) 中被积函数的替代表达式。对于左边，定理 19.11.2(b) 和定理 20.5.1 给出

\[
d(X^\flat) = i_{\operatorname{Curl}(X)}(\Omega_{\mathbb{R}^3_0})|_M = \langle \operatorname{Curl}(X), V \rangle\, \Omega_M, \tag{21.2.2}
\]

其中 \(\Omega_{\mathbb{R}^3_0}\) 为 \(\mathbb{R}^3_0\) 上的体积形式。对于右边，根据定理 19.11.4(e)，

\[
\Omega_{\partial M}(U) = \pm 1_{\partial M}. \tag{21.2.3}
\]

由于 \(X^\flat|_{\partial M}\) 为 \(\mathfrak{X}^*(\partial M) = \Lambda^1(\partial M)\) 中的一个协向量场，由定理 3.3.3、定理 15.8.5 和定理 19.10.2 可知存在唯一的函数 \(f\) 在 \(C^\infty(\partial M)\) 中使得

\[
X^\flat|_{\partial M} = f\, \Omega_{\partial M}. \tag{21.2.4}
\]

我们有

\[
\begin{aligned}
f &= f\, \Omega_{\partial M}(U) && [(21.2.3)]\\
&= X^\flat|_{\partial M}(U) && [(21.2.4)]\\
&= \langle X|_{\partial M}, U \rangle. && [(19.4.1)]
\end{aligned} \tag{21.2.5}
\]

然后 (21.2.4) 和 (21.2.5) 给出

\[
X^\flat|_{\partial M} = \langle X|_{\partial M}, U \rangle\, \Omega_{\partial M}. \tag{21.2.6}
\]

将 (21.2.2) 和 (21.2.6) 代入 (21.2.1) 得到结果。

### 定理 21.2.5（经典散度定理）

设 \((M,\mathfrak{e}|_M,\mathfrak{O}_M)\) 为 \(\mathbb{R}^3_0 = (\mathbb{R}^3,\mathfrak{e})\) 的一个紧致定向黎曼 3-子流形带边界，使得 \((\partial M,\mathfrak{e}|_{\partial M})\) 为 \((M,\mathfrak{e}|_M,\mathfrak{O}_M)\) 的一个黎曼超曲面，且 \(X\) 为 \(\mathfrak{X}(M)\) 中的一个向量场。设 \(V\) 为 \(\mathfrak{X}_M(\partial M)\) 中的唯一外指向单位法向量场（由定理 19.11.3 给出），且 \(\partial M\) 具有由 \(V\) 诱导的定向 \(\mathfrak{O}_{\partial M}\)，使得 \((\partial M,\mathfrak{e}|_{\partial M},\mathfrak{O}_{\partial M})\) 为 \((M,\mathfrak{e}|_M,\mathfrak{O}_M)\) 的一个定向黎曼超曲面。设 \(\Omega_{\partial M}\) 为其体积形式。则

\[
\int_M \operatorname{Div}(X)\, \Omega_M = \int_{\partial M} \langle X, V \rangle\, \Omega_{\partial M}.
\]

证明。我们有

\[
\begin{aligned}
\int_M \operatorname{Div}(X)\, \Omega_M &= \int_{\partial M} i_X(\Omega_M) && [\text{Th 20.4.5}]\\
&= \int_{\partial M} \langle X, V \rangle\, \Omega_{\partial M}. && [\text{Th 19.11.2(b)}]
\end{aligned}
\]

## 第 22 章 物理学应用

### 22.1 洛伦兹向量空间上的线性等距映射

设 \((V,\mathfrak{g},\mathfrak{O})\) 为一个定向洛伦兹向量空间，且 \(\mathcal{E} = (e_1,\dots,e_m)\) 为 \(V\) 的一个正交基，关于 \(\mathfrak{O}\) 为正定向。我们用 \(\mathsf{L}_m\) 表示 \(V\) 上的线性等距映射的集合：

\[
\mathsf{L}_m = \{A \in \operatorname{Lin}(V,V) : A \text{ a linear isometry}\}
\]

其中我们回顾 \(\operatorname{Lin}(V,V)\) 为从 \(V\) 到 \(V\) 的线性映射的向量空间。

### 定理 22.1.1

\(\mathsf{L}_m\) 是一个群（在线性映射的复合下），称为 \(V\) 上的 *洛伦兹群*。

证明。设 \(A\) 为 \(\mathsf{L}_m\) 中的一个映射。由定理 1.1.9 和定理 4.4.1(a) 可知逆 \(A^{-1}\) 存在且在 \(\operatorname{Lin}(V,V)\) 中。设 \(v,w\) 为 \(V\) 中的向量。则

\[
\langle v,w \rangle = \langle A(A^{-1}(v)), A(A^{-1}(w)) \rangle = \langle A^{-1}(v), A^{-1}(w) \rangle,
\]

所以 \(A^{-1}\) 在 \(\mathsf{L}_m\) 中。\(\mathsf{L}_m\) 为群的其余验证是直接的。

设 \(A\) 为 \(\operatorname{Lin}(V,V)\) 中的一个映射。为简便起见，我们用 \([A]_{\mathcal{E}}^{\mathcal{E}}\) 表示为 \([A]\)，并且同样省略其他矩阵记号中的 \(\mathcal{E}\)。对应于 (4.4.1) 和 (4.4.2)，我们有

\[
[A] = \begin{bmatrix} [A]_1^1 & [A]_1^2 \\ [A]_2^1 & \gamma \end{bmatrix}
\]

和

\[
[A]^{-1} = \begin{bmatrix} ([A]_1^1)^\mathrm{T} & -([A]_1^2)^\mathrm{T} \\ -([A]_2^1)^\mathrm{T} & \gamma \end{bmatrix},
\]

分别，其中

\[
[A]_1^1 = \begin{bmatrix} a_1^1 & \cdots & a_{m-1}^1 \\ \vdots & \ddots & \vdots \\ a_1^{m-1} & \cdots & a_{m-1}^{m-1} \end{bmatrix},
\]

\[
[A]_2^1 = \begin{bmatrix} a_m^1 \\ \vdots \\ a_m^{m-1} \end{bmatrix},
\]

和

\[
[A]_1^2 = [a_1^m \cdots a_{m-1}^m].
\]

由定理 2.2.3(b) 给出的环同构限制为 \(\mathsf{L}_m\) 与一般线性群 \(\operatorname{GL}(m) \subset \operatorname{Mult}_{m\times m}\) 的一个子群之间的（乘性）群同构。在下文中，但不再提及，使用此群同构来证明关于 \(\mathsf{L}_m\) 群结构的断言。特别是，我们依赖于以下事实：\(\mathsf{L}_m\) 的一个子集是子群当且仅当对应的 \(\operatorname{GL}(m)\) 的子集是子群。

### 定理 22.1.2

若 \(A\) 为 \(\mathsf{L}_m\) 中的一个映射，则：

- (a) \([A]_1^1([A]_1^1)^T = I_{m-1} + [A]_2^1([A]_2^1)^T = I_{m-1} + ([A]_1^2)^T [A]_1^2\)。
- (b) \(\gamma[A]_2^1 = [A]_1^1([A]_1^2)^T\)。
- (c) \(\gamma[A]_1^2 = ([A]_2^1)^T [A]_1^1\)。
- (d) \(\gamma^2 = 1 + ([A]_2^1)^T [A]_2^1 = 1 + [A]_1^2([A]_1^2)^T\)。

证明。(a)：这由定理 4.4.6 的部分 (a) 和 (d) 得出。
(b)：这由定理 4.4.6(b) 得出。
(c)：这由定理 4.4.6(e) 得出。
(d)：这由定理 4.4.6 的部分 (c) 和 (f) 得出。

由定理 22.1.2(d) 可知

\[
\gamma^2 \ge 1. \tag{22.1.1}
\]

让我们定义

\[
\beta = \frac{\sqrt{\gamma^2 - 1}}{\gamma}. \tag{22.1.2}
\]

