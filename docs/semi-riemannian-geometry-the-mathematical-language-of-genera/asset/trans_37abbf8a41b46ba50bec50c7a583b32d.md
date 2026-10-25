
证明。设 $(e_1,\dots,e_m)$ 是 $V$ 的一个关于 $\mathfrak{O}$ 为正定向的标准正交基，并设 $(\xi^1,\dots,\xi^m)$ 是它的对偶基。由于 $(e_1,\dots,e_m)$ 和 $\mathcal{H}$ 都是正定向的，所以它们是相容的。结果由定理 8.3.1(b) 和定理 8.3.4 得出。

设 $(V,\mathfrak{g},\mathfrak{O})$ 是一个维数 $m \ge 2$ 的定向标量积空间，并设 $\Omega_V$ 是其体积多重余向量。由定理 8.3.4，$\Omega_V$ 诱导出 $\mathfrak{O}$。设 $U$ 是 $V$ 的一个维数为 $m-1$ 的子空间，且 $\mathfrak{g}$ 在 $U$ 上非退化。由定理 4.1.2(b) 得 $U^\perp$ 是 $V$ 的 1 维子空间，由定理 4.1.3 得 $V = U \oplus U^\perp$。设 $u$ 是 $U^\perp$ 中的单位向量，则 $\mathbb{R}u = U^\perp$，从而 $V = U \oplus \mathbb{R}u$。因为 $\mathfrak{g}$ 在 $U$ 上非退化，所以 $\mathfrak{g}|_U$ 是 $U$ 上的一个标量积。由定理 8.2.5 的 (a) 和 (b) 部分，$i_u(\Omega_V)|_U$ 是 $U$ 上的一个定向多重余向量，它诱导出 $U$ 的某个定向，记为 $\mathfrak{O}_U$。因此，$(U,\mathfrak{g}|_U,\mathfrak{O}_U)$ 是维数 $m-1$ 的定向标量积空间。设 $\Omega_U$ 是其体积多重余向量。问题在于：定向多重余向量 $i_u(\Omega_V)|_U$ 与 $\Omega_U$ 是否相关？如下一个结果所示，它们是同一个对象。

## 定理 8.3.6。在上述设定下：

(a)
$i_u(\Omega_V)|_U = \Omega_U.$

(b) 更一般地，若 $v$ 是 $V$ 中的向量，$\epsilon$ 是 $\langle u,u\rangle$ 的符号，则

$i_v(\Omega_V)|_U = \epsilon\langle u,v\rangle\Omega_U.$

证明。(a)：设 $(e_1,\dots,e_{m-1})$ 是 $U$ 的一个关于 $\mathfrak{O}_U$ 为正定向的标准正交基。由定理 8.2.5(a) 得，$(u,e_1,\dots,e_{m-1})$ 是 $V$ 的一个关于 $\mathfrak{O}$ 为正定向的标准正交基，再由定理 8.3.4（应用于 $V$）得 $\Omega_V(u,e_1,\dots,e_{m-1}) = 1$。根据定义，

$i_u(\Omega_V)|_U(e_1,\dots,e_{m-1}) = \Omega_V(u,e_1,\dots,e_{m-1})$，

因此 $i_u(\Omega_V)|_U(e_1,\dots,e_{m-1}) = 1$。现在，由定理 8.3.4（应用于 $U$）给出的 $\Omega_U$ 的唯一性，即可得结果。

(b)：因为 $V = U \oplus \mathbb{R}u$，所以 $v = (v - \bar{v}) + \bar{v}$，其中 $v - \bar{v}$ 和 $\bar{v}$ 分别是 $U$ 和 $\mathbb{R}u$ 中的（唯一）向量。由定理 7.4.1(a)，

$i_v(\Omega_V)|_U = i_{v - \bar{v}}(\Omega_V)|_U + i_{\bar{v}}(\Omega_V)|_U. \quad (8.3.1)$

由于 $w_1,\ldots,w_{m-1}$ 是任意的，

$$i_{v-\bar v}(\Omega_V)|_U=0.$$

对于第二项，由定理 4.1.5 得

$$\bar v=\mathcal P_{\mathop{\rm Rm}(U)}(v)=\epsilon\langle v,u\rangle u,$$

再由定理 7.4.1(b) 和 (a) 部分得

$$i_{\bar v}(\Omega_V)|_U=\epsilon\langle v,u\rangle i_u(\Omega_V)|_U=\epsilon\langle v,u\rangle\Omega_U.$$

将 (8.3.2) 和 (8.3.3) 代入 (8.3.1)，即得结果。

## 8.4 向量积

本节我们将 $\mathbb R^3$ 中众所周知的向量积推广到任意标量积空间。

设 $(V,g)$ 是符号差为 $(\epsilon_1,\ldots,\epsilon_m)$ 的标量积空间，$\mathcal E=(e_1,\ldots,e_m)$ 是 $V$ 的一个标准正交基，$v_1,\ldots,v_{m-1}$ 是 $V$ 中的向量。$v_1,\ldots,v_{m-1}$（按该顺序）关于 $\mathcal E$ 的**向量积**（或叉积）定义为

$$v_1\times\cdots\times v_{m-1}=\sum_{i=1}^{m}\epsilon_i(-1)^{i-1}\det\left(\left[\, [v_1]_{\mathcal E}\ \cdots\ [v_{m-1}]_{\mathcal E}\,\right]^{(i)c}\right)e_i.$$

其中 $(i)^c$ 是多重指标

$$(i)^c=(1,\ldots,\hat{i},\ldots,m).$$

显然，给定向量的向量积取决于它们被取出的顺序以及标准正交基的选取。

> 在本章中，$V$ 上的向量积 $\times$ 是关于 $V$ 的给定标准正交基 $\mathcal E$ 计算的。

> 在全文，$\mathbb R^m_\nu$ 上的向量积 $\times$ 是关于 $\mathbb R^m_\nu$ 的标准基计算的。

我们可以如下得到 (8.4.1) 的一个计算上方便的替代形式。令

$$v_j=\sum_i a_j^i e_i$$

其中 $j=1,\ldots,m-1$，于是由 (2.2.3)，

$$\left[\,[v_1]_{\mathcal E}\ \cdots\ [v_{m-1}]_{\mathcal E}\,\right]=\begin{bmatrix}a_1^1&\cdots&a_{m-1}^1\\ \vdots&\ddots&\vdots\\ a_1^m&\cdots&a_{m-1}^m\end{bmatrix}.$$

则 (8.4.1) 可以表示为如下形式恒等式

v₁ × ⋯ × vₘ₋₁ = det( ⎛⎜⎝ ε₁e₁ a¹₁ ⋯ a¹ₘ₋₁ ⋮ ⋮ ⋱ ⋮ εₘeₘ aᵐ₁ ⋯ aᵐₘ₋₁ ⎞⎟⎠ ), (8.4.3)

只要我们将这个“行列式”沿第一列展开。

当 $m = 2$ 时，即使 (8.4.1) 和 (8.4.3) 左端的记号简化为单个向量，右端仍然可以计算。设 $v = a^1e_1 + a^2e_2$。则 $v$ 的“向量积”为

det( ⎛⎜⎝ ε₁e₁ a¹ ε₂e₂ a² ⎞⎟⎠ ) = ε₁a²e₁ − ε₂a¹e₂.

在上下文有意义的情况下，这个量与 $m \ge 3$ 时的向量积具有相同的性质。

### 例 8.4.1（ℝ₀² 和 ℝ₀³ 上的向量积）。

设 (e₁, e₂) 是 ℝ₀² 的标准基，并设 (a¹, a²) 是 ℝ₀² 中的向量。则 (a¹, a²) 的“向量积”为 (a², −a¹)。

再设 (e₁, e₂, e₃) 是 ℝ₀³ 的标准基，并设 v = (a¹, a², a³) 和 w = (b¹, b², b³) 是 ℝ₀³ 中的向量。则

[v]ℰ = ⎡a¹⎤   以及   [w]ℰ = ⎡b¹⎤
          ⎢a²⎥             ⎢b²⎥
          ⎣a³⎦             ⎣b³⎦

因此

v × w = det( ⎛⎜⎝ e₁ a¹ b¹ e₂ a² b² e₃ a³ b³ ⎞⎟⎠ ) = (det(⎡a² b²⎤), −det(⎡a¹ b¹⎤), det(⎡a¹ b¹⎤)) = (a²b³ − a³b², a³b¹ − a¹b³, a¹b² − a²b¹).

特别地，

e₁ × e₂ = e₃,      e₁ × e₃ = −e₂,      以及      e₂ × e₃ = e₁.

### 例 8.4.2（ℝ₁² 和 ℝ₁³ 上的向量积）。

设 (e₁, e₂) 是 ℝ₁² 的标准基，并设 (a¹, a²) 是 ℝ₁² 中的向量。则 (a¹, a²) 的“向量积”为 (a², a¹)。

再设 (e₁, e₂, e₃) 是 ℝ₁³ 的标准基，并设 v = (a¹, a², a³) 和 w = (b¹, b², b³) 是 ℝ₁³ 中的向量。则

[v]ℰ = ⎡a¹⎤   以及   [w]ℰ = ⎡b¹⎤
          ⎢a²⎥             ⎢b²⎥
          ⎣a³⎦             ⎣b³⎦

因此

v × w = det( ⎛⎜⎝ e1 a1 b1 e2 a2 b2 −e3 a3 b3 ⎞⎟⎠ ) = ( det( [a2 b2; a3 b3] ), −det( [a1 b1; a3 b3] ), −det( [a1 b1; a2 b2] ) ) = (a2 b3 − a3 b2, a3 b1 − a1 b3, a2 b1 − a1 b2).

特别地，

e1 × e2 = −e3,      e1 × e3 = −e2,      以及      e2 × e3 = e1.

向量积具有许多有趣的代数性质，其中若干（不足为奇地）用行列式和楔积表示。

## 定理 8.4.3。设 (V,g) 是维数 m ≥ 2 的标量积空间，ℰ 是 V 的一个标准正交基，v1,...,vm−1,w 是 V 中的向量，c 是实数。则对 i,j = 1,...,m−1：

- (a) ⟨vi, v1 × ⋯ × vm−1⟩ = 0.
- (b) v1 × ⋯ × c vi × ⋯ × vm−1 = c(v1 × ⋯ × vi × ⋯ × vm−1).
- (c) v1 × ⋯ × (c vi + w) × ⋯ × vm−1 = c(v1 × ⋯ × vi × ⋯ × vm−1) + v1 × ⋯ × w × ⋯ × vm−1.
- (d) v1 × ⋯ × vi × ⋯ × vj × ⋯ × vm−1 = −(v1 × ⋯ × vj × ⋯ × vi × ⋯ × vm−1).
- (e) 若 vi = vj，则 v1 × ⋯ × vi × ⋯ × vj × ⋯ × vm−1 = 0.

证明。(a)：由 (8.4.1) 和 (8.4.2)，

⟨vi, v1 × ⋯ × vm−1⟩ = ⟨∑j ai j ej, ∑k εk (−1)k−1 det( [ [v1]ℰ … [vi]ℰ … [vm−1]ℰ ]^(k)c ) ek ⟩ = ∑jk ai j ⟨ej, ek⟩ εk (−1)k−1 det( [ [v1]ℰ … [vi]ℰ … [vm−1]ℰ ]^(k)c ) = ∑j (−1)j−1 ai j det( [ [v1]ℰ … [vi]ℰ … [vm−1]ℰ ]^(j)c ) = det( [ [vi]ℰ [v1]ℰ … [vi]ℰ … ] ).

由于 [[v_i]_E  [v_1]_E … [v_i]_E … [v_{m-1}]_E] 的（至少）两列相等，由定理 2.4.2 可知其行列式等于 0。

(b)–(d)：直接可证。

根据定理 8.4.3(b)，我们去掉括号，并将 $c(v_1 \times \cdots \times v_i \times \cdots \times v_{m-1})$ 记作 $c v_1 \times \cdots \times v_i \times \cdots \times v_{m-1}$。

下一个结果是考虑向量积的主要原因。

## 定理 8.4.4。设 (V,g) 是维数 m ≥ 2 的标量积空间，E 是 V 的一个标准正交基，v_1,…,v_{m-1} 是 V 中的向量，U 是由 v_1,…,v_{m-1} 张成的 V 的子空间。则 v_1 × ⋯ × v_{m-1} 是 U^⊥ 中的向量。

证明。这可由定理 8.4.3(a) 得出。

乍一看，定理 8.4.4 似乎承诺得比它实际给出的更多。为了使结果真正有意义，我们需要 v_1 × ⋯ × v_{m-1} 是非零向量，且 U^⊥ 是 V 的 1 维子空间。这些条件可以通过进一步的假设来满足：参见定理 8.4.8 和定理 8.4.10(a)。

前面曾指出，向量积取决于取用向量的顺序以及标准正交基的选取。根据定理 8.4.3(d)，改变向量的顺序最多影响所得向量积的符号。鉴于 (8.4.1)，人们可能预期，相对于不同的标准正交基进行计算会对结果产生重大影响。正如定理 8.4.4 所示，情况不一定如此：无论选择何种标准正交基，向量积总是在组成它的向量所张成的子空间的正交补中。事实上，后面的一些结果也具有这一特点：参见定理 8.4.5、定理 8.4.6、定理 8.4.8 和定理 8.4.9。

## 定理 8.4.5。设 (V,g) 是维数 m ≥ 2 且指标为 ν 的标量积空间。设 E 是 V 的一个标准正交基，$(\xi^1,\ldots,\xi^m)$ 是它的对偶基，令 u, v_1,\ldots,v_{m-1} 是 V 中的向量。则：
