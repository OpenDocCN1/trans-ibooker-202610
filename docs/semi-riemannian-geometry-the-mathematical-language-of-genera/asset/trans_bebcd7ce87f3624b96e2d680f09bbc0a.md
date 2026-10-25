
圆柱和圆锥。原因在于，圆柱和圆锥可以通过对平面（的一部分）进行光滑变形得到，这种变形涉及弯曲但不涉及拉伸。这使得变形后的平面的“内在”几何保持不变，从而保持每一点的高斯曲率。球面具有恒定的正高斯曲率，而曳物面（形状类似喇叭）具有恒定的负高斯曲率。单叶双曲面（双叶双曲面）的高斯曲率为负（正）但不恒定。环面上存在一个高斯曲率为正的区域和一个为负的区域，其间有一个高斯曲率为0的过渡带。

| 节 | ℝ³₀ 中的几何对象 | 高斯曲率 |
|---|---|---|
| 13.1 | 平面 | 0 |
| 13.2 | 圆柱 | 0 |
| 13.2 | 圆锥 | 0 |
| 13.4 | 球面 | 1/R² |
| 13.5 | 曳物面 | −1 |
| 13.6 | 单叶双曲面 | −1/(2x² + 2y² − 1)² |
| 13.7 | 双叶双曲面 | 1/(2x² + 2y² + 1)² |
| 13.8 | 环面 | cos(φ)/[cos(φ) + R] |

在 ℝ³₁ 中，伪球面具有恒定的正高斯曲率，而双曲空间具有恒定的负高斯曲率。有趣的是，单叶双曲面和伪球面是由同一基础曲面定义的。它们高斯曲率的差异完全源于一个位于内积空间 ℝ³₀ 中，另一个位于洛伦兹向量空间 ℝ³₁ 中。类似的说明也适用于双叶双曲面和双曲空间。

| 节 | ℝ³₁ 中的几何对象 | 高斯曲率 |
|---|---|---|
| 13.9 | 伪球面 | 1 |
| 13.10 | 双曲空间 | −1 |

## 12.9 ℝ³_ν 中的黎曼曲率张量

对于 \(\mathbb{R}^3_\nu\) 中的正则曲面 \((M,g)\)，黎曼曲率张量是如下映射

\[
R: \mathfrak{X}(M)^3 \to \mathfrak{X}(M)
\]

定义为

\[
R(X,Y)Z = \nabla_X(\nabla_Y(Z)) - \nabla_Y(\nabla_X(Z)) - \nabla_{[X,Y]}(Z) \tag{12.9.1}
\]

对所有 \(\mathfrak{X}(M)\) 中的向量场 \(X,Y,Z\) 成立；也就是说，

\[
(R(X,Y)Z)_p = (\nabla_X(\nabla_Y(Z)))_p - (\nabla_Y(\nabla_X(Z)))_p - (\nabla_{[X,Y]}(Z))_p
\]

对所有 \(M\) 中的 \(p\) 成立。加上大括号是为了明确前面恒等式中的四项都是在点 \(p\) 处取值的 \(\mathfrak{X}(M)\) 中的向量场，因此是 \(T_p(M)\) 中的向量。由于 \((R(X,Y)Z)_p\) 不是实数，用“张量”一词来描述 \(R\) 有点用词不当。这一矛盾在定理 19.5.5 中得到解决。表达式 \(R_p(X_p,Y_p)Z_p\) 目前没有意义——至少现在还没有。

我们在示例 12.4.4 中给出了一个二阶协变导数 \(\nabla^2_{X,Y}(Z)\) 和 \(\nabla^2_{Y,X}(Z)\) 不相等的例子。下一个结果表明，这两个向量场之差正是 \(R(X,Y)Z\)。

### 定理 12.9.1。

若 \((M,\mathfrak{g})\) 是 \(\mathbb{R}^3_\nu\) 中的正则曲面，则

\[
R(X,Y)Z = \nabla^2_{X,Y}(Z) - \nabla^2_{Y,X}(Z)
\]

对所有 \(\mathfrak{X}(M)\) 中的向量场 \(X,Y,Z\) 成立。

证明。由定理 12.4.1(a) 和 (12.6.1) 可得

\[
\nabla_{[X,Y]}(Z) = \nabla_{\nabla_X(Y)-\nabla_Y(X)}(Z) = \nabla_{\nabla_X(Y)}(Z) - \nabla_{\nabla_Y(X)}(Z).
\]

于是

\[
\begin{aligned}
R(X,Y)Z
&= \nabla_X(\nabla_Y(Z)) - \nabla_Y(\nabla_X(Z)) - \nabla_{[X,Y]}(Z)\\
&= [\nabla_X(\nabla_Y(Z)) - \nabla_{\nabla_X(Y)}(Z)] - [\nabla_Y(\nabla_X(Z)) - \nabla_{\nabla_Y(X)}(Z)]\\
&= \nabla^2_{X,Y}(Z) - \nabla^2_{Y,X}(Z),
\end{aligned}
\]

其中最后一个等号由 (12.4.4) 得到。

为了计算的目的，给出 \(R\) 的局部坐标表达式是有帮助的。

### 定理 12.9.2。

设 \((M,\mathfrak{g})\) 是 \(\mathbb{R}^3_\nu\) 中的正则曲面，设 \(X,Y,Z\) 是 \(\mathfrak{X}(M)\) 中的向量场，并在局部坐标下令

\[
X\circ\varphi = \sum_i \alpha^i H_i, \qquad Y\circ\varphi = \sum_j \beta^j H_j, \qquad Z\circ\varphi = \sum_k \gamma^k H_k.
\]

则

\[
R(X,Y)Z\circ\varphi = \sum_l \left(\sum_{ijk} \alpha^i\beta^j\gamma^k R^l_{ijk}\right) H_l, \tag{12.9.2}
\]

其中

\[
R^l_{ijk} = \frac{\partial\Gamma^l_{jk}}{\partial r^i} - \frac{\partial\Gamma^l_{ik}}{\partial r^j} + \sum_n \left(\Gamma^l_{in}\Gamma^n_{jk} - \Gamma^l_{jn}\Gamma^n_{ik}\right) \tag{12.9.3}
\]

对 \(i,j,k,l=1,2\) 成立。

证明。由 (12.4.20) 可得

\[
\begin{aligned}
(\nabla^2_{X,Y}(Z)\circ\varphi)^l
&= \sum_{ij}^{(3)}\alpha^i\beta^j \frac{\partial^2\gamma^l}{\partial r^i\partial r^j}
+ \sum_{ijk}^{(4)}\alpha^i\beta^j \frac{\partial\gamma^k}{\partial r^i}\Gamma^l_{jk}
+ \sum_{ijk}^{(5)}\alpha^i\beta^j\gamma^k \frac{\partial\Gamma^l_{jk}}{\partial r^i}\\
&\quad + \sum_{ijk}^{(6)}\alpha^i\beta^j \frac{\partial\gamma^k}{\partial r^j}\Gamma^l_{ik}
+ \sum_{ijkn}^{(7)}\alpha^i\beta^j\gamma^k \Gamma^n_{jk}\Gamma^l_{in}
- \sum_{ijk}^{(8)}\alpha^i\beta^j \frac{\partial\gamma^k}{\partial r^k}\Gamma^l_{ij}
- \sum_{ijkn}^{(9)}\alpha^i\beta^j\gamma^k \Gamma^n_{ij}\Gamma^l_{kn}.
\end{aligned}
\]

交换 \(X\) 和 \(Y\) 的角色可得

\[
\begin{aligned}
(\nabla^2_{Y,X}(Z)\circ\varphi)^l
&= \sum_{ij}\alpha^j\beta^i \frac{\partial^2\gamma^l}{\partial r^i\partial r^j}
+ \sum_{ijk}\alpha^j\beta^i \frac{\partial\gamma^k}{\partial r^i}\Gamma^l_{jk}
+ \sum_{ijk}\alpha^j\beta^i\gamma^k \frac{\partial\Gamma^l_{jk}}{\partial r^i}\\
&\quad + \sum_{ijk}\alpha^j\beta^i \frac{\partial\gamma^k}{\partial r^j}\Gamma^l_{ik}
+ \sum_{ijkn}\alpha^j\beta^i\gamma^k \Gamma^n_{jk}\Gamma^l_{in}
- \sum_{ijk}\alpha^j\beta^i \frac{\partial\gamma^k}{\partial r^k}\Gamma^l_{ij}
- \sum_{ijkn}\alpha^j\beta^i\gamma^k \Gamma^n_{ij}\Gamma^l_{kn}\\
&= \sum_{ij}^{(3)}\alpha^i\beta^j \frac{\partial^2\gamma^l}{\partial r^i\partial r^j}
+ \sum_{ijk}^{(6)}\alpha^i\beta^j \frac{\partial\gamma^k}{\partial r^j}\Gamma^l_{ik}
+ \sum_{ijk}^{(10)}\alpha^i\beta^j\gamma^k \frac{\partial\Gamma^l_{ik}}{\partial r^j}
+ \sum_{ijk}^{(4)}\alpha^i\beta^j \frac{\partial\gamma^k}{\partial r^i}\Gamma^l_{jk}\\
&\quad + \sum_{ijkn}^{(11)}\alpha^i\beta^j\gamma^k \Gamma^n_{ik}\Gamma^l_{jn}
- \sum_{ijk}^{(8)}\alpha^i\beta^j \frac{\partial\gamma^k}{\partial r^k}\Gamma^l_{ij}
- \sum_{ijkn}^{(9)}\alpha^i\beta^j\gamma^k \Gamma^n_{ij}\Gamma^l_{kn},
\end{aligned}
\]

其中继续使用了定理 12.4.3 的证明中开始使用的求和编号。由定理 12.9.1 可得，\(R(X,Y)Z\) 的第 \(l\) 个分量为

\[
\begin{aligned}
((\nabla^2_{X,Y}(Z) - \nabla^2_{Y,X}(Z))\circ\varphi)^l
&= \sum_{ijk}^{(5)}\alpha^i\beta^j\gamma^k \frac{\partial\Gamma^l_{jk}}{\partial r^i}
+ \sum_{ijkn}^{(7)}\alpha^i\beta^j\gamma^k \Gamma^n_{jk}\Gamma^l_{in}\\
&\quad - \sum_{ijk}^{(10)}\alpha^i\beta^j\gamma^k \frac{\partial\Gamma^l_{ik}}{\partial r^j}
- \sum_{ijkn}^{(11)}\alpha^i\beta^j\gamma^k \Gamma^n_{ik}\Gamma^l_{jn}\\
&= \sum_{ijk}\alpha^i\beta^j\gamma^k \left(\frac{\partial\Gamma^l_{jk}}{\partial r^i} - \frac{\partial\Gamma^l_{ik}}{\partial r^j} + \sum_n (\Gamma^l_{in}\Gamma^n_{jk} - \Gamma^l_{jn}\Gamma^n_{ik})\right)\\
&= \sum_{ijk}\alpha^i\beta^j\gamma^k R^l_{ijk}.
\end{aligned}
\]

我们在第 12.3 节观察到克里斯托费尔符号是内蕴的。由 (12.9.2) 和 (12.9.3) 可知，黎曼曲率张量也是如此。

### 示例 12.9.3 (S²)。

将第 13.4 节给出的 \(S^2\) 的克里斯托费尔符号值代入 (12.9.3) 可得

\[
R^1_{122} = \sin^2(\theta), \quad R^1_{212} = -\sin^2(\theta), \quad R^2_{121} = -1, \quad R^2_{211} = 1,
\]

其余 \(R^l_{ijk}\) 等于 \(0\)。注意，\(R^l_{ijk}\) 与 \(\varphi\) 无关，这是球面对称性所预期的。

(12.9.2) 的一个显著特点是表达式中不出现分量函数的偏导数。这一关键观察是接下来两个结果的基础。

### 定理 12.9.4。

若 \((M,\mathfrak{g})\) 是 \(\mathbb{R}^3_\nu\) 中的正则曲面，则 \(R\) 在 \(M\) 上逐点确定，其含义如下：对 \(M\) 中所有点 \(p\)，若 \(X,Y,Z\) 和 \(\tilde{X},\tilde{Y},\tilde{Z}\) 是 \(\mathfrak{X}(M)\) 中的向量场，且满足

\[
(X_p,Y_p,Z_p) = (\tilde{X}_p,\tilde{Y}_p,\tilde{Z}_p),
\]

则

\[
(R(X,Y)Z)_p = (R(\tilde{X},\tilde{Y})\tilde{Z})_p.
\]

证明。在局部坐标下，令

\[
X\circ\varphi = \sum_i \alpha^i H_i \quad \text{and} \quad \tilde{X}\circ\varphi = \sum_i \tilde{\alpha}^i H_i.
\]

则 \(X_p = \tilde{X}_p\) 转化为对 \(i=1,2\) 有 \(\alpha^i(p) = \tilde{\alpha}^i(p)\)，另外两个向量场也有相应的恒等式。于是由 (12.9.2) 可得结论。

### 定理 12.9.5。

若 \((M,\mathfrak{g})\) 是 \(\mathbb{R}^3_\nu\) 中的正则曲面，则 \(R\) 是 \(C^\infty(M)\)-多重线性的，其含义如下：对所有 \(\mathfrak{X}(M)\) 中的向量场 \(X,Y,Z,W\) 和所有 \(C^\infty(M)\) 中的函数 \(f\)，

\[
R(fX+W,Y)Z = fR(X,Y)Z + R(W,Y)Z,
\]

对于 \(R\) 的另外两个参数也有相应的恒等式。

证明。这可由与定理 12.9.4 的证明类似的推理得到。

### 定理 12.9.6。

设 \((M,\mathfrak{g},\mathfrak{O})\) 是 \(\mathbb{R}^3_\nu\) 中的定向正则曲面，设 \(\mathcal{N}\) 为高斯映射，设 \(X,Y\) 是 \(\mathfrak{X}(M)\) 中的向量场。则：

(a)

\[
\langle D_X(Y), \mathcal{N}\rangle = \langle \mathcal{W}(X), Y\rangle.
\]

(b)

\[
D_X(Y) = \nabla_X(Y) + \epsilon_M \langle \mathcal{W}(X), Y\rangle \mathcal{N}. \tag{12.9.4}
\]

证明。 (a)：将 \(D_X\) 作用于 \(\langle Y,\mathcal{N}\rangle = 0\) 两边，并利用定理 12.3.1(e)，可得 \(\langle D_X(Y),\mathcal{N}\rangle = \langle -D_X(\mathcal{N}),Y\rangle\)。现在由定理 12.8.1 即得结论。

(b)：设 \(p\) 是 \(M\) 中一点，并回顾第 12.4 节给出的 \(\tan_p\) 和 \(\operatorname{nor}_p\) 的定义。由定理 4.1.4，

\[
D_X(Y)_p = \tan_p(D_X(Y)_p) + \operatorname{nor}_p(D_X(Y)_p),
\]

且由 (12.4.2)，

\[
\tan_p(D_X(Y)_p) = \nabla_X(Y)_p.
\]

由定理 12.7.3(b) 可得 \(T_p(M)^\perp = \mathbb{R}\mathcal{N}_p\)，再由定理 4.1.5 和 (12.2.3) 可得

\[
\operatorname{nor}_p(D_X(Y)_p) = \frac{\langle \mathcal{N}_p, D_X(Y)_p\rangle}{\langle \mathcal{N}_p,\mathcal{N}_p\rangle}\mathcal{N}_p
= \epsilon_M \langle D_X(Y)_p,\mathcal{N}_p\rangle\mathcal{N}_p.
\]

因此，

\[
D_X(Y)_p = \nabla_X(Y)_p + \epsilon_M \langle D_X(Y)_p,\mathcal{N}_p\rangle\mathcal{N}_p.
\]

由于 \(p\) 是任意的，

\[
D_X(Y) = \nabla_X(Y) + \epsilon_M \langle D_X(Y),\mathcal{N}\rangle\mathcal{N}.
\]

现在由 (a) 部即得结论。

### 定理 12.9.7。

若 \((M,\mathfrak{g},\mathfrak{O})\) 是 \(\mathbb{R}^3_\nu\) 中的定向正则曲面，且 \(X,Y,Z\) 是 \(\mathfrak{X}(M)\) 中的向量场，则：

(a)

\[
R(X,Y)Z = \epsilon_M[\mathfrak{h}(Y,Z)\mathcal{W}(X) - \mathfrak{h}(X,Z)\mathcal{W}(Y)].
\]

(b)

\[
\nabla_X(\mathcal{W}(Y)) - \nabla_Y(\mathcal{W}(X)) = \mathcal{W}([X,Y]).
\]

证明。由定理 12.9.6(b)，

\[
D_X(D_Y(Z)) = D_X(\nabla_Y(Z) + \epsilon_M \langle \mathcal{W}(Y),Z\rangle\mathcal{N}) \tag{12.9.5}
\]

\[
= D_X(\nabla_Y(Z)) + \epsilon_M D_X(\langle \mathcal{W}(Y),Z\rangle\mathcal{N})
\]

且

\[
D_X(\nabla_Y(Z)) = \nabla_X(\nabla_Y(Z)) + \epsilon_M \langle \mathcal{W}(X),\nabla_Y(Z)\rangle\mathcal{N}, \tag{12.9.6}
\]

并由定理 12.3.1 的 (d) 和 (e) 部以及定理 12.8.1，

\[
D_X(\langle \mathcal{W}(Y),Z\rangle\mathcal{N})
= D_X(\langle \mathcal{W}(Y),Z\rangle)\mathcal{N} + \langle \mathcal{W}(Y),Z\rangle D_X(\mathcal{N}) \tag{12.9.7}
\]

\[
= [\langle D_X(\mathcal{W}(Y)),Z\rangle + \langle \mathcal{W}(Y),D_X(Z)\rangle]\mathcal{N} - \langle \mathcal{W}(Y),Z\rangle\mathcal{W}(X).
\]

