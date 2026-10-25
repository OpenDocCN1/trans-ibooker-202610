## 附录 A
### 符号与集合论

在本附录中，我们回顾集合论的一些要素。

设 \(X\) 为一个集合，且 \(S_1\) 和 \(S_2\) 为 \(X\) 的子集。我们说 \(S_1\) 和 \(S_2\) **相交** 或 **重叠** 如果 \(S_1 \cap S_2 \neq \varnothing\)，并且它们 **不相交**、**不重叠** 或 **互不相交** 如果 \(S_1 \cap S_2 = \varnothing\)，其中 \(\varnothing\) 表示 **空集**。\(S_1\) 与 \(S_2\) 的 **差**（按此顺序）为集合

\[
S_1 \setminus S_2 = \{x \in S_1 : x \notin S_2\}.
\]

设 \(\{S_\alpha : \alpha \in A\}\) 为 \(X\) 的子集族，其中 \(A\) 为某个指标集。\(S_\alpha\) 的并集和交集分别记为

\[
\bigcup_{\alpha \in A} S_\alpha \quad \text{and} \quad \bigcap_{\alpha \in A} S_\alpha,
\]

分别。集合 \(X_1, \dots, X_n\) 的 (**笛卡尔**) 积定义为

\[
X_1 \times \cdots \times X_n = \{(x_1, \dots, x_n) : x_i \in X_i \text{ for } i = 1, \dots, n\},
\]

并且每个 \((x_1, \dots, x_n)\) 称为 \(n\)-元组。对于集合 \(X\)，我们用 \(X \times \cdots \times X\) [\(n\) 个拷贝] 表示为 \(X^n\)。

**定理 A.1。** 若 \(X\) 为集合且 \(\{S_\alpha : \alpha \in A\} \cup \{T\}\) 为 \(X\) 的子集族，则

- (a) \[
\left( \bigcup_{\alpha \in A} S_\alpha \right) \cap T = \bigcup_{\alpha \in A} (S_\alpha \cap T).
\]

- (b) \[
\left( \bigcap_{\alpha \in A} S_\alpha \right) \cup T = \bigcap_{\alpha \in A} (S_\alpha \cup T).
\]

- (c) \(X\setminus\left(\bigcup_{\alpha\in A}S_\alpha\right)=\bigcap_{\alpha\in A}(X\setminus S_\alpha).\)

- (d) \(X\setminus\left(\bigcap_{\alpha\in A}S_\alpha\right)=\bigcup_{\alpha\in A}(X\setminus S_\alpha).\)

**定理 A.2。** 若 \(X_i\) 为集合，且 \(S_i\) 和 \(T_i\) 为 \(X_i\) 的子集，\(i=1,\ldots,n\)，则

\[
(S_1\times\cdots\times S_n)\cap(T_1\times\cdots\times T_n)=(S_1\cap T_1)\times\cdots\times(S_n\cap T_n).
\]

设 \(F:X\longrightarrow Y\) 为一个映射。术语“函数”保留给一种特殊的映射类型，如下所述。我们将 \(X\) 和 \(Y\) 分别称为 \(F\) 的 **定义域** 和 **值域**。\(F\) 的 **像** 记为 \(\operatorname{im}(F)\) 或 \(F(X)\) 并定义为

\[
\operatorname{im}(F)=F(X)=\{F(x):x\in X\}.
\]

记号 \(F:X\longrightarrow Y\) 中隐含着 \(F(x)\) 对 \(X\) 中所有 \(x\) 都定义。另一方面，\(F(X)\) 可能是 \(Y\) 的真子集。设 \(T\) 为 \(Y\) 的子集，并定义

\[
F^{-1}(T)=\{x\in X:F(x)\in T\}.
\]

注意我们并不要求 \(T\) 为 \(F(X)\) 的子集。因此，

\[
F^{-1}(T)=F^{-1}(T\cap F(X)).
\]

**定理 A.3。** 设 \(F:X\longrightarrow Y\) 为映射，\(\{S_\alpha:\alpha\in A\}\) 为 \(X\) 的子集族，且 \(\{T_\beta:\beta\in B\}\cup\{T_1,T_2\}\) 为 \(Y\) 的子集族。则：

- (a) \[
F\left(\bigcup_{\alpha\in A}S_\alpha\right)=\bigcup_{\alpha\in A}F(S_\alpha).
\]

- (b) \[
F^{-1}\left(\bigcup_{\beta\in B}T_\beta\right)=\bigcup_{\beta\in B}F^{-1}(T_\beta).
\]

- (c) \[
F^{-1}\left(\bigcap_{\beta\in B}T_\beta\right)=\bigcap_{\beta\in B}F^{-1}(T_\beta).
\]

- (d) \[
F(F^{-1}(T))=T\cap F(X).
\]

- (e) \[
F^{-1}(T_1\setminus T_2)=F^{-1}(T_1)\setminus F^{-1}(T_2).
\]

如果 \(Y\) 中存在元素 \(y_0\) 使得对 \(X\) 中所有 \(x\) 都有 \(F(x)=y_0\)，则称 \(F\) 为 **常值** 或具有 **常值**。这有时表示为 \(F=y_0\)。我们说 \(F\) 为 **单射** 如果 \(X\) 的不同元素映射到 \(Y\) 的不同元素；也就是说，若 \(x_1\) 和 \(x_2\) 为 \(X\) 的元素且 \(F(x_1)=F(x_2)\)，则 \(x_1=x_2\)。我们说 \(F\) 为 **满射** 或 **到上** 如果 \(Y\) 的每个元素都是 \(X\) 中某个元素的像；也就是说，\(F(X)=Y\)。既是单射又是满射的映射称为 **双射**。

\(F:X\longrightarrow Y\) 在 \(X\) 的子集 \(S\) 上的 **限制** 为记为

\[
F|_S:S\longrightarrow Y
\]

的映射，并定义为对 \(S\) 中所有 \(x\) 有 \(F|_S(x)=F(x)\)。有时使用记号 \(F|S\) 而非 \(F|_S\) 是方便的，尤其当 \(S\) 为复杂表达式时。

给定映射 \(G:Y\longrightarrow Z\)，\(G\) 与 \(F\) 的 **复合** 为记为

\[
G\circ F:X\longrightarrow Z
\]

的映射，并定义为

\[
(G\circ F)(x)=G(F(x))
\]

对 \(X\) 中所有 \(x\) 成立。注意我们并不要求 \(F\) 为满射，因此 \(G\circ F(X)\) 可能是 \(G(Y)\) 的真子集。

**恒等映射**

\[
\operatorname{id}_X:X\longrightarrow X
\]

定义为对所有 \(x\in X\) 有 \(\operatorname{id}_X(x)=x\)。

**定理 A.4。** *映射* \(F:X\longrightarrow Y\) *为双射当且仅当存在映射*

\[
F^{-1}:Y\longrightarrow X,
\]

*称为 \(F\) 的* **逆**，*使得* \(F^{-1}\circ F=\operatorname{id}_X\) *且* \(F\circ F^{-1}=\operatorname{id}_Y\)。

设 \(F:X\longrightarrow Y_1\times\cdots\times Y_n\) 为映射，且对每个 \(x\in X\)，我们将 \(F(x)\) 表示为

\[
F(x)=(F_1(x),\ldots,F_n(x)).
\]

这定义了映射 \(F_i:X\longrightarrow Y_i\)，\(i=1,\ldots,n\)，称为 \(F\) 的 **分量映射** 或简单地称为 \(F\) 的 **分量**。我们经常写

\[
F=(F_1,\ldots,F_n).
\]

**定理 A.5。** 设 \(F=(F_1,\ldots,F_n):X\longrightarrow Y_1\times\cdots\times Y_n\) 为映射，且 \(T_i\) 为 \(Y_i\) 的子集，\(i=1,\ldots,n\)。则

\[
F^{-1}(T_1\times\cdots\times T_n)=\bigcap_{i=1}^{n}F_i^{-1}(T_i).
\]

实数集记为 \(\mathbb{R}\)。我们采用以下约定。

**在整个过程中，值域为 \(\mathbb{R}\) 的映射称为函数。**

设 \(f : X \longrightarrow \mathbb{R}\) 为函数，且 \(x\) 为 \(X\) 的元素。我们说 \(f\) 在 \(x\) 处 **消失** 如果 \(f(x)=0\)，在 \(x\) 处 **非消失** 如果 \(f(x)\neq 0\)，并且在 \(X\) 上 **处处非消失** 如果它在 \(X\) 中每个 \(x\) 处都非消失。我们说 \(f\) 为 **严格正** 如果对所有 \(x\in X\) 有 \(f(x)>0\)，且为 **严格负** 如果对所有 \(x\in X\) 有 \(f(x)<0\)。函数 \(f : \mathbb{R} \longrightarrow \mathbb{R}\) 称为 **递增** 如果对所有 \(x_1 < x_2\) 有 \(f(x_1)\leq f(x_2)\)，且为 **严格递增** 如果对所有 \(x_1 < x_2\) 有 \(f(x_1) < f(x_2)\)。递减和严格递减的类似定义也给出。

给定实数 \(c\)，在 \(X\) 上取值为 \(c\) 的常值函数，记为 \(c_X\) 或简单地记为 \(c\)，为将 \(X\) 的所有元素映射到 \(c\) 的函数。特别地，我们有 \(0_X\)，称为零函数，以及 \(1_X\)。

集合 \(X\) 的 **基数** 记为 \(\operatorname{card}(X)\) 并定义为集合中元素的个数，可能是有限的也可能是无限的。两个集合被称为具有相同基数如果它们之间存在双射。已知存在不同类型的“无穷”。例如，我们说集合是 **可数** 的如果它与正整数具有相同的基数。可以证明有理数是可数的，但实数不是。

集合 \(X\) 上的二元关系是 \(X \times X\) 的子集 \(\sim\)。如果 \((x_1,x_2)\) 在 \(\sim\) 中，则称 \(x_1\) 与 \(x_2\) 相关，记为 \(x_1 \sim x_2\)。我们说 \(\sim\) 为 \(X\) 上的等价关系如果：

- [E1] 对所有 \(x\in X\) 有 \(x \sim x\)。 (自反性)
- [E2] 若 \(x_1 \sim x_2\)，则对所有 \(x_1,x_2\in X\) 有 \(x_2 \sim x_1\)。 (对称性)
- [E3] 若 \(x_1 \sim x_2\) 且 \(x_2 \sim x_3\)，则对所有 \(x_1,x_2,x_3\in X\) 有 \(x_1 \sim x_3\)。 (传递性)

如果 \(\sim\) 为 \(X\) 上的等价关系且 \(x\) 为 \(X\) 的元素，则由 \(x\) 确定的等价类记为 \([x]\) 并定义为与 \(x\) 相关的 \(X\) 的元素集合：

\[
[x] = \{ y \in X : y \sim x \}.
\]

特别地，由 [E1] 可知 \([x]\) 包含 \(x\)。

\(X\) 的一个划分是 \(X\) 的一组不相交非空子集，其并集等于 \(X\)。

**定理 A.6。** 给定集合 \(X\) 上的等价关系，不同等价类的集合构成 \(X\) 的一个划分。

非零实数 \(x\) 的 **符号** 定义为

\[
\operatorname{sgn}(x) = \dfrac{x}{|x|}.
\]

\(0\) 的符号未定义。

克罗内克 delta 为函数

\[
\delta : \{1,\dots,n\} \times \{1,\dots,n\} \longrightarrow \{0,1\}
\]



由

$$\delta(i,j)=\begin{cases}1&\text{if }i=j\\0&\text{if }i\ne j.\end{cases}$$

定义。我们此后用等价的表达式 $\delta_{ij}$ 或 $\delta^i_j$ 表示 $\delta(i,j)$，选择取决于上下文。

对于整数 $1\le k\le n$，我们称 $\{1,\ldots,n\}$ 的 $k$ 元组 $(i_1,\ldots,i_k)$ 由不同的元素组成，如果 $i_1<\cdots<i_k$，则为 **多指标**。$\{1,\ldots,n\}$ 上具有 $k$ 个元素的多指标集合记为 $\mathcal I_{k,n}$，而 $\mathcal I_{k,n}$ 中的典型多指标 $I$ 记为 $I=(i_1,\ldots,i_k)$。例如，

$$\mathcal I_{3,4}=\{(1,2,3),(1,2,4),(2,3,4)\}.$$

显然，$\mathcal I_{k,n}$ 有 $\binom{n}{k}$ 个元素，其中 $\binom{n}{k}$ 为二项系数。$\mathcal I_{k,n}$ 中多指标 $I=(i_1,\ldots,i_k)$ 的 **补集** 是 $\mathcal I_{n-k,n}$ 中的多指标 $I^c=(i_{k+1},\ldots,i_n)$，其中

$$\{i_{k+1},\ldots,i_n\}=\{1,\ldots,n\}\setminus\{i_1,\ldots,i_k\}.$$

特别地，$\mathcal I_{1,n}$ 中多指标 $(i)$ 的补集是 $\mathcal I_{n-1,n}$ 中的多指标

$$(i)^c=(1,\ldots,\widehat{i},\ldots,n)=(1,\ldots,i-1,i+1,\ldots,n)$$

其中 $\widehat{\ }$ 表示省略一个表达式，对 $i=1,\ldots,n$。举例说明，对于 $\mathcal I_{1,6}$ 中的 $(2)$ 和 $\mathcal I_{4,6}$ 中的 $(1,2,3,6)$，我们有补集 $(2)^c=(1,3,4,5,6)$ 在 $\mathcal I_{5,6}$ 中，以及 $(1,2,3,6)^c=(4,5)$ 在 $\mathcal I_{2,6}$ 中。
