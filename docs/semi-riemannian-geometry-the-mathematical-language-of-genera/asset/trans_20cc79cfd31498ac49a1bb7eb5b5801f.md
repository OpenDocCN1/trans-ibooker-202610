

**证明。** $(\Rightarrow)$：这由 (3.1.1) 可得。  
$(\Leftarrow)$：显而易见。

定理 3.2.2 表明，只要 $\mathbf b$ 是对称的，$\mathbf b$ 和 $q$ 完全相互确定。

## 定理 3.2.3.
设 $V$ 为向量空间，$\mathbf b$ 为 $V$ 上的双线性函数，$q$ 为相应的二次函数。则以下条件等价：

- (a) $\mathbf b$ 是交错的。  
- (b) $q=0$。  
- (c) $V$ 没有单位向量。

**证明。** (a) $\Rightarrow$ (b)：由于对 $V$ 中所有向量 $v,w$ 都有 $\langle v,w\rangle=-\langle w,v\rangle$，令 $w=v$ 得 $q(v)=-q(v)$，故 $q(v)=0$，对 $V$ 中所有向量 $v$ 成立。

(b) $\Rightarrow$ (a)：这由 (3.1.1) 可得。  
(b) $\Leftrightarrow$ (c)：我们证明逻辑等价的断言：$q\ne0$ 当且仅当 $V$ 有单位向量。若 $q\ne0$，则存在 $V$ 中非零向量 $v$ 使 $q(v)\ne0$，故 $v/\sqrt{|q(v)|}$ 是单位向量。逆命题同样显而易见。

我们特别感兴趣的是存在非零对称双线性函数的向量空间。原因在于此类向量空间具有单位向量，如今我们来证明。

## 定理 3.2.4.
设 $V$ 为向量空间，$\mathbf b$ 为 $V$ 上的对称双线性函数，$q$ 为相应的二次函数。则：

- (a) $\mathbf b=0$ 当且仅当 $q=0$。  
- (b) 若 $\mathbf b\ne0$，则 $V$ 有单位向量。

**证明。** (a) $(\Rightarrow)$：显而易见。  
(a) $(\Leftarrow)$：这由定理 3.2.2 可得。  
(b)：由部分 (a)，$q\ne0$。结果现在由定理 3.2.3 可得。

向量空间的光锥集是一个相当神秘的实体。下一个结果提供了在双线性函数满足某些性质时其刻画。

## 定理 3.2.5.
若 $V$ 为向量空间且 $\mathbf b$ 为 $V$ 上的对称半定双线性函数，则

$$\Lambda=V^\perp\setminus\{0\}.$$

**注。** 上述恒等式等价于说，对 $V$ 中每个非零向量 $v$，$\langle v,v\rangle=0$ 当且仅当对 $V$ 中所有向量 $w$ 都有 $\langle w,w\rangle=0$。

**证明。** 显然，$V^\perp\setminus\{0\}\subseteq\Lambda$。为证明反向包含，设 $q$ 为对应于 $\mathbf b$ 的二次函数，并设 $v$ 和 $w$ 分别为 $\Lambda$ 和 $V$ 中的向量。考虑多项式 $p(t):\mathbb R\to\mathbb R$ 定义为

$$p(t)=q(tv+w)=2\langle v,w\rangle t+\langle w,w\rangle.$$

我们有 $\langle v, w\rangle = 0$；否则 $p(t)$ 取正负值，这与 $\mathbf b$ 的半定性矛盾。由于 $w$ 是任意的，$v$ 在 $V^\perp\setminus\{0\}$ 中。于是，$\Lambda \subseteq V^\perp\setminus\{0\}$。

下一个结果只是柯西-施瓦茨不等式在数学多个领域出现的众多版本之一。我们将在第 4.6 节看到另一个可能更熟悉的变体。

## 定理 3.2.6 (柯西-施瓦茨不等式)。
若 $V$ 为向量空间且 $\mathbf b$ 为 $V$ 上的对称半定双线性函数，则

$$\langle v, w\rangle^2 \le \langle v, v\rangle\langle w, w\rangle \tag{3.2.1}$$

对 $V$ 中所有向量 $v, w$ 成立。

证明。结果在 $v = 0$ 或 $w = 0$ 时是平凡的，故设 $v, w \ne 0$。设 $q$ 为对应于 $\mathbf b$ 的二次函数，并考虑多项式 $p(t) : \mathbb R \longrightarrow \mathbb R$ 定义为

$p(t) = q(tv + w) = \langle v, v\rangle t^2 + 2\langle v, w\rangle t + \langle w, w\rangle$.

若 $\langle v, v\rangle$ 或 $\langle w, w\rangle = 0$，即若 $v$ 或 $w$ 在 $\Lambda$ 中，则由定理 3.2.5，$\langle v, w\rangle = 0$，故 (3.2.1) 成立。现在设 $\langle v, v\rangle, \langle w, w\rangle \ne 0$。根据二次公式，$p(t)$ 有两个（不一定不同）的根

$-2\langle v, w\rangle \pm \sqrt{\langle v, w\rangle^2 - \langle v, v\rangle\langle w, w\rangle} / \langle v, v\rangle$.

根据假设，$\mathbf b$ 是半定的。设 $\mathbf b$ 是正半定的，此时 $p(t) \ge 0$。若 $p(t) > 0$ 对所有 $t$ 在 $\mathbb R$ 中成立，则 $p(t)$ 关于 $t$ 的图像不与 $t$ 轴相交。可知 $p(t)$ 有两个非实复根，这等价于 $\langle v, w\rangle^2 - \langle v, v\rangle\langle w, w\rangle < 0$。于是，(3.2.1) 成立。若对某个 $t_0$ 在 $\mathbb R$ 中有 $p(t_0) = 0$，则 $p(t)$ 关于 $t$ 的图像在单点 $t_0$ 处与 $t$ 轴相交。可知 $t_0$ 是重根，这等价于 $\langle v, w\rangle^2 - \langle v, v\rangle\langle w, w\rangle = 0$。再次，(3.2.1) 成立。这证明了正半定情形。若 $\mathbf b$ 是负半定，则 $-\mathbf b$ 是正半定，我们再次得到 (3.2.1)。

## 3.3 平坦映射与尖锐映射

熟悉张量分析的读者，尤其是在理论物理的背景下，可能对涉及“降指标”和“升指标”的计算有所熟悉。本节和第 4.5 节使用平坦映射和尖锐映射来发展这些思想。

设 $V$ 为向量空间，$\mathbf b$ 为 $V$ 上的双线性函数。对应于 $\mathbf b$ 的平坦映射是线性映射

$F : V \longrightarrow V^*$

**证明。** 对 $V$ 中向量 $w$，定理 1.2.1(d) 给出 $w=\sum_j\theta^j(w)b_j$。则

$$v^F(w)=\langle v,w\rangle=\left\langle\sum_i a^i h_i,\sum_j\theta^j(w)h_j\right\rangle=\sum_{ij}a^i\theta^j(w)\langle h_i,h_j\rangle$$

$$=\sum_{ij}a^i\theta^j(w)b_{ij}=\left(\sum_{ij}b_{ji}a^i\theta^j\right)(w)$$

$$=\left(\sum_i\left[\sum_j b_{ji}a^j\right]\theta^i\right)(w)=\left(\sum_i a_i\theta^i\right)(w).$$

由于 $w$ 是任意的，$v^F=\sum_i a_i\theta^i$。为直接证明简洁版本：

$$[v^F]_\Theta=[F(v)]_\Theta$$
$$=[F]^\Theta_\mathcal{H}[v]_\mathcal{H}\qquad\text{[Th 2.2.4]}$$
$$=b^T_\mathcal{H}[v]_\mathcal{H}.\qquad\text{[Th 3.3.1(b)]}$$

我们从 (3.3.2)–(3.3.4) 看出，将 $v$ 的平坦映射取为 $v^F$ 在经典上被称为通过 $\flat$ **降低指标**。

设 $V$ 为向量空间，$\mathfrak b$ 为 $V$ 上的双线性函数。回顾第 3.1 节，$\mathfrak b$ 在 $V$ 上称为非退化，如果对 $V$ 中所有向量 $v$，$\langle v,w\rangle=0$ 对所有向量 $w$ 在 $V$ 中成立蕴含 $v=0$。

## 定理 3.3.3.
*若 $V$ 为向量空间且 $\mathfrak b$ 为 $V$ 上的双线性函数，则以下条件等价：*

(a) $\mathfrak b$ *在 $V$ 上非退化。*

(b) $V^\perp=\{0\}$。

(c) $F$ *是线性同构。*

(d) *对某个（从而每个）$V$ 的基 $\mathcal{H}$，$b_\mathcal{H}$ 的行（从而列）线性无关。*

(e) $b_\mathcal{H}$ *对某个（从而每个）基 $\mathcal{H}$ 为 $V$ 可逆。*

(f) $\det(b_{ij})\ne0$ *对某个（从而每个）基 $\mathcal{H}$ 为 $V$ 成立。*

**证明。** (a) $\Leftrightarrow$ (b)：显而易见。

(b) $\Leftrightarrow$ (c)：我们有

$$V^\perp=\{v\in V:v^F(w)=0\text{ for all }w\in V\}$$
$$=\{v\in V:v^F=0\}=\ker(F).$$

结果现在由定理 1.1.12、定理 1.1.14 和定理 1.2.1(c) 可得。

(c) $\Leftrightarrow$ (d)：设 $\dim(V)=m$，并设 $\Theta$ 为对应于 $\mathcal{H}$ 的对偶基。则

$$\operatorname{rank}(F)=\operatorname{rank}\left([F]^\Theta_\mathcal{H}\right)\qquad\text{[Th 2.3.1, (2.3.1)]}$$
$$=\operatorname{rank}(b^T_\mathcal{H})\qquad\text{[Th 3.3.1(b)]}$$
$$=\operatorname{rank}(b_\mathcal{H}),\qquad\text{[Th 2.3.2]}$$

证明。 (a)：对 $V$ 中向量 $v$ 和 $w$，我们有

$$\mathrm{F}^*(v)(w) = v(\mathrm{F}(w)) \qquad [(1.3.1)]$$
$$= \mathrm{F}(w)(v) \qquad [(1.2.3)]$$
$$= \langle w, v\rangle = \langle v, w\rangle$$
$$= \mathrm{F}(v)(w). \qquad [(1.2.3)]$$

由于 $v$ 和 $w$ 是任意的，$\mathrm{F}^* = \mathrm{F}$。

(b)：证明与部分 (a) 类似。

## 第 4 章

## 数量积空间

## 4.1 数量积空间

我们现在开始研究一种将占据我们整本书的其余部分的类型向量空间。

设 $V$ 为向量空间，$\mathfrak{g}: V \times V \longrightarrow \mathbb{R}$ 为双线性函数。我们称 $\mathfrak{g}$ 为 $V$ 上的数量积，并称对 $(V,\mathfrak{g})$ 为数量积空间，如果 $\mathfrak{g}$ 在 $V$ 上对称且非退化。回顾第 3.1 节，$\mathfrak{g}$ 在 $V$ 上对称如果对所有向量 $v,w$ 在 $V$ 中 $\langle v,w\rangle = \langle w,v\rangle$；且 $\mathfrak{g}$ 在 $V$ 上非退化如果对所有向量 $v$ 在 $V$ 中，$\langle v,w\rangle = 0$ 对所有向量 $w$ 在 $V$ 中成立蕴含 $v=0$。

设 $(V,\mathfrak{g})$ 确实为数量积空间。由 $\mathfrak{g}$ 的对称性可知，对所有 $V$ 的基 $\mathcal{H}$，$\mathfrak{g}_{\mathcal{H}}$ 是对称矩阵。对应于 $\mathfrak{g}$ 的范数是函数

$$\|\cdot\|: V \longrightarrow \mathbb{R}$$

定义为

$$\|v\| = \sqrt{|\langle v,v\rangle|} = \sqrt{|\mathfrak{g}(v)|} \tag{4.1.1}$$

对 $V$ 中所有向量 $v$ 成立，其中 $\mathfrak{g}$ 为对应于 $\mathfrak{g}$ 的二次函数。在 (4.1.1) 中取绝对值是必要的，因为数量积可能取负值。我们称 $\|v\|$ 为 $v$ 的范数。注意 $\|v\|=0$ 当且仅当 $v=0$ 或 $v$ 为类光；且 $\|v\|=1$ 当且仅当 $v$ 为单位向量。回顾定理 3.3.3，由于 $\mathfrak{g}$ 在 $V$ 上非退化，平坦映射 $F$ 是线性同构，故其逆，尖锐映射 $S$，存在且也是线性同构。

**例 4.1.1 ($\mathbb{R}^m$).** 对整数 $0\leq \nu \leq m$，设

$$\mathfrak{g}: \mathbb{R}^m \times \mathbb{R}^m \longrightarrow \mathbb{R}$$

