

## 4.2 正交基

设 $(V,\mathfrak{g})$ 为维数为 $m$ 的数量积空间，$v, w$ 为 $V$ 中的向量。回顾第 3.1 节，若 $\langle v, w\rangle = 0$ 则称 $v$ 与 $w$ 正交。由于 $\mathfrak{g}$ 是对称的，$v$ 与 $w$ 正交当且仅当 $w$ 与 $v$ 正交。对于整数 $k \ge 2$，$V$ 中 $k$ 个不同的非零向量 $(v_1,\dots,v_k)$ 被称为正交 $k$-元组，如果当 $i \ne j = 1,\dots,k$ 时 $v_i$ 与 $v_j$ 正交。这允许 $v_i$ 与自身正交的可能性；即 $v_i$ 可能是光状向量，对于一个或多个 $1 \le i \le m$。如果正交 $k$-元组 $(v_1,\dots,v_k)$ 由单位向量组成，则称其为标准正交 $k$-元组，此时排除了光状向量。当正交 $m$-元组是 $V$ 的基时，称其为正交基。由定理 4.2.3 可知，标准正交 $m$-元组必然是 $V$ 的基，称为标准正交基。因此，若 $(e_1,\dots,e_m)$ 是 $V$ 的标准正交基，则根据定义，$e_1,\dots,e_m$ 各自为单位向量，所以 $\langle e_i,e_j\rangle = \pm \delta_{ij}$ 对于 $i,j = 1,\dots,m$，其中 $\delta_{ij}$ 为克罗内克 delta。特别地，$\mathbb{R}_\mathfrak{g}^m$ 的标准基是标准正交的。

标准正交基在计算上很方便。事实证明数量积空间总是具有一个，如我们现在所示。

### 定理 4.2.1. 每个非零数量积空间都包含一个单位向量。

证明. 这由定理 3.2.4(b) 和定理 3.3.4 可得。

### 定理 4.2.2. 若 $(V,\mathfrak{g})$ 为维数为 $m$ 的数量积空间且 $1 \le k \le m-1$ 为整数，则 $V$ 中任意标准正交 $k$-元组都可以延拓为 $V$ 中的标准正交 $(k+1)$-元组。

证明. 设 $\mathcal{E} = (e_1,\dots,e_k)$ 为 $V$ 中标准正交 $k$-元组。则 $\mathcal{E}$ 是子空间 $U = \mathbb{R}e_1 + \cdots + \mathbb{R}e_k$ 的标准正交基。由定理 1.1.16，$U = \mathbb{R}e_1 \oplus \cdots \oplus \mathbb{R}e_k$。由于 $(\mathfrak{g}|_U)_{\mathcal{E}}$，即 $\mathfrak{g}|_U$ 关于 $\mathcal{E}$ 的矩阵，是对角矩阵且每个对角元为 $1$ 或 $-1$，$(\mathfrak{g}|_U)_{\mathcal{E}}$ 可逆。由定理 3.3.3 可知 $\mathfrak{g}$ 在 $U$ 上非退化，进而由定理 4.1.3 可知 $\mathfrak{g}$ 在 $U^\perp$ 上非退化。因此，$(U^\perp,\mathfrak{g}|_{U^\perp})$ 是一个非零数量积空间。由定理 4.2.1，$U^\perp$ 包含一个单位向量 $e_{k+1}$。则 $(e_1,\dots,e_{k+1})$ 是 $V$ 中所求的标准正交 $(k+1)$-元组。

### 定理 4.2.3. 若 $(V,\mathfrak{g})$ 为数量积空间且 $(e_1,\dots,e_k)$ 为 $V$ 中标准正交 $k$-元组，则 $e_1,\dots,e_k$ 线性无关。

证明. 若 $\sum_i a_i e_i = 0$，则

$$0 = \left\langle e_i, \sum_j a_j e_j \right\rangle = \sum_j a_j \langle e_i, e_j \rangle = a_i \langle e_i, e_i \rangle = \pm a_i,$$

故 $a_i = 0$ 对于 $i = 1,\dots,m$。

### 定理 4.2.4. 每个非零数量积空间都有一个标准正交基。

证明. 设 $(V,\mathfrak{g})$ 为维数 $m \ge 1$ 的数量积空间。由定理 4.2.1，$V$ 包含一个单位向量。归纳地应用定理 4.2.2，我们构造 $V$ 中一个标准正交 $m$-元组。由定理 4.2.3 可知这是一个标准正交基。

为方便起见，我们限制关注满足一定条件的标准正交基。

**在整个讨论中，所有标准正交基 $(e_1,\dots,e_m)$ 都假定是有序的，使得 $\langle e_1,e_1\rangle,\dots,\langle e_m,e_m\rangle$ 中所有 +1 位于所有 -1 之前。**

举例说明，设 $\mathcal{E} = (e_1,\dots,e_{m-2},e_{m-1},e_m)$ 为 $\mathbb{R}_\nu^m$ 的标准基，$\mathcal{F} = (e_1,\dots,e_{m-2},e_m,e_{m-1})$。则 $\mathcal{E}$ 和 $\mathcal{F}$ 都是 $\mathbb{R}_\nu^m$ 的标准正交基。由于 $\langle e_1,e_1\rangle = \cdots = \langle e_{m-1}\rangle = 1$ 且 $\langle e_m,e_m\rangle = -1$，我们看到 $\mathcal{E}$ 满足上述约定但 $\mathcal{F}$ 不满足。

设 $(V,\mathfrak{g})$ 为数量积空间，且 $V = V^+ \oplus V^-$ 为定理 4.1.6(a) 给出的类型直和。$\mathfrak{g}$ 的指标，也称为 $V$ 的指标，定义为

$$\operatorname{ind}(\mathfrak{g}) = \dim(V^-).$$

我们经常用

$$\operatorname{ind}(\mathfrak{g})\quad\text{表示为}\quad\nu.$$

根据定理 4.1.6(b)，$\operatorname{ind}(\mathfrak{g})$ 与直和的选择无关。以 $\mathbb{R}_\nu^m = (\mathbb{R}^m,\mathfrak{s})$ 为例，我们由 (4.1.2) 可知 $\operatorname{ind}(\mathfrak{s}) = \nu$。

设 $\mathcal{E} = (e_1,\dots,e_m)$ 为 $V$ 的标准正交基。$\mathfrak{g}$ 的符号，也称为 $V$ 的符号，定义为 $m$-元组

$$\bigl(\langle e_1,e_1\rangle,\dots,\langle e_m,e_m\rangle\bigr).$$

鉴于上述关于标准正交基的约定，$\mathfrak{g}$ 的符号在前 $m-\nu$ 个位置有 +1，在剩余 $\nu$ 个位置有 -1。设我们记

$$\langle e_i,e_i\rangle\quad\text{为}\quad\varepsilon_i$$

对于 $i = 1,\dots,m$。则

$$\langle e_i,e_j\rangle = \varepsilon_i\delta_{ij}\tag{4.2.1}$$

对于 $i,j = 1,\dots,m$，其中 $\delta_{ij}$ 为克罗内克 delta。在此记号下，$\mathfrak{g}$ 的符号为

$$(\varepsilon_1,\dots,\varepsilon_m).$$

我们注意到

$$\mathfrak{g}_{\mathcal{E}} = \operatorname{diag}(\varepsilon_1,\dots,\varepsilon_m),\tag{4.2.2}$$

故

$$\det(\mathfrak{g}_{\mathcal{E}}) = \varepsilon_1\cdots\varepsilon_m = (-1)^\nu.\tag{4.2.3}$$

以下是上述恒等的推广。

### 定理 4.2.5. 设 $(V, \mathfrak{g})$ 为符号为 $(\varepsilon_1, \dots, \varepsilon_m)$ 且指标为 $\nu$ 的数量积空间。设 $\mathcal{E}$ 和 $\mathcal{H}$ 为 $V$ 的基，$\mathcal{E}$ 为标准正交基，且 $A : V \longrightarrow V$ 为由 $A(\mathcal{E}) = \mathcal{H}$ 定义的线性同构。则

$\det(\mathfrak{g}_\mathcal{H}) = \varepsilon_1\cdots\varepsilon_m \det(A)^2 = (-1)^\nu \det(A)^2$。

证明. 我们有

$\mathfrak{g}_\mathcal{H} = ([id_V]^\mathcal{E}_{A(\mathcal{E})})^\top \mathfrak{g}_\mathcal{E} [id_V]^\mathcal{E}_{A(\mathcal{E})}$ [Th 3.1.3] $= ([A]^\mathcal{E}_\mathcal{E})^\top \mathfrak{g}_\mathcal{E} [A]^\mathcal{E}_\mathcal{E}$, [Th 2.2.5(a)]

故 $\det(\mathfrak{g}_\mathcal{H}) = \det(\mathfrak{g}_\mathcal{E}) \det(A)^2$。结果现在由 (4.2.3) 可得。

### 定理 4.2.6. 设 $(V,\mathfrak{g})$ 为数量积空间，且 $U$ 为 $V$ 的子空间。若 $\mathfrak{g}$ 在 $U$ 上非退化，则

$\operatorname{ind}(\mathfrak{g}) = \operatorname{ind}(\mathfrak{g}|_U) + \operatorname{ind}(\mathfrak{g}|_{U^\perp})$。

注. 由定理 4.1.3，$\mathfrak{g}$ 在 $U^\perp$ 上非退化。因此，$(U, \mathfrak{g}|_U)$ 和 $(U^\perp, \mathfrak{g}|_{U^\perp})$ 都是数量积空间，故断言有意义。

证明. 设 $\dim(V) = m$ 且 $\dim(U) = k$。我们由定理 4.1.3 有 $V = U \oplus U^\perp$，然后由定理 1.1.18 有 $\dim(U^\perp) = m - k$。设 $(f_1, \dots, f_k)$ 和 $(f_{k+1}, \dots, f_m)$ 分别为 $U$ 和 $U^\perp$ 的标准正交基，且 $e_1, \dots, e_m$ 为 $f_1, \dots, f_m$ 的一个重排，使得 $(e_1, \dots, e_m)$ 为满足上述符号排序约定的 $V$ 的标准正交基。显然，$(e_1, \dots, e_m)$ 中 -1 的个数等于 $(f_1, \dots, f_k)$ 中 -1 的个数加上 $(f_{k+1}, \dots, f_m)$ 中 -1 的个数。结果得证。

### 定理 4.2.7. 设 $(V,\mathfrak{g})$ 为符号为 $(\varepsilon_1, \dots, \varepsilon_m)$ 的数量积空间，$(e_1, \dots, e_m)$ 为 $V$ 的标准正交基，且 $v$ 为 $V$ 中的向量。则

$v = \sum_i \varepsilon_i \langle v, e_i \rangle e_i$。

证明. 设 $v = \sum_j a^j e_j$。则 $\langle v, e_i \rangle = \langle \sum_j a^j e_j, e_i \rangle = \sum_j a^j \langle e_j, e_i \rangle = \varepsilon_i a^i$，故 $a^i = \varepsilon_i \langle v, e_i \rangle$ 对于 $i = 1, \dots, m$。

### 定理 4.2.8. 设 $(V,\mathfrak{g})$ 为符号为 $(\varepsilon_1, \dots, \varepsilon_m)$ 的数量积空间，$(e_1, \dots, e_m)$ 为 $V$ 的标准正交基，且 $v, w$ 为 $V$ 中的向量，其中 $v = \sum_i a^i e_i$ 且 $w = \sum_j b^j e_j$。则

$\langle v, w \rangle = \sum_i \varepsilon_i a^i b^i$。

## 4.3 伴随

*证明.* 我们有

$$\langle v,w\rangle=\left\langle\sum_i a^i e_i,\sum_j b^j e_j\right\rangle=\sum_{ij}a^i b^j\langle e_i,e_j\rangle=\sum_i\varepsilon_i a^i b^i.$$

## 4.3 伴随

我们先前定义了矩阵对称的含义。在本节中，我们引入一种线性映射，在某些情况下具有相关的对称性质（见定理 4.6.10）。

设 $(V,\mathfrak g)$ 和 $(W,\mathfrak h)$ 为数量积空间，且 $A:V\to W$ 为线性映射。我们定义映射

$$A^\dagger:W\longrightarrow V,$$

称为 $A$ 的**伴随**，由

$$A^\dagger=S\circ A^*\circ\mathcal F,$$  (4.3.1)

给出，其中 $S$ 和 $\mathcal F$ 分别对应于 $\mathfrak g$ 和 $\mathfrak h$ 的锐映射和平映射，且 $A^*$ 为由 $A$ 诱导的拉回。设我们记

$$(A^\dagger)^\dagger\quad\text{为}\quad A^{\dagger\dagger}.$$

### 定理 4.3.1. 在上述设定下：
(a) $A^\dagger$ 是唯一的线性映射，使得

$$\langle A(v),w\rangle=\langle v,A^\dagger(w)\rangle$$

对所有 $V$ 中的向量 $v$ 和 $W$ 中的向量 $w$ 成立。

(b) 若 $V=W$，则 $\det(A^\dagger)=\det(A)$。

证明. (a): 我们有

$$A^\dagger=S\circ A^*\circ\mathcal F$$
$$\Longleftrightarrow\quad A^*\circ\mathcal F=F\circ A^\dagger$$
$$\Longleftrightarrow\quad (A^*\circ\mathcal F)(w)=(F\circ A^\dagger)(w)\ \text{对所有 }w\text{ 在 }W\text{ 中}$$
$$\Longleftrightarrow\quad A^*(\mathcal F(w))(v)=F(A^\dagger(w))(v)\ \text{对所有 }v\text{ 在 }V\text{ 中和 }w\text{ 在 }W\text{ 中}$$
$$\Longleftrightarrow\quad \mathcal F(w)(A(v))=F(A^\dagger(w))(v)\ \text{对所有 }v\text{ 在 }V\text{ 中和 }w\text{ 在 }W\text{ 中}$$
$$\Longleftrightarrow\quad \langle w,A(v)\rangle=\langle A^\dagger(w),v\rangle\ \text{对所有 }v\text{ 在 }V\text{ 中和 }w\text{ 在 }W\text{ 中}$$
$$\Longleftrightarrow\quad \langle A(v),w\rangle=\langle v,A^\dagger(w)\rangle\ \text{对所有 }v\text{ 在 }V\text{ 中和 }w\text{ 在 }W\text{ 中}.$$

这表明 $A^\dagger$ 具有所需性质，同时也表明它由该性质唯一确定。
(b): 当 $\mathcal F=F$ 时，我们有 $S=F^{-1}$，故结果由定理 2.4.5 和 (4.3.1) 可得。

