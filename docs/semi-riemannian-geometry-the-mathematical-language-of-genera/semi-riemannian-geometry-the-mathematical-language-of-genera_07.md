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





证明。设 \(\mathcal H=(h_1,\ldots,h_m)\) 且 \(\widetilde{\mathcal H}=(\widetilde h_1,\ldots,\widetilde h_m)\)，设 \(\Theta=(\theta^1,\ldots,\theta^m)\) 和 \(\widetilde\Theta=(\widetilde\theta^1,\ldots,\widetilde\theta^m)\) 为相应的对偶基。我们有

\[
[\operatorname{id}_V^*]_{\widetilde\Theta}^{\Theta}=\left([\operatorname{id}_V^*]_{\Theta}^{\widetilde\Theta}\right)^{-1}\quad [\mathrm{Th\ 2.2.5(b)}]
\]
\[
=\left(([\operatorname{id}_V]^*_{\Theta}^{\widetilde\Theta})^{-1}\right)
\]
\[
=\left(([\operatorname{id}_V]_{\widetilde{\mathcal H}}^{\mathcal H})^{\mathsf T}\right)^{-1}\quad [\mathrm{Th\ 2.2.7}]
\]
\[
=\left(([\operatorname{id}_V]_{\widetilde{\mathcal H}}^{\mathcal H})^{-1}\right)^{\mathsf T}\quad [\mathrm{Th\ 2.1.3(c)}]
\]
\[
=[b_j^i]^{\mathsf T}.
\]

利用 (2.2.6) 和 (2.2.7) 得

\[
\widetilde A^{i_1\ldots i_r}_{j_1\ldots j_s}=A(\widetilde\theta^{i_1},\ldots,\widetilde\theta^{i_r},\widetilde h_{j_1},\ldots,\widetilde h_{j_s})
\]
\[
=A\left(\sum_{k_1}b^{i_1}_{k_1}\theta^{k_1},\ldots,\sum_{k_r}b^{i_r}_{k_r}\theta^{k_r},\sum_{l_1}a^{l_1}_{j_1}h_{l_1},\ldots,\sum_{l_s}a^{l_s}_{j_s}h_{l_s}\right)
\]
\[
=\sum_{\substack{1\le k_1,\ldots,k_r\le m\\1\le l_1,\ldots,l_s\le m}}b^{i_1}_{k_1}\cdots b^{i_r}_{k_r}a^{l_1}_{j_1}\cdots a^{l_s}_{j_s}A(\theta^{k_1},\ldots,\theta^{k_r},h_{l_1},\ldots,h_{l_s})
\]
\[
=\sum_{\substack{1\le k_1,\ldots,k_r\le m\\1\le l_1,\ldots,l_s\le m}}b^{i_1}_{k_1}\cdots b^{i_r}_{k_r}a^{l_1}_{j_1}\cdots a^{l_s}_{j_s}A^{k_1\ldots k_r}_{l_1\ldots l_s}.
\]

正如下一个结果所示，张量的乘积的分量由相应分量的乘积给出。

## 定理 5.1.5（张量的乘积）。

设 \(V\) 为向量空间，设 \(\mathcal A\) 和 \(\mathcal B\) 分别为 \(T^r_s(V)\) 和 \(T^{r'}_{s'}(V)\) 中的张量，设 \(\mathcal H=(h_1,\ldots,h_m)\) 为 \(V\) 的基，设 \((\theta^1,\ldots,\theta^m)\) 为其对偶基。设 \(A^{i_1\ldots i_r}_{j_1\ldots j_s}\) 和 \(B^{k_1\ldots k_{r'}}_{l_1\ldots l_{s'}}\) 分别为 \(\mathcal A\) 和 \(\mathcal B\) 关于 \(\mathcal H\) 的分量，使得

\[
\mathcal A=\sum_{\substack{1\le i_1,\ldots,i_r\le m\\1\le j_1,\ldots,j_s\le m}}A^{i_1\ldots i_r}_{j_1\ldots j_s}h_{i_1}\otimes\cdots\otimes h_{i_r}\otimes\theta^{j_1}\otimes\cdots\otimes\theta^{j_s}
\]

且

\[
\mathcal B=\sum_{\substack{1\le k_1,\ldots,k_{r'}\le m\\1\le l_1,\ldots,l_{s'}\le m}}B^{k_1\ldots k_{r'}}_{l_1\ldots l_{s'}}h_{k_1}\otimes\cdots\otimes h_{k_{r'}}\otimes\theta^{l_1}\otimes\cdots\otimes\theta^{l_{s'}}.
\]

则 \(\mathcal A\otimes\mathcal B\) 关于 \(\mathcal H\) 的分量为 \(A^{i_1\ldots i_r}_{j_1\ldots j_s}B^{k_1\ldots k_{r'}}_{l_1\ldots l_{s'}}\)，使得

\[
\mathcal A\otimes\mathcal B=\sum_{\substack{1\le i_1,\ldots,i_r,k_1,\ldots,k_{r'}\le m\\1\le j_1,\ldots,j_s,l_1,\ldots,l_{s'}\le m}}A^{i_1\ldots i_r}_{j_1\ldots j_s}B^{k_1\ldots k_{r'}}_{l_1\ldots l_{s'}}h_{i_1}\otimes\cdots\otimes h_{i_r}\otimes h_{k_1}\otimes\cdots\otimes h_{k_{r'}}\otimes\theta^{j_1}\otimes\cdots\otimes\theta^{j_s}\otimes\theta^{l_1}\otimes\cdots\otimes\theta^{l_{s'}}.
\]

证明。由定理 5.1.2 可知

𝒜 ⊗ ℬ = ∑_{1 ≤ i₁,...,i_r,k₁,...,k_{r'} ≤ m 
1 ≤ j₁,...,j_s,l₁,...,l_{s'} ≤ m} 𝒜^{i₁...i_r}_{j₁...j_s} ℬ^{k₁...k_{r'}}_{l₁...l_{s'}} (h_{i₁} ⊗ ⋯ ⊗ h_{i_r} ⊗ θ^{j₁} ⊗ ⋯ ⊗ θ^{j_s}) ⊗ (h_{k₁} ⊗ ⋯ ⊗ h_{k_{r'}} ⊗ θ^{l₁} ⊗ ⋯ ⊗ θ^{l_{s'}}).

我们需要验证

(h_{i₁} ⊗ ⋯ ⊗ h_{i_r} ⊗ θ^{j₁} ⊗ ⋯ ⊗ θ^{j_s}) ⊗ (h_{k₁} ⊗ ⋯ ⊗ h_{k_{r'}} ⊗ θ^{l₁} ⊗ ⋯ ⊗ θ^{l_{s'}}) = (h_{i₁} ⊗ ⋯ ⊗ h_{i_r} ⊗ h_{k₁} ⊗ ⋯ ⊗ h_{k_{r'}}) ⊗ (θ^{j₁} ⊗ ⋯ ⊗ θ^{j_s} ⊗ θ^{l₁} ⊗ ⋯ ⊗ θ^{l_{s'}}).

设 η¹,...,η^{r+r'} 为 \(V^*\) 中的余向量，设 v₁,...,v_{s+s'} 为 \(V\) 中的向量。我们由 (5.1.4) 知，将上述表达式的左端作用于 (η¹,...,η^{r+r'}, v₁,...,v_{s+s'}) 得

[h_{i₁}(η¹)⋯h_{i_r}(η^{r})][θ^{j₁}(v₁)⋯θ^{j_s}(v_s)] · [h_{k₁}(η^{r+1})⋯h_{k_{r'}}(η^{r+r'})][θ^{l₁}(v_{s+1})⋯θ^{l_{s'}}(v_{s+s'})],

而将右端作用则得

[h_{i₁}(η¹)⋯h_{i_r}(η^{r})][h_{k₁}(η^{r+1})⋯h_{k_{r'}}(η^{r+r'})] · [θ^{j₁}(v₁)⋯θ^{j_s}(v_s)][θ^{l₁}(v_{s+1})⋯θ^{l_{s'}}(v_{s+s'})].

结论随之得出。

定理 5.1.5表明，每个张量均以 (5.1.5) 给出的标准格式表示时，张量的乘积仍可表示为该相同格式。

### 例 5.1.6（克罗内克 Delta）。设 \(V\) 为向量空间，设 ℋ = (h₁,...,h_m) 为 \(V\) 的基，设 (θ¹,...,θ^m) 为其对偶基。如上所述，在经典文献中，张量是通过给出其关于 \(V\) 的选定基的分量来指定的。从该角度看，\(\mathcal{T}^1_1(V)\) 中最基本的张量或许是分量为克罗内克 delta 的 δⁱ_j 的张量。因此，我们考虑 (1,1)-张量

δ = ∑_{ij} δⁱ_j h_i ⊗ θ^j .

(c) $(B \circ A)^* = A^* \circ B^*.$

证明。显而易见。

## 5.3 张量的表示

设 $V$ 为向量空间，设 $\mathcal{H} = (h_1, \dots, h_m)$ 为 $V$ 的基，设 $(\theta^1, \dots, \theta^m)$ 为其对偶基，设 $s \ge 1$ 为整数。沿用第 B.5 节和第 B.6 节，我们用 $\mathrm{Mult}(V^s, V)$ 表示从 $V^s$ 到 $V$ 的 $\mathbb{R}$-多线性映射的向量空间。令我们定义一个映射

$\mathfrak{R}_s : \mathrm{Mult}(V^s, V) \longrightarrow \mathcal{T}_s^1(V),$

称为表示映射，由

$\mathfrak{R}_s(\Psi)(\eta, v_1, \dots, v_s) = \eta\bigl(\Psi(v_1, \dots, v_s)\bigr)$ \quad (5.3.1)

对所有 $\mathrm{Mult}(V^s, V)$ 中的映射 $\Psi$、$V^*$ 中的余向量 $\eta$ 以及 $V$ 中的向量 $v_1, \dots, v_s$ 成立。当 $s = 1$ 时，我们记

$\mathfrak{R}_1$ 为 $\mathfrak{R}$。

## 定理 5.3.1（张量的表示）。在上述设定下：

+   (a) $\mathfrak{R}_s$ 是线性同构：
+   
+   $\mathcal{T}_s^1(V) \approx \mathrm{Mult}(V^s, V).$

+   (b) 对所有 $\mathrm{Mult}(V^s, V)$ 中的映射 $\Psi$，张量 $\mathfrak{R}_s(\Psi)$ 在 $\mathcal{T}_s^1(V)$ 中关于 $\mathcal{H}$ 的分量为
+   
+   $\mathfrak{R}_s(\Psi)^i_{j_1 \dots j_s} = \theta^i\bigl(\Psi(h_{j_1}, \dots, h_{j_s})\bigr).$

+   (c) 对所有 $\mathcal{T}_s^1(V)$ 中的张量 $\mathcal{A}$，
+   
+   $\mathfrak{R}_s^{-1}(\mathcal{A})(h_{j_1}, \dots, h_{j_s}) = \sum_i \mathcal{A}^i_{j_1 \dots j_s} h_i,$
+   
+   其中 $\mathcal{A}^i_{j_1 \dots j_s}$ 为 $\mathcal{A}$ 关于 $\mathcal{H}$ 的分量。

证明。 (a)：容易证明 $\mathfrak{R}_s$ 为线性映射。若 $\mathfrak{R}_s(\Psi) = 0$，则 $\eta\bigl(\Psi(v_1, \dots, v_s)\bigr) = 0$ 对所有 $V^*$ 中的余向量 $\eta$ 和 $V$ 中的向量 $v_1, \dots, v_s$ 成立。特别地，每个 $\theta^i\bigl(\Psi(v_1, \dots, v_s)\bigr) = 0$。因为 $\Psi(v_1, \dots, v_s)$ 为 $V$ 中的向量，由定理 1.2.1(d)，

$\Psi(v_1, \dots, v_s) = \sum_i \theta^i\bigl(\Psi(v_1, \dots, v_s)\bigr) h_i,$

故 $\Psi(v_1, \dots, v_s) = 0$。由于 $v_1, \dots, v_s$ 为任意，$\Psi = 0$。于是，$\ker(\mathfrak{R}_s) = \{0\}$。由定理 5.1.3(c)，$\mathcal{T}_s^1(V)$ 的维数为 $m^{s+1}$，且可以证明

## 5.4 张量的收缩

张量的收缩与矩阵的迹密切相关。像对偶向量空间一样，它是另一种看似简单却应用广泛的代数构造。

## 定理 5.4.1。设 $V$ 为向量空间，并定义线性函数

C₁¹ : 𝒯₁¹(V) → ℝ，

称为 (1,1)-收缩，由

C₁¹ = tr ∘ 𝔅⁻¹，

其中 tr 为迹且 𝔅 由 (5.3.2) 给出。则 C₁¹ 是唯一的线性函数，使得

C₁¹(v ⊗ η) = v(η) (5.4.1)

对所有 $V$ 中的向量 $v$ 和 $V^*$ 中的余向量 $\eta$ 成立。

证明。设 ℋ = (h₁, …, hₘ) 为 $V$ 的基，设 (θ¹, …, θᵐ) 为其对偶基。

存在性。由定理 5.3.2(c)，

[𝔅⁻¹(v ⊗ η)]ℋ = [(v ⊗ η)ⁱⱼ] . (5.4.2)

由于

(v ⊗ η)ⁱⱼ = (v ⊗ η)(θⁱ, hⱼ) = v(θⁱ) η(hⱼ)，

由定理 1.2.1(d) 和 (1.2.3) 可知

tr([(v ⊗ η)ⁱⱼ]) = Σᵢ v(θⁱ) η(hᵢ) = η( Σᵢ θⁱ(v) hᵢ ) = η(v). (5.4.3)

于是，

tr(𝔅⁻¹(v ⊗ η)) = tr([𝔅⁻¹(v ⊗ η)]ℋ) [(2.5.1)] = tr([(v ⊗ η)ⁱⱼ]) [(5.4.2)] = η(v) [(5.4.3)] = v(η), [(1.2.3)]

这表明 tr ∘ 𝔅⁻¹ 满足 (5.4.1)。

唯一性。设 ℓ : 𝒯₁¹(V) → ℝ 为满足 (5.4.1) 的线性函数，设 𝒜 为 𝒯₁¹(V) 中的张量，设 𝒜ⁱⱼ 为 𝒜 关于


ℋ. 则

```
ℓ(𝒜) = ℓ(∑_{ij} 𝒜_j^i h_i ⊗ θ^j) = ∑_{ij} 𝒜_j^i ℓ(h_i ⊗ θ^j)
= ∑_{ij} 𝒜_j^i h_i(θ^j)  [(5.4.1)]
= ∑_i 𝒜_i^i = tr([𝒜_j^i])
= tr([ℛ^{-1}(𝒜)]_ℋ^ℋ)  [Th 5.3.2(c)]
= tr(ℛ^{-1}(𝒜)).  [(2.5.1)]
```

由于 𝒜 是任意的，ℓ = tr ∘ ℛ^{-1}。

## 定理 5.4.2. 设 V 是向量空间，ℋ 是 V 的基，𝒜 是 𝒯_1^1(V) 中的张量，且 𝒜_j^i 是 𝒜 关于 ℋ 的分量。则

```
C_1^1(𝒜) = ∑_i 𝒜_i^i.
```

证明. 这在定理 5.4.1 的证明中已经给出。

定理 5.4.2 解释了为什么文献中有时将“缩并”称为“迹”。下一个结果是定理 5.4.1 的推广。

## 定理 5.4.3. 若 V 是向量空间，且 1 ≤ k ≤ r 和 1 ≤ l ≤ s 是整数，则存在唯一的线性映射

```
C_l^k : 𝒯_s^r(V) ⟶ 𝒯_{s-1}^{r-1}(V),
```

称为 (k,l)-缩并，使得

```
C_l^k(v_1 ⊗ ⋯ ⊗ v_r ⊗ η^1 ⊗ ⋯ ⊗ η^s)
= v_k(η^l) v_1 ⊗ ⋯ ⊗ v̂_k ⊗ ⋯ ⊗ v_r ⊗ η^1 ⊗ ⋯ ⊗ η̂^l ⊗ ⋯ ⊗ η^s
```

对所有 V 中的向量 v_1,...,v_r 及 V* 中的余向量 η^1,...,η^s 成立，其中 ^ 表示该项被省略。

注记. 根据 (5.1.2)，当 r = s = 1 时，我们重新得到 (5.4.1)。

证明. 存在性. 设 𝒜 是 𝒯_s^r(V) 中的张量，ζ^1,...,ζ^{k-1},ζ^{k+1},...,ζ^r 是 V* 中的余向量，w_1,...,w_{l-1},w_{l+1},...,w_s 是 V 中的向量。我们暂时将前述余向量和向量视为固定，并通过如下赋值定义 𝒯_1^1(V) 中的张量 Ã：

```
(ζ, w) ⟼ 𝒜(ζ^1,...,ζ^{k-1},ζ,ζ^{k+1},...,ζ^r,w_1,...,w_{l-1},w,w_{l+1},...,w_s)
```

对所有 V* 中的余向量 ζ 和 V 中的向量 w 成立。利用定理 5.4.1，我们得到相应的 (1,1)-缩并 C_1^1(Ã)。令 ζ^1,...,ζ^{k-1},ζ^{k+1},...,ζ^r 和 \(w_1,\ldots,w_{l-1},w_{l+1},\ldots,w_s\) 变化，定义了 \(\mathcal T^{r-1}_{s-1}(V)\) 中的张量 \(C_l^k(\mathcal A)\)。由

\[v_1\otimes\cdots\otimes v_r\otimes\eta^1\otimes\cdots\otimes\eta^s\]

\[(\zeta^1,\ldots,\zeta^{k-1},\zeta^{k+1},\ldots,\zeta^r,w_1,\ldots,w_{l-1},w,w_{l+1},\ldots,w_s)\]

\[= [v_1(\zeta^1)\cdots v_{k-1}(\zeta^{k-1})]v_k(\zeta)[v_{k+1}(\zeta^{k+1})\cdots v_r(\zeta^r)]\]

\[\cdot[\eta^1(w_1)\cdots\eta^{l-1}(w_{l-1})]\eta^l(w)[\eta^{l+1}(w_{l+1})\cdots\eta^s(w_s)]\]

\[= [v_1(\zeta^1)\cdots v_{k-1}(\zeta^{k-1})][v_{k+1}(\zeta^{k+1})\cdots v_r(\zeta^r)]\]

\[\cdot[\eta^1(w_1)\cdots\eta^{l-1}(w_{l-1})][\eta^{l+1}(w_{l+1})\cdots\eta^s(w_s)](v_k\otimes\eta^l)(\zeta,w)\]

以及定理 5.4.1 可得

\[C_l^k(v_1\otimes\cdots\otimes v_r\otimes\eta^1\otimes\cdots\otimes\eta^s)\]

\[(\zeta^1,\ldots,\zeta^{k-1},\zeta^{k+1},\ldots,\zeta^r,w_1,\ldots,w_{l-1},w_{l+1},\ldots,w_s)\]

\[= [v_1(\zeta^1)\cdots v_{k-1}(\zeta^{k-1})][v_{k+1}(\zeta^{k+1})\cdots v_r(\zeta^r)]\]

\[\cdot[\eta^1(w_1)\cdots\eta^{l-1}(w_{l-1})][\eta^{l+1}(w_{l+1})\cdots\eta^s(w_s)]C_1^1(v_k\otimes\eta^l)\]

\[= [v_1(\zeta^1)\cdots v_{k-1}(\zeta^{k-1})][v_{k+1}(\zeta^{k+1})\cdots v_r(\zeta^r)]\]

\[\cdot[\eta^1(w_1)\cdots\eta^{l-1}(w_{l-1})][\eta^{l+1}(w_{l+1})\cdots\eta^s(w_s)]v_k(\eta^l)\]

\[= v_k(\eta^l)\,v_1\otimes\cdots\otimes\widehat{v_k}\otimes\cdots\otimes v_r\otimes\eta^1\otimes\cdots\otimes\widehat{\eta^l}\otimes\cdots\otimes\eta^s\]

\[(\zeta^1,\ldots,\zeta^{k-1},\zeta^{k+1},\ldots,\zeta^r,w_1,\ldots,w_{l-1},w_{l+1},\ldots,w_s).\]

由于 \(\zeta^1,\ldots,\zeta^{k-1},\zeta^{k+1},\ldots,\zeta^r,w_1,\ldots,w_{l-1},w_{l+1},\ldots,w_s\) 是任意的，结论成立。

**唯一性.** 这可由定理 5.4.1 中描述的 \(C_1^1\) 的唯一性推出。

我们称 \((k,l)\) 为 \((k,l)\)-缩并的**阶**。不同阶的缩并的复合简称为**缩并**。我们有时将这里定义的缩并类型称为**普通**缩并，以区别于将在 6.5 节中描述的一种变体。

**例 5.4.4.** 设 V 是向量空间，令 \(\mathcal H=(h_1,\ldots,h_m)\) 为 V 的基，令 \((\theta^1,\ldots,\theta^m)\) 为其对偶基。设 \(\mathcal A\) 是 \(\mathcal T^2_2(V)\) 中的张量，且 \(A^{ij}_{kl}\) 是 \(\mathcal A\) 关于 \(\mathcal H\) 的分量，使得

\[\mathcal A=\sum_{ijkl} A^{ij}_{kl}\,h_i\otimes h_j\otimes\theta^k\otimes\theta^l.\]

由此即得结论。

**例 5.4.6.** 设 V 是向量空间，𝒜 是 𝒯₁¹(V) 中的张量，η 是 V* 中的余向量，v 是 V 中的向量。我们断言

𝒜(η, v) = C₁¹ ∘ C₂²(v ⊗ 𝒜 ⊗ η).

设 ℋ = (h₁, …, hₘ) 是 V 的基，令 (θ¹, …, θᵐ) 为其对偶基，并在局部坐标中设

𝒜 = Σᵢⱼ 𝒜ⁱⱼ hᵢ ⊗ θʲ,      η = Σₖ aₖ θᵏ,      v = Σₗ bˡ hₗ.

则

𝒜(η, v) = (Σᵢⱼ 𝒜ⁱⱼ hᵢ ⊗ θʲ)(Σₖ aₖ θᵏ, Σₗ bˡ hₗ) = Σᵢⱼ 𝒜ⁱⱼ [hᵢ(Σₖ aₖ θᵏ) θʲ(Σₗ bˡ hₗ)] = Σᵢⱼ 𝒜ⁱⱼ[(Σₖ aₖ hᵢ(θᵏ))(Σₗ bˡ θʲ(hₗ))] = Σᵢⱼ 𝒜ⁱⱼ aᵢ bʲ  (5.4.4)

且

v ⊗ 𝒜 ⊗ η = (Σₗ bˡ hₗ) ⊗ (Σᵢⱼ 𝒜ⁱⱼ hᵢ ⊗ θʲ) ⊗ (Σₖ aₖ θᵏ) = Σᵢⱼₖₗ bˡ 𝒜ⁱⱼ aₖ hₗ ⊗ hᵢ ⊗ θʲ ⊗ θᵏ,

因此

C₂²(v ⊗ 𝒜 ⊗ η) = Σᵢⱼₖₗ bˡ 𝒜ⁱⱼ aₖ C₂²(hₗ ⊗ hᵢ ⊗ θʲ ⊗ θᵏ) = Σᵢⱼₖₗ bˡ 𝒜ⁱⱼ aₖ hᵢ(θᵏ) hₗ ⊗ θʲ = Σᵢⱼₗ bˡ 𝒜ⁱⱼ aᵢ hₗ ⊗ θʲ,

所以

C₁¹ ∘ C₂²(v ⊗ 𝒜 ⊗ η) = Σᵢⱼₗ bˡ 𝒜ⁱⱼ aᵢ C₁¹(hₗ ⊗ θʲ) = Σᵢⱼₗ bˡ 𝒜ⁱⱼ aᵢ hₗ(θʲ) = Σᵢⱼ aᵢ bʲ 𝒜ⁱⱼ.  (5.4.5)

结论可由 (5.4.4) 和 (5.4.5) 得出。

## 定理 5.4.7. 设 V 是向量空间，𝒜 是 𝒯ₛʳ(V) 中的张量，η¹, …, ηʳ 是 V* 中的余向量，v₁, …, vₛ 是 V 中的向量。则在 V 上存在一个缩并 C，使得

𝒜(η¹, …, ηʳ, v₁, …, vₛ) = C(v₁ ⊗ ⋯ ⊗ vₛ ⊗ 𝒜 ⊗ η¹ ⊗ ⋯ ⊗ ηʳ).

证明. 本证明是对例 5.4.6 的详细展开。设 ℋ = (h₁, …, hₘ) 是 V 的基，令 (θ¹, …, θᵐ) 为其对偶基，并在局部坐标中设

𝒜 = Σ_{1 ≤ i₁,…,i_r ≤ m 
 1 ≤ j₁,…,j_s ≤ m} 𝒜^{i₁…i_r}_{j₁…j_s} h_{i₁} ⊗ ⋯ ⊗ h_{i_r} ⊗ θ^{j₁} ⊗ ⋯ ⊗ θ^{j_s},

η¹ = Σ_{k₁} a_{1k₁} θ^{k₁}, …, ηʳ = Σ_{k_r} a_{rk_r} θ^{k_r}

且

v₁ = Σ_{l₁} b^{1l₁} h_{l₁}, …, v_s = Σ_{l_s} b^{sl_s} h_{l_s}.

则

𝒜(η¹, …, ηʳ, v₁, …, vₛ) = ( Σ_{1 ≤ i₁,…,i_r ≤ m 
 1 ≤ j₁,…,j_s ≤ m} 𝒜^{i₁…i_r}_{j₁…j_s} h_{i₁} ⊗ ⋯ ⊗ h_{i_r} ⊗ θ^{j₁} ⊗ ⋯ ⊗ θ^{j_s} ) ( Σ_{k₁} a_{1k₁} θ^{k₁}, …, Σ_{k_r} a_{rk_r} θ^{k_r}, Σ_{l₁} b^{1l₁} h_{l₁}, …, Σ_{l_s} b^{sl_s} h_{l_s} ) = Σ_{1 ≤ i₁,…,i_r ≤ m 
 1 ≤ j₁,…,j_s ≤ m} 𝒜^{i₁…i_r}_{j₁…j_s} [ h_{i₁}( Σ_{k₁} a_{1k₁} θ^{k₁}) … h_{i_r}( Σ_{k_r} a_{rk_r} θ^{k_r}) · θ^{j₁}( Σ_{l₁} b^{1l₁} h_{l₁}) … θ^{j_s}( Σ_{l_s} b^{sl_s} h_{l_s}) ] (5.4.6)

= Σ_{1 ≤ i₁,…,i_r ≤ m 
 1 ≤ j₁,…,j_s ≤ m} 𝒜^{i₁…i_r}_{j₁…j_s} [ ( Σ_{k₁} a_{1k₁} h_{i₁}(θ^{k₁}) ) … ( Σ_{k_r} a_{rk_r} h_{i_r}(θ^{k_r}) ) · ( Σ_{l₁} b^{1l₁} θ^{j₁}(h_{l₁}) ) … ( Σ_{l_s} b^{sl_s} θ^{j_s}(h_{l_s}) ) ] = Σ_{1 ≤ i₁,…,i_r ≤ m 
 1 ≤ j₁,…,j_s ≤ m} 𝒜^{i₁…i_r}_{j₁…j_s} a_{1i₁} … a_{ri_r} b^{j₁1} … b^{j_s s}

且

v₁ ⊗ ⋯ ⊗ v_s ⊗ 𝒜 ⊗ η¹ ⊗ ⋯ ⊗ ηʳ = ( Σ_{l₁} b^{1l₁} h_{l₁} ) ⊗ ⋯ ⊗ ( Σ_{l_s} b^{sl_s} h_{l_s} ) ⊗ Σ_{1 ≤ i₁,…,i_r ≤ m 
 1 ≤ j₁,…,j_s ≤ m} 𝒜^{i₁…i_r}_{j₁…j_s} h_{i₁} ⊗ ⋯ ⊗ h_{i_r} ⊗ θ^{j₁} ⊗ ⋯ ⊗ θ^{j_s} ⊗ ( Σ_{k₁} a_{1k₁} θ^{k₁} ) ⊗ ⋯ ⊗ ( Σ_{k_r} a_{rk_r} θ^{k_r} )

$$=\sum_{\substack{1\le i_1,\ldots,i_r\le m\\1\le j_1,\ldots,j_s\le m}} b^{l_1}_{\ \ \ }\cdots b^{l_s}_{\ \ \ }A^{i_1\cdots i_r}_{j_1\cdots j_s}a_{1k_1}\cdots a_{rk_r}\\\cdot(h_{l_1}\otimes\cdots\otimes h_{l_s})\otimes(h_{i_1}\otimes\cdots\otimes h_{i_r})\otimes\theta^{j_1}\otimes\cdots\otimes\theta^{j_s}\\\otimes(\theta^{k_1}\otimes\cdots\otimes\theta^{k_r}).$$

为了完成证明，现在只需定义一系列 $(k,l)$-缩并，将其作用于 $v_1\otimes\cdots\otimes v_s\otimes\mathcal A\otimes\eta^1\otimes\cdots\otimes\eta^r$，使得

$h_{i_1}$ 与 $\theta^{k_1}$ 配对，得到 $h_{i_1}(\theta^{k_1})$，然后

$\vdots$

$h_{i_r}$ 与 $\theta^{k_r}$ 配对，得到 $h_{i_r}(\theta^{k_r})$，然后

$\theta^{j_1}$ 与 $h_{l_1}$ 配对，得到 $\theta^{j_1}(h_{l_1})$，然后

$\vdots$

$\theta^{j_s}$ 与 $h_{l_s}$ 配对，得到 $\theta^{j_s}(h_{l_s})$.

用 $C$ 表示这些缩并的复合，我们得到

$$C(v_1\otimes\cdots\otimes v_s\otimes A\otimes\eta^1\otimes\cdots\otimes\eta^r)\\=\sum_{\substack{1\le i_1,\ldots,i_r\le m\\1\le j_1,\ldots,j_s\le m}}b^{1j_1}\cdots b^{sj_s}A^{i_1\cdots i_r}_{j_1\cdots j_s}a_{1i_1}\cdots a_{ri_r}.$$

现在由 (5.4.6) 和 (5.4.7) 即得结论。




