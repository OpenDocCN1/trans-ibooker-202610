

对于 \(s \ge 2\) 且 \(l=s\)：

\[
S_s^k(A)(\omega^1,\ldots,\omega^{r+1},X_1,\ldots,X_{s-1})
= A(\omega^1,\ldots,\widehat{\omega^k},\ldots,\omega^{r+1},X_1,\ldots,X_{s-1},\omega^k S).
\]

**定理 19.4.3.** 在上述设定下，\(\Gamma_l^k\) 与 \(S_l^k\) 是互逆的 \(C^\infty(M)\)-模同构。

*证明.* 容易证明 \(\Gamma_l^k\) 和 \(S_l^k\) 是 \(C^\infty(M)\)-模同态。它们互为 \(C^\infty(M)\)-模同构并且互逆，分别由定理 6.3.7 和定理 19.4.2 的流形版本得出。

## 19.5 张量场的表示

光滑流形上张量场的表示已在第 15.7 节中讨论过。现在我们利用半黎曼流形所提供的额外结构回到该主题。我们引入第 5.3 节和第 6.4 节中分别介绍的表示映射和标量积映射的光滑流形对应物，并通过第 15.5 节和第 15.7 节中定义的刻画映射将它们联系起来。诚然，符号相当繁琐，但其思想本质上是在简单得多的标量积空间背景下引入的那些思想。

设 \((M,g)\) 为半黎曼流形。遵循第 B.5 节，我们记

\[
\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))
\]

为从 \(\mathfrak{X}(M)^s\) 到 \(\mathfrak{X}(M)\) 的 \(C^\infty(M)\)-多重线性映射构成的 \(C^\infty(M)\)-模，并记

\[
\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}^*(M)^r\times\mathfrak{X}(M)^s,C^\infty(M))
\]

为从 \(\mathfrak{X}^*(M)^r\times\mathfrak{X}(M)^s\) 到 \(C^\infty(M)\) 的 \(C^\infty(M)\)-多重线性映射构成的 \(C^\infty(M)\)-模。

我们定义一个映射

\[
\mathcal{S}_s:\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\longrightarrow \operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^{s+1},C^\infty(M)),
\]

称为**标量积映射**，定义为

\[
\mathcal{S}_s(\Psi)(X_1,\ldots,X_{s+1})=\langle X_1,\Psi(X_2,\ldots,X_{s+1})\rangle
\]

对所有 \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\) 中的映射 \(\Psi\) 以及 \(\mathfrak{X}(M)\) 中所有向量场 \(X_1,\ldots,X_{s+1}\) 成立。我们注意到 \(\mathcal{S}_s\) 是由 (6.4.1) 定义的映射的光滑流形对应物。

**定理 19.5.1.** 若 \((M,g)\) 是半黎曼流形，则 \(\mathcal{S}_s\) 是 \(C^\infty(M)\)-模同构：

\[
\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\cong \operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^{s+1},C^\infty(M)).
\]

**证明.** 容易证明 \(\mathcal{S}_s\) 是 \(C^\infty(M)\)-模同态。我们需要证明 \(\mathcal{S}_s\) 是双射。

**单射性.** 假设 \(\Psi\) 是 \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\) 中的映射且 \(\mathcal{S}_s(\Psi)=0\)。则对 \(\mathfrak{X}(M)\) 中所有向量场 \(X_1,\ldots,X_{s+1}\) 以及 \(M\) 中所有 \(p\)，有 \(\langle X_1|_p,\Psi(X_2,\ldots,X_{s+1})|_p\rangle=0\)。由于 \(g\) 非退化，对 \(\mathfrak{X}(M)\) 中所有向量场 \(X_2,\ldots,X_{s+1}\) 以及 \(M\) 中所有 \(p\)，有 \(\Psi(X_2,\ldots,X_{s+1})|_p=0\)。因此，对 \(\mathfrak{X}(M)\) 中所有向量场 \(X_2,\ldots,X_{s+1}\)，有 \(\Psi(X_2,\ldots,X_{s+1})=0\)。因此 \(\Psi=0\)，所以 \(\ker(\mathcal{S}_s)=\{0\}\)。由定理 B.5.3，\(\mathcal{S}_s\) 是单射。

**满射性.** 设 \(\Upsilon\) 是 \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^{s+1},C^\infty(M))\) 中的映射。我们需要找到 \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\) 中的映射 \(\widetilde{\Upsilon}\) 使得 \(\mathcal{S}_s(\widetilde{\Upsilon})=\Upsilon\)。根据定理 19.4.2，定义 \(\mathfrak{X}^*(M)\) 中的余向量场等价于定义 \(\mathfrak{X}(M)\) 中的向量场。对于给定的 \(\mathfrak{X}(M)\) 中向量场 \(X_2,\ldots,X_{s+1}\)，定义 \(\mathfrak{X}^*(M)\) 中的余向量场 \(\widetilde{\Upsilon}(X_2,\ldots,X_{s+1})^\flat\) 为

\[
\widetilde{\Upsilon}(X_2,\ldots,X_{s+1})^\flat(X_1)=\Upsilon(X_1,\ldots,X_{s+1})
\]

对所有 \(\mathfrak{X}(M)\) 中的向量场 \(X_1\) 成立。则

\[
\mathcal{S}_s(\widetilde{\Upsilon})(X_1,\ldots,X_{s+1})
=\langle X_1,\widetilde{\Upsilon}(X_2,\ldots,X_{s+1})\rangle
=\langle\widetilde{\Upsilon}(X_2,\ldots,X_{s+1}),X_1\rangle
=\widetilde{\Upsilon}(X_2,\ldots,X_{s+1})^\flat(X_1)
=\Upsilon(X_1,\ldots,X_{s+1}).
\]

由于 \(X_1,\ldots,X_{s+1}\) 是任意的，\(\mathcal{S}_s(\widetilde{\Upsilon})=\Upsilon\)。因此，\(\mathcal{S}_s\) 是满射。□

现在我们来定义一个映射

\[
\mathfrak{R}_s:\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\longrightarrow\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}^*(M)\times\mathfrak{X}(M)^s,C^\infty(M)),
\]

称为**表示映射**，定义为

\[
\mathfrak{R}_s(\Psi)(\omega,X_1,\ldots,X_s)=\omega(\Psi(X_1,\ldots,X_s)) \tag{19.5.1}
\]

对所有 \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\) 中的映射 \(\Psi\)，所有 \(\mathfrak{X}^*(M)\) 中的余向量场 \(\omega\)，以及所有 \(\mathfrak{X}(M)\) 中的向量场 \(X_1,\ldots,X_s\) 成立。我们注意到 \(\mathfrak{R}_s\) 是由 (5.3.1) 定义的映射的光滑流形对应物。当 \(s=1\) 时，我们记

\[
\mathfrak{R}_1\quad\text{为}\quad\mathfrak{R}
\]

并得到

\[
\mathfrak{R}:\operatorname{Lin}_{C^\infty(M)}(\mathfrak{X}(M),\mathfrak{X}(M))\longrightarrow\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}^*(M)\times\mathfrak{X}(M),C^\infty(M)).
\]

则

\[
\mathfrak{R}(\Psi)(\omega,X)=\omega(\Psi(X)) \tag{19.5.2}
\]

对所有 \(\operatorname{Lin}_{C^\infty(M)}(\mathfrak{X}(M),\mathfrak{X}(M))\) 中的映射 \(\Psi\)，所有 \(\mathfrak{X}^*(M)\) 中的余向量场 \(\omega\)，以及所有 \(\mathfrak{X}(M)\) 中的向量场 \(X\) 成立。

**定理 19.5.2.** 若 \((M,g)\) 是半黎曼流形，则 \(\mathfrak{R}_s\) 是 \(C^\infty(M)\)-模同构：

\[
\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\cong \operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}^*(M)\times\mathfrak{X}(M)^s,C^\infty(M)).
\]

**证明.** 回顾第 15.7 节中定义的刻画映射 \(\mathcal{C}_s^r\)。让我们定义“尖锐”映射

\[
\mathfrak{P}_1^1:\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^{s+1},C^\infty(M))\longrightarrow \operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}^*(M)\times\mathfrak{X}(M)^s,C^\infty(M))
\]

为

\[
\mathfrak{P}_1^1=\mathcal{C}_s^1\circ S_1^1\circ(\mathcal{C}_{s+1}^0)^{-1}. \tag{19.5.3}
\]

由定理 15.7.2 和定理 19.4.3，我们知道 \(\mathcal{C}_s^1\)、\(\mathcal{C}_{s+1}^0\) 和 \(S_1^1\) 是 \(C^\infty(M)\)-模同构，因此 \(\mathfrak{P}_1^1\) 也是。

我们断言

\[
\mathfrak{R}_s=\mathfrak{P}_1^1\circ\mathcal{C}_s, \tag{19.5.4}
\]

这是定理 6.4.1(d) 的流形版本。一旦证明了这一点，结果可由定理 19.5.1 得出。设 \(\Upsilon\) 是 \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^{s+1},C^\infty(M))\) 中的映射。利用定理 15.7.2 中固有的等同，可以证明

\[
\mathfrak{P}_1^1(\Upsilon)(\omega,X_1,\ldots,X_s)=\Upsilon(\omega^\sharp,X_1,\ldots,X_s)
\]

对所有 \(\mathfrak{X}^*(M)\) 中的余向量场 \(\omega\) 和所有 \(\mathfrak{X}(M)\) 中的向量场 \(X_1,\ldots,X_s\) 成立。对于 \(\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\) 中的映射 \(\Psi\)，我们有

\[
\begin{aligned}
\mathfrak{R}_s(\Psi)(\omega,X_1,\ldots,X_s)
&=\omega(\Psi(X_1,\ldots,X_s))
=(\omega^\sharp)^\flat(\Psi(X_1,\ldots,X_s))\\
&=\langle\omega^\sharp,\Psi(X_1,\ldots,X_s)\rangle
=\mathcal{S}_s(\Psi)(\omega^\sharp,X_1,\ldots,X_s)\\
&=\mathfrak{P}_1^1\circ\mathcal{S}_s(\Psi)(\omega,X_1,\ldots,X_s).
\end{aligned}
\]

由于 \(\Psi,\omega\) 和 \(X_1,\ldots,X_s\) 是任意的，这证明了该断言。◻

**定理 19.5.3.** 在上述设定下，图 19.5.1 是 \(C^\infty(M)\)-模同构的交换图。也就是说，任意两个 \(C^\infty(M)\)-模都可通过取中间各 \(C^\infty(M)\)-模同构的复合而构成的 \(C^\infty(M)\)-模同构相连。

**证明.** 由 (19.5.3) 和 (19.5.4) 可得

\[
\mathfrak{R}_s=\mathcal{C}_s^1\circ S_1^1\circ(\mathcal{C}_{s+1}^0)^{-1}\circ\mathcal{C}_s.
\]
◻

**定理 19.5.4（协变张量场的表示）。** 若 \((M,g)\) 是半黎曼流形，则

\[
\mathcal{T}^0_{s+1}(M)\cong\operatorname{Mult}_{C^\infty(M)}(\mathfrak{X}(M)^s,\mathfrak{X}(M))\cong\mathcal{T}^1_s(M).
\]

**证明.** 这是定理 6.4.2 的流形版本，可由定理 19.5.3 得出。◻

