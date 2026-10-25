## 第 3 章

## 双线性函数

内积是读者可能熟悉的双线性函数的例子。在本书中，我们更感兴趣的是称为标量积的、限制较少的双线性函数类型，但我们不会完全忽略内积。为了判断一个双线性函数是否为标量积，我们需要确定它是否对称且非退化。双线性函数的这些性质将是本章的重点。

### 3.1 双线性函数

设 $V$ 是一个向量空间。函数

$$\mathfrak{b}: V \times V \longrightarrow \mathbb{R}$$

被称为 **双线性（在 $V$ 上）**，如果它在两个变量上都是线性的；也就是说，

$$\mathfrak{b}(cu+v,w)=c\mathfrak{b}(u,w)+\mathfrak{b}(v,w)$$

并且

$$\mathfrak{b}(u,cv+w)=c\mathfrak{b}(u,v)+\mathfrak{b}(u,w)$$

对所有 $V$ 中的向量 $u,v,w$ 和所有实数 $c$ 成立。在文献中，双线性函数有时被称为 **二次型**。我们通常用

$$\mathfrak{b}(\cdot,\cdot)\quad\text{by}\quad\langle\cdot,\cdot\rangle,$$

记作 $\langle v,w\rangle$ 代替 $\mathfrak{b}(v,w)$。

我们说 b 是：

- **对称** 的，如果 $\langle v, w\rangle = \langle w, v\rangle$ 对所有 $v, w$ 在 $V$ 中成立。
- **交错** 的，如果 $\langle v, w\rangle = -\langle w, v\rangle$ 对所有 $v, w$ 在 $V$ 中成立。
- **非退化** 的，如果对所有 $v$ 在 $V$ 中，$\langle v, w\rangle = 0$ 对所有 $w$ 在 $V$ 成立蕴含 $v = 0$。
- **退化** 的，如果 b 不是非退化的。
- **正定** 的，如果对所有非零 $v$ 在 $V$ 中 $\langle v, v\rangle > 0$。
- **负定** 的，如果对所有非零 $v$ 在 $V$ 中 $\langle v, v\rangle < 0$。
- **定** 的，如果 b 是正定或负定。
- **不定** 的，如果 b 不是定的。
- **正半定** 的，如果对所有 $v$ 在 $V$ 中 $\langle v, v\rangle \ge 0$。
- **负半定** 的，如果对所有 $v$ 在 $V$ 中 $\langle v, v\rangle \le 0$。
- **半定** 的，如果 b 是正半定或负半定。

设 $U$ 是 $V$ 的子空间。为简便起见，我们用 $b|_{U\times U}$ 表示限制

$b|_{U\times U}$ 由 $b|_U$ 表示。

事实上，我们通常完全省略 “$|_U$” 的记号，例如说 b 在 $U$ 上对称，如果 $b|_U$ 对称。显然，b 在 $U$ 上是双线性的，并且如果 b 在 $V$ 上对称（分别地，交错、定、半定），则 b 在 $U$ 上对称（分别地，交错、定、半定）。非退化的情况更为复杂。如果 b 在 $V$ 上非退化，则不一定推出 b 在 $U$ 上非退化。我们稍后会探讨这一现象。

### 例 3.1.1. 下表给出了 $\mathbb{R}^3$ 上双线性函数的例子。注意双线性函数可以是对称的也可以不是，与此独立，可以是非退化的也可以不是。

| $\langle(x^1, x^2, x^3), (y^1, y^2, y^3)\rangle$ | 对称 | 定 | 半定 | 非退化 |
|---|---|---|---|---|
| $x^1y^1 + x^2y^2 + x^3y^3$ | yes | yes | yes | yes |
| $x^1y^1 + x^2y^2$ | yes | no | yes | no |
| $x^1y^1 + x^2y^2 - x^3y^3$ | yes | no | no | yes |
| $x^1y^1 - x^3y^3$ | yes | no | no | no |
| $x^1y^1 + x^2y^3 - x^3y^2$ | no | no | yes | yes |
| $x^1y^1 + x^2y^3$ | no | no | no | no |

借用狭义相对论的术语，我们说（相对于 b）$V$ 中的向量 $v$ 是：

- **类空** 的，如果 $v = 0$ 或 $\langle v, v\rangle > 0$。
- **类时** 的，如果 $\langle v, v\rangle < 0$。
- **类光** 的，如果 $v \neq 0$ 且 $\langle v, v\rangle = 0$。

零向量通常被认为是类空的（这里也是如此），但在某些文献中它被认为是类光的。显然，每个向量都是类空、类时或类光的，这导致了 $V$ 的相应划分。（本书在不同地方对“划分”一词有几种标准但不同的用法。这里所指的定义见附录 A。）

设 b 是向量空间 $V$ 上的双线性函数，且 $H = (h_1, \dots, h_m)$ 是 $V$ 的一组基。b 相对于 $H$ 的矩阵记为 $b_H$ 并定义为

```
b_H = [ b_{11} … b_{1m} ; ⋮ ⋱ ⋮ ; b_{m1} … b_{mm} ]
```

其中

```
b_{ij} = ⟨h_i, h_j⟩
```

对 $i, j = 1, …, m$。

接下来的两个结果说明了矩阵表示如何有助于涉及双线性函数的计算。

## 定理 3.1.2. 设 $V$ 是一个维数为 $m$ 的向量空间，b 是 $V$ 上的双线性函数，且 $H$ 是 $V$ 的一组基。则：

- (a) 对所有 $V$ 中的向量 $v, w$，
  $\langle v, w\rangle = ([v]_H)^T b_H [w]_H$。
- (b) 对所有整数 $1 \le k \le m$ 和所有向量 $v_1, …, v_k, w_1, …, w_k$ 在 $V$ 中，
  $\begin{bmatrix} \langle v_1,w_1\rangle & \dots & \langle v_1,w_k\rangle \\ \vdots & \ddots & \vdots \\ \langle v_k,w_1\rangle & \dots & \langle v_k,w_k\rangle \end{bmatrix} = [ [v_1]_H \dots [v_k]_H ]^T b_H [ [w_1]_H \dots [w_k]_H ]$。

证明. (a): 设 $H = (h_1, …, h_m)$，且 $v = \sum_i a^i h_i$ 且 $w = \sum_j b^j h_j$。则

```
([v]_H)^T = [a^1 … a^m] and [w]_H = [b^1 … b^m]^T,
```

故

```
⟨v, w⟩ = ⟨ Σ_i a^i h_i, Σ_j b^j h_j ⟩ = Σ_{ij} a^i b^j ⟨h_i, h_j⟩ = Σ_{ij} a^i b^j b_{ij} = ([v]_H)^T b_H [w]_H.
```

(b): 它由定理 2.1.5(b) 和部分 (a) 推出

$[[v_1]_H ... [v_k]_H]^T b_H [[w_1]_H ... [w_k]_H] = [([v_i]_H)^T b_H [w_j]_H] = [\langle v_i, w_j\rangle]$

## 定理 3.1.3（基变换）。设 $V$ 是一个向量空间，b 是 $V$ 上的双线性函数，且 $H$ 和 $F$ 是 $V$ 的基。则

$b_F = ([id_V]_F^H)^T b_H [id_V]_F^H$。

证明. 我们有

$([id_V]_F^H)^T b_H [id_V]_F^H = [[f_1]_H ... [f_m]_H]^T b_H [[f_1]_H ... [f_m]_H]$ [(2.2.7)] $= [\langle f_i, f_j\rangle]$ [Th 3.1.2(b)] $= b_F$。

我们以几个稍后有用的定义结束本节。设 $V$ 是一个向量空间，且 b 是 $V$ 上的双线性函数。对应于 b 的二次函数是函数

$q : V \longrightarrow \mathbb{R}$

定义为

$q(v) = \langle v, v\rangle$

对所有 $V$ 中的向量 $v$。容易证明

$q(v + w) - q(v) - q(w) = b(v, w) + b(w, v)$ (3.1.1)

对所有 $V$ 中的向量 $v, w$。

若干先前的定义可以用 $q$ 表示。具体地，b 是：

- 正定的，如果 $q(v) > 0$ 对所有非零 $v$ 在 $V$ 中。
- 负定的，如果 $q(v) < 0$ 对所有非零 $v$ 在 $V$ 中。
- 正半定的，如果 $q(v) \ge 0$ 对所有 $v$ 在 $V$ 中。
- 负半定的，如果 $q(v) \le 0$ 对所有 $v$ 在 $V$ 中。
- 类空的，如果 $v = 0$ 或 $q(v) > 0$。
- 类时的，如果 $q(v) < 0$。
- 类光的，如果 $v \neq 0$ 且 $q(v) = 0$。

我们说 $v$ 是单位向量，如果 $\langle v, v\rangle = \pm 1$，并且 $v$ 与 $w$ 正交，如果 $\langle v, w\rangle = 0$。特别地，零向量与 $V$ 中的每个向量正交，

并且类光向量与自身正交。除非 **b** 对称，$\langle w,v\rangle$ 可能不等于 $\langle v,w\rangle$。因此，即使 $v$ 与 $w$ 正交，也不一定推出 $w$ 与 $v$ 正交。

设 $U$ 是 $V$ 的子空间。**$U$ 的 perp（在 $V$ 中相对于 $\mathbf b$）** 记为 $U^\perp$ 并定义为与 $U$ 中每个向量正交（垂直）的 $V$ 中向量的集合：

$U^\perp = \{v \in V : \langle v, u\rangle = 0 \text{ for all } u \in U\}.$

容易证明 $U^\perp$ 是 $V$ 的子空间。设我们记

$(U^\perp)^\perp$ 为 $U^{\perp\perp}$。

$V$ 的 **光集** 记为 $\Lambda$ 并定义为 $V$ 中类光向量的集合：

$\Lambda = \{v \in V : \langle v, v\rangle = 0\} \setminus \{0\}.$

### 3.2 对称双线性函数

设 $V$ 是一个向量空间，且 **b** 是 $V$ 上的双线性函数。回顾第 3.1 节，**b** 在 $V$ 上被称为对称，如果 $\langle v,w\rangle = \langle w,v\rangle$ 对所有向量 $v,w$ 在 $V$ 中。

## 定理 3.2.1. 若 $V$ 是一个向量空间且 **b** 是 $V$ 上的双线性函数，则以下等价：

(a) **b** 是对称的（作为双线性函数）。

(b) $\mathbf b_{\mathcal H}$ 是对称的（作为矩阵）对某个 $V$ 的基 $\mathcal H$。

(c) $\mathbf b_{\mathcal H}$ 是对称的（作为矩阵）对每个 $V$ 的基 $\mathcal H$。

**证明.** (a) ⇒ (c): 显然。

(c) ⇒ (b): 显然。

(b) ⇒ (a): 我们有

\[
\begin{aligned}
\langle v,w\rangle &= ([v]_{\mathcal H})^{\mathsf T}\mathbf b_{\mathcal H}[w]_{\mathcal H} && [\text{Th 3.1.2(a)}]\\
&=([v]_{\mathcal H})^{\mathsf T}\mathbf b_{\mathcal H}^{\mathsf T}[w]_{\mathcal H} &&\\
&=(([w]_{\mathcal H})^{\mathsf T}\mathbf b_{\mathcal H}[v]_{\mathcal H})^{\mathsf T} && [\text{Th 2.1.2(b)}]\\
&=\langle w,v\rangle^{\mathsf T} && [\text{Th 3.1.2(a)}]\\
&=\langle w,v\rangle.
\end{aligned}
\]

## 定理 3.2.2. 设 $V$ 是一个向量空间，**b** 是 $V$ 上的双线性函数，且 $q$ 是相应的二次函数。则 **b** 对称当且仅当

\[
\mathbf b(v,w)=\frac{q(v+w)-q(v)-q(w)}{2}
\]

（极化恒等式）

对所有向量 $v,w$ 在 $V$ 中。





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
