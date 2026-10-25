## 第20章 半黎曼流形上的微分算子

### 20.1 霍奇星算子

设 $(M, g, \mathfrak{O})$ 是指标为 $\nu$ 的定向半黎曼 $m$-流形，并设 $\Omega_M$ 为其体积形式。霍奇星算子是一族线性映射

$$\star : \Lambda^s(M) \longrightarrow \Lambda^{m-s}(M)$$

对 $0 \le s \le m$ 定义，它是第8.5节所讨论的内积空间上的霍奇星算子的流形版本。因此，对 $\Lambda^s(M)$ 中的每个形式 $\omega$，在 $\Lambda^{m-s}(M)$ 中存在唯一的形式 $\star(\omega)$，使得

$$\omega \wedge \xi = \langle \star(\omega), \xi \rangle^{\Lambda} \Omega_M$$

对所有 $\Lambda^{m-s}(M)$ 中的形式 $\xi$ 成立。

**定理20.1.1.** 设 $(M, g, \mathfrak{O})$ 是指标为 $\nu$ 的定向半黎曼 $m$-流形，设 $\Omega_M$ 为其体积形式，并设 $\omega, \xi$ 为 $\Lambda^s(M)$ 中的形式。则：

- (a) $\star(1_M) = (-1)^\nu \Omega_M$。
- (b) $\star(\Omega_M) = 1_M$。
- (c) $\star^2(\omega) = (-1)^{s(m-s)+\nu} \omega$。
- (d) $\omega \wedge \star(\xi) = (-1)^\nu \langle \omega, \xi \rangle^{\Lambda} \Omega_M$。

**证明。** 这是定理8.5.3的流形版本。

**定理20.1.2.** 设 $(M, g, \mathfrak{O})$ 是指标为 $\nu$ 的定向半黎曼 $m$-流形，设 $\Omega_M$ 为其体积形式，并设 $X$ 为 $\mathfrak{X}(M)$ 中的向量场。则

$$i_X(\Omega_M) = (-1)^\nu \star(X^\flat).$$

**证明。** 这是定理8.5.7的流形版本。

**例20.1.3（电动力学）。** 设 $(x, y, z, t)$ 是 $\mathbb{R}_1^4$ 上的标准坐标，设 $\alpha_1, \alpha_2, \alpha_3, \alpha_4$ 为 $C^\infty(\mathbb{R}_1^4)$ 中的函数，并定义 $\mathfrak{X}^*(\mathbb{R}_1^4)$ 中的余向量场 $\omega$ 为

$$\omega = \alpha_1 d(x) + \alpha_2 d(y) + \alpha_3 d(z) + \alpha_4 d(t).$$

利用例8.5.6，我们有

$$\star(\omega) = \alpha_1 \star d(x) + \alpha_2 \star d(y) + \alpha_3 \star d(z) + \alpha_4 \star d(t)$$

$$= -\alpha_1 d(y) \wedge d(z) \wedge d(t) + \alpha_2 d(x) \wedge d(z) \wedge d(t) - \alpha_3 d(x) \wedge d(y) \wedge d(t) - \alpha_4 d(x) \wedge d(y) \wedge d(z),$$

因此

$$d\star(\omega) = -d(\alpha_1) \wedge d(y) \wedge d(z) \wedge d(t) + d(\alpha_2) \wedge d(x) \wedge d(z) \wedge d(t)$$

$$- d(\alpha_3) \wedge d(x) \wedge d(y) \wedge d(t) - d(\alpha_4) \wedge d(x) \wedge d(y) \wedge d(z).$$

我们还有

$$d(\alpha_1) \wedge d(y) \wedge d(z) \wedge d(t)$$

$$= \left(\frac{\partial \alpha_1}{\partial x} d(x) + \frac{\partial \alpha_1}{\partial y} d(y) + \frac{\partial \alpha_1}{\partial z} d(z) + \frac{\partial \alpha_1}{\partial t} d(t)\right) \wedge d(y) \wedge d(z) \wedge d(t)$$

$$= \frac{\partial \alpha_1}{\partial x} d(x) \wedge d(y) \wedge d(z) \wedge d(t) + \frac{\partial \alpha_1}{\partial y} d(y) \wedge d(y) \wedge d(z) \wedge d(t) + \frac{\partial \alpha_1}{\partial z} d(z) \wedge d(y) \wedge d(z) \wedge d(t) + \frac{\partial \alpha_1}{\partial t} d(t) \wedge d(y) \wedge d(z) \wedge d(t)$$

$$= \frac{\partial \alpha_1}{\partial x} d(x) \wedge d(y) \wedge d(z) \wedge d(t);$$

类似地，

$$d(\alpha_2) \wedge d(x) \wedge d(z) \wedge d(t) = -\frac{\partial \alpha_2}{\partial y} d(x) \wedge d(y) \wedge d(z) \wedge d(t),$$

$$d(\alpha_3) \wedge d(x) \wedge d(y) \wedge d(t) = \frac{\partial \alpha_3}{\partial z} d(x) \wedge d(y) \wedge d(z) \wedge d(t),$$

$$d(\alpha_4) \wedge d(x) \wedge d(y) \wedge d(z) = -\frac{\partial \alpha_4}{\partial t} d(x) \wedge d(y) \wedge d(z) \wedge d(t).$$

于是，

$$d\star(\omega) = \left(-\frac{\partial \alpha_1}{\partial x} - \frac{\partial \alpha_2}{\partial y} - \frac{\partial \alpha_3}{\partial z} + \frac{\partial \alpha_4}{\partial t}\right) d(x) \wedge d(y) \wedge d(z) \wedge d(t).$$

### 20.2 余微分

设 $(M, g, \mathfrak{O})$ 是指标为 $\nu$ 的定向半黎曼 $m$-流形。余微分是一族线性映射

$$\delta : \Lambda^s(M) \longrightarrow \Lambda^{s-1}(M)$$





(c): 由于 \(\star(\omega)\) 是 \(\Lambda^{m-s}(M)\) 中的一个形式，

$$
\begin{aligned}
\delta(\star(\omega))
 &= (-1)^{m(m-s+1)+1+\nu} \star d \star (\star(\omega)) \qquad [(20.2.1)] \\
 &= (-1)^{m(m-s+1)+1+\nu} \star d(\star^2(\omega)) \\
 &= (-1)^{[m(m-s+1)+1+\nu]+[s(m-s)+\nu]} \star d(\omega). \qquad [\mathrm{Th}\ 20.1.1(c)]
\end{aligned}
$$

由于

$$
[m(m-s+1)+1+\nu] + [s(m-s)+\nu] = m(m+1) + 2\nu + 1 - s^2,
$$

因为 \(m(m+1)\) 和 \(2\nu\) 都是偶数，且 \(s^2\) 正好在 \(s\) 为偶数（奇数）时为偶数（奇数），

$$
(-1)^{[m(m-s+1)+1+\nu]+[s(m-s)+\nu]} = (-1)^{s+1}.
$$

结果得证。

### Example 20.2.2 (Electrodynamics)

我们继续 Example 16.1.10，只是将 \(\mathbb{R}^4\) 替换为 \(\mathbb{R}_1^4\)。协向量场 \(\xi\) 和形式 \(\zeta\) 与 \(\varphi\)，现在被视为 \(\mathfrak{X}^*(\mathbb{R}_1^4)\) 和 \(\Lambda^2(\mathbb{R}_1^4)\) 的元素，分别由

$$
\xi = \alpha_1\,dx + \alpha_2\,dy + \alpha_3\,dz,
$$

$$
\zeta = \beta_1\,dy \wedge dz + \beta_2\,dz \wedge dx + \beta_3\,dx \wedge dy,
$$

和

$$
\begin{aligned}
\varphi &= \xi \wedge dt + \zeta \\
&= \alpha_1\,dx \wedge dt + \alpha_2\,dy \wedge dt + \alpha_3\,dz \wedge dt \\
&\quad + \beta_1\,dy \wedge dz + \beta_2\,dz \wedge dx + \beta_3\,dx \wedge dy.
\end{aligned}
$$

我们寻求 \(\delta(\varphi)\) 的表达式。令 \(m=4\)，\(\nu=1\)，且 \(s=2\) 代入 (20.2.1) 得 \(\delta = \star d \star\)。利用 Example 8.5.6 得

$$
\begin{aligned}
\star(\varphi)
&= \alpha_1 \star(dx \wedge dt) + \alpha_2 \star(dy \wedge dt) + \alpha_3 \star(dz \wedge dt) \\
&\quad + \beta_1 \star(dy \wedge dz) + \beta_2 \star(dz \wedge dx) + \beta_3 \star(dx \wedge dy) \\
&= \alpha_1\,dy \wedge dz + \alpha_2\,dz \wedge dx + \alpha_3\,dx \wedge dy \\
&\quad - \bigl[\beta_1\,dx \wedge dt + \beta_2\,dy \wedge dt + \beta_3\,dz \wedge dt\bigr],
\end{aligned}
$$

故

$$
\begin{aligned}
d\star(\varphi)
&= d\alpha_1 \wedge dy \wedge dz + d\alpha_2 \wedge dz \wedge dx + d\alpha_3 \wedge dx \wedge dy \\
&\quad - \bigl[d\beta_1 \wedge dx \wedge dt + d\beta_2 \wedge dy \wedge dt + d\beta_3 \wedge dz \wedge dt\bigr].
\end{aligned}
$$

我们也有

$$
\begin{aligned}
d\alpha_1 \wedge dy \wedge dz
&= \left(\frac{\partial\alpha_1}{\partial x}\,dx + \frac{\partial\alpha_1}{\partial y}\,dy + \frac{\partial\alpha_1}{\partial z}\,dz + \frac{\partial\alpha_1}{\partial t}\,dt\right) \wedge dy \wedge dz \\
&= \frac{\partial\alpha_1}{\partial x}\,dx \wedge dy \wedge dz + \frac{\partial\alpha_1}{\partial y}\,dy \wedge dy \wedge dz \\
&\quad + \frac{\partial\alpha_1}{\partial z}\,dz \wedge dy \wedge dz + \frac{\partial\alpha_1}{\partial t}\,dt \wedge dy \wedge dz \\
&= \frac{\partial\alpha_1}{\partial x}\,dx \wedge dy \wedge dz + \frac{\partial\alpha_1}{\partial t}\,dy \wedge dz \wedge dt;
\end{aligned}
$$

同样，

$$
\begin{aligned}
d\alpha_2 \wedge dz \wedge dx
&= \frac{\partial\alpha_2}{\partial y}\,dx \wedge dy \wedge dz - \frac{\partial\alpha_2}{\partial t}\,dx \wedge dz \wedge dt, \\
d\alpha_3 \wedge dx \wedge dy
&= \frac{\partial\alpha_3}{\partial z}\,dx \wedge dy \wedge dz + \frac{\partial\alpha_3}{\partial t}\,dx \wedge dy \wedge dt, \\
d\beta_1 \wedge dx \wedge dt
&= - \frac{\partial\beta_1}{\partial y}\,dx \wedge dy \wedge dt - \frac{\partial\beta_1}{\partial z}\,dx \wedge dz \wedge dt, \\
d\beta_2 \wedge dy \wedge dt
&= \frac{\partial\beta_2}{\partial x}\,dx \wedge dy \wedge dt - \frac{\partial\beta_2}{\partial z}\,dy \wedge dz \wedge dt, \\
d\beta_3 \wedge dz \wedge dt
&= \frac{\partial\beta_3}{\partial x}\,dx \wedge dz \wedge dt + \frac{\partial\beta_3}{\partial y}\,dy \wedge dz \wedge dt.
\end{aligned}
$$

于是，

$$
\begin{aligned}
d\star(\varphi)
&= \left(\frac{\partial\alpha_1}{\partial x} + \frac{\partial\alpha_2}{\partial y} + \frac{\partial\alpha_3}{\partial z}\right) dx \wedge dy \wedge dz \\
&\quad - \left(\frac{\partial\beta_2}{\partial x} - \frac{\partial\beta_1}{\partial y} - \frac{\partial\alpha_3}{\partial t}\right) dx \wedge dy \wedge dt \\
&\quad + \left(\frac{\partial\beta_1}{\partial z} - \frac{\partial\beta_3}{\partial x} - \frac{\partial\alpha_2}{\partial t}\right) dx \wedge dz \wedge dt \\
&\quad - \left(\frac{\partial\beta_3}{\partial y} - \frac{\partial\beta_2}{\partial z} - \frac{\partial\alpha_1}{\partial t}\right) dy \wedge dz \wedge dt.
\end{aligned}
$$

再次利用 Example 8.5.6，我们得到

$$
\begin{aligned}
\delta(\varphi)
&= \star d \star (\varphi) \\
&= \left(\frac{\partial\alpha_1}{\partial x} + \frac{\partial\alpha_2}{\partial y} + \frac{\partial\alpha_3}{\partial z}\right) \star(dx \wedge dy \wedge dz) \\
&\quad - \left(\frac{\partial\beta_2}{\partial x} - \frac{\partial\beta_1}{\partial y} - \frac{\partial\alpha_3}{\partial t}\right) \star(dx \wedge dy \wedge dt) \\
&\quad + \left(\frac{\partial\beta_1}{\partial z} - \frac{\partial\beta_3}{\partial x} - \frac{\partial\alpha_2}{\partial t}\right) \star(dx \wedge dz \wedge dt) \\
&\quad - \left(\frac{\partial\beta_3}{\partial y} - \frac{\partial\beta_2}{\partial z} - \frac{\partial\alpha_1}{\partial t}\right) \star(dy \wedge dz \wedge dt) \\
&= \left(\frac{\partial\beta_3}{\partial y} - \frac{\partial\beta_2}{\partial z} - \frac{\partial\alpha_1}{\partial t}\right) dx
 + \left(\frac{\partial\beta_1}{\partial z} - \frac{\partial\beta_3}{\partial x} - \frac{\partial\alpha_2}{\partial t}\right) dy \\
&\quad + \left(\frac{\partial\beta_2}{\partial x} - \frac{\partial\beta_1}{\partial y} - \frac{\partial\alpha_3}{\partial t}\right) dz
 - \left(\frac{\partial\alpha_1}{\partial x} + \frac{\partial\alpha_2}{\partial y} + \frac{\partial\alpha_3}{\partial z}\right) dt.
\end{aligned}
$$

## 20.3 Gradient

设 \((M,\mathfrak{g})\) 为半黎曼流形。**梯度** 是映射

$$
\operatorname{Grad}: C^\infty(M) \longrightarrow \mathfrak{X}(M)
$$

定义为

$$
\operatorname{Grad}(f) = d(f)^\sharp,
$$

或等价地，由

$$
\operatorname{Grad}(f)^\flat = d(f)
$$

对所有 \(C^\infty(M)\) 中的函数 \(f\) 成立。我们称 \(\operatorname{Grad}(f)\) 为 \(f\) 的**梯度**。

### Theorem 20.3.1

设 \((M,\mathfrak{g})\) 为半黎曼流形，\(X\) 为 \(\mathfrak{X}(M)\) 中的向量场，\(f\) 为 \(C^\infty(M)\) 中的函数。则：

$$
\langle \operatorname{Grad}(f), X \rangle = d(f)(X) = X(f).
$$

**Proof.** 我们有

$$
\begin{aligned}
\langle \operatorname{Grad}(f), X \rangle
&= \operatorname{Grad}(f)^\flat(X) \qquad [(19.4.1)] \\
&= d(f)(X) \qquad [(20.3.2)] \\
&= X(f). \qquad [\mathrm{Th}\ 15.4.5(b)]
\end{aligned}
$$

### Theorem 20.3.2 (Local Coordinate Expression for Gradient)

若 \((M,\mathfrak{g})\) 为半黎曼 \(m\)-流形且 \(f\) 为 \(C^\infty(M)\) 中的函数，则在局部坐标下，\(\operatorname{Grad}(f)\) 的分量为

$$
\operatorname{Grad}(f)^i = \sum_j g^{ij} \frac{\partial f}{\partial x^j}
$$

对 \(i=1,\ldots,m\)。

**Proof.** 根据定义，

$$
\operatorname{Grad}(f) = \sum_i \operatorname{Grad}(f)^i \frac{\partial}{\partial x^i}.
$$

由 Theorem 20.3.1 可得

$$
\frac{\partial f}{\partial x^j}
= \left\langle \frac{\partial}{\partial x^j}, \operatorname{Grad}(f) \right\rangle
= \left\langle \frac{\partial}{\partial x^j}, \sum_k \operatorname{Grad}(f)^k \frac{\partial}{\partial x^k} \right\rangle
= \sum_k \mathfrak{g}_{jk} \operatorname{Grad}(f)^k,
$$

故

$$
\sum_j \mathfrak{g}^{ij} \frac{\partial f}{\partial x^j}
= \sum_j \mathfrak{g}^{ij} \left( \sum_k \mathfrak{g}_{jk} \operatorname{Grad}(f)^k \right)
= \sum_k \left( \sum_j \mathfrak{g}^{ij} \mathfrak{g}_{jk} \right) \operatorname{Grad}(f)^k
= \sum_k \delta^i_k \operatorname{Grad}(f)^k
= \operatorname{Grad}(f)^i.
$$

### Theorem 20.3.3

若 \((M,\mathfrak{g})\) 为半黎曼 \(m\)-流形且 \(f,g\) 为 \(C^\infty(M)\) 中的函数，则

$$
\operatorname{Grad}(fg) = f\operatorname{Grad}(g) + g\operatorname{Grad}(f).
$$

**Proof.** 根据 Theorem 19.4.3，\(\flat\) 是 \(C^\infty(M)\)-线性的。结果现在由 Theorem 15.4.6(b) 和 (20.3.1) 得出。

前述结果与 Theorem 10.6.1(a) 有明显的相似性。

### Example 20.3.4 \((\mathbb{R}_\nu^m)\)

设 \((r^1,\ldots,r^m)\)，\(\left(\partial/\partial r^1,\ldots,\partial/\partial r^m\right)\)，和 \((\varepsilon_1,\ldots,\varepsilon_m)\) 分别为 \(\mathbb{R}_\nu^m = (\mathbb{R}^m,\mathfrak{g})\) 的标准坐标、对应的标准坐标标架和符号。 由于 \(\mathfrak{g}^{ij} = \mathfrak{g}_{ij} = \varepsilon_i\delta_{ij}\)，我们由 Theorem 20.3.2 得 \(\operatorname{Grad}(f)^i = \varepsilon_i(\partial f/\partial r^i)\)，故

$$
\operatorname{Grad}(f) = \left( \varepsilon_1 \frac{\partial f}{\partial r^1}, \ldots, \varepsilon_m \frac{\partial f}{\partial r^m} \right).
$$

在 \(\mathbb{R}_0^3\) 中，标准坐标为 \((x,y,z)\)，这变为

$$
\operatorname{Grad}(f) = \left( \frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}, \frac{\partial f}{\partial z} \right) = \operatorname{grad}(f),
$$

而在 \(\mathbb{R}_1^4\) 中，标准坐标为 \((x,y,z,t)\)，它变为

$$
\operatorname{Grad}(f) = \left( \frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}, \frac{\partial f}{\partial z}, -\frac{\partial f}{\partial t} \right).
$$

for \(i = 1, \ldots, m\), where the \(\alpha^{ij}\) are uniquely determined functions in \(C^\infty(U)\). Then

$$
\langle [X,E_i], E_i \rangle = \sum_j \langle E_i, E_j \rangle \alpha^{ij} = \varepsilon_i \alpha^{ii},
$$

where \((\varepsilon_1, \ldots, \varepsilon_m)\) is the signature of \(M\). It follows from Theorem 20.4.2 that

$$
\operatorname{Div}(X)|_U = - \sum_i \alpha^{ii}.
$$

Then

$$
\begin{aligned}
f &= f\,\Omega_M|_U(E_1,\ldots,E_m) \qquad [\mathrm{Th}\ 19.10.1] \\
&= \mathcal{L}_X(\Omega_M|_U)(E_1,\ldots,E_m) \\
&= X(\Omega_M|_U(E_1,\ldots,E_m)) \\
&\quad - \sum_i \Omega_M|_U(E_1,\ldots,E_{i-1},[X,E_i],E_{i+1},\ldots,E_m) \qquad [\mathrm{Th}\ 16.4.4] \\
&= - \sum_i \Omega_M|_U(E_1,\ldots,E_{i-1},[X,E_i],E_{i+1},\ldots,E_m) \qquad [\mathrm{Th}\ 19.10.1] \\
&= - \sum_{ij} \alpha^{ij}\,\Omega_M|_U(E_1,\ldots,E_{i-1},E_j,E_{i+1},\ldots,E_m) \qquad [(20.4.3)] \\
&= - \sum_i \alpha^{ii}\,\Omega_M|_U(E_1,\ldots,E_{i-1},E_i,E_{i+1},\ldots,E_m) \qquad [\mathrm{cf.\ Th}\ 7.1.2] \\
&= - \sum_i \alpha^{ii}. \qquad [\mathrm{Th}\ 19.10.1]
\end{aligned}
$$

Thus, \(\operatorname{Div}(X)|_U = f\). Since \(p\) was arbitrary, the first equality follows. For the second equality, we have

$$
\begin{aligned}
\operatorname{Div}(X)\,\Omega_M
&= \mathcal{L}_X(\Omega_M) \qquad \text{[first equality]} \\
&= d \circ i_X(\Omega_M) + i_X \circ d(\Omega_M) \qquad [\mathrm{Th}\ 16.5.3] \\
&= d \circ i_X(\Omega_M). \qquad [\mathrm{cf.\ Th}\ 7.2.12(b)]
\end{aligned}
$$

(b): Since \(X^\flat\) is a covector field in \(\mathfrak{X}^*(M) = \Lambda^1(M)\), we have from (20.2.1) that \(\delta = (-1)^{\nu+1} \star d \star\), where \(\nu\) is the index of \(M\). Then

$$
\begin{aligned}
\operatorname{Div}(X)
&= \operatorname{Div}(X)\,\star(\Omega_M) \qquad [\mathrm{Th}\ 20.1.1(b)] \\
&= \star(\operatorname{Div}(X)\,\Omega_M) \\
&= \star\, d(i_X(\Omega_M)) \qquad \text{[part (a)]} \\
&= (-1)^\nu \star d \star (X^\flat) \qquad [\mathrm{Th}\ 20.1.2] \\
&= -\delta(X^\flat).
\end{aligned}
$$

### Theorem 20.4.4

设 \((M,\mathfrak{g},\mathfrak{O})\) 为定向半黎曼流形，\(f\) 为 \(C^\infty(M)\) 中的函数，\(X\) 为 \(\mathfrak{X}(M)\) 中的向量场。则

$$
\operatorname{Div}(fX) = f\operatorname{Div}(X) + \langle \operatorname{Grad}(f), X \rangle.
$$


证明：在定理16.5.1(e)中令ω = d(f)和ξ = Ω_M，得到

d(f) ∧ i_X(Ω_M) = i_X(d(f)) ∧ Ω_M − i_X(d(f) ∧ Ω_M) = X(f) Ω_M [(16.5.2), 定理7.2.12(b)] = ⟨Grad(f), X⟩ Ω_M, [定理20.3.1]

因此

Div(f X) Ω_M = d(i_{f X}(Ω_M)) [定理20.4.3(a)] = d(f i_X(Ω_M)) [定理16.5.1(b)] = d(f) ∧ i_X(Ω_M) + f d(i_X(Ω_M)) [(16.1.1)] = [⟨Grad(f), X⟩ + f Div(X)]Ω_M. [定理20.4.3(a)]

由于(Ω_M)是Λ^m(M)的一个基，结论成立。

前面的结果与定理10.6.1(b)明显相似。

## 定理20.4.5（半黎曼流形的散度定理）。设(M, g, 𝔒)是紧致带边界的定向半黎曼流形，Ω_M为其体积形式，X为𝔛(M)中的向量场。则 ∫_M Div(X) Ω_M = ∫_{∂M} i_X(Ω_M)。

证明：我们有 ∫_M Div(X) Ω_M = ∫_M d(i_X(Ω_M)) [定理20.4.3(a)] = ∫_{∂M} i_X(Ω_M)。 [定理17.4.1]

## 例20.4.6（ℝ_ν^m）。设(r^1, …, r^m)、(∂/∂r^1, …, ∂/∂r^m)和(ε_1, …, ε_m)分别是ℝ_ν^m的标准坐标、相应的标准坐标标架和符号差。由例15.1.6，(∂/∂r^1, …, ∂/∂r^m) = (e_1, …, e_m)，其中(e_1, …, e_m)是ℝ_ν^m的标准基。设X = (α^1, …, α^m) = Σ_i α^i e_i = Σ_i α^i ∂/∂r^i 是𝔛(ℝ_ν^m)中的向量场。由(18.2.2)、定理19.3.5和定理20.4.1可得Div(X) = Σ_i ∂α^i/∂r^i。

在ℝ_0^3中，使用标准坐标(x, y, z)，这变为Div(X) = ∂α^1/∂x + ∂α^2/∂y + ∂α^3/∂z = div(X)，

而在ℝ_1^4中，使用标准坐标(x, y, z, t)，它变为Div(X) = ∂α^1/∂x + ∂α^2/∂y + ∂α^3/∂z + ∂α^4/∂t。

## 20.5 旋度

设(M,g,𝒟)是指标为ν的定向半黎曼3-流形，Ω_M为其体积形式。旋度是映射

\[\operatorname{Curl}:\mathfrak{X}(M)\longrightarrow\mathfrak{X}(M)\]

定义为

\[\operatorname{Curl}(X)^\mathrm{F}=\star d(X^\mathrm{F}) \tag{20.5.1}\]

对所有𝔛(M)中的向量场X成立。由于X^F是𝔛^*(M)=Λ^1(M)中的余向量场，可知d(X^F)是Λ^2(M)中的形式，因此⋆ d(X^F)是Λ^1(M)中的向量场，所以该定义有意义。我们将Curl(X)称为X的旋度。

## 定理20.5.1. 在上述设定下，

\[i_{\operatorname{Curl}(X)}(\Omega_M)=d(X^\mathrm{F}).\]

证明：我们有

\[i_{\operatorname{Curl}(X)}(\Omega_M)=(-1)^\nu\star(\operatorname{Curl}(X)^\mathrm{F})\qquad [\mathrm{Th\ 20.1.2}]\]
\[\phantom{i_{\operatorname{Curl}(X)}(\Omega_M)}=(-1)^\nu\star^2d(X^\mathrm{F}).\qquad [(20.5.1)]\]

由于d(X^F)是Λ^2(M)中的形式，在定理20.1.1(c)中令m=3和s=2可得

\[\star^2(d(X^\mathrm{F}))=(-1)^\nu d(X^\mathrm{F}).\]

结论成立。

尽管定义(20.5.1)显得陌生，下面的例子表明在ℝ^3_0中Curl与经典旋度算子是一致的。

## 例20.5.2.

设(x,y,z)和(d(x),d(y),d(z))分别是ℝ^3_0上的标准坐标和相应的标准对偶坐标标架，并设X=(α^1,α^2,α^3)是𝔛(ℝ^3_0)中的向量场。由定理3.3.2的流形版本，

\[X^\mathrm{F}=\alpha^1d(x)+\alpha^2d(y)+\alpha^3d(z).\]

则(16.1.2)给出

\[
d(X^\mathrm{F})=\left(\frac{\partial\alpha^3}{\partial y}-\frac{\partial\alpha^2}{\partial z}\right)d(y)\wedge d(z)+\left(\frac{\partial\alpha^1}{\partial z}-\frac{\partial\alpha^3}{\partial x}\right)d(z)\wedge d(x)
\]
\[
\qquad+\left(\frac{\partial\alpha^2}{\partial x}-\frac{\partial\alpha^1}{\partial y}\right)d(x)\wedge d(y).
\]

因此

Curl(X)^F = *d(X^F)
= (∂α³/∂y - ∂α²/∂z) * (d(y) ∧ d(z)) + (∂α¹/∂z - ∂α³/∂x) * (d(z) ∧ d(x))
+ (∂α²/∂x - ∂α¹/∂y) * (d(x) ∧ d(y))
= (∂α³/∂y - ∂α²/∂z) d(x) + (∂α¹/∂z - ∂α³/∂x) d(y)
+ (∂α²/∂x - ∂α¹/∂y) d(z)
= (∂α³/∂y - ∂α²/∂z, ∂α¹/∂z - ∂α³/∂x, ∂α²/∂x - ∂α¹/∂y)^F
= curl(X)^F,

其中第三个等号由例8.5.4得出，第四个等号由定理3.3.2的流形版本得出，最后一个等号由表10.6.1得出。因此，Curl(X) = curl(X)。

## 20.6 Hesse算子

设(M, 𝔤)是半黎曼流形。Hesse算子是映射

Hess : C^∞(M) ⟶ T₂⁰(M)

定义为

Hess(f) = ∇²(f)  (20.6.1)

对所有C^∞(M)中的函数f成立，其中我们记∇ ∘ ∇为∇²。

我们将Hess(f)称为f的海塞（Hessian）。

## 定理20.6.1. 设(M, 𝔤)是半黎曼m-流形，f是C^∞(M)中的函数，X、Y是𝔛(M)中的向量场。则：

- (a)

Hess(f)(X, Y) = X(Y(f)) - ∇_X(Y)(f)
= ∇_X(∇_Y(f)) - ∇_{∇_X(Y)}(f)
= ⟨∇_X(Grad(f)), Y⟩。

- (b) Hess(f)是对称的。

证明：(a)：前两个等号由(18.4.3)得出。我们有

X(Y(f)) = X(⟨Grad(f), Y⟩)  [定理20.3.1]
= ⟨∇_X(Grad(f)), Y⟩ + ⟨Grad(f), ∇_X(Y)⟩  [(19.3.1)]
= ⟨∇_X(Grad(f)), Y⟩ + ∇_X(Y)(f)。  [定理20.3.1]

第三个等号现在由第一个等号得出。

(b)：由于Levi-Civita联络∇是对称的，

∇_X(Y)(f) − ∇_Y(X)(f) = [X,Y](f) = X(Y(f)) − Y(X(f)),

所以

Hess(f)(X, Y) = X(Y(f)) − ∇_X(Y)(f)  [部分(a)]
= Y(X(f)) − ∇_Y(X)(f)
= Hess(f)(Y, X)。  [部分(a)]

## 定理20.6.2（Hesse算子的局部坐标表达式）。如果(M, g)是半黎曼m-流形，且f是C^∞(M)中的函数，则在局部坐标下，Hess(f)的分量为

Hess(f)_{ij} = ∂²f/∂x^i∂x^j − Σ_k Γ^k_{ij} ∂f/∂x^k

对i, j = 1, …, m。

证明：我们有

Hess(f)_{ij} = Hess(f)(∂/∂x^i, ∂/∂x^j)
= ∂/∂x^i (∂f/∂x^j) − ∇_{∂/∂x^i}(∂/∂x^j)(f)  [定理20.6.1(a)]
= ∂²f/∂x^i∂x^j − Σ_k Γ^k_{ij} ∂f/∂x^k。  [(18.2.1)]

## 例20.6.3（ℝ^m_ν）。设(r¹, …, r^m)和(∂/∂r¹, …, ∂/∂r^m)分别是ℝ^m_ν上的标准坐标和相应的标准坐标标架。由定理19.3.5和定理20.6.2可得

Hess(f)_{ij} = ∂²f/∂r^i∂r^j

对i, j = 1, …, m。

## 20.7 拉普拉斯算子

设$(M,\mathfrak{g})$是半黎曼流形。拉普拉斯算子是映射

Lap : $C^{\infty}(M) \longrightarrow C^{\infty}(M)$

定义为

Lap$(f) = \text{Div} \circ \text{Grad}(f)$ (20.7.1)

对所有$C^{\infty}(M)$中的函数$f$成立。我们将Lap$(f)$称为$f$的拉普拉斯量（Laplacian）。

## 定理20.7.1. 如果$(M,\mathfrak{g})$是半黎曼流形，且$f$是$C^{\infty}(M)$中的函数，则

Lap$(f) = C_{12} \circ \text{Hess}(f)$。

证明：我们有

Lap$(f) = \text{Div} \circ \text{Grad}(f)$
$= C_{1}^{1} \circ \nabla \circ S_{1}^{1}(d(f))$ [流形版本的练习6.3.1, (20.3.1), (20.4.1)]
$= C_{1}^{1} \circ S_{1}^{1} \circ \nabla(\nabla(f))$ [定理18.4.3, 定理19.6.3]
$= C_{1}^{1} \circ S_{1}^{1} \circ \nabla^{2}(f)$
$= C_{12} \circ \text{Hess}(f)$。 [(19.6.3), (20.6.1)]

## 定理20.7.2（拉普拉斯算子的标准正交标架表达式）。设$(M,\mathfrak{g})$是带有符号差$(\varepsilon_{1},...,\varepsilon_{m})$的半黎曼流形，$f$是$C^{\infty}(M)$中的函数，$(E_{1},...,E_{m})$是$M$中开集$U$上的标准正交标架。则

Lap$(f) = \sum_{i} \varepsilon_{i} \text{Hess}(f)(E_{i},E_{i})$

在$U$上成立。

证明：这可由定理19.6.2(b)和定理20.7.1得出。

## 例20.7.3（$\mathbb{R}_{\nu}^{m}$）。设$(r^{1},...,r^{m})$、$(\partial/\partial r^{1},...,\partial/\partial r^{m})$和$(\varepsilon_{1},...,\varepsilon_{m})$分别是$\mathbb{R}_{\nu}^{m}$的标准坐标、相应的标准坐标标架和符号差。由例20.6.3，我们有

Hess$(f)\left(\frac{\partial}{\partial r^{i}},\frac{\partial}{\partial r^{j}}\right) = \text{Hess}(f)_{ij} = \frac{\partial^{2} f}{\partial r^{i}\partial r^{j}}$,

并且由例15.1.6，$(\partial/\partial r^{1},...,\partial/\partial r^{m}) = (e_{1},...,e_{m})$，其中$(e_{1},...,e_{m})$是$\mathbb{R}_{\nu}^{m}$的标准基。因此，$(\partial/\partial r^{1},...,\partial/\partial r^{m})$是$\mathbb{R}_{\nu}^{m}$上的一个标准正交标架。那么定理20.7.2给出

Lap$(f) = \sum_{i} \varepsilon_{i} \frac{\partial^{2} f}{\partial (r^{i})^{2}}$。




在 ℝ₀³ 中，使用标准坐标 (x, y, z)，它变为

Lap(f) = ∂²f/∂x² + ∂²f/∂y² + ∂²f/∂z² = lap(f),

而在 ℝ₁⁴ 中，使用标准坐标 (x, y, z, t)，它为

Lap(f) = ∂²f/∂x² + ∂²f/∂y² + ∂²f/∂z² − ∂²f/∂t².

在物理文献中，Lap 在 ℝ₁⁴ 的上下文中通常用 □ 表示（有时用 □²）并称为 d’Alembert 算子。

## 20.8 Laplace–de Rham 算子

设 (M, 𝔤, 𝔒) 为定向半黎曼流形。Laplace–de Rham 算子是线性映射族

Δ : Λˢ(M) ⟶ Λˢ(M)

定义为 1 ≤ s ≤ m 时

Δ = dδ + δd;  (20.8.1)

即

Δ(ω) = dδ(ω) + δd(ω)

对所有 Λˢ(M) 中的形式 ω 成立。由定理 16.1.1(c) 回顾 d² = 0，且由定理 20.2.1(a) 得 δ² = 0。因此，

(d + δ) ∘ (d + δ) = d² + dδ + δd + δ² = Δ,

可表示为

Δ = (d + δ)².

下一个结果表明，除符号外，Laplace–de Rham 算子是 Laplace 算子的推广。

## 定理 20.8.1. 若 (M, 𝔤, 𝔒) 为定向半黎曼流形且 f 为 C^∞(M) 中的函数，则

Lap(f) = −Δ(f).

证明. 我们有

Lap(f) = Div(Grad(f))  [(20.7.1)]
= −δ(Grad(f)^♭)  [Th 20.4.3(b)]
= −δd(f)  [(20.3.2)]
= −(dδ + δd)(f)  [(20.2.2)]
= −Δ(f).  [(20.8.1)]

### 20.9 对称 2-协变张量场的散度

设 (M,g) 为半黎曼流形，且 Σ(M) 为 𝒯⁰₂(M) 的子空间，由对称 (2-协变) 张量场组成。例如，g 在 Σ(M) 中。**对称 2-协变张量场的散度** 是映射

$$\operatorname{Div}:\Sigma(M)\longrightarrow \mathfrak{X}^*(M)$$

定义为

$$\operatorname{Div}(\mathcal{A})=C_{13}\circ\nabla(\mathcal{A})$$

对所有 Σ(M) 中的张量场 𝒜 成立。由于 ∇(𝒜) 在 𝒯⁰₃(M) 中，C₁₃(∇(𝒜)) 在 𝒯⁰₁(M)=𝔛*(M) 中，故定义有意义。顺便指出，由定理 19.3.1，∇(g)=0，故 Div(g)=0.

**定理 20.9.1 (散度的正交标架表达式).** 设 (M,g) 为签名为 (ε₁,…,εₘ) 的半黎曼流形，(E₁,…,Eₘ) 为 M 中开集 U 上的正交标架，𝒜 为 𝒯⁰₂(M) 中的对称张量场，X 为 𝔛(M) 中的向量场。则

$$\operatorname{Div}(\mathcal{A})(X)=\sum_i\epsilon_i\nabla_{E_i}(\mathcal{A})(X,E_i)=\sum_i\epsilon_i\nabla_{E_i}(\mathcal{A})(E_i,X)$$

在 U 上成立。

*证明.* 我们有

$$\begin{aligned}
\operatorname{Div}(\mathcal{A})(X)&=C_{13}(\nabla(\mathcal{A}))(X)\\
&=\sum_i\epsilon_i\nabla(\mathcal{A})(E_i,X,E_i)\qquad [\text{Th 19.6.2(a)}]\\
&=\sum_i\epsilon_i\nabla_{E_i}(\mathcal{A})(X,E_i),\qquad [(18.4.1)]
\end{aligned}$$

这证明了第一个等式。容易利用 (18.1.3) 证明由于 𝒜 对称，∇_{E_i}(𝒜) 也是对称的。第二个等式现在由第一个等式得出。□

**定理 20.9.2.** 若 (M,g) 为半黎曼流形且 f 为 C^∞(M) 中的函数，则

$$\operatorname{Div}(fg)=d(f).$$

*证明.* 设 (ε₁,…,εₘ) 为 M 的签名，p 为 M 中的点。由定理 19.3.7，存在 p 的邻域 U 上的正交标架 (E₁,…,Eₘ)。设 X 为 𝔛(M) 中的向量场，且 X=∑ᵢαⁱEᵢ 在 U 上，其中 αⁱ 为 C^∞(U) 中唯一确定的函数。由流形版本的定理 4.2.7，

$$X=\sum_i\epsilon_i\langle X,E_i\rangle E_i.\tag{20.9.1}$$

由定理 19.3.8，

$$\nabla_{E_i}(fg)=\nabla_{E_i}(g\otimes f)=g\otimes\nabla_{E_i}(f)=\nabla_{E_i}(f)g,$$

故

$$\nabla_{E_i}(fg)(X,E_i)=\nabla_{E_i}(f)g(X,E_i)=\langle X,E_i\rangle\nabla_{E_i}(f). \tag{20.9.2}$$

则

$$d(f)(X)=X(f) \qquad\text{[Th 15.4.5(b)]}$$

$$\qquad=\sum_i\varepsilon_i\langle X,E_i\rangle\nabla_{E_i}(f) \qquad\text{[(18.1.1), (20.9.1)]}$$

$$\qquad=\sum_i\varepsilon_i\nabla_{E_i}(fg)(X,E_i) \qquad\text{[(20.9.2)]}$$

$$\qquad=\operatorname{Div}(fg)(X). \qquad\text{[Th 20.9.1]}$$

由于 X 和 p 任意，结果得证。

其中第二个等式由定理 14.7.1(b) 推出，第三个等式由定理 10.1.10 推出，最后一个等式由定理 14.9.2 推出。则

$$\left\langle \frac{d\lambda}{ds}(s),\frac{d\lambda}{ds}(s)\right\rangle = \left(\frac{dr}{ds}(s)\right)^2\left\langle \frac{\partial\sigma}{\partial r}(r(s),s),\frac{\partial\sigma}{\partial r}(r(s),s)\right\rangle + 2\frac{dr}{ds}(s)\left\langle \frac{\partial\sigma}{\partial r}(r(s),s),\frac{\partial\sigma}{\partial s}(r(s),s)\right\rangle + \left\langle \frac{\partial\sigma}{\partial s}(r(s),s),\frac{\partial\sigma}{\partial s}(r(s),s)\right\rangle. \tag{21.1.4}$$

我们寻求 (21.1.4) 中第一行和第二行的内积的替代表达式。对于第一行，定理 19.7.4 的部分 (a) 和 (21.1.3) 给出

$$\left\langle \frac{\partial\sigma}{\partial r}(r(s),s),\frac{\partial\sigma}{\partial r}(r(s),s)\right\rangle = 1. \tag{21.1.5}$$

对于第二行，我们由定理 18.11.4 的部分 (c) 和 (d) 得到

$$\frac{\partial\sigma}{\partial r}(r(s),s)=d_{r(s)}\psi(s)(\exp_p)(\psi(s))$$

且

$$\frac{\partial\sigma}{\partial s}(r(s),s)=r(s)d_{r(s)}\psi(s)(\exp_p)\left(\frac{d\psi}{ds}(s)\right),$$

故

$$\begin{aligned} \left\langle \frac{\partial\sigma}{\partial r}(r(s),s),\frac{\partial\sigma}{\partial s}(r(s),s)\right\rangle &= \left\langle d_{r(s)}\psi(s)(\exp_p)(r(s)\psi(s)),d_{r(s)}\psi(s)(\exp_p)\left(\frac{d\psi}{ds}(s)\right)\right\rangle \\ &= r(s)\left\langle \psi(s),\frac{d\psi}{ds}(s)\right\rangle. \end{aligned} \tag{21.1.6}$$

其中第一个等式由 $d_{r(s)}\psi(s)(\exp_p)$ 的线性性推出，第二个等式由定理 19.7.5 推出。我们由 (21.1.3) 得到

$$2\left\langle \frac{d\psi}{ds}(s),\psi(s)\right\rangle = \frac{d}{dt}\left(\langle\psi(s),\psi(s)\rangle\right)=0. \tag{21.1.7}$$

则 (21.1.6) 和 (21.1.7) 给出

$$\left\langle \frac{\partial\sigma}{\partial r}(r(s),s),\frac{\partial\sigma}{\partial s}(r(s),s)\right\rangle = 0. \tag{21.1.8}$$

将 (21.1.5) 和 (21.1.8) 代入 (21.1.4) 得

$$\left\langle \frac{d\lambda}{ds}(s),\frac{d\lambda}{ds}(s)\right\rangle = \left(\frac{dr}{ds}(s)\right)^2 + \left\langle \frac{\partial\sigma}{\partial s}(r(s),s),\frac{\partial\sigma}{\partial s}(r(s),s)\right\rangle,$$

这由于 M 为黎曼流形，等价于

$$\left\|\frac{d\lambda}{ds}(s)\right\|^2=\left\|\frac{dr}{ds}(s)\right\|^2+\left\|\frac{\partial\sigma}{\partial s}(r(s),s)\right\|^2.$$

于是，

$$\left\|\frac{d\lambda}{ds}(s)\right\|\geq\left\|\frac{dr}{ds}(s)\right\| \tag{21.1.9}$$

对所有 s∈(0,1] 成立。对实数 0<ε<1，

$$\begin{aligned}
L(\lambda)&\geq\int_{\varepsilon}^{1}\left\|\frac{d\lambda}{ds}(s)\right\|\,ds &&[19.2.1]\\
&\geq\int_{\varepsilon}^{1}\left\|\frac{dr}{ds}(s)\right\|\,ds &&[21.1.9]\\
&\geq\left|\int_{\varepsilon}^{1}\frac{dr}{ds}(s)\,ds\right|\\
&=|r(1)-r(\varepsilon)|, &&[\mathrm{FTC}]
\end{aligned} \tag{21.1.10}$$

其中 FTC 表示微积分基本定理。由 (21.1.2) 和 (21.1.10) 可得

$$L(\lambda)\geq\lim_{\varepsilon\to0^+}|r(1)-r(\varepsilon)|=|r(1)|=L(\rho_{p,v}).$$

对定理 21.1.1 的一个观察是，在几何上，(21.1.8) 表明纬度曲线 σ_r 与经度曲线 σ_s 沿径向测地线 σ_s 正交相交，这在某些情况下，如球面， intuitively 清楚。

## 21.2 经典向量分析定理

第 10.6 节给出了微分向量分析的若干经典定理。在本书的各处，我们遇到了它们的现代对应物，如下表所概括：

| 经典 | 现代 |
|---|---|
| 定理 10.6.1(a) | 定理 20.3.3 |
| 定理 10.6.1(b) | 定理 20.4.4 |
| 定理 10.6.1(c) | 例 16.1.7 |
| 定理 10.6.1(d) | 例 16.1.8 |
| 定理 10.6.2 | 定理 16.9.2, 16.9.3, 16.9.8 |





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
