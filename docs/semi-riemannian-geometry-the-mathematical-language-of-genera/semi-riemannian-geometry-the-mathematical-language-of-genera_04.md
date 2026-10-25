## 第 2 章
## 矩阵与行列式

在本章中，我们回顾矩阵和行列式理论中的一些基本结果。

### 2.1 矩阵

设 $\mathrm{Mat}_{m\times n}$ 表示具有实数元素的 $m \times n$ 矩阵的集合（即 $m$ 行 $n$ 列）。当 $m = n$ 时，我们称矩阵为方阵。容易证明，在通常的矩阵加法和标量乘法下，$\mathrm{Mat}_{m\times n}$ 是一个向量空间，并且在通常的矩阵乘法下，$\mathrm{Mat}_{m\times m}$ 是一个环。

设 $P$ 是 $\mathrm{Mat}_{m\times n}$ 中的一个矩阵，其中

```
P = [p_j^i] = [ p_1^1  ...  p_n^1 ; ... ; p_1^m  ...  p_n^m ]
```

$P$ 的转置是 $\mathrm{Mat}_{n\times m}$ 中的矩阵 $P^T$ 定义为

```
P^T = [p_i^j] = [ p_1^1  ...  p_1^m ; ... ; p_n^1  ...  p_n^m ]
```

$P$ 的行矩阵为

```
[p_1^1 ... p_n^1]   [p_1^2 ... p_n^2]   ...   [p_1^m ... p_n^m]
```

而 $P$ 的列矩阵为

[p1^1; :; p1^m]   [p2^1; :; p2^m]   ...   [p_n^1; :; p_n^m].

### 例 2.1.1. 对于

P = [1 2 3; 4 5 6; 7 8 9],

其转置为

P^T = [1 4 7; 2 5 8; 3 6 9]

而列矩阵为

[1;4;7]   [2;5;8]   [3;6;9].

### 定理 2.1.2. 若 $P$ 和 $Q$ 分别是 $\mathrm{Mat}_l\times m$ 和 $\mathrm{Mat}_m\times n$ 中的矩阵，则：

- (a) $(P^T)^T = P$.
- (b) $(PQ)^T = Q^T P^T$.

证明。直接可证。

我们称 $\mathrm{Mat}_m\times m$ 中的矩阵 $Q = [q^i_j]$ 为对称的，如果 $Q = Q^T$，并且为对角的，如果对所有 $i \neq j$ 有 $q^i_j = 0$。显然，对角矩阵是对称的。给定 $\mathbb{R}^m$ 中的向量 $(a^1, …, a^m)$，相应的对角矩阵定义为

diag(a^1, …, a^m) = [a^1 … 0; : ⋱ :; 0 … a^m],

其中不在（左上到右下）对角线上的所有元素都等于 0。例如，

diag(1) = [1]      diag(1,2) = [1 0; 0 2]      diag(1,2,3) = [1 0 0; 0 2 0; 0 0 3].

$\mathrm{Mat}_m\times n$ 中的零矩阵，用 $O_m\times n$ 表示，是所有元素都等于 0 的矩阵。$\mathrm{Mat}_m\times m$ 中的单位矩阵定义为

I_m = diag(1, …, 1),

因此，例如，

```
I₁ = [1]      I₂ = [1 0; 0 1]      I₃ = [1 0 0; 0 1 0; 0 0 1].
```

我们称 $\mathrm{Mat}_m\times m$ 中的矩阵 $Q$ 为可逆的，如果存在 $\mathrm{Mat}_m\times m$ 中的矩阵，记为 $Q^{-1}$ 并称为 $Q$ 的逆，使得

```
QQ⁻¹ = Q⁻¹Q = Iₘ.
```

容易证明，如果矩阵的逆存在，则它是唯一的。

### 定理 2.1.3. 若 $P$ 和 $Q$ 是 $\mathrm{Mat}_m\times m$ 中的可逆矩阵，则：

- (a) $(P^{-1})^{-1} = P$.
- (b) $(PQ)^{-1} = Q^{-1}P^{-1}$.
- (c) $(P^{-1})ᵀ = (Pᵀ)^{-1}$.

证明。 (a), (b)：直接可证。

(c)：由定理 2.1.2(b)，

```
Iₘ = (Iₘ)ᵀ = (PP⁻¹)ᵀ = (P⁻¹)ᵀPᵀ,
```

由此可得结果。

多指标记号，引入于附录 A，提供了一种指定矩阵子矩阵的便捷方法。设 $1 \le r \le m$ 且 $1 \le s \le n$ 为整数，并设 $I = (i₁,…,iᵣ)$ 和 $J = (j₁,…,jₛ)$ 分别为 $\mathcal{I}_{r,m}$ 和 $\mathcal{I}_{s,n}$ 中的多指标。对于 $\mathrm{Mat}_{m\times n}$ 中的矩阵 $P = [pⱼⁱ]$，我们用

```
P₍ⱼ₁,…,ⱼₛ₎⁽ⁱ¹,…,ⁱʳ⁾,    P_J^I,    [pⱼⁱ]₍ⱼ₁,…,ⱼₛ₎⁽ⁱ¹,…,ⁱʳ⁾,    or    [pⱼⁱ]_J^I
```

表示 $P$ 的 $r \times s$ 子矩阵，由行 $i₁,i₂,…,iᵣ$ 和列 $j₁,j₂,…,jₛ$ 的重叠构成（按该顺序）；即

```
P₍ⱼ₁,…,ⱼₛ₎⁽ⁱ¹,…,ⁱʳ⁾ = [pⱼ₁ⁱ¹ … pⱼₛⁱ¹; ⋮ ⋱ ⋮; pⱼ₁ⁱʳ … pⱼₛⁱʳ].
```

当 $r = m$，此时 $(i₁,…,iₘ) = (1,…,m)$，我们记

```
P₍ⱼ₁,…,ⱼₛ₎⁽¹,…,ᵐ⁾   为   P₍ⱼ₁,…,ⱼₛ₎,
```

并且当 $s = n$，此时 $(j₁,…,jₙ) = (1,…,n)$，我们记

```
P₍₁,…,ₙ₎⁽ⁱ¹,…,ⁱʳ⁾   为   P⁽ⁱ¹,…,ⁱʳ⁾.
```

当 $r = 1$，即 $I = (i)$ 对某个 $1 \le i \le m$ 时，我们有

```
P⁽ⁱ⁾ = [p₁ⁱ … pₙⁱ],
```

对于 $\mathrm{Mat}_{m\times m}$ 中的矩阵 $P = [p^i_j]$，$P$ 的迹定义为

$\operatorname{tr}(P) = \sum_i p^i_i.$

### 定理 2.1.7. 若 $P$ 和 $Q$ 是 $\mathrm{Mat}_{m\times m}$ 中的矩阵，则：

- (a) $\operatorname{tr}(P+Q)=\operatorname{tr}(P)+\operatorname{tr}(Q)$.
- (b) $\operatorname{tr}(PQ)=\operatorname{tr}(QP)$.
- (c) 若 $Q$ 可逆，则 $\operatorname{tr}(Q^{-1}PQ)=\operatorname{tr}(P)$。

证明。 (a)：直接可证。

(b)：设 $P = [p^i_j]$ 且 $Q = [q^i_j]$。由定理 2.1.5(a)，

$\operatorname{tr}(PQ)=\sum_i P^{(i)}Q_{(i)}=\sum_i\left(\sum_j p^i_j q^j_i\right)=\sum_j\left(\sum_i q^i_j p^j_i\right)$
$=\sum_j Q^{(j)}P_{(j)}=\operatorname{tr}(QP)$。

(c)：这由部分 (b) 得出。

### 2.2 矩阵表示

矩阵具有许多理想的计算性质。因此，在向量空间中计算时，通常将论证重新表述为矩阵形式是很方便的。我们经常使用这种方法。

设 $V$ 为向量空间，设 $\mathcal{H}=(h_1,\dots,h_m)$ 为 $V$ 的基，设 $v$ 为 $V$ 中的向量，其中

$v = \sum_i a^i h_i.$

$v$ 关于 $\mathcal{H}$ 的矩阵表示记为 $[v]_{\mathcal{H}}$ 并定义为

$[v]_{\mathcal{H}} = \begin{bmatrix} a^1 \\ \vdots \\ a^m \end{bmatrix}.$

我们称 $a^1,\dots,a^m$ 为 $v$ 关于 $\mathcal{H}$ 的分量。特别地，

$[h_i]_{\mathcal{H}} = \begin{bmatrix} 0 \\ \vdots \\ 1 \\ \vdots \\ 0 \end{bmatrix}, \qquad (2.2.1)$

其中 1 在第 $i$ 个位置，其余为 0，对 $i=1,\dots,m$。

### 定理 2.2.1（向量的表示）

若 $V$ 为维数为 $m$ 的向量空间且 $\mathcal{H}$ 为 $V$ 的基，则映射

$$\mathcal{L}_{\mathcal{H}}: V \longrightarrow \operatorname{Mat}_{m\times 1}$$

定义为

$$\mathcal{L}_{\mathcal{H}}(v)=[v]_{\mathcal{H}}$$

对所有 $V$ 中的向量 $v$ 为线性同构：

$$V\cong\operatorname{Mat}_{m\times 1}.$$

*证明。* 直接可证。

设 $V$ 和 $\mathcal{H}$ 如上，设 $W$ 为另一个向量空间，并设 $\mathcal{F}=(f_1,\ldots,f_n)$ 为 $W$ 的基。设 $A:V\longrightarrow W$ 为线性映射，其中

$$A(h_j)=\sum_i a_j^i f_i,$$

从而

$$[A(h_j)]_{\mathcal{F}}=\begin{bmatrix}a_j^1\\\vdots\\a_j^n\end{bmatrix}$$

对 $j=1,\ldots,m$。$A$ 关于 $\mathcal{H}$ 和 $\mathcal{F}$ 的矩阵表示记为 $[A]_{\mathcal{H}}^{\mathcal{F}}$ 并定义为 $n\times m$ 矩阵

$$[A]_{\mathcal{H}}^{\mathcal{F}}=\begin{bmatrix}a_1^1&\cdots&a_m^1\\\vdots&\ddots&\vdots\\a_1^n&\cdots&a_m^n\end{bmatrix}=\left[\,[A(h_1)]_{\mathcal{F}}\ \cdots\ [A(h_m)]_{\mathcal{F}}\,\right].$$

例如，考虑线性映射 $A:\mathbb{R}^2\longrightarrow\mathbb{R}^3$ 给出为 $A(x,y)=(y,2x,3x+4y)$，并设 $\mathcal{E}$ 和 $\mathcal{F}$ 分别为 $\mathbb{R}^2$ 和 $\mathbb{R}^3$ 的标准基。则

$$[A]_{\mathcal{E}}^{\mathcal{F}}=\begin{bmatrix}0&1\\2&0\\3&4\end{bmatrix}.$$

### 定理 2.2.2.

设 $U$、$V$ 和 $W$ 为向量空间，设 $\mathcal{H}$、$\mathcal{F}$ 和 $\mathcal{G}$ 为各自的基，设 $A,B:U\longrightarrow V$ 和 $C:V\longrightarrow W$ 为线性映射，设 $c$ 为实数。则：

- (a) $[A+B]_{\mathcal{H}}^{\mathcal{F}}=[A]_{\mathcal{H}}^{\mathcal{F}}+[B]_{\mathcal{H}}^{\mathcal{F}}$.
- (b) $[cA]_{\mathcal{H}}^{\mathcal{F}}=c[A]_{\mathcal{H}}^{\mathcal{F}}$.
- (c) $[C\circ A]_{\mathcal{H}}^{\mathcal{G}}=[C]_{\mathcal{F}}^{\mathcal{G}}[A]_{\mathcal{H}}^{\mathcal{F}}$.





**证明。** (a)、(b)：直接可得。

(c)：设 \(\mathcal{H}=(h_1,\ldots,h_m)\)，\(\mathcal{F}=(f_1,\ldots,f_n)\)，以及 \(\mathcal{G}=(g_1,\ldots,g_r)\)，并设 \([A]_{\mathcal{H}}^{\mathcal{F}}=[a_j^i]\) 和 \([C]_{\mathcal{F}}^{\mathcal{G}}=[c_l^k]\)。根据 (2.2.2) 和 (2.2.3)，

\[
A(h_j)=\sum_i a_j^i f_i \qquad\text{and}\qquad C(f_l)=\sum_k c_l^k g_k.
\]

于是

\[
\begin{aligned}
C(A(h_j))
&=C\left(\sum_k a_j^k f_k\right)
=\sum_k a_j^k C(f_k)
=\sum_k a_j^k\left(\sum_i c_k^i g_i\right)\\
&=\sum_i\left(\sum_k c_k^i a_j^k\right)g_i
=\sum_i\left([C]_{\mathcal{F}}^{\mathcal{G}}\right)^{(i)}_{(\cdot)}
\left([A]_{\mathcal{H}}^{\mathcal{F}}\right)_{(\cdot)j}g_i,
\end{aligned}
\]

由此可得结果。

### 定理 2.2.3（线性映射的表示）

设 \(V\) 和 \(W\) 分别是维数为 \(m\) 和 \(n\) 的向量空间，并设 \(\mathcal{H}\) 和 \(\mathcal{F}\) 分别为其基。定义映射

\[
\mathcal{L}_{\mathcal{H}}^{\mathcal{F}}:\operatorname{Lin}(V,W)\longrightarrow \operatorname{Mat}_{n\times m}
\]

为

\[
\mathcal{L}_{\mathcal{H}}^{\mathcal{F}}(A)=[A]_{\mathcal{H}}^{\mathcal{F}}
\]

其中 \(A\) 是 \(\operatorname{Lin}(V,W)\) 中的所有映射，\(\operatorname{Lin}(V,W)\) 在例 1.1.1 中定义。

则：

- (a) \(\mathcal{L}_{\mathcal{H}}^{\mathcal{F}}\) 关于 \(\operatorname{Lin}(V,W)\) 的加法结构是一个线性同构：

  \[
  \operatorname{Lin}(V,W)\cong\operatorname{Mat}_{n\times m}.
  \]

- (b) 若 \(V=W\)，则 \(\mathcal{L}_{\mathcal{H}}^{\mathcal{F}}\) 关于 \(\operatorname{Lin}(V,V)\) 的乘法结构是一个环同构。

**注。** 我们在例 1.1.1 中说明了 \(\operatorname{Lin}(V,V)\) 既是向量空间又是环，并且在 2.1 节开头提到 \(\operatorname{Mat}_{m\times m}\) 也是如此，因此 (b) 部分的断语是有意义的。

**证明。** (a)：由定理 2.2.2 的 (a) 和 (b) 部分，\(\mathcal{L}_{\mathcal{H}}^{\mathcal{F}}\) 是一个线性映射，且容易证明 \(\mathcal{L}_{\mathcal{H}}^{\mathcal{F}}\) 是单射。我们断言 \(\operatorname{Mat}_{n\times m}\) 和 \(\operatorname{Lin}(V,W)\) 的维数均为 \(mn\)。设 \(E_{ij}\) 是 \(\operatorname{Mat}_{n\times m}\) 中的矩阵，其第 \(ij\) 位为 1，其余位置为 0，其中 \(i=1,\ldots,n\)，\(j=1,\ldots,m\)。容易验证这些 \(E_{ij}\) 构成 \(\operatorname{Mat}_{n\times m}\) 的一组基，因此 \(\operatorname{Mat}_{n\times m}\) 的维数为 \(mn\)。设 \(\mathcal{H}=(h_1,\ldots,h_m)\) 和 \(\mathcal{F}=(f_1,\ldots,f_n)\)，利用定理 1.1.10 定义 \(\operatorname{Lin}(V,W)\) 中的线性映射 \(L_{ij}\) 如下：

\[
L_{ij}(h_k)=
\begin{cases}
f_i & \text{if } k=j,\\
0 & \text{if } k\ne j.
\end{cases}
\]

其中 \(i=1,\ldots,n\)，\(j=1,\ldots,m\)。于是

\[
\left(\sum_{ij}a^i_jL_{ij}\right)(h_k)=\sum_{ij}a^i_jL_{ij}(h_k)=\sum_i a^i_kf_i
\]

对所有实数 \(a^i_j\) 成立。设 \(B\) 是 \(\operatorname{Lin}(V,W)\) 中的一个映射，且 \([B]_{\mathcal H}^{\mathcal F}=[b^i_j]\)。由 (2.2.4) 可得

\[
\left(\sum_{ij}b^i_jL_{ij}\right)(h_k)=\sum_i b^i_kf_i=B(h_k)
\]

对 \(k=1,\ldots,m\) 成立。根据定理 1.1.10，\(\sum_{ij}b^i_jL_{ij}=B\)，因此 \(L_{ij}\) 张成 \(\operatorname{Lin}(V,W)\)。设存在实数 \(a^i_j\) 使得 \(\sum_{ij}a^i_jL_{ij}=0\)，其中 \(0\) 表示 \(\operatorname{Lin}(V,W)\) 中的零映射。由 (2.2.4) 可知 \(\sum_i a^i_kf_i=0\) 对 \(k=1,\ldots,m\) 成立。由于 \(\mathcal F\) 是 \(W\) 的基，所以对 \(i=1,\ldots,n\) 和 \(k=1,\ldots,m\) 有 \(a^i_k=0\)，因此 \(L_{ij}\) 线性无关。因此，\(L_{ij}\) 构成 \(\operatorname{Lin}(V,W)\) 的一组基，从而 \(\operatorname{Lin}(V,W)\) 的维数为 \(mn\)。这完成了断言的证明。设存在 \(\operatorname{Lin}(V,W)\) 中的映射 \(A\) 使得 \(\mathcal L^{\mathcal F}_{\mathcal H}(A)=O_{n\times m}\)。由定理 1.1.10、(2.2.2) 和 (2.2.3) 可知 \(A\) 是零映射，因此 \(\ker(\mathcal L^{\mathcal F}_{\mathcal H})=\{0\}\)。于是结论由定理 1.1.12 和定理 1.1.14 得出。

(b)：这可由定理 2.2.2(c) 和 (a) 部分得出。

### 定理 2.2.4

设 \(V\) 和 \(W\) 是向量空间，\(\mathcal H\) 和 \(\mathcal F\) 分别为其基，设 \(A:V\to W\) 是线性映射，并设 \(v\) 是 \(V\) 中的一个向量。则

\[
[A(v)]_{\mathcal F}=[A]_{\mathcal H}^{\mathcal F}[v]_{\mathcal H}.
\]

**证明。** 设 \(\mathcal H=(h_1,\ldots,h_m)\) 且 \(v=\sum_i a^i h_i\)。由 (2.2.1) 和 (2.2.2) 可得

\[
[A(h_i)]_{\mathcal F}=[A]_{\mathcal H}^{\mathcal F}[h_i]_{\mathcal H}.
\]

根据定理 2.2.2 的 (a) 和 (b) 部分，

\[
\begin{aligned}
[A(v)]_{\mathcal F}
&=\left[A\left(\sum_i a^i h_i\right)\right]_{\mathcal F}
=\left[\sum_i a^i A(h_i)\right]_{\mathcal F}
=\sum_i a^i [A(h_i)]_{\mathcal F}\\
&=\sum_i a^i\left([A]_{\mathcal H}^{\mathcal F}[h_i]_{\mathcal H}\right)
=[A]_{\mathcal H}^{\mathcal F}\left(\sum_i a^i [h_i]_{\mathcal H}\right)
=[A]_{\mathcal H}^{\mathcal F}\begin{bmatrix}a^1\\ \vdots\\ a^m\end{bmatrix}
=[A]_{\mathcal H}^{\mathcal F}[v]_{\mathcal H}.
\end{aligned}
\]

设 \(V\) 是一个向量空间，并设 \(\mathcal H\) 和 \(\mathcal F\) 是 \(V\) 的基。在定理 2.2.4 中取 \(A=\operatorname{id}_V\) 可得

\[
[v]_{\mathcal F}=[\operatorname{id}_V]_{\mathcal H}^{\mathcal F}[v]_{\mathcal H}.
\]

这表明 \([\operatorname{id}_V]_{\mathcal H}^{\mathcal F}\) 是将关于 \(\mathcal H\) 的分量变换为关于 \(\mathcal F\) 的分量的矩阵。因此，\([\operatorname{id}_V]_{\mathcal H}^{\mathcal F}\) 被称为从 \(\mathcal H\) 到 \(\mathcal F\) 的基变换矩阵。设 \([\operatorname{id}_V]_{\mathcal H}^{\mathcal F}=[a^i_j]\)。那么 (2.2.2) 和 (2.2.3) 变为

\[
h_j=\sum_i a^i_j f_i
\]

对 \(j=1,\ldots,m\) 成立，以及

\[
[\operatorname{id}_V]_{\mathcal H}^{\mathcal F}
=\left[\,[h_1]_{\mathcal F}\quad\cdots\quad [h_m]_{\mathcal F}\,\right].
\]

### 定理 2.2.5

设 \(V\) 和 \(W\) 是向量空间，\(\mathcal H\) 和 \(\mathcal F\) 分别为其基，并设 \(A:V\to W\) 是一个线性同构。则：

- (a) \([A]_{\mathcal H}^{\mathcal F}=[\operatorname{id}_W]_{A(\mathcal H)}^{\mathcal F}\)
- (b) \([A^{-1}]_{\mathcal F}^{\mathcal H}=\left([A]_{\mathcal H}^{\mathcal F}\right)^{-1}.\)

**注。** 根据定理 1.1.13(a)，\(A(\mathcal H)\) 是 \(W\) 的基，因此 (a) 部分的断语是有意义的。

**证明。** (a)：设 \(\mathcal H=(h_1,\ldots,h_m)\)，则

\[
[A]_{\mathcal H}^{\mathcal F}=\left[\,[A(h_1)]_{\mathcal F}\quad\cdots\quad[A(h_m)]_{\mathcal F}\,\right]
= [\operatorname{id}_W]_{A(\mathcal H)}^{\mathcal F}.
\]

(b)：由定理 2.2.2(c)，

\[
I_m=[\operatorname{id}_V]_{\mathcal H}^{\mathcal H}=[A^{-1}\circ A]_{\mathcal H}^{\mathcal H}
=[A^{-1}]_{\mathcal F}^{\mathcal H}[A]_{\mathcal H}^{\mathcal F},
\]

由此可得结论。\(\square\)

### 定理 2.2.6（基变换）

设 \(V\) 是一个向量空间，\(\mathcal H\) 和 \(\mathcal F\) 是 \(V\) 的基，并设 \(A:V\to V\) 是一个线性映射。则

\[
[A]_{\mathcal F}^{\mathcal F}=\left([\operatorname{id}_V]_{\mathcal F}^{\mathcal H}\right)^{-1}[A]_{\mathcal H}^{\mathcal H}[\operatorname{id}_V]_{\mathcal F}^{\mathcal H}.
\]

**证明。** 由定理 2.2.2(c)，

\[
[A]_{\mathcal F}^{\mathcal F}=[\operatorname{id}_V\circ A\circ\operatorname{id}_V]_{\mathcal F}^{\mathcal F}
=[\operatorname{id}_V]_{\mathcal H}^{\mathcal F}[A]_{\mathcal H}^{\mathcal H}[\operatorname{id}_V]_{\mathcal F}^{\mathcal H}.
\]

结论现在由定理 2.2.5(b) 得出。\(\square\)

### 定理 2.2.7

设 \(V\) 是一个向量空间，\(\mathcal H\) 和 \(\widetilde{\mathcal H}\) 是 \(V\) 的基，设 \(\Theta\) 和 \(\widetilde{\Theta}\) 是对应的对偶基，并设 \(A:V\to V\) 是一个线性映射。则

\[
[A^*]_{\widetilde{\Theta}}^{\Theta}=\left([A]_{\mathcal H}^{\widetilde{\mathcal H}}\right)^T,
\]

其中 \(A^*\) 是 \(A\) 的拉回（pullback）。

**证明。** 设 \(\mathcal H=(h_1,\ldots,h_m)\) 和 \(\widetilde{\mathcal H}=(\tilde h_1,\ldots,\tilde h_m)\)，设 \(\Theta=(\theta^1,\ldots,\theta^m)\) 和 \(\widetilde{\Theta}=(\tilde\theta^1,\ldots,\tilde\theta^m)\)，并设 \([A]_{\mathcal H}^{\widetilde{\mathcal H}}=[a^i_j]\)。由 (2.2.2) 和 (2.2.3)，\(A(h_j)=\sum_i a^i_j \tilde h_i\)。于是

\[
\begin{aligned}
A^*(\tilde\theta^j)
&=\sum_i A^*(\tilde\theta^j)(h_i)\,\theta^i\\
&=\sum_i \tilde\theta^j(A(h_i))\,\theta^i\\
&=\sum_i \tilde\theta^j\left(\sum_k a^k_i \tilde h_k\right)\theta^i
=\sum_{i,k} a^k_i \tilde\theta^j(\tilde h_k)\,\theta^i\\
&=\sum_i a^j_i\,\theta^i.
\end{aligned}
\]

再次由 (2.2.2) 和 (2.2.3)，

\[
[A^*]_{\widetilde{\Theta}}^{\Theta}=[a^j_i]=\left([a^i_j]\right)^T=\left([A]_{\mathcal H}^{\widetilde{\mathcal H}}\right)^T.
\]

## 2.3 矩阵的秩

考虑由行矩阵构成的 \(n\) 维向量空间 \(\operatorname{Mat}_{1\times n}\) 和由列矩阵构成的 \(m\) 维向量空间 \(\operatorname{Mat}_{m\times 1}\)。设 \(P\) 是 \(\operatorname{Mat}_{m\times n}\) 中的一个矩阵。矩阵 \(P\) 的行秩定义为由 \(P\) 的各行张成的 \(\operatorname{Mat}_{1\times n}\) 的子空间的维数：

\[
\operatorname{rowrank}(P)=\dim(\operatorname{span}\{P^{(1)},\ldots,P^{(m)}\}).
\]

类似地，矩阵 \(P\) 的列秩定义为由 \(P\) 的各列张成的 \(\operatorname{Mat}_{m\times 1}\) 的子空间的维数：

\[
\operatorname{colrank}(P)=\dim(\operatorname{span}\{P_{(1)},\ldots,P_{(n)}\}).
\]

例如，对于

\[
P=\begin{bmatrix}1&0&0\\0&1&0\end{bmatrix},
\]

有 \(\operatorname{rowrank}(P)=\operatorname{colrank}(P)=2\)。如下所示，\(P\) 的行秩和列秩相等并非巧合。

### 定理 2.3.1

设 \(V\) 和 \(W\) 是向量空间，\(\mathcal H\) 和 \(\mathcal F\) 分别为其基，并设 \(A:V\to W\) 是一个线性映射。则

\[
\operatorname{rank}(A)=\operatorname{colrank}([A]_{\mathcal H}^{\mathcal F}).
\]

**证明。** 设 \(\mathcal H=(h_1,\ldots,h_m)\)。由定理 2.2.1 和 (2.2.3) 可得

\[
([A]_{\mathcal H}^{\mathcal F})_{(j)}=[A(h_j)]_{\mathcal F}=\mathcal{L}_{\mathcal F}(A(h_j)).
\]



for $j = 1, \ldots, m$. Since $\mathfrak{L}_{\mathcal{F}}$ is an isomorphism and $A(h_1), \ldots, A(h_m)$ span the image of $A$, we have

colrank$\left([A]_{\mathcal{H}}^{\mathcal{F}}\right) = \dim\left(\operatorname{span}\left(\{A(h_1), \ldots, A(h_m)\}\right)\right) = \dim(\operatorname{im}(A))$ $= \operatorname{rank}(A)$.

## Theorem 2.3.2. If $P$ is a matrix in $\operatorname{Mat}_{m\times n}$, then

rowrank$(P) = $ colrank$(P)$.

Proof. Let $\mathcal{E}$ and $\mathcal{F}$ be the standard bases for $\mathbb{R}^n$ and $\mathbb{R}^m$, respectively, and let $\Xi$ and $\Phi$ be the corresponding dual bases. Let $P = [p_j^i]$, and let $A : \mathbb{R}^n \longrightarrow \mathbb{R}^m$ be the linear map defined by

$A(x^1, \ldots, x^n) = \left(\sum_j p_j^1 x^j, \ldots, \sum_j p_j^m x^j\right)$.

Then $[A]_{\mathcal{E}}^{\mathcal{F}} = P$. By Theorem 2.3.1,

rank$(A) = $ colrank$(P)$.

We have from Theorem 2.2.7 that $[A^*]_{\Phi}^{\Xi} = ([A]_{\mathcal{E}}^{\mathcal{F}})^\mathrm{T} = P^\mathrm{T}$, so a similar argument gives

rank$(A^*) = $ colrank$(P^\mathrm{T}) = $ rowrank$(P)$.

The result now follows from Theorem 1.4.3(a).

In light of Theorem 2.3.2, the common value of the row rank and column rank of $P$ is denoted by rank$(P)$ and called the rank of $P$. Thus,

rank$(P) = $ rowrank$(P) = $ colrank$(P)$. (2.3.1)

## 2.4 Determinant of Matrices

This section presents the basic results on the determinant of matrices.

Consider the $m$-dimensional vector space $\operatorname{Mat}_{m\times 1}$ of column matrices and the corresponding product vector space $(\operatorname{Mat}_{m\times 1})^m$. We denote by $(E_1, \ldots, E_m)$ the standard basis for $\operatorname{Mat}_{m\times 1}$, where $E_j$ has 1 in the $j$th row and 0s elsewhere for $j = 1, \ldots, m$. Let

$\Delta : (\operatorname{Mat}_{m\times 1})^m \longrightarrow \mathbb{R}$

be an arbitrary function, and let $\sigma$ be a permutation in $\mathcal{S}_m$, the symmetric group on $\{1,2,\ldots,m\}$. We define a function

$\sigma(\Delta) : (\operatorname{Mat}_{m\times 1})^m \longrightarrow \mathbb{R}$

by

$\sigma(\Delta)(P_1, \ldots, P_m) = \Delta(P_{\sigma(1)}, \ldots, P_{\sigma(m)})$

for all matrices $P_1, \ldots, P_m$ in $\operatorname{Mat}_{m\times 1}$.

## Theorem 2.4.1. If Δ : (Mat_{m×1})^m ⟶ ℝ is a function, and σ and ρ are permutations in S_m, then

(σρ)(Δ) = σ(ρ(Δ)).

Proof. Setting P_{σ(1)} = Q_1, …, P_{σ(m)} = Q_m, we have

(σρ)(Δ)(P_1,…,P_m) = Δ(P_{σρ(1)},…,P_{σρ(m)}) = Δ(P_{σ(ρ(1))},…,P_{σ(ρ(m))}) = Δ(Q_{ρ(1)},…,Q_{ρ(m)}) = ρ(Δ)(Q_1,…,Q_m) = ρ(Δ)(P_{σ(1)},…,P_{σ(m)}) = σ(ρ(Δ))(P_1,…,P_m).

Since P_1,…,P_m were arbitrary, the result follows.

A function Δ : (Mat_{m×1})^m ⟶ ℝ is said to be multilinear if for all matrices P_1,…,P_m,Q in Mat_{m×1} and all real numbers c, Δ(P_1,…,cP_i+Q,…,P_m) = c Δ(P_1,…,P_i,…,P_m) + Δ(P_1,…,Q,…,P_m) for i = 1,…,m. We say that Δ is alternating if for all matrices P_1,…,P_m in Mat_{m×1}, Δ(P_1,…,P_i,…,P_j,…,P_m) = −Δ(P_1,…,P_j,…,P_i,…,P_m) for all 1 ≤ i < j ≤ m. Equivalently, Δ is alternating if τ(Δ) = −Δ for all transpositions τ in S_m.

## Theorem 2.4.2. If Δ : (Mat_{m×1})^m ⟶ ℝ is a multilinear function, then the following are equivalent:

- (a) Δ is alternating.
- (b) σ(Δ) = sgn(σ) Δ for all permutations σ in S_m.
- (c) If P_1,…,P_m are matrices in Mat_{m×1} and two (or more) of them are equal, then Δ(P_1,…,P_m) = 0.
- (d) If P_1,…,P_m are matrices in Mat_{m×1} and Δ(P_1,…,P_m) ≠ 0, then P_1,…,P_m are linearly independent.

Proof. (a) ⇒ (b): Let σ = τ_1⋯τ_k be a decomposition of σ into transpositions. By Theorem 2.4.1, σ(Δ) = (τ_1⋯τ_k)(Δ) = (τ_1⋯τ_{k−1})(τ_k(Δ)) = (τ_1⋯τ_{k−1})(−Δ) = −(τ_1⋯τ_{k−1})(Δ). Repeating the process k − 1 more times gives σ(Δ) = (−1)^k Δ. By Theorem B.2.3, sgn(σ) = (−1)^k.

(b) ⇒ (a): If τ is a transposition in S_m, then, by Theorem B.2.3, τ(Δ) = sgn(τ) Δ = −Δ.

(a) ⇒ (c): If P_i = P_j for some 1 ≤ i < j ≤ m, then Δ(P_1,…,P_i,…,P_j,…,P_m) = Δ(P_1,…,P_j,…,P_i,…,P_m).

On the other hand, since Δ is alternating,

Δ(P₁, …, Pⱼ, …, Pᵢ, …, Pₘ) = −Δ(P₁, …, Pᵢ, …, Pⱼ, …, Pₘ).

Thus,

Δ(P₁, …, Pᵢ, …, Pⱼ, …, Pₘ) = −Δ(P₁, …, Pᵢ, …, Pⱼ, …, Pₘ),

from which the result follows.

(c) ⇒ (a): For 1 ≤ i < j ≤ m, we have

0 = Δ(P₁, …, Pᵢ + Pⱼ, …, Pᵢ + Pⱼ, …, Pₘ)
= Δ(P₁, …, Pᵢ, …, Pᵢ, …, Pₘ) + Δ(P₁, …, Pᵢ, …, Pⱼ, …, Pₘ)
+ Δ(P₁, …, Pⱼ, …, Pᵢ, …, Pₘ) + Δ(P₁, …, Pⱼ, …, Pⱼ, …, Pₘ)
= Δ(P₁, …, Pᵢ, …, Pⱼ, …, Pₘ) + Δ(P₁, …, Pⱼ, …, Pᵢ, …, Pₘ),

from which the result follows.

To prove (c) ⇔ (d), we replace the assertion in part (d) with the following logically equivalent assertion:
(d′): If P₁, …, Pₘ are linearly dependent matrices in Matₘ×₁, then Δ(P₁, …, Pₘ) = 0.

(c) ⇒ (d′): Since P₁, …, Pₘ are linearly dependent, one of them can be expressed as a linear combination of the others, say, P₁ = Σᵢ₌₂ᵐ aⁱ Pᵢ. Then

Δ(P₁, …, Pₘ) = Δ( Σᵢ₌₂ᵐ aⁱ Pᵢ, P₂, …, Pₘ ) = Σᵢ₌₂ᵐ aⁱ Δ(Pᵢ, P₂, …, Pₘ).

Since Δ(Pᵢ, P₂, …, Pₘ) has Pᵢ in (at least) two positions, Δ(Pᵢ, P₂, …, Pₘ) = 0 for i = 2, …, m. Thus, Δ(P₁, …, Pₘ) = 0.

(d′) ⇒ (c): If two (or more) of P₁, …, Pₘ are equal, then P₁, …, Pₘ are linearly dependent, so Δ(P₁, …, Pₘ) = 0.

A function Δ : (Matₘ×₁)ᵐ ⟶ ℝ is said to be a determinant function (on Matₘ×₁) if it is both multilinear and alternating.

## Theorem 2.4.3. Let Δ : (Matₘ×₁)ᵐ ⟶ ℝ be a determinant function, and let P₁, …, Pₘ be matrices in Matₘ×₁, with

P₁ = [p₁¹; ⋮; p₁ᵐ],      …      Pₘ = [pₘ¹; ⋮; pₘᵐ].

Then

Δ(P₁, …, Pₘ) = Δ(E₁, …, Eₘ) Σ_{σ ∈ Sₘ} sgn(σ) p₁^{σ(1)} … pₘ^{σ(m)}.

**Proof.** We have

\[
P_1=\sum_{i_1=1}^{m}p_{1}^{i_1}E_{i_1},\qquad \cdots \qquad P_m=\sum_{i_m=1}^{m}p_{m}^{i_m}E_{i_m},
\]

hence

\[
\begin{aligned}
\Delta(P_1,\ldots,P_m)
&=\Delta\left(\sum_{i_1=1}^{m}p_1^{i_1}E_{i_1},\ldots,\sum_{i_m=1}^{m}p_m^{i_m}E_{i_m}\right)\\
&=\sum_{1\le i_1,\ldots,i_m\le m}p_1^{i_1}\cdots p_m^{i_m}\Delta(E_{i_1},\ldots,E_{i_m})\\
&=\sum_{\substack{1\le i_1,\ldots,i_m\le m\\i_1,\ldots,i_m\text{ distinct}}}p_1^{i_1}\cdots p_m^{i_m}\Delta(E_{i_1},\ldots,E_{i_m})\\
&=\sum_{\sigma\in S_m}p_1^{\sigma(1)}\cdots p_m^{\sigma(m)}\Delta(E_{\sigma(1)},\ldots,E_{\sigma(m)})\\
&=\sum_{\sigma\in S_m}p_1^{\sigma(1)}\cdots p_m^{\sigma(m)}\sigma(\Delta)(E_1,\ldots,E_m)\\
&=\Delta(E_1,\ldots,E_m)\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma)p_1^{\sigma(1)}\cdots p_m^{\sigma(m)}.
\end{aligned}
\]


## Theorem 2.4.4 (Existence of Determinant Function).

There is a unique determinant function

\[
\det:(\operatorname{Mat}_{m\times1})^m\longrightarrow\mathbb{R}
\]

on $\operatorname{Mat}_{m\times1}$ such that $\det(E_1,\ldots,E_m)=1$. More specifically,

\[
\det(P_1,\ldots,P_m)=\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma)p_1^{\sigma(1)}\cdots p_m^{\sigma(m)}
\]
\[
=\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma)p_{\sigma(1)}^1\cdots p_{\sigma(m)}^m.
\tag{2.4.1}
\]

for all matrices $P_1,\ldots,P_m$ in $\operatorname{Mat}_{m\times1}$, where

\[
P_1=\begin{bmatrix}p_1^1\\\vdots\\p_1^m\end{bmatrix},\qquad \cdots \qquad P_m=\begin{bmatrix}p_m^1\\\vdots\\p_m^m\end{bmatrix}.
\]

**Proof.** Let us begin by showing that the right-hand sides of (2.4.1) are equal. We have

\[
\begin{aligned}
\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma)p_1^{\sigma(1)}\cdots p_m^{\sigma(m)}
&=\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma)p_{\sigma^{-1}(1)}^1\cdots p_{\sigma^{-1}(m)}^m\\
&=\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma^{-1})p_{\sigma^{-1}(1)}^1\cdots p_{\sigma^{-1}(m)}^m\\
&=\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma)p_{\sigma(1)}^1\cdots p_{\sigma(m)}^m.
\end{aligned}
\]

where the second equality follows from Theorem B.2.2(b), and the third equality from the observation that as $\sigma$ varies over $S_m$, so does $\sigma^{-1}$.

**Existence.** With $P_1,\ldots,P_m$ as above, define a function $\det:(\mathrm{Mat}_{m\times1})^m\to\mathbb{R}$ using the first equality in (2.4.1). It is easily shown that $\det$ is multilinear. For a given permutation $\rho\in S_m$, we have

$$P_{\rho(1)}=\begin{bmatrix}p^1_{\rho(1)}\\ \vdots\\ p^m_{\rho(1)}\end{bmatrix},\quad\ldots\quad P_{\rho(m)}=\begin{bmatrix}p^1_{\rho(m)}\\ \vdots\\ p^m_{\rho(m)}\end{bmatrix},$$

hence

$$\begin{aligned}
\rho(\det)(P_1,\ldots,P_m)&=\det(P_{\rho(1)},\ldots,P_{\rho(m)})\\
&=\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma)p^1_{\rho(\sigma(1))}\cdots p^m_{\rho(\sigma(m))}\\
&=\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma)p^{\sigma\rho^{-1}(1)}_1\cdots p^{\sigma\rho^{-1}(m)}_m\\
&=\operatorname{sgn}(\rho)\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma\rho^{-1})p^{\sigma\rho^{-1}(1)}_1\cdots p^{\sigma\rho^{-1}(m)}_m\\
&=\operatorname{sgn}(\rho)\det(P_1,\ldots,P_m),
\end{aligned}$$

where the fourth equality follows from Theorem B.2.2, and the last equality follows from the observation that as $\sigma$ varies over $S_m$, so does $\sigma\rho^{-1}$. Since $P_1,\ldots,P_m$ were arbitrary, $\rho(\det)=\operatorname{sgn}(\rho)\det$. By Theorem 2.4.2, $\det$ is alternating. Thus, $\det$ is a determinant function. A straightforward computation shows that $\det(E_1,\ldots,E_m)=1$.

**Uniqueness.** This follows from Theorem 2.4.3.

Let $P$ be a matrix in $\mathrm{Mat}_{m\times m}$, and recall that in multi-index notation the column matrices of $P$ are $P_{(1)},\ldots,P_{(m)}$. Setting

$$P=[P_{(1)}\ \cdots\ P_{(m)}],$$

we henceforth view $P$ as an $m$-tuple of column matrices. In this way, the vector spaces $\mathrm{Mat}_{m\times m}$ and $(\mathrm{Mat}_{m\times1})^m$ are identified. Accordingly, we now express the determinant function in Theorem 2.4.4 as

$$\det:\mathrm{Mat}_{m\times m}\longrightarrow\mathbb{R},$$

so that

$$\det(P)=\det(P_{(1)},\ldots,P_{(m)}),$$

where $\det(P)$ is referred to as the **determinant of $P$**. In particular, the condition $\det(E_1,\ldots,E_m)=1$ in Theorem 2.4.4 becomes

$$\det(I_m)=1. \tag{2.4.2}$$





## 2.4 矩阵的行列式

### 定理 2.4.5

若 \(P\) 和 \(Q\) 是 \(\operatorname{Mat}_{m\times m}\) 中的矩阵，则：

- (a) \(\det(PQ)=\det(P)\det(Q)\)。
- (b) 若 \(P\) 可逆，则 \(\det(P^{-1})=\det(P)^{-1}\)。
- (c) \(\det(P^{\mathsf T})=\det(P)\)。

**Proof.** (a): 设我们定义一个函数 \(\Delta:(\operatorname{Mat}_{m\times 1})^m\to\mathbb R\) 为

\[
\Delta(R_1,\ldots,R_m)=\det([PR_1\ \cdots\ PR_m])
\]

对所有 \(\operatorname{Mat}_{m\times 1}\) 中的矩阵 \(R_1,\ldots,R_m\)。容易证明 \(\Delta\) 是一个行列式函数。我们有

\[
\begin{aligned}
\det([PQ_{(1)}\ \cdots\ PQ_{(m)}])
&=\Delta(Q_{(1)},\ldots,Q_{(m)})\\
&=\Delta(E_1,\ldots,E_m)\det([Q_{(1)}\ \cdots\ Q_{(m)}]) \qquad [(2.4.1),\text{ Th 2.4.3}]\\
&=\det(P)\det(Q).
\end{aligned}
\]

(b): 我们由 (2.4.1) 和部分 (a) 得到

\[
1=\det(I_m)=\det(PP^{-1})=\det(P)\det(P^{-1}),
\]

由此结果得出。

(c): 设 \(P^{\mathsf T}=[p_j^i]^{\mathsf T}=[q_j^i]\)。则

\[
\det(P^{\mathsf T})
=\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma)\,q_{\sigma(1)}^1\cdots q_{\sigma(m)}^m
=\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma)\,p_1^{\sigma(1)}\cdots p_m^{\sigma(m)}
=\det(P),
\]

其中第一个和最后一个等式由 (2.4.1) 得出，而第二个等式由观察 \(q_{\sigma(i)}^i=p_i^{\sigma(i)}\) 对所有 \(S_m\) 中的置换 \(\sigma\) 成立得出。

设 \(P\) 是 \(\operatorname{Mat}_{m\times m}\) 中的矩阵。\(P\) 的 \(ij\)-余子式定义为

\[
c_j^i=(-1)^{i+j}\det\left(P^{(1,\ldots,\hat i,\ldots,m)}_{(1,\ldots,\hat j,\ldots,m)}\right),
\]

其中帽子表示省略一个表达式。于是，\(P^{(1,\ldots,\hat i,\ldots,m)}_{(1,\ldots,\hat j,\ldots,m)}\) 是通过删除 \(P\) 的第 \(i\) 行和第 \(j\) 列得到的 \(\operatorname{Mat}_{(m-1)\times(m-1)}\) 中的矩阵。\(P\) 的伴随矩阵是 \(\operatorname{Mat}_{m\times m}\) 中定义的矩阵

\[
\operatorname{adj}(P)=[c_j^i]^{\mathsf T}.
\]

### 定理 2.4.6 (行列式的列展开)

若 \(P=[p_j^i]\) 是 \(\operatorname{Mat}_{m\times m}\) 中的矩阵且 \(1\le j\le m\)，则

\[
\det(P)=\sum_i c_j^i p_j^i,
\]

这称为 \(\det(P)\) 沿 \(P\) 的第 \(j\) 列的展开。

**Proof.** 我们有

\[
P=\begin{bmatrix} P_{(1,\ldots,j-1)} & \sum_i p_j^i E_i & P_{(j+1,\ldots,m)} \end{bmatrix}
\]

故

\[
\det(P)=\sum_i p_j^i\det\left(\begin{bmatrix} P_{(1,\ldots,j-1)} & E_i & P_{(j+1,\ldots,m)} \end{bmatrix}\right).
\]

我们也有

\[
\begin{aligned}
\det\left(\begin{bmatrix} P_{(1,\ldots,j-1)} & E_i & P_{(j+1,\ldots,m)} \end{bmatrix}\right)
&=\det\left(\begin{bmatrix}
P^{(1,\ldots,i-1)}_{(1,\ldots,j-1)} & O_{(i-1)\times1} & P^{(1,\ldots,i-1)}_{(j+1,\ldots,m)}\\
P^{(i)}_{(1,\ldots,j-1)} & 1 & P^{(i)}_{(j+1,\ldots,m)}\\
P^{(i+1,\ldots,m)}_{(1,\ldots,j-1)} & O_{(m-i)\times1} & P^{(i+1,\ldots,m)}_{(j+1,\ldots,m)}
\end{bmatrix}\right)\\
&=(-1)^{(i-1)+(j-1)}\det\left(\begin{bmatrix}
1 & P^{(i)}_{(1,\ldots,j-1)} & P^{(i)}_{(j+1,\ldots,m)}\\
O_{(i-1)\times1} & P^{(1,\ldots,i-1)}_{(1,\ldots,j-1)} & P^{(1,\ldots,i-1)}_{(j+1,\ldots,m)}\\
O_{(m-i)\times1} & P^{(i+1,\ldots,m)}_{(1,\ldots,j-1)} & P^{(i+1,\ldots,m)}_{(j+1,\ldots,m)}
\end{bmatrix}\right)\\
&=(-1)^{i+j}\det\left(\begin{bmatrix}
1 & P^{(i)}_{(1,\ldots,\hat j,\ldots,m)}\\
O_{(m-1)\times1} & P^{(1,\ldots,\hat i,\ldots,m)}_{(1,\ldots,\hat j,\ldots,m)}
\end{bmatrix}\right)\\
&=(-1)^{i+j}\det\left(P^{(1,\ldots,\hat i,\ldots,m)}_{(1,\ldots,\hat j,\ldots,m)}\right)=c_j^i,
\end{aligned}
\]

其中第二个等式是通过交换 \(i-1\) 个相邻行，然后交换 \(j-1\) 个相邻列得到的，其中第四个等式由 (2.4.1) 得出。结合上述恒等式得到结果。

### 定理 2.4.7

若 \(P\) 是 \(\operatorname{Mat}_{m\times m}\) 中的矩阵，则

\[
P\operatorname{adj}(P)=\det(P)I_m.
\]

**Proof.** 设 \(P=[p_j^i]\)。对于给定的整数 \(1\le k,l\le m\)，设 \(Q=[q_j^i]\) 是通过用 \(P\) 的第 \(l\) 列替换 \(P\) 的第 \(k\) 列得到的 \(\operatorname{Mat}_{m\times m}\) 中的矩阵；即

\[
Q=[P_{(1)}\ \cdots\ P_{(k-1)}\ P_{(l)}\ P_{(k+1)}\ \cdots\ P_{(m)}].
\]

设 \(\operatorname{adj}(P)=[c_j^i]^{\mathsf T}\) 且 \(\operatorname{adj}(Q)=[d_j^i]^{\mathsf T}\)。由定理 2.4.6，

\[
\sum_i c_k^i p_k^i=\det(P).
\]

若 \(k\ne l\)，则 \(Q\) 的两列相等且

\[
Q^{(1,\ldots,\hat i,\ldots,m)}_{(1,\ldots,\hat k,\ldots,m)}=P^{(1,\ldots,\hat i,\ldots,m)}_{(1,\ldots,\hat k,\ldots,m)}.
\]

由定理 2.4.2 和 2.4.6 可得

\[
\sum_i c_k^i p_l^i=\sum_i d_k^i q_l^i=\det(Q)=0.
\]

于是，

\[
\sum_i c_k^i p_l^i=\det(P)\,\delta_{kl}
\]

对 \(k,l=1,\ldots,m\) 成立，其中 \(\delta_{kl}\) 是克罗内克 delta。则 \([c_j^i][p_j^i]^{\mathsf T}=\det(P)I_m\)。取转置得到结果。

### 定理 2.4.8

若 \(P\) 是 \(\operatorname{Mat}_{m\times m}\) 中的矩阵，则以下条件等价：

- (a) \(P\) 可逆。
- (b) \(\det(P)\ne 0\)。
- (c) \(\operatorname{rank}(P)=m\)。

若上述等价条件之一成立，则

\[
P^{-1}=\frac{1}{\det(P)}\operatorname{adj}(P).
\]

**Proof.** (a) \(\Rightarrow\) (b): 由定理 2.4.5(b)，\(\det(P)\det(P^{-1})=1\)，故 \(\det(P)\ne 0\)。

(b) \(\Rightarrow\) (c): 由于 \(\det([P_{(1)}\ \cdots\ P_{(m)}])=\det(P)\ne0\)，由定理 2.4.2 可知 \(P_{(1)},\ldots,P_{(m)}\) 线性无关，所以

\[
\operatorname{rank}(P)=\operatorname{rank}([P_{(1)}\ \cdots\ P_{(m)}])=m.
\]

(c) \(\Rightarrow\) (a): 由于 \(\operatorname{rank}(P)=m\)，\(\mathscr P=(P_{(1)},\ldots,P_{(m)})\) 是 \(\operatorname{Mat}_{m\times1}\) 的基。设 \(Q=[[E_1]_{\mathscr P}\ \cdots\ [E_m]_{\mathscr P}]\)。根据定义，\(E_j=P[E_j]_{\mathscr P}\) 对 \(j=1,\ldots,m\) 成立，故

\[
I_m=[E_1\ \cdots\ E_m]=PQ,
\]

所以 \(Q=P^{-1}\)。最后的断言由定理 2.4.7 得出。

### 定理 2.4.9

\(\operatorname{Mat}_{m\times1}\) 中的矩阵 \(P_1,\ldots,P_m\) 线性无关当且仅当 \(\det([P_1\ \cdots\ P_m])\ne0\)。

**Proof.** 我们有

\[
\begin{aligned}
P_1,\ldots,P_m\text{ 线性无关}
&\Longleftrightarrow \operatorname{rank}([P_1\ \cdots\ P_m])=m\\
&\Longleftrightarrow \det([P_1\ \cdots\ P_m])\ne0. \qquad [\text{Th 2.4.8}]
\end{aligned}
\]

下一个结果通常不包含在行列式的概述中，但它在以后将证明非常有价值。

### 定理 2.4.10 (Cauchy–Binet 恒等式)

设 \(1\le k\le m\) 为整数。

- (a) 若 \(P\) 和 \(Q\) 是 \(\operatorname{Mat}_{m\times k}\) 中的矩阵，则

\[
\det(P^{\mathsf T}Q)=\sum_{I\in\mathcal I_{k,m}}\det(P^I)\det(Q^I).
\]

- (b) 若 \(R\) 和 \(S\) 是 \(\operatorname{Mat}_{k\times m}\) 中的矩阵，则

\[
\det(RS^{\mathsf T})=\sum_{J\in\mathcal I_{k,m}}\det(R_J)\det(S_J).
\]

**Proof.** (a): 设 \(Q=[q_j^i]\)，于是

\[
Q=[Q_{(1)}\ \cdots\ Q_{(k)}],\qquad Q_{(j)}=[q_j^1\ \cdots\ q_j^m]^{\mathsf T},
\]

且

\[
P^{\mathsf T}Q_{(j)}=\sum_i q_j^i(P^{\mathsf T})_{(i)}
\]

对 \(j=1,\ldots,k\)。我们有

\[
\begin{aligned}
\det(P^{\mathsf T}Q)
&=\det\left([P^{\mathsf T}Q_{(1)}\ \cdots\ P^{\mathsf T}Q_{(k)}]\right)\\
&=\det\left(\left[\sum_{i_1}q_1^{i_1}(P^{\mathsf T})_{(i_1)}\ \cdots\ \sum_{i_k}q_k^{i_k}(P^{\mathsf T})_{(i_k)}\right]\right)\\
&=\sum_{1\le i_1,\ldots,i_k\le m}q_1^{i_1}\cdots q_k^{i_k}\det\left([(P^{\mathsf T})_{(i_1)}\ \cdots\ (P^{\mathsf T})_{(i_k)}]\right)\\
&=\sum_{\substack{1\le i_1,\ldots,i_k\le m\\ i_1,\ldots,i_k\text{ 互不相同}}}q_1^{i_1}\cdots q_k^{i_k}\det\left([(P^{\mathsf T})_{(i_1)}\ \cdots\ (P^{\mathsf T})_{(i_k)}]\right)\\
&=\sum_{1\le i_1<\cdots<i_k\le m}\left(\sum_{\sigma\in S_k}q_1^{i_{\sigma(1)}}\cdots q_k^{i_{\sigma(k)}}\det\left([(P^{\mathsf T})_{(i_{\sigma(1)})}\ \cdots\ (P^{\mathsf T})_{(i_{\sigma(k)})}]\right)\right).
\end{aligned}
\]

其中第三个等式由定理 2.4.2 得出。前一显示中最后一行的带大括号的项可以表示为

\[
\begin{aligned}
&\sum_{\sigma\in S_k}q_1^{i_{\sigma(1)}}\cdots q_k^{i_{\sigma(k)}}\det\left([(P^{\mathsf T})_{(i_{\sigma(1)})}\ \cdots\ (P^{\mathsf T})_{(i_{\sigma(k)})}]\right)\\
&=\sum_{\sigma\in S_k}q_1^{i_{\sigma(1)}}\cdots q_k^{i_{\sigma(k)}}\operatorname{sgn}(\sigma)\det\left([(P^{\mathsf T})_{(i_1)}\ \cdots\ (P^{\mathsf T})_{(i_k)}]\right)\\
&=\det\left([(P^{\mathsf T})_{(i_1)}\ \cdots\ (P^{\mathsf T})_{(i_k)}]\right)\left(\sum_{\sigma\in S_k}\operatorname{sgn}(\sigma)q_1^{i_{\sigma(1)}}\cdots q_k^{i_{\sigma(k)}}\right)\\
&=\det\left(P^{\mathsf T}_{(i_1,\ldots,i_k)}\right)\det\left(Q^{(i_1,\ldots,i_k)}\right).
\end{aligned}
\]

于是，

\[
\det(P^{\mathsf T}Q)=\sum_{1\le i_1<\cdots<i_k\le m}\det\left(P^{\mathsf T}_{(i_1,\ldots,i_k)}\right)\det\left(Q^{(i_1,\ldots,i_k)}\right),
\]

这就是 Cauchy–Binet 恒等式的经典版本。

### 定理 2.4.12

若 \(V\) 是向量空间，且 \(\mathcal H\) 和 \(\mathcal F\) 是 \(V\) 的基，则

\[
\det([\operatorname{id}_V]_{\mathcal H}^{\mathcal F})\ne 0.
\]

**Proof.** 我们有

\[
1=\det([\operatorname{id}_V]_{\mathcal H}^{\mathcal H}) \qquad [(2.4.2)]
\]

\[
=\det([\operatorname{id}_V]_{\mathcal F}^{\mathcal H})\det([\operatorname{id}_V]_{\mathcal H}^{\mathcal F}) \qquad [\text{Th 2.2.2(c), Th 2.4.5(a)}],
\]

由此结果得出。

在与定理 2.2.3 相关的讨论中提到，\(\operatorname{Mat}_{m\times m}\) 在通常的矩阵加法和矩阵乘法运算下是一个环，故它在矩阵加法下是一个群。但 \(\operatorname{Mat}_{m\times m}\) 显然不是在矩阵乘法下是一个群，因为并非所有矩阵都有逆。然而，\(\operatorname{Mat}_{m\times m}\) 包含若干矩阵群。其中最大的是由所有可逆矩阵组成的一般线性群：

\[
GL(m)=\{P\in\operatorname{Mat}_{m\times m}:\det(P)\ne0\},
\]

其中使用行列式的刻画由定理 2.4.8 得出。

我们说 \(\operatorname{Mat}_{m\times m}\) 中的矩阵 \(P\) 是正交的，如果 \(P^{\mathsf T}P=I_m\)。如果是，则 \(P^{\mathsf T}=P^{-1}\)，故 \(PP^{\mathsf T}=PP^{-1}=I_m\)。于是，\(P^{\mathsf T}P=I_m\) 当且仅当 \(PP^{\mathsf T}=I_m\)。正交群是 \(GL(m)\) 的子群，由所有正交矩阵组成：

\[
O(m)=\{P\in GL(m):P^{\mathsf T}P=I_m\}.
\]

对于 \(O(m)\) 中的矩阵 \(P\)，我们有

\[
1=\det(I_m)=\det(P^{\mathsf T}P)=\det(P^{\mathsf T})\det(P)=\det(P)^2,
\]

故 \(\det(P)=\pm1\)。特殊正交群是 \(O(m)\) 的子群，由所有行列式等于 \(1\) 的矩阵组成：

\[
SO(m)=\{P\in O(m):\det(P)=1\}.
\]

## 2.5 线性映射的迹与行列式

矩阵的迹在第 2.1 节中定义，行列式在第 2.4 节中定义。我们现在将这些概念推广到线性映射。

设 \(V\) 是向量空间，\(\mathcal H\) 是 \(V\) 的基，\(A:V\longrightarrow V\) 是线性映射。\(A\) 的迹定义为

\[
\operatorname{tr}(A)=\operatorname{tr}([A]_{\mathcal H}^{\mathcal H}), \qquad (2.5.1)
\]

\(A\) 的行列式定义为

\[
\det(A)=\det([A]_{\mathcal H}^{\mathcal H}). \qquad (2.5.2)
\]





## 定理 2.5.1. 在上述设定下，$\operatorname{tr}(A)$ 和 $\det(A)$ 与 $V$ 的基的选择无关。

*证明.* 设 $\mathcal{F}$ 是 $V$ 的另一组基。由定理 2.2.6，

$$[A]_{\mathcal{H}}^{\mathcal{H}}=\left([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}\right)^{-1}[A]_{\mathcal{F}}^{\mathcal{F}}[\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}.$$

我们由定理 2.1.7(c) 得到

$$\operatorname{tr}\left([A]_{\mathcal{H}}^{\mathcal{H}}\right)=\operatorname{tr}\left([A]_{\mathcal{F}}^{\mathcal{F}}\right),$$

并且由定理 2.4.5 得到

$$\det\left([A]_{\mathcal{H}}^{\mathcal{H}}\right)=\det\left([A]_{\mathcal{F}}^{\mathcal{F}}\right).$$

## 定理 2.5.2. 若 $V$ 是一个向量空间且 $A,B:V\longrightarrow V$ 是线性映射，则：

- (a) $\det(\operatorname{id}_V)=1$。
- (b) $\det(A\circ B)=\det(A)\det(B)$。
- (c) 若 $A$ 是线性同构，则 $\det(A^{-1})=\det(A)^{-1}$。
- (d) $\det(A^*)=\det(A)$。

*证明.* (a): 这由定理 2.2.3(b) 和 (2.4.2) 推出。

(b): 这由定理 2.2.3(b) 和定理 2.4.5(a) 推出。

(c): 这由定理 2.2.3(b) 和定理 2.4.5(b) 推出。

(d): 这由定理 2.2.7 和定理 2.4.5(c) 推出。

## 定理 2.5.3. 设 $V$ 是一个向量空间，且 $A:V\longrightarrow V$ 是一个线性映射。则 $A$ 是线性同构当且仅当 $\det(A)\ne 0$。

*证明.* 这由定理 2.2.3(b) 和定理 2.4.8 推出。
