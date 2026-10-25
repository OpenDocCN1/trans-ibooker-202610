

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

