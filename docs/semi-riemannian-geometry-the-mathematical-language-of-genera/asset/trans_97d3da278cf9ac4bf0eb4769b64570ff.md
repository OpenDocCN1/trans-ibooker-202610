

另一方面，若 \(\mathcal{T}^+ \subseteq A(\mathcal{T}^-)\)，则定理4.9.3(b)给出 \(\mathcal{T}^+ \subseteq -A(\mathcal{T}^+)\)，从而 \(\mathcal{T}^- = -\mathcal{T}^+ \subseteq A(\mathcal{T}^+) \subseteq \mathcal{T}^+\)，矛盾。因此，\(A(\mathcal{T}^+) = \mathcal{T}^+\)。现在假设 \(A(\mathcal{T}^+) \subseteq \mathcal{T}^-\)。类似于刚才的论证表明 \(A(\mathcal{T}^+) = \mathcal{T}^-\)。

令 \(v, w\) 为 \(\mathcal{T}^-\) 中的向量。类似于刚才的论证表明，要么 \(A(\mathcal{T}^-) = \mathcal{T}^-\)，要么 \(A(\mathcal{T}^-) = \mathcal{T}^+\)。

设 \((V, \mathfrak{g})\) 为洛伦兹向量空间，并设 \((e_1,\dots,e_m)\) 为 \(V\) 的一组标准正交基。由于 \(e_m\) 是类时的，它必属于 \(\mathcal{T}^+\) 或 \(\mathcal{T}^-\)。\(V\) 中时间锥的标记是任意的，因此我们可以自由采用以下约定。

**给定洛伦兹向量空间中的一组标准正交基 \((e_1,\dots,e_m)\)，\(\mathcal{T}^+\) 是包含 \(e_m\) 的时间锥。**

因此，
\[
\mathcal{T}^+ = \{v \in \mathcal{T} : \langle v, e_m\rangle < 0\} \tag{4.9.8}
\]
且
\[
\Lambda^+ = \{u \in \Lambda : \langle u, e_m\rangle < 0\},
\]
这里我们注意到 \(e_m\) 属于 \(\mathcal{T}^+\)。需要强调的是，\(\mathcal{T}^+\) 和 \(\Lambda^+\) 依赖于标准正交基的选择。特别地，如果我们决定使用标准正交基 \((e_1,\dots,-e_m)\) 而不是 \((e_1,\dots,e_m)\)，那么“未来”和“过去”就互换了。

**定理4.9.7.** 在上述设定下，设 \(v\) 为 \(\mathcal{T}\) 中的向量，且 \(v = \sum_i a^i e_i\)。则 \(v\) 属于 \(\mathcal{T}^+\) 当且仅当 \(a^m > 0\)。

*证明.* 我们有
\[
\langle v, e_m\rangle = \left\langle \sum_i a^i e_i, e_m\right\rangle = \langle e_m, e_m\rangle a^m = -a^m,
\]
由此结论成立。

继续前面的例子，我们选择 \(\mathbb{R}^3_1\) 的标准基 \((e_1,e_2,e_3)\)。根据上述约定，\(e_3 = (0,0,1)\) 是一个未来方向向量，因此 \(\mathcal{T}_3^+ = \{(x,y,z) \in \mathcal{T}_3 : z > 0\}\) 且 \(\Lambda_3^+ = \{(x,y,z) \in \Lambda_3 : z > 0\}\)。

我们以或许最能说明洛伦兹向量空间几何与内积空间几何之间差异的例子来结束本节。我们在与定理4.6.5相关的讨论中观察到，在内积空间中，斜穿矩形场地比绕角走更短。下一个结果表明，相比之下，在洛伦兹向量空间中，当矩形场地由等价的类时向量确定时，情况正好相反。

## 第五章 向量空间上的张量

### 5.1 张量

设 \(V\) 为向量空间，并设 \(r, s \ge 1\) 为整数。按照B.5节和B.6节，我们用 \(\operatorname{Mult}(V^{*r} \times V^s, \mathbb{R})\) 表示从 \(V^{*r} \times V^s\) 到 \(\mathbb{R}\) 的 \(\mathbb{R}\)-多重线性函数构成的向量空间，其中加法和标量乘法定义如下：对所有函数 \(\mathcal{A}, \mathcal{B}\)（属于 \(\operatorname{Mult}(V^{*r} \times V^s, \mathbb{R})\)）及所有实数 \(c\)，
\[
(\mathcal{A}+\mathcal{B})(\eta^1,\dots,\eta^r,v_1,\dots,v_s)
= \mathcal{A}(\eta^1,\dots,\eta^r,v_1,\dots,v_s) + \mathcal{B}(\eta^1,\dots,\eta^r,v_1,\dots,v_s)
\]
且
\[
(c\mathcal{A})(\eta^1,\dots,\eta^r,v_1,\dots,v_s)
= c\,\mathcal{A}(\eta^1,\dots,\eta^r,v_1,\dots,v_s)
\]
对所有余向量 \(\eta^1,\dots,\eta^r \in V^*\) 及所有向量 \(v_1,\dots,v_s \in V\) 成立。为简洁起见，我们将 \(\operatorname{Mult}(V^{*r} \times V^s, \mathbb{R})\) 记为 \(\mathcal{T}_s^r(V)\)。

我们将 \(\mathcal{T}_s^r(V)\) 中的函数 \(\mathcal{A}\) 称为一个 \((r,s)\)-张量，一个 \(r\)-逆变-\(s\)-协变张量，或简称为 \(V\) 上的张量，并将 \(\mathcal{A}\) 的秩定义为 \((r,s)\)。当 \(s = 0\) 时，\(\mathcal{A}\) 被称为 \(r\)-逆变张量；当 \(r = 0\) 时，\(\mathcal{A}\) 被称为 \(s\)-协变张量。在1.2节中，我们使用术语“余向量”来描述现在所称的 \(1\)-协变张量。注意，张量的加法仅在它们具有相同秩时才有定义。\(\mathcal{T}_s^r(V)\) 中的零元素称为零张量，记为 \(0\)，它是将所有 \(V^{*r} \times V^s\) 中的元素映到实数 \(0\) 的张量。若 \(\mathcal{T}_s^r(V)\) 中的张量不是零张量，则称其为非零张量。我们注意到
\[
\mathcal{T}_1^0(V) = V^* \quad\text{且}\quad \mathcal{T}_0^1(V) = V^{**} = V. \tag{5.1.1}
\]

为了完整性，我们定义
\[
\mathcal{T}_0^0(V) = \mathbb{R}. \tag{5.1.2}
\]

重要的是不要将上述向量空间运算与多重线性性质混淆。后者意味着张量在其每个参数上都是线性的。即，对所有余向量 \(\eta^1,\dots,\eta^r,\zeta \in V^*\)，所有向量 \(v_1,\dots,v_s,w \in V\) 及所有实数 \(c\)，我们有
\[
\mathcal{A}(\eta^1,\dots,c\eta^i+\zeta,\dots,\eta^r,v_1,\dots,v_s)
= c\mathcal{A}(\eta^1,\dots,\eta^i,\dots,\eta^r,v_1,\dots,v_s)
+ \mathcal{A}(\eta^1,\dots,\zeta,\dots,\eta^r,v_1,\dots,v_s)
\]
对 \(i = 1,\dots,r\) 成立，且
\[
\mathcal{A}(\eta^1,\dots,\eta^r,v_1,\dots,cv_j+w,\dots,v_s)
= c\mathcal{A}(\eta^1,\dots,\eta^r,v_1,\dots,v_j,\dots,v_s)
+ \mathcal{A}(\eta^1,\dots,\eta^r,v_1,\dots,w,\dots,v_s)
\]
对 \(j = 1,\dots,s\) 成立。

**例5.1.1.** 张量的经典例子是行列式函数 \(\det\)，它是 \(\mathrm{Mat}_{m\times 1}\) 上的一个 \(m\)-协变张量。另一个明显的例子是向量空间 \(V\) 上的双线性函数 \(b\)，它是 \(V\) 上的一个 \(2\)-协变张量。下面再给出两个张量的例子。对于给定的向量 \(v,w \in V\)，函数 \(\mathcal{A}: V^{*2} \to \mathbb{R}\)（定义为对所有余向量 \(\eta,\zeta \in V^*\)，\(\mathcal{A}(\eta,\zeta) = \eta(v)\zeta(w)\)）是 \(V\) 上的一个 \(2\)-逆变张量。类似地，对于给定的余向量 \(\eta,\zeta \in V^*\)，函数 \(\mathcal{B}: V^2 \to \mathbb{R}\)（定义为对所有向量 \(v,w \in V\)，\(\mathcal{B}(v,w) = \eta(v)\zeta(w)\)）是 \(V\) 上的一个 \(2\)-协变张量。

涉及张量的计算在很大程度上依赖于恒等关系 \(V^{**} = V\)，该关系包含在(5.1.1)中。我们在此重申1.2节末尾的评注：对于 \(V\) 中的向量 \(v\) 和 \(V^*\) 中的余向量 \(\eta\)，\(V^{**}\) 中对应于 \(v\) 的元素也记为 \(v\)，因此根据定义，
\[
v(\eta) = \eta(v). \tag{5.1.3}
\]
于是，例5.1.1中的 \(\mathcal{B}\) 可表示为 \(\mathcal{B}(v,w) = v(\eta)w(\zeta)\)。尽管有(5.1.3)，我们通常仍更喜欢用 \(\eta(v)\) 而不是 \(v(\eta)\)。

我们现在定义一种张量的乘法。张量积是一族线性映射
\[
\otimes : \mathcal{T}_s^r(V) \times \mathcal{T}_{s'}^{r'}(V) \to \mathcal{T}_{s+s'}^{r+r'}(V)
\]
对 \(r, s, r', s' \ge 0\) 定义为
\[
(\mathcal{A} \otimes \mathcal{B})(\eta^1,\dots,\eta^{r+r'},v_1,\dots,v_{s+s'})
= \mathcal{A}(\eta^1,\dots,\eta^r,v_1,\dots,v_s)\,\mathcal{B}(\eta^{r+1},\dots,\eta^{r+r'},v_{s+1},\dots,v_{s+s'}) \tag{5.1.4}
\]
对所有张量 \(\mathcal{A} \in \mathcal{T}_s^r(V)\) 和 \(\mathcal{B} \in \mathcal{T}_{s'}^{r'}(V)\)，所有余向量 \(\eta^1,\dots,\eta^{r+r'} \in V^*\)，以及所有向量 \(v_1,\dots,v_{s+s'} \in V\) 成立。我们将 \(\mathcal{A} \otimes \mathcal{B}\) 称为 \(\mathcal{A}\) 与 \(\mathcal{B}\) 的张量积。与加法不同，张量的乘积不要求 \(\mathcal{A}\) 和 \(\mathcal{B}\) 具有相同的秩。通常，\(\mathcal{A} \otimes \mathcal{B} \ne \mathcal{B} \otimes \mathcal{A}\)，因此张量积一般不可交换。

下一个结果给出张量的基本代数性质。

**定理5.1.2.** 设 \(V\) 为向量空间，设 \(\mathcal{A}, \mathcal{A}_1, \mathcal{A}_2\)、\(\mathcal{B}, \mathcal{B}_1, \mathcal{B}_2\) 和 \(\mathcal{C}\) 分别为 \(\mathcal{T}_s^r(V)\)、\(\mathcal{T}_{s'}^{r'}(V)\) 和 \(\mathcal{T}_{s''}^{r''}(V)\) 中的张量，并设 \(c\) 为实数。则：

- (a) \((\mathcal{A}_1 + \mathcal{A}_2) \otimes \mathcal{B} = \mathcal{A}_1 \otimes \mathcal{B} + \mathcal{A}_2 \otimes \mathcal{B}\)。
- (b) \(\mathcal{A} \otimes (\mathcal{B}_1 + \mathcal{B}_2) = \mathcal{A} \otimes \mathcal{B}_1 + \mathcal{A} \otimes \mathcal{B}_2\)。
- (c) \((c\mathcal{A}) \otimes \mathcal{B} = c(\mathcal{A} \otimes \mathcal{B}) = \mathcal{A} \otimes (c\mathcal{B})\)。
- (d) \((\mathcal{A} \otimes \mathcal{B}) \otimes \mathcal{C} = \mathcal{A} \otimes (\mathcal{B} \otimes \mathcal{C})\)。

*证明.* 直接计算即可。

鉴于定理5.1.2(d)，我们在张量积周围省略括号，例如将 \((\mathcal{A} \otimes \mathcal{B}) \otimes \mathcal{C}\) 和 \(\mathcal{A} \otimes (\mathcal{B} \otimes \mathcal{C})\) 记为 \(\mathcal{A} \otimes \mathcal{B} \otimes \mathcal{C}\)，对多于三项的张量积也有相应的记号。通过对向量和余向量的集合形成张量积，我们可以构造任意秩的张量。举例来说，由 \(V\) 中的向量 \(w_1,\dots,w_r\) 和 \(V^*\) 中的余向量 \(\zeta^1,\dots,\zeta^s\)，我们得到 \(\mathcal{T}_s^r(V)\) 中的张量 \(w_1 \otimes \cdots \otimes w_r \otimes \zeta^1 \otimes \cdots \otimes \zeta^s\)。于是(5.1.4)给出
\[
w_1 \otimes \cdots \otimes w_r \otimes \zeta^1 \otimes \cdots \otimes \zeta^s(\eta^1,\dots,\eta^r,v_1,\dots,v_s)
= w_1(\eta^1)\cdots w_r(\eta^r)\,\zeta^1(v_1)\cdots\zeta^s(v_s)
\]
对所有余向量 \(\eta^1,\dots,\eta^r \in V^*\) 和所有向量 \(v_1,\dots,v_s \in V\) 成立。

设 \(V\) 为向量空间，设 \(\mathcal{A}\) 为 \(\mathcal{T}_s^r(V)\) 中的张量，设 \(\mathcal{H} = (h_1,\dots,h_m)\) 为 \(V\) 的一组基，并设 \(\Theta = (\theta^1,\dots,\theta^m)\) 为其对偶基。\(\mathcal{A}\) 关于 \(\mathcal{H}\) 的分量定义为
\[
\mathcal{A}^{i_1\dots i_r}_{j_1\dots j_s} = \mathcal{A}(\theta^{i_1},\dots,\theta^{i_r},h_{j_1},\dots,h_{j_s})
\]
对所有 \(1 \le i_1,\dots,i_r \le m\) 和 \(1 \le j_1,\dots,j_s \le m\) 成立。

**定理5.1.3（\(\mathcal{T}_s^r(V)\) 的基）。** 在上述设定下：

*证明.* (a)、(b)：根据前面的评注，每个 \(h_{i_1} \otimes \cdots \otimes h_{i_r} \otimes \theta^{j_1} \otimes \cdots \otimes \theta^{j_s}\) 都属于 \(\mathcal{T}_s^r(V)\)。设 \(\widetilde{\mathcal{A}}\) 为 \(\mathcal{T}_s^r(V)\) 中的张量，定义为
\[
\widetilde{\mathcal{A}} = \sum_{\substack{1\le i_1,\dots,i_r\le m \\ 1\le j_1,\dots,j_s\le m}} \mathcal{A}^{i_1\dots i_r}_{j_1\dots j_s}\, h_{i_1} \otimes \cdots \otimes h_{i_r} \otimes \theta^{j_1} \otimes \cdots \otimes \theta^{j_s}.
\]
则
\[
\begin{aligned}
\widetilde{\mathcal{A}}(\theta^{i_1},\dots,\theta^{i_r},h_{j_1},\dots,h_{j_s})
&= \sum_{\substack{1\le k_1,\dots,k_r\le m \\ 1\le l_1,\dots,l_s\le m}} \mathcal{A}^{k_1\dots k_r}_{l_1\dots l_s}\, h_{k_1} \otimes \cdots \otimes h_{k_r} \otimes \theta^{l_1} \otimes \cdots \otimes \theta^{l_s}(\theta^{i_1},\dots,\theta^{i_r},h_{j_1},\dots,h_{j_s})\\
&= \sum_{\substack{1\le k_1,\dots,k_r\le m \\ 1\le l_1,\dots,l_s\le m}} \mathcal{A}^{k_1\dots k_r}_{l_1\dots l_s}\, \delta^{i_1}_{k_1}\cdots\delta^{i_r}_{k_r}\,\delta^{l_1}_{j_1}\cdots\delta^{l_s}_{j_s}\\
&= \mathcal{A}^{i_1\dots i_r}_{j_1\dots j_s}\\
&= \mathcal{A}(\theta^{i_1},\dots,\theta^{i_r},h_{j_1},\dots,h_{j_s}).
\end{aligned}
\]
由于 \(\mathcal{H}\) 和 \(\Theta\) 分别是 \(V\) 和 \(V^*\) 的基，且 \(\mathcal{A}\) 和 \(\widetilde{\mathcal{A}}\) 是多重线性的，因此 \(\mathcal{A} = \widetilde{\mathcal{A}}\)。从而 \(h_{i_1} \otimes \cdots \otimes h_{i_r} \otimes \theta^{j_1} \otimes \cdots \otimes \theta^{j_s}\) 张成 \(\mathcal{T}_s^r(V)\)。若对某些实数 \(a^{i_1\dots i_r}_{j_1\dots j_s}\)，有 \(\sum_{\substack{1\le i_1,\dots,i_r\le m \\ 1\le j_1,\dots,j_s\le m}} a^{i_1\dots i_r}_{j_1\dots j_s}\, h_{i_1} \otimes \cdots \otimes h_{i_r} \otimes \theta^{j_1} \otimes \cdots \otimes \theta^{j_s} = 0\)，则将上述等式两边作用于 \((\theta^{k_1},\dots,\theta^{k_r},h_{l_1},\dots,h_{l_s})\) 得到 \(a^{k_1\dots k_r}_{l_1\dots l_s} = 0\)。因此，\(h_{i_1} \otimes \cdots \otimes h_{i_r} \otimes \theta^{j_1} \otimes \cdots \otimes \theta^{j_s}\) 是线性无关的。(c)：由(a)部分直接得出。

我们从定理5.1.3可以看出，一个张量完全由其关于给定基的分量决定。事实上，这正是定义张量的经典方式。选择另一组基通常会使分量取不同的值。下一个结果展示了当从一组基变换到另一组基时如何转换分量。

**定理5.1.4（基变换）。** 设 \(V\) 为向量空间，设 \(\mathcal{H}\) 和 \(\widetilde{\mathcal{H}}\) 为 \(V\) 的基，并设 \([\mathrm{id}_V]_{\widetilde{\mathcal{H}}}^{\mathcal{H}} = [a_j^i]\) 和 \(([\mathrm{id}_V]_{\widetilde{\mathcal{H}}}^{\mathcal{H}})^{-1} = [b_j^i]\)。设 \(\mathcal{A}\) 为 \(\mathcal{T}_s^r(V)\) 中的张量，并设 \(\mathcal{A}^{i_1\dots i_r}_{j_1\dots j_s}\) 和 \(\widetilde{\mathcal{A}}^{i_1\dots i_r}_{j_1\dots j_s}\) 分别为 \(\mathcal{A}\) 关于 \(\mathcal{H}\) 和 \(\widetilde{\mathcal{H}}\) 的分量。则
\[
\widetilde{\mathcal{A}}^{i_1\dots i_r}_{j_1\dots j_s}
= \sum_{\substack{1\le k_1,\dots,k_r\le m \\ 1\le l_1,\dots,l_s\le m}}
b^{i_1}_{k_1}\dots b^{i_r}_{k_r}\, a^{l_1}_{j_1}\dots a^{l_s}_{j_s}\, \mathcal{A}^{k_1\dots k_r}_{l_1\dots l_s}.
\]

