# 第6章
# 标量积空间上的张量

## 6.1 张量的收缩

在第5.4节中，我们讨论了向量空间上张量的收缩。在标量积空间的设定下，还有更多内容需要说明。

定理 6.1.1（普通收缩的正交基表达式）。设 (V,g) 为符号为 (ε₁,…,εₘ) 的标量积空间，设 (e₁,…,eₘ) 为 V 的正交基，设 𝒜 为 𝒯ₛ¹(V) 中的张量。则：

- (a) 对于整数 s ≥ 2 和 1 ≤ l ≤ s − 1，张量 Cₗ¹(𝒜) 在 𝒯ₛ₋₁⁰(V) 中由下式给出
- 
- Cₗ¹(𝒜)(v₁,…,vₛ₋₁) = Σᵢ εᵢ⟨ℛₛ⁻¹(𝒜)(v₁,…,vₗ₋₁,eᵢ,vₗ,…,vₛ₋₁), eᵢ⟩
- 
- 对所有向量 v₁,…,vₛ₋₁ 在 V 中成立，其中 ℛₛ 由 (5.3.1) 给出。

- (b) 对于 s = 1，实数 C₁¹(𝒜) 由下式给出
- 
- C₁¹(𝒜) = Σᵢ εᵢ⟨ℛ⁻¹(𝒜)(eᵢ), eᵢ⟩,
- 
- 其中 ℛ 由 (5.3.2) 给出。

证明。 (a)：根据定理 5.4.5，Cₗ¹(𝒜) 关于 (e₁,…,eₘ) 的分量为

Cₗ¹(𝒜)(eⱼ₁,…,eⱼₛ₋₁) = Cₗ¹(𝒜)ⱼ₁…ⱼₛ₋₁ = Σₚ 𝒜ⱼ₁…ⱼₗ₋₁ p p jₗ…ⱼₛ₋₁. (6.1.1)

我们由定理 5.3.1(c) 得到

𝔅ₛ⁻¹(𝒜)(e_{j₁},…,e_{j_{i-1}},e_p,e_{j_i},…,e_{j_{s-1}}) = Σ_q 𝒜_{j₁…j_{i-1}p j_i…j_{s-1}}^q e_q,

故

⟨𝔅ₛ⁻¹(𝒜)(e_{j₁},…,e_{j_{i-1}},e_p,e_{j_i},…,e_{j_{s-1}}), e_p⟩ = ⟨Σ_q 𝒜_{j₁…j_{i-1}p j_i…j_{s-1}}^q e_q, e_p⟩
= Σ_q 𝒜_{j₁…j_{i-1}p j_i…j_{s-1}}^q ⟨e_q, e_p⟩
= ε_p 𝒜_{j₁…j_{i-1}p j_i…j_{s-1}}^p,

所以

𝒜_{j₁…j_{i-1}p j_i…j_{s-1}}^p = ε_p ⟨𝔅ₛ⁻¹(𝒜)(e_{j₁},…,e_{j_{i-1}},e_p,e_{j_i},…,e_{j_{s-1}}), e_p⟩. (6.1.2)

于是 (6.1.1) 和 (6.1.2) 给出

C_l¹(𝒜)(e_{j₁},…,e_{j_{s-1}}) = Σ_p ε_p ⟨𝔅ₛ⁻¹(𝒜)(e_{j₁},…,e_{j_{i-1}},e_p,e_{j_i},…,e_{j_{s-1}}), e_p⟩.

对任意向量 v₁,…,v_{s-1} 在 V 中的结果由 C_l¹(𝒜) 和 𝔅ₛ⁻¹(𝒜) 的多线性性得出。

(b)：我们有

Σ_i ε_i ⟨𝔅⁻¹(𝒜)(e_i), e_i⟩ = Σ_i ε_i ⟨Σ_j 𝒜_i^j e_j, e_i⟩ [Th 5.3.2(c)]
= Σ_{ij} ε_i ⟨e_j, e_i⟩ 𝒜_i^j = Σ_i 𝒜_i^i
= C₁¹(𝒜). [Th 5.4.2]

## 6.2 平坦映射

设 (V,𝔤) 为标量积空间，设 ℋ = (h₁,…,h_m) 为 V 的基，设 (θ¹,…,θ^m) 为其对偶基。由 (5.1.1) 回顾可知 𝒯₀¹(V) = V 且 𝒯₁⁰(V) = V*。第 3.3 节的平坦映射因此可表示为

F : 𝒯₀¹(V) ⟶ 𝒯₁⁰(V).

更一般地，设 1 ≤ k ≤ r 且 1 ≤ l ≤ s + 1 为整数（从而 r ≥ 1 且 s ≥ 0）。(k,l)-平坦映射记为

F_l^k : 𝒯_s^r(V) ⟶ 𝒯_{s+1}^{r-1}(V)
并对所有张量 𝒜 在 𝒯_s^r(V) 中，所有余向量 η¹,…,η^{r-1} 在 V* 中，以及所有向量 v₁,…,v_{s+1} 在 V 中定义如下，其中 ^ 表示省略的表达式：

[F1] 对于 r = 1（从而 k = 1）：

F_l^1(𝒜)(v₁, ..., v_{s+1}) = 𝒜(v_l^F, v₁, ..., \widehat{v_l}, ..., v_{s+1}).

[F2] 对于 r ≥ 2 且 1 ≤ k ≤ r − 1：

F_l^k(𝒜)(η¹, ..., η^{r−1}, v₁, ..., v_{s+1}) = 𝒜(η¹, ..., η^{k−1}, v_l^F, η^k, ..., η^{r−1}, v₁, ..., \widehat{v_l}, ..., v_{s+1}).

[F3] 对于 r ≥ 2 且 k = r：

F_l^r(𝒜)(η¹, ..., η^{r−1}, v₁, ..., v_{s+1}) = 𝒜(η¹, ..., η^{r−1}, v_l^F, v₁, ..., \widehat{v_l}, ..., v_{s+1}).

计算过程对 [F2] 最为透明：提取第 l 个协变位置的向量，使其平坦化，并将结果插入第 k 个逆变位置。

例 6.2.1。 为说明 [F1]，设 r = 1（从而 k = 1）且 s = 0（从而 l = 1），并设 w 为 𝒯₀¹(V) = V 中的向量。则 𝒯₁⁰(V) = V* 中的余向量 F₁¹(w) 由下式给出

F₁¹(w)(v) = w(v^F) 对所有向量 v 在 V 中成立。我们由 (1.2.3) 和 (3.3.1) 得到 w(v^F) = v^F(w) = ⟨v, w⟩ = ⟨w, v⟩ = w^F(v) = F(w)(v)，故 F₁¹(w)(v) = F(w)(v)。由于 v 和 w 是任意的，F₁¹ = F。

为说明 [F2]，设 r = 2（从而 k = 1）且 s = 0（从而 l = 1），并设 𝒜 为 𝒯₀²(V) 中的张量。则 𝒯₁¹(V) 中的张量 F₁¹(𝒜) 由 F₁¹(𝒜)(η, v) = 𝒜(v^F, η) 给出。

为说明 [F3]，设 r = 2（从而 k = 2）且 s = 0（从而 l = 1），并设 𝒜 为 𝒯₀²(V) 中的张量。则 𝒯₁¹(V) 中的张量 F₁²(𝒜) 由 F₁²(𝒜)(η, v) = 𝒜(η, v^F) 给出。

例 6.2.2。 设 𝒜 为 𝒯₁¹(V) 中的张量。对于 r = s = 1，存在 k = 1 且 l = 1,2 的平坦映射。对于 k = 1 且 l = 1，张量 F₁¹(𝒜) 在 𝒯₂⁰(V) 中由 F₁¹(𝒜)(v₁, v₂) = 𝒜(v₁^F, v₂) 给出。对于 k = 1 且 l = 2，张量 F₂¹(𝒜) 在 𝒯₂⁰(V) 中由 F₂¹(𝒜)(v₁, v₂) = 𝒜(v₂^F, v₁) 给出。

例 6.2.3。 对于 𝒯₂²(V) 中的张量 𝒜，张量 F₁²(𝒜) 在 𝒯₃¹(V) 中由下式给出

F₁²(𝒜)(η¹, v₁, v₂, v₃) = 𝒜(η¹, v₁^F, v₂, v₃).

由定理 3.3.1(a)，hⱼ^F = Σₚ gⱼₚ θᵖ，所以 F₁²(𝒜) 具有分量

```
F₁²(𝒜)ⁱ_{jkl} = F₁²(𝒜)(θⁱ, hⱼ, hₖ, hₗ) = 𝒜(θⁱ, hⱼ^F, hₖ, hₗ)
= 𝒜(θⁱ, Σₚ gⱼₚ θᵖ, hₖ, hₗ) = Σₚ gⱼₚ 𝒜(θⁱ, θᵖ, hₖ, hₗ)
= Σₚ gⱼₚ 𝒜ⁱᵖ_{kl}
```

关于 ℋ。

定理 6.2.4。 设 (V, g) 为标量积空间，设 ℋ 为 V 的基，并设 g_ℋ = [gᵢⱼ]。设 𝒜 为 𝒯ₛʳ(V) 中的张量，并设 1 ≤ k ≤ r 且 1 ≤ l ≤ s+1 为整数。则 𝒯_{s+1}^{r-1}(V) 中的张量 Fₗᵏ(𝒜) 具有分量

```
Fₗᵏ(𝒜)^{i₁...i_{k-1} p i_k...i_{r-1}}_{j₁...j_{l-1} j_{l+1}...j_{s+1}} = Σₚ gⱼₗₚ 𝒜^{i₁...i_{k-1} p i_k...i_{r-1}}_{j₁...ĵₗ...j_{s+1}}
```

关于 ℋ。

证明。证明是例 6.2.3 的详细阐述。我们仅考虑 [F2] 的情形。𝒯_{s+1}^{r-1}(V) 中的张量 Fₗᵏ(𝒜) 由下式给出

```
Fₗᵏ(𝒜)(η¹,...,η^{r-1}, v₁,...,v_{s+1})
= 𝒜(η¹,...,η^{k-1}, vₗ^F, η^k,...,η^{r-1}, v₁,...,v̂ₗ,...,v_{s+1})
```

对所有余向量 η¹,...,η^{r-1} 在 V* 中以及所有向量 v₁,...,v_{s+1} 在 V 中成立。设 ℋ = (h₁,...,h_m)，并设 (θ¹,...,θ^m) 为其对偶基。由定理 3.3.1(a)，hⱼ^F = Σₚ gⱼₚ θᵖ，所以 Fₗᵏ(𝒜) 具有分量

```
Fₗᵏ(𝒜)^{i₁...i_{r-1}}_{j₁...j_{s+1}} = Fₗᵏ(𝒜)(θ^{i₁},...,θ^{i_k},...,θ^{i_{r-1}}, h_{j₁},...,h_{j_{s+1}})
= 𝒜(θ^{i₁},...,θ^{i_{k-1}}, h_{jₗ}^F, θ^{i_k},...,θ^{i_{r-1}}, h_{j₁},...,h_{j_{l-1}}, h_{j_{l+1}},...,h_{j_{s+1}})
= 𝒜(θ^{i₁},...,θ^{i_{k-1}}, Σₚ gⱼₗₚ θᵖ, θ^{i_k},...,θ^{i_{r-1}}, h_{j₁},...,h_{j_{l-1}}, h_{j_{l+1}},...,h_{j_{s+1}})
= Σₚ gⱼₗₚ 𝒜(θ^{i₁},...,θ^{i_{k-1}}, θᵖ, θ^{i_k},...,θ^{i_{r-1}}, h_{j₁},...,h_{j_{l-1}}, h_{j_{l+1}},...,h_{j_{s+1}})
= Σₚ gⱼₗₚ 𝒜^{i₁...i_{k-1} p i_k...i_{r-1}}_{j₁...ĵₗ...j_{s+1}}
```

关于 ℋ。

## 6.2 平坦映射

**定理 6.2.5.**

设 $(V,\mathfrak g)$ 为标量积空间，设 $\mathcal H$ 为 $V$ 的基，并设 $\mathfrak g=[g_{ij}]$。设 $\mathcal A$ 为 $T^r_s(V)$ 中的张量，并设 $r\ge 2$ 且 $s>0$ 为整数。则：

- (a) 对于整数 $1\le k<l\le r$ 和 $1\le n\le s$，张量 $C_n^k\circ F_n^l(\mathcal A)$ 在 $T^{r-2}_{s}(V)$ 中关于 $\mathcal H$ 的分量为

$$C_n^k\circ F_n^l(\mathcal A)^{i_1\cdots i_{r-2}}_{j_1\cdots j_s}=\sum_{pq}g_{pq}\mathcal A^{i_1\cdots i_{k-1}\,p\,i_{k}\cdots i_{l-2}\,q\,i_{l-1}\cdots i_{r-2}}_{j_1\cdots j_s}. \tag{6.2.1}$$

- (b) 对于整数 $1\le k\le r$ 和 $1\le n\le s$，张量 $C_n^k\circ F_n^k(\mathcal A)$ 在 $T^{r-2}_{s}(V)$ 中关于 $\mathcal H$ 的分量为

$$C_n^k\circ F_n^k(\mathcal A)^{i_1\cdots i_{r-2}}_{j_1\cdots j_s}=C_n^k\circ F_n^{k+1}(\mathcal A)^{i_1\cdots i_{r-2}}_{j_1\cdots j_s}$$
$$=\sum_{pq}g_{pq}\mathcal A^{i_1\cdots i_{k-1}\,p\,q\,i_k\cdots i_{r-2}}_{j_1\cdots j_s}. \tag{6.2.2}$$

**证明.** 由定理 6.2.4，张量 $F_n^l(\mathcal A)$ 在 $T^{r-1}_{s+1}(V)$ 中关于 $\mathcal H$ 的分量为

$$F_n^l(\mathcal A)^{i_1\cdots i_{r-1}}_{j_1\cdots j_{s+1}}=\sum_q g_{j_nq}\mathcal A^{i_1\cdots i_{l-1}\,q\,i_l\cdots i_{r-1}}_{j_1\cdots \widehat{j_n}\cdots j_{s+1}}. \tag{6.2.3}$$

(a): 我们有

$$C_n^k\circ F_n^l(\mathcal A)^{i_1\cdots\widehat{i_k}\cdots i_{r-1}}_{j_1\cdots\widehat{j_n}\cdots j_{s+1}}=\sum_pF_n^l(\mathcal A)^{i_1\cdots i_{k-1}\,p\,i_{k+1}\cdots i_{r-1}}_{j_1\cdots j_{n-1}\,p\,j_{n+1}\cdots j_{s+1}}$$

$$=\sum_p\left(\sum_qg_{pq}\mathcal A^{i_1\cdots i_{k-1}\,p\,i_{k+1}\cdots i_{l-1}\,q\,i_l\cdots i_{r-1}}_{j_1\cdots\widehat{j_n}\cdots j_{s+1}}\right)$$

$$=\sum_{pq}g_{pq}\mathcal A^{i_1\cdots i_{k-1}\,p\,i_{k+1}\cdots i_{l-1}\,q\,i_l\cdots i_{r-1}}_{j_1\cdots\widehat{j_n}\cdots j_{s+1}}.$$

重标指标 $(i_{k+1},\ldots,i_{l-1},q,i_l,\ldots,i_{r-1})$ 为 $(i_k,\ldots,i_{l-2},q,i_{l-1},\ldots,i_{r-2})$，以及 $(j_{n+1},\ldots,j_{s+1})$ 为 $(j_n,\ldots,j_s)$，得到结果。

(b): 我们有

$$C_n^k\circ F_n^k(\mathcal A)^{i_1\cdots\widehat{i_k}\cdots i_{r-1}}_{j_1\cdots\widehat{j_n}\cdots j_{s+1}}=\sum_pF_n^k(\mathcal A)^{i_1\cdots i_{k-1}\,p\,i_{k+1}\cdots i_{r-1}}_{j_1\cdots j_{n-1}\,p\,j_{n+1}\cdots j_{s+1}}$$

$$=\sum_p\left(\sum_qg_{pq}\mathcal A^{i_1\cdots i_{k-1}\,q\,p\,i_{k+1}\cdots i_{r-1}}_{j_1\cdots\widehat{j_n}\cdots j_{s+1}}\right)$$

$$=\sum_{pq}g_{pq}\mathcal A^{i_1\cdots i_{k-1}\,q\,p\,i_{k+1}\cdots i_{r-1}}_{j_1\cdots\widehat{j_n}\cdots j_{s+1}}$$

$$=\sum_{pq}g_{qp}\mathcal A^{i_1\cdots i_{k-1}\,p\,q\,i_{k+1}\cdots i_{r-1}}_{j_1\cdots\widehat{j_n}\cdots j_{s+1}}.$$





and

C_n^k ∘ F_n^{k+1}(𝒜)^{i_1...î_k...i_{r-1}}_{j_1...ĵ_n...j_{s+1}} = Σ_p F_n^{k+1}(𝒜)^{i_1...i_{k-1} p i_{k+1}...i_{r-1}}_{j_1...j_n-1 p j_{n+1}...j_{s+1}} [Th 5.4.5] = Σ_p ( Σ_q 𝔤_{pq} 𝒜^{i_1...i_{k-1} p q i_{k+1}...i_{r-1}}_{j_1...j_n...j_{s+1}} ) [(6.2.3)] = Σ_{pq} 𝔤_{pq} 𝒜^{i_1...i_{k-1} p q i_{k+1}...i_{r-1}}_{j_1...j_n...j_{s+1}},

这给出了第一个等式。重新标记指标 (i_{k+1},...,i_{r-1}) 为 (i_k,...,i_{r-2})，并将 (j_{n+1},...,j_{s+1}) 重新标记为 (j_m,...,j_s)，得到第二个等式。

我们观察到 p 和 q 在 (6.2.1) 中分别位于 𝒜 的第 k 个和第 l 个逆变位置，而 p 和 q 在 (6.2.2) 中分别位于 𝒜 的第 k 个和第 (k+1) 个逆变位置。我们还注意到 (6.2.1) 右边与 n 无关。这是有意义的，因为 F_n^l 将第 l 个逆变指标降低到第 n 个协变位置，然后 C_n^k 对第 k 个逆变指标和第 n 个协变指标进行收缩。可以说，第 n 个协变位置只是降低后的第 l 个逆变指标在与第 k 个逆变指标收缩之前暂时驻留的位置。

## 定理 6.2.6. 设 (V,𝔤) 为数量积空间，设 𝒜 为 𝒯_s^r(V) 中的张量，且 1 ≤ k ≤ r 为整数。则

F_1^k(𝒜) = C_1^k(𝔤 ⊗ 𝒜).

证明。以下所有分量均相对于 V 的基 ℋ 计算。根据定理 6.2.4，张量 F_1^k(𝒜) 在 𝒯_{s+1}^{r-1}(V) 中具有分量

F_1^k(𝒜)^{i_1...i_{r-1}}_{j_1...j_{s+1}} = Σ_p 𝔤_{j_1 p} 𝒜^{i_1...i_{k-1} p i_k...i_{r-1}}_{j_2...j_{s+1}}. (6.2.4)

根据定理 5.1.5，张量 𝔤 ⊗ 𝒜 在 𝒯_{s+2}^r(V) 中具有分量

(𝔤 ⊗ 𝒜)^{i_1...i_r}_{j_1...j_{s+2}} = 𝔤_{j_1 j_2} 𝒜^{i_1...i_r}_{j_3...j_{s+1}},

然后根据定理 5.4.5，张量 C_1^k(𝔤 ⊗ 𝒜) 在 𝒯_{s+1}^{r-1}(V) 中具有分量

C_1^k(𝔤 ⊗ 𝒜)^{i_1...î_k...i_r}_{j_2...j_{s+2}} = Σ_p 𝔤_{p j_2} 𝒜^{i_1...i_{k-1} p i_{k+1}...i_r}_{j_3...j_{s+2}}.

重新标记 (i_{k+1},...,i_r) 为 (i_k,...,i_{r-1})，并将 (j_2,...,j_{s+2}) 重新标记为 (j_1,...,j_{s+1})，得到

C_1^k(𝔤 ⊗ 𝒜)^{i_1...i_{r-1}}_{j_1...j_{s+1}} = Σ_p 𝔤_{p j_1} 𝒜^{i_1...i_{k-1} p i_k...i_{r-1}}_{j_2...j_{s+1}}. (6.2.5)

结果现在由 (6.2.4) 和 (6.2.5) 推出。

## 6.3 尖映射

设 $(V,\mathfrak{g})$ 为数量积空间，设 $\mathcal{H} = (h_1,\dots,h_m)$ 为 V 的基，设 $(\theta^1,\dots,\theta^m)$ 为其对偶基。回顾 (5.1.1) 中 $\mathcal{T}_0^1(V)=V$ 且 $\mathcal{T}_1^0(V)=V^*$。尖映射因此可以表示为

S : $\mathcal{T}_1^0(V) \longrightarrow \mathcal{T}_0^1(V)$.

更一般地，设 $1 \le k \le r+1$ 且 $1 \le l \le s$ 为整数（从而 $r \ge 0$ 且 $s \ge 1$）。$(\boldsymbol{k},\boldsymbol{l})$-尖映射记为

S$_l^k : \mathcal{T}_s^r(V) \longrightarrow \mathcal{T}_{s-1}^{r+1}(V)$

并对所有张量 $\mathcal{A}$ 在 $\mathcal{T}_s^r(V)$ 中，所有余向量 $\eta^1,\dots,\eta^{r+1}$ 在 $V^*$ 中，以及所有向量 $v_1,\dots,v_{s-1}$ 在 $V$ 中定义如下，其中 $\widehat{\phantom{x}}$ 表示省略表达式：

[S1] 对于 $s = 1$（从而 $l = 1$）：

S$_1^k(\mathcal{A})(\eta^1,\dots,\eta^{r+1}) = \mathcal{A}(\eta^1,\dots,\widehat{\eta^k},\dots,\eta^{r+1},\eta^{k\sharp})$

[S2] 对于 $s \ge 2$ 且 $1 \le l \le s-1$：

S$_l^k(\mathcal{A})(\eta^1,\dots,\eta^{r+1},v_1,\dots v_{s-1}) = \mathcal{A}(\eta^1,\dots,\widehat{\eta^k},\dots,\eta^{r+1},v_1,\dots,v_{l-1},\eta^{k\sharp},v_l,\dots,v_{s-1})$

[S3] 对于 $s \ge 2$ 且 $l = s$：

S$_s^k(\mathcal{A})(\eta^1,\dots,\eta^{r+1},v_1,\dots,v_{s-1}) = \mathcal{A}(\eta^1,\dots,\widehat{\eta^k},\dots,\eta^{r+1},v_1,\dots,v_{s-1},\eta^{k\sharp})$

计算过程对 [S2] 最为透明：提取第 $k$ 个逆变位置中的余向量，对其进行尖化，并将结果插入第 $l$ 个协变位置。

## 例 6.3.1. 为说明 [S1]，设 $r = 0$（从而 $k = 1$）且 $s = 1$（从而 $l = 1$），并设 $\zeta$ 为 $\mathcal{T}_1^0(V)=V^*$ 中的余向量。则张量 S$_1^1(\zeta)$ 在 $\mathcal{T}_0^1(V)=V$ 中由

S$_1^1(\zeta)(\eta) = \zeta(\eta^{\sharp})$

对所有 $V^*$ 中的余向量 $\eta$ 给出。我们由 (1.2.3) 和 (4.5.2) 得

$\zeta(\eta^{\sharp}) = \langle \zeta,\eta \rangle^* = \langle \eta,\zeta \rangle^* = \eta(\zeta^{\sharp}) = \zeta^{\sharp}(\eta) = S(\zeta)(\eta)$

故 S$_1^1(\zeta)(\eta) = S(\zeta)(\eta)$。由于 $\eta$ 和 $\zeta$ 为任意，

S$_1^1 = S.$

为说明 [S2]，设 $r = 0$（从而 $k = 1$）且 $s = 2$（从而 $l = 1$），并设 $\mathcal{A}$ 为 $\mathcal{T}_2^0(V)$ 中的张量。则张量 S$_1^1(\mathcal{A})$ 在 $\mathcal{T}_1^1(V)$ 中由

S$_1^1(\mathcal{A})(\eta, v) = \mathcal{A}(\eta^{\sharp}, v)$.

给出。

为说明 [S3]，设 $r=0$（从而 $k=1$）且 $s=2$（从而 $l=2$），并设 $\mathcal A$ 为 $\mathcal T^0_2(V)$ 中的张量。则张量 $S^1_2(\mathcal A)$ 在 $\mathcal T^1_1(V)$ 中由

$$S^1_2(\mathcal A)(\eta,v)=\mathcal A(v,\eta^S).$$

给出。

**例 6.3.2.** 设 $\mathcal A$ 为 $\mathcal T^1_1(V)$ 中的张量。对于 $r=s=1$，存在 $k=1,2$ 且 $l=1$ 的尖映射。对于 $k=1$ 且 $l=1$，张量 $S^1_1(\mathcal A)$ 在 $\mathcal T^0_2(V)$ 中由

$$S^1_1(\mathcal A)(\eta^1,\eta^2)=\mathcal A(\eta^2,\eta^{1S}).$$

给出。对于 $k=2$ 且 $l=1$，张量 $S^2_1(\mathcal A)$ 在 $\mathcal T^0_2(V)$ 中由

$$S^2_1(\mathcal A)(\eta^1,\eta^2)=\mathcal A(\eta^1,\eta^{2S}).$$

给出。

**例 6.3.3.** 对于 $\mathcal T^1_3(V)$ 中的张量 $\mathcal B$，张量 $S^2_1(\mathcal B)$ 在 $\mathcal T^2_2(V)$ 中由

$$S^2_1(\mathcal B)(\eta^1,\eta^2,v_1,v_2)=\mathcal B(\eta^1,\eta^{2S},v_1,v_2).$$

给出。根据定理 4.5.3(a)，$\theta^jS=\sum_p g^{jp}h_p$，所以 $S^2_1(\mathcal B)$ 具有分量

$$S^2_1(\mathcal B)^{ij}_{kl}=S^2_1(\mathcal B)(\theta^i,\theta^j,h_k,h_l)=\mathcal B(\theta^i,\theta^{jS},h_k,h_l)$$
$$=\mathcal B\left(\theta^i,\sum_p g^{jp}h_p,h_k,h_l\right)=\sum_p g^{jp}\mathcal B(\theta^i,h_p,h_k,h_l)$$
$$=\sum_p g^{jp}\mathcal B^i_{pkl}.$$

相对于 $\mathcal H$。

## 定理 6.3.4. 设 $(V,\mathfrak g)$ 为数量积空间，设 $\mathcal H$ 为 V 的基，并设 $g_{\mathcal H}^{-1}=[g^{ij}]$。设 $\mathcal A$ 为 $\mathcal T^r_s(V)$ 中的张量，并设 $1\leq k\leq r+1$ 且 $1\leq l\leq s$ 为整数。则张量 $S^k_l(\mathcal A)$ 在 $\mathcal T^{r+1}_{s-1}(V)$ 中具有分量

$$S^k_l(\mathcal A)^{i_1\cdots i_{r+1}}_{j_1\cdots j_{s-1}}=\sum_p g^{i_kp}\mathcal A^{i_1\cdots \widehat{i_k}\cdots i_{r+1}}_{j_1\cdots j_{l-1}\,p\,j_l\cdots j_{s-1}}.$$

相对于 $\mathcal H$。

**证明.** 证明是对例 6.3.3 的阐述。我们仅考虑 [S2] 的情形。张量 $S^k_l(\mathcal A)$ 在 $\mathcal T^{r+1}_{s-1}(V)$ 中由

$$S^k_l(\mathcal A)(\eta^1,\ldots,\eta^{r+1},v_1,\ldots,v_{s-1})$$
$$=\mathcal A(\eta^1,\ldots,\widehat{\eta^k},\ldots,\eta^{r+1},v_1,\ldots,v_{l-1},\eta^{kS},v_{l+1},\ldots,v_{s-1})$$

对所有 $V^*$ 中的余向量 $\eta^1,\ldots,\eta^{r+1}$ 和所有 $V$ 中的向量 $v_1,\ldots,v_{s+1}$ 给出。设 $\mathcal H=(h_1,\ldots,h_m)$，并设 $(\theta^1,\ldots,\theta^m)$ 为其对偶基。根据定理 4.5.3(a)，$\theta^jS=$

∑ₚ 𝔤ⁱᵏᵖ hₚ, 所以 Sₗᵏ(𝒜) 具有分量

Sₗᵏ(𝒜)ⁱ¹...ⁱʳ⁺¹
j₁...jₛ₋₁
= Sₗᵏ(𝒜)(θⁱ¹, ..., θⁱᵏ, ..., θⁱʳ⁺¹, hⱼ₁, ..., hⱼₗ, ..., hⱼₛ₋₁)
= 𝒜(θⁱ¹, ..., θⁱᵏ⁻¹, θⁱᵏ⁺¹, ..., θⁱʳ⁺¹, hⱼ₁, ..., hⱼₗ₋₁, θⁱᵏˢ, hⱼₗ, ..., hⱼₛ₋₁)
= 𝒜( θⁱ¹, ..., θⁱᵏ⁻¹, θⁱᵏ⁺¹, ..., θⁱʳ⁺¹, hⱼ₁, ..., hⱼₗ₋₁, ∑ₚ 𝔤ⁱᵏᵖ hₚ, hⱼₗ, ..., hⱼₛ₋₁ )
= ∑ₚ 𝔤ⁱᵏᵖ 𝒜(θⁱ¹, ..., θⁱᵏ⁻¹, θⁱᵏ⁺¹, ..., θⁱʳ⁺¹, hⱼ₁, ..., hⱼₗ₋₁, hₚ, hⱼₗ, ..., hⱼₛ₋₁)
= ∑ₚ 𝔤ⁱᵏᵖ 𝒜ⁱ₁...îᵢₖ...ⁱʳ⁺¹
j₁...jₗ₋₁ p jₗ...jₛ₋₁

相对于 ℋ。

## 定理 6.3.5. 设 (V,𝔤) 为数量积空间，设 ℋ 为 V 的基，并设 𝔤_ℋ⁻¹ = [𝔤ⁱʲ]。设 𝒜 为 𝒯ₛʳ(V) 中的张量，并设 r ≥ 0 且 s ≥ 2 为整数。则：

- (a) 对于整数 1 ≤ k < l ≤ s 且 1 ≤ n ≤ r，张量 Cₖⁿ ∘ Sₗⁿ(𝒜) 在 𝒯ₛ₋₂ʳ(V) 中相对于 ℋ 的分量为
Cₖⁿ ∘ Sₗⁿ(𝒜)ⁱ₁...ⁱʳ
j₁...jₛ₋₂ = ∑ₚq 𝔤ᵖq 𝒜ⁱ₁...ⁱʳ
j₁...jₖ₋₁ p jₖ...jₗ₋₂ q jₗ₋₁...jₛ₋₂. (6.3.2)

- (b) 对于整数 1 ≤ k ≤ s 且 1 ≤ n ≤ r，张量 Cₖⁿ ∘ Sₖⁿ(𝒜) 在 𝒯ₛ₋₂ʳ(V) 中相对于 ℋ 的分量为
Cₖⁿ ∘ Sₖⁿ(𝒜)ⁱ₁...ⁱʳ
j₁...jₛ₋₂ = Cₖⁿ ∘ Sₖ₊₁ⁿ(𝒜)ⁱ₁...ⁱʳ
j₁...jₛ₋₂
= ∑ₚq 𝔤ᵖq 𝒜ⁱ₁...ⁱʳ
j₁...jₖ₋₁ p q jₖ...jₛ₋₂. (6.3.3)

证明。由定理 6.3.4，张量 Sₗⁿ(𝒜) 在 𝒯ₛ₋₁ʳ⁺¹(V) 中相对于 ℋ 的分量为
Sₗⁿ(𝒜)ⁱ₁...ⁱʳ⁺¹
j₁...jₛ₋₁ = ∑_q 𝔤ⁱₙ q 𝒜ⁱ₁...îₙ...ⁱʳ⁺¹
j₁...jₗ₋₁ q jₗ...jₛ₋₁. (6.3.4)

(a): 我们有
Cₖⁿ ∘ Sₗⁿ(𝒜)ⁱ₁...îₙ...ⁱʳ⁺¹
j₁...ĵₗ...jₛ₋₁ = ∑ₚ Sₗⁿ(𝒜)ⁱ₁...ⁱₙ₋₁ p îₙ₊₁...ⁱʳ⁺¹
j₁...jₖ₋₁ p jₖ₊₁...jₛ₋₁ [Th 5.4.5]
= ∑ₚ ( ∑_q 𝔤ᵖq 𝒜ⁱ₁...îₙ...ⁱʳ⁺¹
j₁...jₖ₋₁ p jₖ₊₁...jₗ₋₁ q jₗ...jₛ₋₁ ) [(6.3.4)]
= ∑ₚq 𝔤ᵖq 𝒜ⁱ₁...îₙ...ⁱʳ⁺¹
j₁...jₖ₋₁ p jₖ₊₁...jₗ₋₁ q jₗ...jₛ₋₁.





将指标 $(i_{n+1},\ldots,i_{r+1})$ 重新标记为 $(i_n,\ldots,i_r)$，并将 $(j_{k+1},\ldots,j_{s-1},q,j_l,\ldots,j_{s-1})$ 重新标记为 $(j_k,\ldots,j_{l-2},q,j_{l-1},\ldots,j_{s-2})$，即得结果。

**(b)：** 我们有

$$C_k^n\circ S_k^n(\mathcal A)^{i_1\ldots\widehat{i_k}\ldots i_{r+1}}_{j_1\ldots\widehat{j_k}\ldots j_{s-1}}=\sum_p S_k^n(\mathcal A)^{i_1\ldots i_{n-1}\,p\,i_{n+1}\ldots i_{r+1}}_{j_1\ldots j_{k-1}\,p\,j_{k+1}\ldots j_{s-1}}$$

$$=\sum_p\left(\sum_q g^{pq}\mathcal A^{i_1\ldots\widehat{i_n}\ldots i_{r+1}}_{j_1\ldots j_{k-1}\,q\,j_{k+1}\ldots j_{s-1}}\right)$$

$$=\sum_{pq}g^{pq}\mathcal A^{i_1\ldots\widehat{i_n}\ldots i_{r+1}}_{j_1\ldots j_{k-1}\,q\,j_{k+1}\ldots j_{s-1}}$$

$$=\sum_{pq}g^{pq}\mathcal A^{i_1\ldots\widehat{i_n}\ldots i_{r+1}}_{j_1\ldots j_{k-1}\,p\,q\,j_{k+1}\ldots j_{s-1}}$$

以及

$$C_k^n\circ S_{k+1}^n(\mathcal A)^{i_1\ldots\widehat{i_k}\ldots i_{r+1}}_{j_1\ldots\widehat{j_k}\ldots j_{s-1}}=\sum_p S_{k+1}^n(\mathcal A)^{i_1\ldots i_{n-1}\,p\,i_{n+1}\ldots i_{r+1}}_{j_1\ldots j_{k-1}\,p\,j_{k+1}\ldots j_{s-1}}$$

$$=\sum_p\left(\sum_q g^{pq}\mathcal A^{i_1\ldots i_{n-1}\,i_{n+1}}_{j_1\ldots j_{k-1}\,p\,q\,j_{k+1}\ldots j_{s-1}}\right)$$

$$=\sum_{pq}g^{pq}\mathcal A^{i_1\ldots i_{n-1}\,i_{n+1}}_{j_1\ldots j_{k-1}\,p\,q\,j_{k+1}\ldots j_{s-1}},$$

这给出了第一个等式。将指标 $(i_{n+1},\ldots,i_{r+1})$ 重新标记为 $(i_n,\ldots,i_r)$，并将 $(j_{k+1},\ldots,j_{s-1})$ 重新标记为 $(j_k,\ldots,j_{s-2})$，即得第二个等式。 ■

我们观察到，在 (6.3.2) 中 $p$ 和 $q$ 分别位于 $\mathcal A$ 的第 $k$ 和第 $l$ 个协变位置，而在 (6.3.3) 中 $p$ 和 $q$ 分别位于 $\mathcal A$ 的第 $k$ 和第 $(k+1)$ 个协变位置。我们还注意到，(6.3.2) 的右边与 $n$ 无关。参见定理 6.2.5 之后的相应评注。

**示例 6.3.6.** 对于 $\mathcal T^2_2(V)$ 中的张量 $\mathcal A$，由示例 6.2.3 我们有

$$F_1^2(\mathcal A)^{ij}_{pkl}=\sum_q g_{pq}\mathcal A^{iq}_{kl}.$$

将 $\mathcal B=F_1^2(\mathcal A)$（如上式所给出）代入 (6.3.1)，得到

$$S_1^2\circ F_1^2(\mathcal A)^{ij}_{kl}=\sum_p g^{jp}F_1^2(\mathcal A)^i_{pkl}=\sum_p g^{jp}\left(\sum_q g_{pq}\mathcal A^{iq}_{kl}\right)$$
$$=\sum_q\left(\sum_p g^{jp}g_{pq}\right)\mathcal A^{iq}_{kl}=\sum_q\delta^j_q\mathcal A^{iq}_{kl}=\mathcal A^{ij}_{kl}.$$

因此，$S_1^2\circ F_1^2(\mathcal A)=\mathcal A$。类似地，$F_1^2\circ S_1^2(\mathcal A)=\mathcal A$。由于 $\mathcal A$ 是任意的，由定理 A.4 可知 $F_1^2$ 和 $S_1^2$ 互为逆映射，因而是线性同构。 ◇

### 定理 6.3.7.

若 $(V,\mathfrak g)$ 是标量积空间，且 $1\leq k\leq r$、$1\leq l\leq s$ 为整数，则 $F_l^k$ 和 $S_l^k$ 是互为逆的线性同构。

**证明。** 该证明是示例 6.3.6 的细化。设 $\mathcal H$ 为 $V$ 的一组基，$\mathcal A$ 和 $\mathcal B$ 分别为 $\mathcal T_s^r(V)$ 和 $\mathcal T_{s+1}^{r-1}(V)$ 中的张量，并设 $\mathfrak g_\mathcal H=[g_{ij}]$。由定理 6.2.4，$F_l^k(\mathcal A)$ 是 $\mathcal T_{s+1}^{r-1}(V)$ 中的张量，其关于 $\mathcal H$ 的分量为

$$F_l^k(\mathcal A)^{i_1\ldots i_{k-1}i_k}_{j_1\ldots j_{s+1}}=\sum_q g_{j_1q}\mathcal A^{i_1\ldots i_{k-1}q i_{k+1}\ldots i_r}_{j_2\ldots j_{s+1}}$$

将指标 $(i_k,\ldots,i_{r-1})$ 重新标记为 $(i_{k+1},\ldots,i_r)$，并将 $(j_1,\ldots,j_{s+1})$ 重新标记为 $(p,j_1,\ldots,j_s)$，得到

$$F_l^k(\mathcal A)^{i_1\ldots i_k\ldots i_r}_{j_1\ldots j_{l-1}p j_{l+1}\ldots j_s}=\sum_q g_{pq}\mathcal A^{i_1\ldots i_{k-1}q i_{k+1}\ldots i_r}_{j_1\ldots j_s}. \tag{6.3.5}$$

由定理 6.3.4，$\mathcal T_s^r(V)$ 中的张量 $S_l^k(\mathcal B)$ 关于 $\mathcal H$ 的分量为

$$S_l^k(\mathcal B)^{i_1\ldots i_r}_{j_1\ldots j_s}=\sum_p g^{i_kp}\mathcal B^{i_1\ldots i_{k-1} \widehat{i_k} i_{k+1}\ldots i_r}_{j_1\ldots j_{l-1}p j_{l+1}\ldots j_s}. \tag{6.3.6}$$

将 $\mathcal B=F_l^k(\mathcal A)$（由 (6.3.5) 给出）代入 (6.3.6)，得到

$$\begin{aligned}
S_l^k\circ F_l^k(\mathcal A)^{i_1\ldots i_r}_{j_1\ldots j_s}
&=\sum_p g^{i_kp}F_l^k(\mathcal A)^{i_1\ldots \widehat{i_k}\ldots i_r}_{j_1\ldots j_{l-1}p j_{l+1}\ldots j_s}\\
&=\sum_p g^{i_kp}\left(\sum_q g_{pq}\mathcal A^{i_1\ldots i_{k-1}q i_{k+1}\ldots i_r}_{j_1\ldots j_s}\right)\\
&=\sum_q\left(\sum_p g^{i_kp}g_{pq}\right)\mathcal A^{i_1\ldots i_{k-1}q i_{k+1}\ldots i_r}_{j_1\ldots j_s}\\
&=\sum_q\delta_q^{i_k}\mathcal A^{i_1\ldots i_{k-1}q i_{k+1}\ldots i_r}_{j_1\ldots j_s}\\
&=\mathcal A^{i_1\ldots i_{k-1}i_k i_{k+1}\ldots i_r}_{j_1\ldots j_s}.
\end{aligned}$$

证明的其余部分使用示例 6.3.6 中给出的论证。

## 6.4 张量的表示

第 5.3 节讨论了向量空间上张量的表示问题。现在我们将该讨论推广到标量积空间上的张量。

设 $(V,\mathfrak g)$ 为标量积空间，$\mathcal H=(h_1,\ldots,h_m)$ 为 $V$ 的一组基，$(\theta^1,\ldots,\theta^m)$ 为其对偶基，并设 $\mathfrak g_\mathcal H=[g_{ij}]$。遵循第 B.5 节和第 B.6 节，我们用 $\operatorname{Mult}(V^s,V)$ 表示从 $V^s$ 到 $V$ 的 $\mathbb R$-多重线性映射构成的向量空间。我们定义一个映射

$$\widetilde S_s:\operatorname{Mult}(V^s,V)\longrightarrow\mathcal T^0_{s+1}(V),$$

称之为 **标量积映射**，定义为

$$\mathcal{S}_s(\Psi)(v_1,\ldots,v_{s+1})=\langle v_1,\Psi(v_2,\ldots,v_{s+1})\rangle \tag{6.4.1}$$

其中 $\Psi$ 为 $\operatorname{Mult}(V^s,V)$ 中的任意映射，$v_1,\ldots,v_{s+1}$ 为 $V$ 中的任意向量。当 $s=1$ 时，我们将

$$\mathcal{S}_1\quad\text{记作}\quad\mathcal{S}.$$

### 定理 6.4.1（张量的表示）。*使用上述设置：*

+   (a) $\mathcal{S}_s$ 是一个线性同构：
+   
+   $$\mathcal{T}^{0}_{s+1}(V)\cong\operatorname{Mult}(V^s,V).$$

+   (b) *对于* $\operatorname{Mult}(V^s,V)$ *中的所有映射* $\Psi$，张量 $\mathcal{S}_s(\Psi)$ *在* $\mathcal{T}^{0}_{s+1}(V)$ *中关于* $\mathcal{H}$ *的分量为*
+   
+   $$\mathcal{S}_s(\Psi)_{j_1\ldots j_{s+1}}=\sum_i g_{ij_1}\,\theta^i\bigl(\Psi(h_{j_2},\ldots,h_{j_{s+1}})\bigr).$$

+   (c) *对于* $\mathcal{T}^{0}_{s+1}(V)$ *中的所有张量* $\mathcal{A}$，
+   
+   $$\mathcal{S}_s^{-1}(\mathcal{A})(h_{j_1},\ldots,h_{j_s})=\sum_i \mathcal{S}_1^1(\mathcal{A})_{j_1\ldots j_s i}h_i,$$
+   
+   其中 $\mathcal{S}_1^1$ 在第 6.3 节中定义。

+   (d)
+   
+   $$\mathcal{R}_s=\mathcal{S}_1^1\circ\mathcal{S}_s,$$
+   
+   其中 $\mathcal{R}_s$ 由 (5.3.1) 给出。

**证明。** (a)：容易证明 $\mathcal{S}_s$ 是线性映射。若 $\mathcal{S}_s(\Psi)=0$，则对于 $V$ 中的所有向量 $v_1,\ldots,v_{s+1}$，有 $\langle\Psi(v_2,\ldots,v_{s+1}),v_1\rangle=0$。由于 $\mathfrak{g}$ 非退化，我们有 $\Psi(v_2,\ldots,v_{s+1})=0$，又因为 $v_2,\ldots,v_{s+1}$ 是任意的，所以 $\Psi=0$。因此，$\ker(\mathcal{S}_s)=\{0\}$。由定理 5.1.3(c)，$\mathcal{T}^{0}_{s+1}(V)$ 的维数为 $m^{s+1}$，且可以证明 $\operatorname{Mult}(V^s,V)$ 具有相同维数。由定理 1.1.12 和定理 1.1.14 可知，$\mathcal{S}_s$ 是线性同构。

(b)：由于 $\Psi(h_{j_2},\ldots,h_{j_{s+1}})$ 是 $V$ 中的向量，由定理 1.2.1(d) 有

$$\Psi(h_{j_2},\ldots,h_{j_{s+1}})=\sum_i\theta^i\bigl(\Psi(h_{j_2},\ldots,h_{j_{s+1}})\bigr)h_i,$$

再由 (6.4.1) 有

$$\begin{aligned}
\mathcal{S}_s(\Psi)_{j_1\ldots j_{s+1}}&=\mathcal{S}_s(\Psi)(h_{j_1},\ldots,h_{j_{s+1}})=\langle h_{j_1},\Psi(h_{j_2},\ldots,h_{j_{s+1}})\rangle\\
&=\left\langle h_{j_1},\sum_i\theta^i\bigl(\Psi(h_{j_2},\ldots,h_{j_{s+1}})\bigr)h_i\right\rangle\\
&=\sum_i g_{ij_1}\,\theta^i\bigl(\Psi(h_{j_2},\ldots,h_{j_{s+1}})\bigr).
\end{aligned}$$

(c)：由于 𝔖_s⁻¹(𝒜) 是 Mult(V^s, V) 中的映射，所以 𝔖_s⁻¹(𝒜)(h_{j2},…,h_{js+1}) 是 V 中的向量。由定理 1.2.1(d) 有

𝔖_s⁻¹(𝒜)(h_{j2},…,h_{js+1}) = Σ_i θ^i(𝔖_s⁻¹(𝒜)(h_{j2},…,h_{js+1})) h_i.  (6.4.2)

于是

𝒜_{j1…js+1} = 𝒜(h_{j1},…,h_{js+1})
= 𝔖_s(𝔖_s⁻¹(𝒜))(h_{j1},…,h_{js+1})  [由 (a)]
= ⟨h_{j1}, 𝔖_s⁻¹(𝒜)(h_{j2},…,h_{js+1})⟩  [(6.4.1)]
= ⟨h_{j1}, Σ_i θ^i(𝔖_s⁻¹(𝒜)(h_{j2},…,h_{js+1})) h_i⟩  [(6.4.2)]
= Σ_i 𝔤_{ij1} θ^i(𝔖_s⁻¹(𝒜)(h_{j2},…,h_{js+1})),

因此

Σ_{j1} 𝔤^{ij1} 𝒜_{j1…js+1} = Σ_{j1} 𝔤^{ij1} ( Σ_k 𝔤_{kj1} θ^k(𝔖_s⁻¹(𝒜)(h_{j2},…,h_{js+1})) )
= Σ_k ( Σ_{j1} 𝔤^{ij1} 𝔤_{j1k} ) θ^k(𝔖_s⁻¹(𝒜)(h_{j2},…,h_{js+1}))
= Σ_k δ^i_k θ^k(𝔖_s⁻¹(𝒜)(h_{j2},…,h_{js+1}))
= θ^i(𝔖_s⁻¹(𝒜)(h_{j2},…,h_{js+1})),

所以

Σ_{ij1} 𝔤^{ij1} 𝒜_{j1…js+1} h_i = Σ_i θ^i(𝔖_s⁻¹(𝒜)(h_{j2},…,h_{js+1})) h_i.  (6.4.3)

由定理 6.3.4，

S₁¹(𝒜)^i_{j2…js+1} = Σ_{j1} 𝔤^{ij1} 𝒜_{j1…js+1},

因此

Σ_i S₁¹(𝒜)^i_{j2…js+1} h_i = Σ_{ij1} 𝔤^{ij1} 𝒜_{j1…js+1} h_i.  (6.4.4)

综合 (6.4.2)–(6.4.4) 得到

𝔖_s⁻¹(𝒜)(h_{j2},…,h_{js+1}) = Σ_i S₁¹(𝒜)^i_{j2…js+1} h_i,

并将指标 (j2,…,js+1) 重新标记为 (j1,…,js) 即得结果。




## 6.5 张量的度量缩并

(d)

𝔅 = S₁¹ ∘ 𝔖，

其中 𝔅 由 (5.3.2) 给出。

证明. (a)、(b)、(d)：这些可由定理 6.4.1 的相应部分推出。

(c)：由定理 6.4.1(c)，

𝔖⁻¹(𝒜)(hⱼ) = Σᵢ S₁¹(𝒜)ⁱⱼ hᵢ，

因此由 (2.2.2) 和 (2.2.3) 得

[𝔖⁻¹(𝒜)]ᴴᵧ = [S₁¹(𝒜)ⁱⱼ]。

由定理 6.3.4，

S₁¹(𝒜)ⁱⱼ = Σₚ 𝔤ⁱᵖ 𝒜ₚⱼ，

因此

[S₁¹(𝒜)ⁱⱼ] = [𝔤ⁱʲ][𝒜ᵢⱼ]。

结论由此得出。

对于向量空间上的张量，我们只能在一个逆变指标和一个协变指标上进行缩并。然而，对于标量积空间上的张量，平映射和尖映射使得在两个逆变指标或两个协变指标上进行缩并成为可能，由此得到的缩并称为度量缩并。

设 (V, 𝔤) 是标量积空间。下面的定义受定理 6.2.5 和定理 6.3.5 启发。

对于整数 r ≥ 2、s ≥ 0 以及 1 ≤ k < l ≤ r，定义 (k,l)-逆变度量缩并为线性映射

C^{kl} : 𝒯_s^r(V) ⟶ 𝒯_s^{r-2}(V)

其定义为

C^{kl} = C₁^k ∘ F₁^l. (6.5.1)

由定理 6.2.5(b) 可得

C^{k,k+1} = C₁^k ∘ F₁^k. (6.5.2)

对于整数 r ≥ 0、s ≥ 2 以及 1 ≤ k < l ≤ s，定义 (k,l)-协变度量缩并为线性映射

C_{kl} : 𝒯_s^r(V) ⟶ 𝒯_{s-2}^r(V)

其定义为

C_{kl} = C_k^1 \circ S_l^1.  (6.5.3)

由定理 6.3.5(b) 可得

C_{k,k+1} = C_k^1 \circ S_k^1.  (6.5.4)

为了避免将度量缩并与第 5.4 节中定义的缩并混淆，我们指出后者有时被称为普通缩并。

## 定理 6.5.1（度量缩并的标准正交基表达式）. 设 (V,𝔤) 是符号差为 (ε₁,…,ε_m) 的标量积空间，设 (e₁,…,e_m) 是 V 的标准正交基，并设 𝒜 是 𝒯_s^0(V) 中的张量。则：

- (a) 对于整数 s ≥ 3 和 1 ≤ k < l ≤ s，𝒯_{s-2}^0(V) 中的张量 C_{kl}(𝒜) 由下式给出

C_{kl}(𝒜)(v₁,…,v_{s-2}) = Σ_i ε_i 𝒜(v₁,…,v_{k-1},e_i,v_k,…,v_{l-2},e_i,v_{l-1},…,v_{s-2})  (6.5.5)

对所有 V 中的向量 v₁,…,v_{s-2} 成立。

- (b) 当 s = 2 时，实数 C₁₂(𝒜) 由下式给出：C₁₂(𝒜) = Σ_i ε_i 𝒜(e_i,e_i)。

证明. (a)：设 ℰ = (e₁,…,e_m)，𝔤ℰ = [𝔤_{ij}]，并注意 𝔤^{ij} = 𝔤_{ij} = ε_i δ_{ij}。由定理 6.3.5(a) 和 (6.5.3) 可得，C_{kl}(𝒜) 关于 ℋ 的分量为

C_{kl}(𝒜)_{j₁…j_{s-2}} = Σ_{pq} 𝔤^{pq} 𝒜_{j₁…j_{k-1}p j_k…j_{l-2}q j_{l-1}…j_{s-2}} = Σ_p ε_p 𝒜_{j₁…j_{k-1}p j_k…j_{l-2}p j_{l-1}…j_{s-2}} = Σ_p ε_p 𝒜(e_{j₁},…,e_{j_{k-1}},e_p,e_{j_k},…,e_{j_{l-2}},e_p,e_{j_{l-1}},…,e_{j_{s-2}})

任意向量 v₁,…,v_{s-2} ∈ V 时的结果由 𝒜 和 C_{kl}(𝒜) 的多重线性性得出。

(b)：我们有

C₁₂(𝒜) = C₁¹ ∘ S₁¹(𝒜)    [(6.5.4)]
= Σᵢ εᵢ⟨ℜ⁻¹ ∘ S₁¹(𝒜)(eᵢ), eᵢ⟩    [定理 6.1.1(b)]
= Σᵢ εᵢ⟨eᵢ, 𝔖⁻¹(𝒜)(eᵢ)⟩    [定理 6.4.3(d)]
= Σᵢ εᵢ 𝔖(𝔖⁻¹(𝒜))(eᵢ, eᵢ)    [(6.4.5)]
= Σᵢ εᵢ 𝒜(eᵢ, eᵢ)。

注意在 (6.5.5) 中，eᵢ 出现在第 k 个和第 l 个位置。

我们在定理 5.4.1 中看到“缩并”与“迹”是相关的。下面是同一现象的又一体现。

## 定理 6.5.2. 设 (V, g) 是标量积空间，则

C₁₂ = tr ∘ 𝔖⁻¹，

其中 tr 是迹，𝔖 由 (6.4.5) 给出。

证明. 我们有

C₁₂ = C₁¹ ∘ S₁¹    [(6.5.4)]
= (tr ∘ ℜ⁻¹) ∘ (ℜ ∘ 𝔖⁻¹)    [定理 5.4.1，定理 6.4.3(d)]
= tr ∘ 𝔖⁻¹。

## 6.6 (0, 4)-张量的对称性

设 V 是向量空间，设 𝒜 是 𝒯₄^0(V) 中的张量。考虑 𝒜 对所有 V 中的向量 v₁、v₂、v₃、v₄ 可能满足的以下对称性：

- [S1] 𝒜(v₁, v₂, v₃, v₄) = −𝒜(v₂, v₁, v₃, v₄)。
- [S2] 𝒜(v₁, v₂, v₃, v₄) = −𝒜(v₁, v₂, v₄, v₃)。
- [S3] 𝒜(v₁, v₂, v₃, v₄) = 𝒜(v₃, v₄, v₁, v₂)。
- [S4] 𝒜(v₁, v₂, v₃, v₄) + 𝒜(v₂, v₃, v₁, v₄) + 𝒜(v₃, v₁, v₂, v₄) = 0。

注意 [S4] 的左边是通过循环置换 v₁、v₂、v₃ 而保持 v₄ 不动得到的。由 [S1] 可得

𝒜(v₁, v₁, v₂, v₃) = 0，    (6.6.1)

由 [S2] 可得

𝒜(v₁, v₂, v₃, v₃) = 0    (6.6.2)

对所有 V 中的向量 v₁、v₂、v₃ 成立。

下面的结果表明，[S1]–[S4] 并非彼此独立。

## 定理 6.6.1. 设 V 是向量空间，𝒜 是 𝒯₄^0(V) 中的张量。如果 𝒜 满足 [S1]、[S2] 和 [S4]，则它满足 [S3]。

证明. 四次使用 [S4] 可得

```
𝒜(v₃, v₁, v₄, v₂) + 𝒜(v₁, v₄, v₃, v₂) + 𝒜(v₄, v₃, v₁, v₂) = 0
𝒜(v₁, v₄, v₂, v₃) + 𝒜(v₄, v₂, v₁, v₃) + 𝒜(v₂, v₁, v₄, v₃) = 0
𝒜(v₄, v₂, v₃, v₁) + 𝒜(v₂, v₃, v₄, v₁) + 𝒜(v₃, v₄, v₂, v₁) = 0
𝒜(v₂, v₃, v₁, v₄) + 𝒜(v₃, v₁, v₂, v₄) + 𝒜(v₁, v₂, v₃, v₄) = 0.  (6.6.3)
```

将 [S1] 和 [S2] 应用到 (6.6.3) 的第二列和第三列，得到

```
𝒜(v₃, v₁, v₄, v₂) − 𝒜(v₁, v₄, v₂, v₃) − 𝒜(v₃, v₄, v₁, v₂) = 0
𝒜(v₁, v₄, v₂, v₃) − 𝒜(v₄, v₂, v₃, v₁) + 𝒜(v₁, v₂, v₃, v₄) = 0
𝒜(v₄, v₂, v₃, v₁) − 𝒜(v₂, v₃, v₁, v₄) − 𝒜(v₃, v₄, v₁, v₂) = 0
𝒜(v₂, v₃, v₁, v₄) − 𝒜(v₃, v₁, v₄, v₂) + 𝒜(v₁, v₂, v₃, v₄) = 0.  (6.6.4)
```

将 (6.6.4) 两边相加得

```
2𝒜(v₁, v₂, v₃, v₄) − 2𝒜(v₃, v₄, v₁, v₂) = 0，
```

由此即得结果。

设 V 是向量空间，设 𝒮(V) 是 𝒯₄^0(V) 中满足 [S1] 和 [S2] 的张量构成的集合。容易证明 𝒮(V) 是 𝒯₄^0(V) 的子空间。设 𝒜 是 𝒯₄^0(V) 中的张量，考虑由下式定义的函数 𝒜̅：

```
𝒜̅(v₁, v₂) = 𝒜(v₁, v₂, v₂, v₁)
```

对所有 V 中的向量 v₁、v₂ 成立。注意若 𝒜 ∈ 𝒮(V)，则

```
𝒜̅(v₁, v₂) = 𝒜(v₁, v₂, v₂, v₁) = 𝒜(v₂, v₁, v₁, v₂) = 𝒜̅(v₂, v₁)，
```

所以 𝒜̅ 是对称的。

## 定理 6.6.2. 设 V 是二维向量空间，𝒜 是 𝒮(V) 中的张量。则以下命题等价：

- (a) 𝒜 是零张量。
- (b) 对 V 的某个基 (h₁, h₂)，𝒜̅(h₁, h₂) = 0。
- (c) 对 V 的每一个基 (h₁, h₂)，𝒜̅(h₁, h₂) = 0。

证明. 设 ℋ = (h₁, h₂) 是 V 的一个基。我们暂时不对 𝒜 和 ℋ 做进一步假设。设 v₁、v₂、v₃、v₄ 是 V 中的向量，且

```
[v₁]ℋ = [a¹; a²]    [v₂]ℋ = [b¹; b²]    [v₃]ℋ = [c¹; c²]    [v₄]ℋ = [d¹; d²].
```

由 [S1] 和 (6.6.1) 可得

𝒜(v₁, v₂, v₃, v₄) = 𝒜(a¹h₁ + a²h₂, b¹h₁ + b²h₂, v₃, v₄) = a¹b¹𝒜(h₁, h₁, v₃, v₄) + a¹b²𝒜(h₁, h₂, v₃, v₄) + a²b¹𝒜(h₂, h₁, v₃, v₄) + a²b²𝒜(h₂, h₂, v₃, v₄) = (a¹b² − a²b¹)𝒜(h₁, h₂, v₃, v₄)，

由 [S2] 和 (6.6.2) 可得

𝒜(h₁, h₂, v₃, v₄) = 𝒜(h₁, h₂, c¹h₁ + c²h₂, d¹h₁ + d²h₂) = c¹d¹𝒜(h₁, h₂, h₁, h₁) + c¹d²𝒜(h₁, h₂, h₁, h₂) + c²d¹𝒜(h₁, h₂, h₂, h₁) + c²d²𝒜(h₁, h₂, h₂, h₂) = −(c¹d² − c²d¹)𝒜(h₁, h₂, h₂, h₁) = −(c¹d² − c²d¹)𝒜̅(h₁, h₂)。

因此，

𝒜(v₁, v₂, v₃, v₄) = −(a¹b² − a²b¹)(c¹d² − c²d¹)𝒜̅(h₁, h₂)。 (6.6.5)

(a) ⇒ (b)：由于 𝒜 是零张量，所以 𝒜̅(h₁, h₂) = 𝒜(h₁, h₂, h₂, h₁) = 0。

(b) ⇒ (a)：由于 𝒜̅(h₁, h₂) = 0，由 (6.6.5) 可知 𝒜(v₁, v₂, v₃, v₄) = 0。因为 v₁、v₂、v₃、v₄ 是任意的，所以 𝒜 是零张量。

(b) ⇔ (c)：设 (f₁, f₂) 是 V 的另一个基。令 v₁ = v₄ = f₁，v₂ = v₃ = f₂，则对 i = 1,2 有 aⁱ = dⁱ，bⁱ = cⁱ。于是 (6.6.5) 给出

𝒜̅(f₁, f₂) = (a¹b² − a²b¹)²𝒜̅(h₁, h₂)。 (6.6.6)

由 (2.2.6)、(2.2.7) 和定理 2.4.12，

0 ≠ det([id_V]ℱℋ) = det([[a¹ b¹][a² b²]]) = a¹b² − a²b¹。

结论由此得出。

## 定理 6.6.3. 设 (V, g) 是二维标量积空间，设 𝒟 是由下式定义的函数

𝒟(v₁, v₂, v₃, v₄) = det([[⟨v₁, v₃⟩ ⟨v₁, v₄⟩][⟨v₂, v₃⟩ ⟨v₂, v₄⟩]]) (6.6.7)

对所有 V 中的向量 v₁、v₂、v₃、v₄ 成立。则：

(a) 𝒟 是 𝒮(V) 中的一个非零张量，且 𝒟̅ 由下式给出

𝒟̅(v₁, v₂) = −det([[⟨v₁, v₁⟩ ⟨v₁, v₂⟩][⟨v₂, v₁⟩ ⟨v₂, v₂⟩]])

对所有 V 中的向量 v₁、v₂ 成立。

(b) \(S(V)\) 是由 \(\mathcal D\) 张成的 \(T^0_4(V)\) 的一维子空间。

**证明.** (a)：断言 \(\mathcal D\) 是 \(S(V)\) 中的张量可由行列式的性质推出，或者更简单地从以下观察得出：

\[\mathcal D(v_1,v_2,v_3,v_4)=\langle v_1,v_3\rangle\langle v_2,v_4\rangle-\langle v_2,v_3\rangle\langle v_1,v_4\rangle.\]

设 \((e_1,e_2)\) 是 \(V\) 的标准正交基。则

\[\mathcal D(e_1,e_2,e_2,e_1)=-\langle e_1,e_1\rangle\langle e_2,e_2\rangle\ne 0，\]

因此 \(\mathcal D\) 非零。

(b)：设 \(\mathcal A\) 是 \(S(V)\) 中的张量。由 (6.6.5) 有

\[\mathcal A(v_1,v_2,v_3,v_4)=-(a^1b^2-a^2b^1)(c^1d^2-c^2d^1)\overline{\mathcal A}(h_1,h_2)\]

且

\[\mathcal D(v_1,v_2,v_3,v_4)=-(a^1b^2-a^2b^1)(c^1d^2-c^2d^1)\overline{\mathcal D}(h_1,h_2)，\]

因此

\[\mathcal A(v_1,v_2,v_3,v_4)=\frac{\overline{\mathcal A}(h_1,h_2)}{\overline{\mathcal D}(h_1,h_2)}\mathcal D(v_1,v_2,v_3,v_4)，\]

其中定理 6.6.2 和 (a) 部分保证分母非零。由于 \(v_1,v_2,v_3,v_4\) 是任意的，

\[\mathcal A=\frac{\overline{\mathcal A}(h_1,h_2)}{\overline{\mathcal D}(h_1,h_2)}\mathcal D，\]

所以 \(\mathcal D\) 张成 \(S(V)\)。

## **定理 6.6.4.** 设 (V,g) 是二维标量积空间，设 \(\mathcal A\) 是 \(S(V)\) 中的张量，并设 \((h_1,h_2)\) 和 \((f_1,f_2)\) 是 \(V\) 的基。则

\[\frac{\overline{\mathcal A}(h_1,h_2)}{\overline{\mathcal D}(h_1,h_2)}=\frac{\overline{\mathcal A}(f_1,f_2)}{\overline{\mathcal D}(f_1,f_2)}.\]

**证明.** 仿照定理 6.6.3 (b) 部分的证明，并将 (6.6.5) 换成 (6.6.6)，即可得到结论。





这一点隐含在(7.1.1)的表述方式中。令 \((w_1,w_2,w_3)=(v_1,v_3,v_2)\)，由(7.1.1)可得

$$\sigma(\mathcal A)(v_1,v_3,v_2)=\sigma(\mathcal A)(w_1,w_2,w_3)=\mathcal A(w_{\sigma(1)},w_{\sigma(2)},w_{\sigma(3)})$$
$$=\mathcal A(w_2,w_3,w_1)=\mathcal A(v_3,v_2,v_1).$$

在接下来的大多数计算中，指标都是按顺序排列的，从而避免了这一问题。

## 定理 7.1.1.
设 $V$ 是一个向量空间，$\sigma,\rho$ 是 $S_s$ 中的置换，$\mathcal A$ 是 $T_s^0(V)$ 中的一个张量。则

$$(\sigma\rho)(\mathcal A)=\sigma(\rho(\mathcal A)).$$

*证明.* 设 $v_1,\ldots,v_s$ 是 $V$ 中的向量，令 $(w_1,\ldots,w_s)=(v_{\sigma(1)},\ldots,v_{\sigma(s)})$。则

$$(\sigma\rho)(\mathcal A)(v_1,\ldots,v_s)=\mathcal A(v_{(\sigma\rho)(1)},\ldots,v_{(\sigma\rho)(s)})=\mathcal A(v_{\sigma(\rho(1))},\ldots,v_{\sigma(\rho(s))})$$
$$=\mathcal A(w_{\rho(1)},\ldots,w_{\rho(s)})=\rho(\mathcal A)(w_1,\ldots,w_s)$$
$$=\rho(\mathcal A)(v_{\sigma(1)},\ldots,v_{\sigma(s)})=\sigma(\rho(\mathcal A))(v_1,\ldots,v_s).$$

由于 $v_1,\ldots,v_s$ 是任意的，结论得证。

我们称 $T_s^0(V)$ 中的张量 $\mathcal A$ 是**对称的**，如果对所有 $S_s$ 中的置换 $\sigma$ 都有 $\sigma(\mathcal A)=\mathcal A$；也就是说，

$$\mathcal A(v_{\sigma(1)},\ldots,v_{\sigma(s)})=\mathcal A(v_1,\ldots,v_s)$$

对所有 $V$ 中的向量 $v_1,\ldots,v_s$ 成立。我们用 $\Sigma_s(V)$ 表示 $T_s^0(V)$ 中对称张量的集合。容易证明 $\Sigma_s(V)$ 是 $T_s^0(V)$ 的子空间。

$T_s^0(V)$ 中的张量 $\mathcal B$ 被称为**交错的**，如果对 $S_s$ 中的所有对换 $\tau$ 都有 $\tau(\mathcal B)=-\mathcal B$，或者等价地，如果

$$\mathcal B(v_1,\ldots,v_j,\ldots,v_i,\ldots,v_s)=-\mathcal B(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s)$$

对所有 $V$ 中的向量 $v_1,\ldots,v_s$ 以及所有 $1\le i<j\le s$ 成立。$T_s^0(V)$ 中交错张量的集合记为 $\Lambda^s(V)$。不难证明 $\Lambda^s(V)$ 是 $T_s^0(V)$ 的子空间。$\Lambda^s(V)$ 的元素称为**s-余向量**或**多重余向量**。当 $s=1$ 时，交错性条件是空泛的，1-余向量就是余向量。因此，

$$\Lambda^1(V)=V^*.$$ \tag{7.1.2}

由于 $\Lambda^s(V)$ 是 $T_s^0(V)$ 的子空间，$\Lambda^s(V)$ 中的**零多重向量**正是 $T_s^0(V)$ 中的零张量。$\Lambda^s(V)$ 中的多重向量是**非零**的，当它作为 $T_s^0(V)$ 中的张量非零时。为了与(5.1.2)保持一致，我们定义

$$\Lambda^0(V)=\mathbb R.$$ \tag{7.1.3}

在这些定义下，行列式函数 $\det:(\operatorname{Mat}_{m\times1})^m\to\mathbb R$ 可看作 $\Lambda^m(\operatorname{Mat}_{m\times1})$ 中的一个多重向量。

有几种等价的方式来刻画 $\Lambda^s(V)$ 中的多重向量。

## 定理 7.1.2.
设 $V$ 是一个向量空间，$\eta$ 是 $T^0_s(V)$ 中的一个张量。则以下命题等价：

- (a) $\eta$ 是 $\Lambda^s(V)$ 中的多重向量。
- (b) 对所有 $S_s$ 中的置换 $\sigma$，有 $\sigma(\eta)=\operatorname{sgn}(\sigma)\eta$。
- (c) 如果 $v_1,\ldots,v_s$ 是 $V$ 中的向量，且其中有两个（或更多）相等，则 $\eta(v_1,\ldots,v_s)=0$。
- (d) 如果 $v_1,\ldots,v_s$ 是 $V$ 中的向量，且 $\eta(v_1,\ldots,v_s)\ne0$，则 $v_1,\ldots,v_s$ 线性无关。

**证明.** 证明与定理 2.4.2 的证明类似。

$(a)\Rightarrow(b)$：设 $\sigma=\tau_1\cdots\tau_k$ 是将 $\sigma$ 分解为对换的分解式。由定理 7.1.1，

$$\sigma(\eta)=(\tau_1\cdots\tau_k)(\eta)=(\tau_1\cdots\tau_{k-1})(\tau_k(\eta))$$
$$\qquad=(\tau_1\cdots\tau_{k-1})(-\eta)=-(\tau_1\cdots\tau_{k-1})(\eta).$$

再重复该过程 $k-1$ 次，得到 $\sigma(\eta)=(-1)^k\eta$。由定理 B.2.3，$\operatorname{sgn}(\sigma)=(-1)^k$。

$(b)\Rightarrow(a)$：如果 $\tau$ 是 $S_s$ 中的一个对换，则由定理 B.2.3，$\tau(\eta)=\operatorname{sgn}(\tau)\eta=-\eta$。

$(a)\Rightarrow(c)$：如果对某个 $1\le i<j\le s$ 有 $v_i=v_j$，则

$$\eta(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s)=\eta(v_1,\ldots,v_j,\ldots,v_i,\ldots,v_s).$$

另一方面，由于 $\eta$ 是交错的，

$$\eta(v_1,\ldots,v_j,\ldots,v_i,\ldots,v_s)=-\eta(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s).$$

因此，

$$\eta(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s)=-\eta(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s),$$

由此结论得证。

$(c)\Rightarrow(a)$：对所有 $1\le i<j\le s$，我们有

$$0=\eta(v_1,\ldots,v_i+v_j,\ldots,v_i+v_j,\ldots,v_s)$$
$$\quad=\eta(v_1,\ldots,v_i,\ldots,v_i,\ldots,v_s)+\eta(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s)$$
$$\quad+\eta(v_1,\ldots,v_j,\ldots,v_i,\ldots,v_s)+\eta(v_1,\ldots,v_j,\ldots,v_j,\ldots,v_s)$$
$$\quad=\eta(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s)+\eta(v_1,\ldots,v_j,\ldots,v_i,\ldots,v_s),$$

由此结论得证。

为了证明 $(c)\Leftrightarrow(d)$，我们将(d)部分中的断言替换为以下逻辑上等价的断言：

$(d')$：如果 $v_1,\ldots,v_s$ 是 $V$ 中线性相关的向量，则 $\eta(v_1,\ldots,v_s)=0$。

$(c)\Rightarrow(d')$：由于 $v_1,\ldots,v_s$ 线性相关，其中一个可以表示为其余向量的线性组合，不妨设 $v_1=\sum_{i=2}^s a^i v_i$。则

$$\eta(v_1,\ldots,v_s)=\eta\left(\sum_{i=2}^s a^i v_i,v_2,\ldots,v_s\right)=\sum_{i=2}^s a^i\eta(v_i,v_2,\ldots,v_s).$$

由于 $\eta(v_i,v_2,\ldots,v_s)$ 在（至少）两个位置含有 $v_i$，对 $i=2,\ldots,s$ 有 $\eta(v_i,v_2,\ldots,v_s)=0$。因此，$\eta(v_1,\ldots,v_s)=0$。

$(d')\Rightarrow(c)$：如果 $v_1,\ldots,v_s$ 中有两个（或更多）相等，则它们线性相关，所以 $\eta(v_1,\ldots,v_s)=0$。

现在我们介绍一种将多重向量与给定张量联系起来的方法。设 $V$ 是一个向量空间。**交错映射**是如下线性映射族

$$\operatorname{Alt}:\mathcal{T}^0_s(V)\longrightarrow\mathcal{T}^0_s(V)$$

对 $s\ge 0$ 定义为

$$\operatorname{Alt}(\mathcal{A})=\frac{1}{s!}\sum_{\sigma\in S_s}\operatorname{sgn}(\sigma)\,\sigma(\mathcal{A}) \tag{7.1.4}$$

其中 $\mathcal{A}$ 是 $\mathcal{T}^0_s(V)$ 中的任意张量。

## 定理 7.1.3.
设 $V$ 是一个向量空间，$\mathcal{A}$ 是 $\mathcal{T}^0_s(V)$ 中的张量，$\sigma$ 是 $S_s$ 中的置换，$\eta$ 是 $\Lambda^s(V)$ 中的多重向量。则：

(a) $\sigma(\operatorname{Alt}(\mathcal{A}))=\operatorname{sgn}(\sigma)\operatorname{Alt}(\mathcal{A})=\operatorname{Alt}(\sigma(\mathcal{A}))$。

(b) $\operatorname{Alt}(\mathcal{A})$ 是 $\Lambda^s(V)$ 中的多重向量。

(c) $\operatorname{Alt}(\eta)=\eta$。

(d) $\operatorname{Alt}(\operatorname{Alt}(\mathcal{A}))=\operatorname{Alt}(\mathcal{A})$。

(e) $\operatorname{Alt}(\mathcal{T}^0_s(V))=\Lambda^s(V)$。

**证明.** (a)：我们有

$$\begin{aligned}
\sigma(\operatorname{Alt}(\mathcal{A}))
&=\sigma\left(\frac{1}{s!}\sum_{\rho\in S_s}\operatorname{sgn}(\rho)\,\rho(\mathcal{A})\right) &&\text{[(7.1.4)]}\\
&=\frac{1}{s!}\sum_{\rho\in S_s}\operatorname{sgn}(\rho)\,\sigma(\rho(\mathcal{A}))\\
&=\frac{1}{s!}\sum_{\rho\in S_s}\operatorname{sgn}(\rho)\,(\sigma\rho)(\mathcal{A}) &&\text{[定理 7.1.1]}\\
&=\operatorname{sgn}(\sigma)\frac{1}{s!}\sum_{\rho\in S_s}\operatorname{sgn}(\sigma\rho)\,(\sigma\rho)(\mathcal{A}) &&\text{[定理 B.2.2]}\\
&=\operatorname{sgn}(\sigma)\operatorname{Alt}(\mathcal{A}), &&\text{[(7.1.4)]}
\end{aligned}$$

其中最后一个等号源自这样的观察：当 $\rho$ 遍历 $S_s$ 时，$\sigma\rho$ 也遍历 $S_s$。我们还

$$\begin{aligned}
\operatorname{Alt}(\sigma(\mathcal{A}))
&=\frac{1}{s!}\sum_{\rho\in S_s}\operatorname{sgn}(\rho)\,\rho(\sigma(\mathcal{A})) &&\text{[(7.1.4)]}\\
&=\operatorname{sgn}(\sigma)\frac{1}{s!}\sum_{\rho\in S_s}\operatorname{sgn}(\rho\sigma)\,(\rho\sigma)(\mathcal{A}) &&\text{[定理 7.1.1, 定理 B.2.2]}\\
&=\operatorname{sgn}(\sigma)\operatorname{Alt}(\mathcal{A}).
\end{aligned}$$

其中最后一个等号的理由同上。

(b)：对所有 S_s 中的对换 τ，由(a)部分可得 τ(Alt(A)) = −Alt(A)，所以 Alt(A) 是交错的。

(c)：我们有

Alt(η) = 1/s! ∑_{σ∈S_s} sgn(σ) σ(η)   [(7.1.4)]
= 1/s! ∑_{σ∈S_s} η   [定理 7.1.2(b)]
= η。   [基数(S_s) = s!]

(d)：这由(b)和(c)部分得出。

(e)：由(b)部分我们有 Alt(T_s^0(V)) ⊆ Λ^s(V)。由于 Λ^s(V) ⊆ T_s^0(V)，由(c)部分，

Λ^s(V) = Alt(Λ^s(V)) ⊆ Alt(T_s^0(V))。

鉴于定理 7.1.3(b)，我们可以将(7.1.4)中的映射替换为
Alt : T_s^0(V) ⟶ Λ^s(V)。

## 7.2 楔积

在第 5.1 节中，我们介绍了一种张量乘法，称为张量积。我们的下一个任务是为多重余向量定义相应的运算。

设 V 是一个向量空间。楔积是如下线性映射族
∧ : Λ^s(V) × Λ^{s'}(V) ⟶ Λ^{s+s'}(V)

对 s, s' ≥ 0 定义为

η ∧ ζ = (s + s')! / (s! s'!) Alt(η ⊗ ζ)   (7.2.1)

对所有 Λ^s(V) 中的多重余向量 η 和 Λ^{s'}(V) 中的多重余向量 ζ。也就是说，

(η ∧ ζ)(v₁, …, v_{s+s'}) = (s + s')! / (s! s'!) [ 1/(s + s')! ∑_{σ∈S_{s+s'}} sgn(σ) σ(η ⊗ ζ)(v_{σ(1)}, …, v_{σ(s+s')}) ] = 1/(s! s'!) ∑_{σ∈S_{s+s'}} sgn(σ) η(v_{σ(1)}, …, v_{σ(s)}) ζ(v_{σ(s+1)}, …, v_{σ(s+s')})

对所有 V 中的向量 v₁, …, v_{s+s'}，其中第一个等号由(7.2.1)得出，第二个等号由(5.1.4)和(7.1.1)得出。





## 例 7.2.1.

设 \(\eta,\zeta\) 是 \(\Lambda^1(V)=V^*\) 中的余向量，设 \(v_1,v_2\) 是 \(V\) 中的向量。取 \(\mathcal{S}_2=\{\mathrm{id},(1\,2)\}\)，我们有

\[
(\eta\wedge\zeta)(v_1,v_2)=\frac{1}{1!1!}\sum_{\sigma\in\mathcal{S}_2}\operatorname{sgn}(\sigma)\,\eta(v_{\sigma(1)})\zeta(v_{\sigma(2)})=\eta(v_1)\zeta(v_2)-\eta(v_2)\zeta(v_1).
\]

现在设 \(\eta\) 是 \(\Lambda^2(V)\) 中的多重余向量，设 \(\zeta\) 是 \(\Lambda^1(V)=V^*\) 中的余向量，并设 \(v_1,v_2,v_3\) 是 \(V\) 中的向量。取

\[
\mathcal{S}_3=\{\mathrm{id},(1\,2),(1\,3),(2\,3),(1\,2\,3),(1\,3\,2)\},
\]

我们有

\[
\begin{aligned}
(\eta\wedge\zeta)(v_1,v_2,v_3)
&=\frac{1}{2!1!}\sum_{\sigma\in\mathcal{S}_3}\operatorname{sgn}(\sigma)\,\eta(v_{\sigma(1)},v_{\sigma(2)})\zeta(v_{\sigma(3)})\\
&=\frac{1}{2}\left[\eta(v_1,v_2)\zeta(v_3)-\eta(v_2,v_1)\zeta(v_3)-\eta(v_3,v_2)\zeta(v_1)-\eta(v_1,v_3)\zeta(v_2)+\eta(v_2,v_3)\zeta(v_1)+\eta(v_3,v_1)\zeta(v_2)\right]\\
&=\frac{1}{2}\left[\eta(v_1,v_2)\zeta(v_3)+\eta(v_1,v_2)\zeta(v_3)+\eta(v_2,v_3)\zeta(v_1)+\eta(v_3,v_1)\zeta(v_2)+\eta(v_2,v_3)\zeta(v_1)+\eta(v_3,v_1)\zeta(v_2)\right]\\
&=\eta(v_1,v_2)\zeta(v_3)+\eta(v_2,v_3)\zeta(v_1)+\eta(v_3,v_1)\zeta(v_2).
\end{aligned}
\]

楔积在基本代数结构方面具有良好性质。

## 定理 7.2.2.

设 \(V\) 是向量空间，设 \(\eta,\eta^1,\eta^2\) 和 \(\zeta,\zeta^1,\zeta^2\) 分别是 \(\Lambda^s(V)\) 和 \(\Lambda^{s'}(V)\) 中的多重余向量，设 \(c\) 是实数。则：

- (a) \((\eta^1+\eta^2)\wedge\zeta=\eta^1\wedge\zeta+\eta^2\wedge\zeta\).
- (b) \(\eta\wedge(\zeta^1+\zeta^2)=\eta\wedge\zeta^1+\eta\wedge\zeta^2\).
- (c) \((c\eta)\wedge\zeta=\eta\wedge(c\zeta)=c(\eta\wedge\zeta)\).

证明. 直接可得。

## 定理 7.2.3.

如果 \(V\) 是向量空间，\(\eta\) 和 \(\zeta\) 分别是 \(\Lambda^s(V)\) 和 \(\Lambda^{s'}(V)\) 中的多重余向量，则：

- (a) \(\eta\wedge\zeta=(-1)^{ss'}\zeta\wedge\eta\).
- (b) 若 \(s=s'=1\)，则 \(\eta\wedge\zeta=-\zeta\wedge\eta\).
- (c) 若 \(s=1\)，则 \(\eta\wedge\eta=0\).

证明. (a)：设 \(v_1,\dots,v_{s+s'}\) 是 \(V\) 中的向量，定义 \(\mathcal{S}_{s+s'}\) 中的置换 \(\sigma\) 为

\[
\sigma=
\begin{pmatrix}
1 & \cdots & s & s+1 & \cdots & s+s'\\
s'+1 & \cdots & s'+s & 1 & \cdots & s'
\end{pmatrix}.
\]

我们有

\[
\begin{aligned}
\sigma(\eta\otimes\zeta)(v_1,\dots,v_{s+s'})
&=\eta\otimes\zeta(v_{\sigma(1)},\dots,v_{\sigma(s)},v_{\sigma(s+1)},\dots,v_{\sigma(s+s')})\\
&=\eta\otimes\zeta(v_{s'+1},\dots,v_{s'+s},v_1,\dots,v_{s'})\\
&=\eta(v_{s'+1},\dots,v_{s'+s})\,\zeta(v_1,\dots,v_{s'})\\
&=\zeta(v_1,\dots,v_{s'})\,\eta(v_{s'+1},\dots,v_{s'+s})\\
&=\zeta\otimes\eta(v_1,\dots,v_{s'},v_{s'+1},\dots,v_{s+s'}).
\end{aligned}
\]

由于 \(v_1,\dots,v_{s+s'}\) 是任意的，所以 \(\sigma(\eta\otimes\zeta)=\zeta\otimes\eta\)。于是

\[
\operatorname{Alt}(\zeta\otimes\eta)
=\operatorname{Alt}(\sigma(\eta\otimes\zeta))
=\operatorname{sgn}(\sigma)\operatorname{Alt}(\eta\otimes\zeta)\quad[\text{定理 7.1.3(a)}]
=(-1)^{ss'}\operatorname{Alt}(\eta\otimes\zeta),\quad[\text{定理 B.2.4}]
\]

因此

\[
\eta\wedge\zeta
=\frac{(s+s')!}{s!s'!}\operatorname{Alt}(\eta\otimes\zeta)
=(-1)^{ss'}\frac{(s'+s)!}{s'!s!}\operatorname{Alt}(\zeta\otimes\eta)
=(-1)^{ss'}\zeta\wedge\eta.
\]

- (b)：由 (a) 部分可得。
- (c)：由 (b) 部分可得。

## 定理 7.2.4.

设 \(V\) 是向量空间，\(\mathcal{A}\) 和 \(\mathcal{B}\) 分别是 \(\Lambda^s(V)\) 和 \(\Lambda^{s'}(V)\) 中的多重余向量，则

\[
\operatorname{Alt}(\operatorname{Alt}(\mathcal{A})\otimes\mathcal{B})
=\operatorname{Alt}(\mathcal{A}\otimes\mathcal{B})
=\operatorname{Alt}(\mathcal{A}\otimes\operatorname{Alt}(\mathcal{B})).
\]

证明. 如下定义映射 \(\iota:\mathcal{S}_s\longrightarrow\mathcal{S}_{s+s'}\)。对于 \(\mathcal{S}_s\) 中的每个置换 \(\sigma\)，令 \(\iota(\sigma)\) 为 \(\mathcal{S}_{s+s'}\) 中的置换，它在 \(\{1,\dots,s\}\) 上限制为 \(\sigma\)，并固定 \(s+1,\dots,s+s'\)；即

\[
\iota(\sigma)=
\begin{pmatrix}
1 & \cdots & s & s+1 & \cdots & s+s'\\
\sigma(1) & \cdots & \sigma(s) & s+1 & \cdots & s+s'
\end{pmatrix}.
\]

显然，\(\iota\) 是 \(\mathcal{S}_s\) 与 \(\mathcal{S}_{s+s'}\) 的子群 \(\iota(\mathcal{S}_s)\) 之间的群同构。容易证明

\[
\operatorname{sgn}(\iota(\sigma))=\operatorname{sgn}(\sigma)
\quad\text{and}\quad
\iota(\sigma)(\mathcal{A}\otimes\mathcal{B})=\sigma(\mathcal{A})\otimes\mathcal{B}. \tag{7.2.2}
\]

于是

\[
\begin{aligned}
\operatorname{Alt}(\operatorname{Alt}(\mathcal{A})\otimes\mathcal{B})
&=\operatorname{Alt}\left(\left[\frac{1}{s!}\sum_{\sigma\in\mathcal{S}_s}\operatorname{sgn}(\sigma)\sigma(\mathcal{A})\right]\otimes\mathcal{B}\right)\quad[(7.1.4)]\\
&=\operatorname{Alt}\left(\frac{1}{s!}\sum_{\sigma\in\mathcal{S}_s}\operatorname{sgn}(\sigma)\left(\sigma(\mathcal{A})\otimes\mathcal{B}\right)\right)\quad[\text{定理 5.1.2(a)}]\\
&=\frac{1}{s!}\sum_{\sigma\in\mathcal{S}_s}\operatorname{sgn}(\sigma)\operatorname{Alt}\left(\sigma(\mathcal{A})\otimes\mathcal{B}\right)\\
&=\frac{1}{s!}\sum_{\sigma\in\mathcal{S}_s}\operatorname{sgn}(\iota(\sigma))\operatorname{Alt}\left(\iota(\sigma)(\mathcal{A}\otimes\mathcal{B})\right)\quad[(7.2.2)]\\
&=\frac{1}{s!}\sum_{\sigma\in\mathcal{S}_s}\operatorname{Alt}(\mathcal{A}\otimes\mathcal{B})\quad[\text{定理 7.1.3(a)}]\\
&=\operatorname{Alt}(\mathcal{A}\otimes\mathcal{B}),\quad[\operatorname{card}(\mathcal{S}_s)=s!]
\end{aligned}
\]

这就证明了第一个等式。第二个等式的证明类似。

用归纳法证明。设 \(k\ge 4\)，并假设对所有小于 \(k\) 的指标，断言成立。则

\[
\begin{aligned}
(\eta^1\wedge\eta^2)\wedge\eta^3\wedge\cdots\wedge\eta^k
&=\frac{(s_1+s_2+s_3+\cdots+s_k)!}{(s_1+s_2)!s_3!\cdots s_k!}
\operatorname{Alt}\left((\eta^1\wedge\eta^2)\otimes\eta^3\otimes\cdots\otimes\eta^k\right)\\
&=\frac{(s_1+s_2+s_3+\cdots+s_k)!}{(s_1+s_2)!s_3!\cdots s_k!}
\operatorname{Alt}\left(\frac{(s_1+s_2)!}{s_1!s_2!}\operatorname{Alt}(\eta^1\otimes\eta^2)\otimes(\eta^3\otimes\cdots\otimes\eta^k)\right)\\
&=\frac{(s_1+\cdots+s_k)!}{s_1!\cdots s_k!}
\operatorname{Alt}\left(\operatorname{Alt}(\eta^1\otimes\eta^2)\otimes(\eta^3\otimes\cdots\otimes\eta^k)\right)\\
&=\frac{(s_1+\cdots+s_k)!}{s_1!\cdots s_k!}
\operatorname{Alt}(\eta^1\otimes\cdots\otimes\eta^k),
\end{aligned}
\]

其中第一个等式由归纳假设得出，第二个等式由 (7.2.1) 得出，最后一个等式由定理 7.2.4 得出。

下一个结果说明楔积与行列式密切相关，这一点并不令人意外。

## 定理 7.2.8.

设 \(V\) 是向量空间，设 \(\eta^1,\dots,\eta^s\) 是 \(V^*\) 中的余向量，设 \(v_1,\dots,v_s\) 是 \(V\) 中的向量。则

\[
\eta^1\wedge\cdots\wedge\eta^s(v_1,\dots,v_s)
=
\det
\begin{pmatrix}
\eta^1(v_1) & \cdots & \eta^1(v_s)\\
\vdots & \ddots & \vdots\\
\eta^s(v_1) & \cdots & \eta^s(v_s)
\end{pmatrix}.
\]

证明. 我们有

\[
\begin{aligned}
\eta^1\wedge\cdots\wedge\eta^s(v_1,\dots,v_s)
&=s!\,\operatorname{Alt}(\eta^1\otimes\cdots\otimes\eta^s)(v_1,\dots,v_s) && [\text{定理 7.2.7}]\\
&=\sum_{\sigma\in S_s}\operatorname{sgn}(\sigma)\,\sigma(\eta^1\otimes\cdots\otimes\eta^s)(v_1,\dots,v_s) && [(7.1.4)]\\
&=\sum_{\sigma\in S_s}\operatorname{sgn}(\sigma)\,\eta^1\otimes\cdots\otimes\eta^s(v_{\sigma(1)},\dots,v_{\sigma(s)}) && [(7.1.1)]\\
&=\sum_{\sigma\in S_s}\operatorname{sgn}(\sigma)\,\eta^1(v_{\sigma(1)})\cdots\eta^s(v_{\sigma(s)})\\
&=\det([\eta^i(v_j)]). && [(2.4.1)]
\end{aligned}
\]

## 定理 7.2.9.

设 \(V\) 是向量空间，设 \((h_1,\dots,h_m)\) 是 \(V\) 的基，设 \((\theta^1,\dots,\theta^m)\) 是它的对偶基。则根据定理 7.2.8，

\[
\theta^{j_1}\wedge\cdots\wedge\theta^{j_s}(h_{i_1},\dots,h_{i_s})
=
\det\left((\theta^i(h_j))_{(i_1,\dots,i_s)}^{(j_1,\dots,j_s)}\right)
=
\det\left((I_m)_{(i_1,\dots,i_s)}^{(j_1,\dots,j_s)}\right).
\]

若 \((i_1,\dots,i_s)=(j_1,\dots,j_s)\)，则 \((I_m)_{(i_1,\dots,i_s)}^{(j_1,\dots,j_s)}=I_s\)；若 \((i_1,\dots,i_s)\ne(j_1,\dots,j_s)\)，则 \((I_m)_{(i_1,\dots,i_s)}^{(j_1,\dots,j_s)}\) 至少有一行（且至少有一列）为 \(0\)。结论得证。

## 定理 7.2.10.

设 \(V\) 是向量空间，设 \(\eta^1,\dots,\eta^s\) 是 \(V^*\) 中的余向量，设 \(\sigma\) 是 \(S_s\) 中的置换。则

\[
\eta^{\sigma(1)}\wedge\cdots\wedge\eta^{\sigma(s)}
=\operatorname{sgn}(\sigma)\,\eta^1\wedge\cdots\wedge\eta^s.
\]

证明. 对于 \(V\) 中的向量 \(v_1,\dots,v_s\)，我们有

\[
\begin{aligned}
\eta^{\sigma(1)}\wedge\cdots\wedge\eta^{\sigma(s)}(v_1,\dots,v_s)
&=\det\left([\eta^{\sigma(i)}(v_j)]\right) && [\text{定理 7.2.8}]\\
&=\det\left([\eta^{\sigma(j)}(v_i)]\right) && [\text{定理 2.4.5(c)}]\\
&=\operatorname{sgn}(\sigma)\det\left([\eta^i(v_i)]\right) && [\text{定理 2.4.2}]\\
&=\operatorname{sgn}(\sigma)\det\left([\eta^i(v_j)]\right) && [\text{定理 2.4.5(c)}]\\
&=\operatorname{sgn}(\sigma)\,\eta^1\wedge\cdots\wedge\eta^s(v_1,\dots,v_s), && [\text{定理 7.2.8}]
\end{aligned}
\]

其中 \([\eta^{\sigma(j)}(v_i)]=([\eta^{\sigma(i)}(v_j)])^T\)。由于 \(v_1,\dots,v_s\) 是任意的，结论得证。

## 定理 7.2.11.

设 \(V\) 是向量空间，设 \(H=(h_1,\dots,h_m)\) 和 \(F\) 是 \(V\) 的基，设 \((\varphi^1,\dots,\varphi^m)\) 是对应于 \(F\) 的对偶基。则

\[
\varphi^1\wedge\cdots\wedge\varphi^m(h_1,\dots,h_m)=\det\left([\mathrm{id}_V]^F_H\right).
\]

证明. 根据定理 1.2.1(d)，对于 \(j=1,\dots,m\)，有 \(h_j=\sum_i\varphi^i(h_j)f_i\)。于是由 (2.2.6) 和 (2.2.7) 得 \([\mathrm{id}_V]^F_H=[\varphi^i(h_j)]\)。现在由定理 7.2.8 即得结论。

## 定理 7.2.12（\(\Lambda^s(V)\) 的基）.

设 \(V\) 是向量空间，设 \((h_1,\dots,h_m)\) 是 \(V\) 的基，设 \((\theta^1,\dots,\theta^m)\) 是它的对偶基。则：

- (a) 若 \(1\le s\le m\)，则

  \[
  \{\theta^{i_1}\wedge\cdots\wedge\theta^{i_s}:1\le i_1<\cdots<i_s\le m\}
  \]

  是 \(\Lambda^s(V)\) 的一个无序基。

- (b) 若 \(s>m\)，则 \(\Lambda^s(V)=\{0\}\)。
- (c) \(\Lambda^s(V)\) 的维数为 \(\binom{m}{s}\)。
- (d) \(\Lambda^m(V)\) 中任何非零多重余向量都张成 \(\Lambda^m(V)\)。
- (e) \((\eta^1,\dots,\eta^m)\) 是 \(\Lambda^{m-1}(V)\) 的基，其中

  \[
  \eta^i=(-1)^{i-1}\theta^1\wedge\cdots\wedge\widehat{\theta^i}\wedge\cdots\wedge\theta^m
  \]

  对 \(i=1,\dots,m\) 成立，且 \(\widehat{\theta^i}\) 表示省略一个表达式。

证明. 设

\[
\Omega_s=\{\theta^{i_1}\otimes\cdots\otimes\theta^{i_s}:1\le i_1,\dots,i_s\le m\}
\]

和

\[
\Theta_s=\{\theta^{i_1}\wedge\cdots\wedge\theta^{i_s}:1\le i_1<\cdots<i_s\le m\}.
\]

则

\[
\begin{aligned}
\operatorname{span}(\Theta_s)
&=\operatorname{span}(\operatorname{Alt}(\Omega_s)) && [\text{定理 7.2.7}]\\
&=\operatorname{Alt}(\operatorname{span}(\Omega_s))\\
&=\operatorname{Alt}(\mathcal{T}_s^0(V)) && [\text{定理 5.1.3(b)}]\\
&=\Lambda^s(V), && [\text{定理 7.1.3(e)}]
\end{aligned}
\]

所以 \(\Theta_s\) 对所有 \(s\ge1\) 张成 \(\Lambda^s(V)\)。

- (a)：若

  \[
  \sum_{1\le i_1<\cdots<i_s\le m} a_{i_1\dots i_s}\,\theta^{i_1}\wedge\cdots\wedge\theta^{i_s}=0
  \]

  对某些实数 \(a_{i_1\dots i_s}\) 成立，则在 \((h_{j_1},\dots,h_{j_s})\) 处求上述恒等式两边的值，并利用定理 7.2.9，可得对所有 \(1\le j_1<\cdots<j_s\le m\)，有 \(a_{j_1\dots j_s}=0\)。因此，\(\Theta_s\) 线性无关。

- (b)：由于 \(s>m\)，\(\Theta_s\) 的每个元素都含有重复的余向量，所以由定理 7.2.6 即得结论。
- (c)：由 (a) 和 (b) 部分可得。
- (d)：由 (c) 部分，\(\Lambda^m(V)\) 是一维的。
- (e)：对于 \(s=m-1\)，有 \(\Theta_{m-1}=\{(-1)^{i-1}\eta^i:i=1,\dots,m\}\)，所以由 (a) 部分即得结论。

下一个结果的 (a) 部分是定理 1.2.1(e) 的推广。

## 定理 7.2.13.

设 \(V\) 是向量空间，设 \(\eta\) 是 \(\Lambda^s(V)\) 中的多重余向量，设 \((h_1,\dots,h_m)\) 是 \(V\) 的基，设 \((\theta^1,\dots,\theta^m)\) 是它的对偶基。则：

- (a)

  \[
  \eta=\sum_{1\le i_1<\cdots<i_s\le m}\eta(h_{i_1},\dots,h_{i_s})\,\theta^{i_1}\wedge\cdots\wedge\theta^{i_s}.
  \]

- (b) 若 \(s=m\)，则

  \[
  \eta=\eta(h_1,\dots,h_m)\,\theta^1\wedge\cdots\wedge\theta^m.
  \]

证明. (a)：根据定理 7.2.12(a)，

\[
\eta=\sum_{1\le i_1<\cdots<i_s\le m}a_{i_1\dots i_s}\,\theta^{i_1}\wedge\cdots\wedge\theta^{i_s}
\]

对某些实数 \(a_{i_1\dots i_s}\) 成立。在 \((h_{j_1},\dots,h_{j_s})\) 处求上述恒等式两边的值，并利用定理 7.2.9，可得对所有 \(1\le j_1<\cdots<j_s\le m\)，有 \(a_{j_1\dots j_s}=\eta(h_{j_1},\dots,h_{j_s})\)。

(b)：由 (a) 部分可得。





## 7.3 多协向量的拉回

协变张量的拉回已在第 5.2 节中简要讨论过。多协向量的相应理论则丰富得多。

在继续之前，我们先暂停来考虑多指标记法，该记法在第 2.1 节中已在矩阵的上下文中讨论过。设 \(V\) 为向量空间，设 \((h_1,\dots,h_m)\) 为 \(V\) 的一组基，设 \((\theta^1,\dots,\theta^m)\) 为其对偶基。对于整数 \(1 \le s \le m\)，设 \(I = (i_1,\dots,i_s)\) 为 \(\mathcal{I}_{s,m}\) 中的一个多指标，并令我们记

\[
(h_{i_1},\dots,h_{i_s}) \quad\text{by}\quad h_I
\]

和

\[
\theta^{i_1}\wedge\cdots\wedge\theta^{i_s} \quad\text{by}\quad \theta^I.
\]

在此记法下，定理 7.2.12(a) 中 \(\Lambda^s(V)\) 的无序基可以简洁地表示为 \(\{\theta^I : I \in \mathcal{I}_{s,m}\}\)，而定理 7.2.13(a) 中的恒等式变为

\[
\eta = \sum_{I\in\mathcal{I}_{s,m}} \eta(h_I)\,\theta^I.
\]

如果我们以某种方式对 \(\mathcal{I}_{s,m}\) 进行排序，例如字典序，那么定理 7.2.12(a) 的基 \(\{\theta^I : I \in \mathcal{I}_{s,m}\}\) 也可以类似地排序，从而得到 \(\Lambda^s(V)\) 的一个有序基，我们记为

\[
(\theta^I : I \in \mathcal{I}_{s,m}).
\]

克罗内克 delta 可以推广到多指标情形。设 \(J = (j_1,\dots,j_s)\) 为 \(\mathcal{I}_{s,m}\) 中的另一个多指标，并定义

\[
\delta^J_I = \begin{cases} 1 & \text{if } I=J \\ 0 & \text{if } I\ne J. \end{cases}
\]

则定理 7.2.9 中的条件恒等式就是 \(\theta^J(h_I)=\delta^J_I\)。如附录 A 中所讨论的，\(\mathcal{I}_{1,m}\) 中多指标 \((i)\) 的补为

\[
(i)^c = (1,\dots,\hat{i},\dots,m) = (1,\dots,i-1,i+1,\dots,m)
\]

对于 \(i=1,\dots,m\)。在此记法下，定理 7.2.12(e) 中基的组成多协向量可以表示为 \(\eta^i = (-1)^{i-1}\theta^{(i)^c}\)。我们将在下文中发现多指标记法非常有用。

设 \(V\) 和 \(W\) 为向量空间，设 \(A: V \longrightarrow W\) 为线性映射。由 \(A\) 的拉回（针对多协向量）是线性映射族

\[
A^*: \Lambda^s(W) \longrightarrow \Lambda^s(V)
\]

对 \(s\ge1\) 定义为

\[
A^*(\eta)(v_1,\dots,v_s) = \eta(A(v_1),\dots,A(v_s)) \tag{7.3.1}
\]

对于所有 \(\Lambda^s(W)\) 中的多协向量 \(\eta\) 和所有 \(V\) 中的向量 \(v_1,\dots,v_s\)。由 \(\eta\) 的多线性与交替性质以及 \(A\) 的线性可知 \(A^*(\eta)\) 在 \(\Lambda^s(V)\) 中，因此定义是有意义的。我们称 \(A^*(\eta)\) 为 \(\eta\) 由 \(A\) 拉回。

### 定理 7.3.1

设 \(V\) 和 \(W\) 为向量空间，设 \(A: V \longrightarrow W\) 为线性映射，设 \(\eta\)、\(\zeta\) 为 \(\Lambda^s(W)\) 中的多协向量，设 \(\xi\) 为 \(\Lambda^{s'}(W)\) 中的多协向量。则：

- (a) \(A^*(\eta+\zeta)=A^*(\eta)+A^*(\zeta)\)。
- (b) \(A^*(\eta\wedge\xi)=A^*(\eta)\wedge A^*(\xi)\)。

**证明。** (a) 显而易见。

(b) 我们先给出一个初步结果。设 \(\zeta^1,\dots,\zeta^{s+s'}\) 为 \(W^*\) 中的（不一定相异）协向量。对于 \(V\) 中的向量 \(v_1,\dots,v_{s+s'}\)，我们有

\[
\begin{aligned}
A^*(\zeta^1\wedge\cdots\wedge\zeta^{s+s'})(v_1,\dots,v_{s+s'})
&=\bigl(\zeta^1\wedge\cdots\wedge\zeta^{s+s'}\bigr)(A(v_1),\dots,A(v_{s+s'}))\\
&=\det([\zeta^i(A(v_j))]) &&\text{[Th 7.2.8]}\\
&=\det([A^*(\zeta^i)(v_j)])\\
&=A^*(\zeta^1)\wedge\cdots\wedge A^*(\zeta^{s+s'})(v_1,\dots,v_{s+s'}). &&\text{[Th 7.2.8]}
\end{aligned}
\]

由于 \(v_1,\dots,v_{s+s'}\) 是任意的，

\[
A^*(\zeta^1\wedge\cdots\wedge\zeta^{s+s'}) = A^*(\zeta^1)\wedge\cdots\wedge A^*(\zeta^{s+s'}). \tag{7.3.2}
\]

现在进行主要证明。设 \((\varphi^1,\dots,\varphi^n)\) 为对应于 \(W\) 的一组基的对偶基。根据定理 7.2.12(a)，\(\eta\) 和 \(\xi\) 可以表示为

\[
\eta = \sum_{I\in\mathcal{I}_{s,n}} a_I \varphi^I \qquad \text{and}\qquad \xi = \sum_{J\in\mathcal{I}_{s',n}} b_J \varphi^J.
\]

则

\[
\eta\wedge\xi = \left(\sum_{I\in\mathcal{I}_{s,n}} a_I\varphi^I\right)\wedge \left(\sum_{J\in\mathcal{I}_{s',n}} b_J\varphi^J\right)
= \sum_{I\in\mathcal{I}_{s,n},\,J\in\mathcal{I}_{s',n}} a_I b_J\, \varphi^I\wedge\varphi^J,
\]

故

\[
\begin{aligned}
A^*(\eta\wedge\xi)
&= \sum_{I\in\mathcal{I}_{s,n},\,J\in\mathcal{I}_{s',n}} a_I b_J\, A^*(\varphi^I\wedge\varphi^J)\\
&= \sum_{I\in\mathcal{I}_{s,n},\,J\in\mathcal{I}_{s',n}} a_I b_J\, A^*(\varphi^I)\wedge A^*(\varphi^J)\\
&= \left(\sum_{I\in\mathcal{I}_{s,n}} a_I A^*(\varphi^I)\right)\wedge \left(\sum_{J\in\mathcal{I}_{s',n}} b_J A^*(\varphi^J)\right)\\
&= A^*(\eta)\wedge A^*(\xi),
\end{aligned}
\]

其中第二个等式由 (7.3.2) 推出。

### 定理 7.3.2（多协向量的拉回）

设 \(V\) 和 \(W\) 分别为维数为 \(m\) 和 \(n\) 的向量空间，设 \(\mathcal{H}\) 和 \(\mathcal{F}\) 为各自的基，设 \((\theta^1,\dots,\theta^m)\) 和 \((\varphi^1,\dots,\varphi^n)\) 为相应的对偶基。设 \(A:V\longrightarrow W\) 为线性映射，设 \(I\) 为 \(\mathcal{I}_{s,n}\) 中的多指标，其中 \(s\le \min(m,n)\)。则

\[
A^*(\varphi^I) = \sum_{J\in\mathcal{I}_{s,m}} \det\!\left(\left([A]_{\mathcal{H}}^{\mathcal{F}}\right)^I_J\right)\theta^J.
\]

**证明。** 设 \(\mathcal{H}=(h_1,\dots,h_m)\)。根据定理 7.2.13(a)，

\[
A^*(\varphi^I) = \sum_{J\in\mathcal{I}_{s,m}} A^*(\varphi^I)(h_J)\,\theta^J. \tag{7.3.3}
\]

设 \([A]_{\mathcal{H}}^{\mathcal{F}}=[a^i_j]\)。则 (2.2.2) 和 (2.2.3) 给出

\[
\varphi^i(A(h_j)) = \varphi^i\!\left(\sum_k a^k_j f_k\right)=\sum_k a^k_j\,\varphi^i(f_k)=a^i_j,
\]

故

\[
[A]_{\mathcal{H}}^{\mathcal{F}} = [\varphi^i(A(h_j))]. \tag{7.3.4}
\]

设 \(I=(i_1,\dots,i_s)\) 且 \(J=(j_1,\dots,j_s)\)。我们有

\[
\begin{aligned}
A^*(\varphi^I)(h_J)
&= A^*(\varphi^{i_1}\wedge\cdots\wedge\varphi^{i_s})(h_{j_1},\dots,h_{j_s})\\
&= \bigl(A^*(\varphi^{i_1})\wedge\cdots\wedge A^*(\varphi^{i_s})\bigr)(h_{j_1},\dots,h_{j_s}) &&\text{[Th 7.3.1(b)]}\\
&= \det\!\left(\left[A^*(\varphi^i)(h_j)\right]_{(i_1,\dots,i_s)}^{(j_1,\dots,j_s)}\right) &&\text{[Th 7.2.8]} \tag{7.3.5}\\
&= \det\!\left(\left[\varphi^i(A(h_j))\right]_{(i_1,\dots,i_s)}^{(j_1,\dots,j_s)}\right)\\
&= \det\!\left(\left([A]_{\mathcal{H}}^{\mathcal{F}}\right)^I_J\right). &&\text{[(7.3.4)]}
\end{aligned}
\]

将 (7.3.5) 代入 (7.3.3) 得到结果。

定理 7.3.2 的特殊情形提供了若干有用的恒等式。

### 定理 7.3.3

设 \(V\) 和 \(W\) 为维数均为 \(m\) 的向量空间，设 \(\mathcal{H}=(h_1,\dots,h_m)\) 和 \(\mathcal{F}=(f_1,\dots,f_m)\) 为各自的基，设 \((\theta^1,\dots,\theta^m)\) 和 \((\varphi^1,\dots,\varphi^m)\) 为相应的对偶基。设 \(A:V\longrightarrow W\) 为线性映射，设 \(\eta\) 为 \(\Lambda^m(W)\) 中的多协向量。则：

- (a) \(A^*(\varphi^1\wedge\cdots\wedge\varphi^m) = \det\!\left([A]_{\mathcal{H}}^{\mathcal{F}}\right)\,\theta^1\wedge\cdots\wedge\theta^m\)。
- (b) \(A^*(\eta)(h_1,\dots,h_m) = \det\!\left([A]_{\mathcal{H}}^{\mathcal{F}}\right)\,\eta(f_1,\dots,f_m)\)。

**证明。** (a) 由于 \(\mathcal{I}_{m,m}=\{(1,\dots,m)\}\)，这由定理 7.3.2 推出。

(b) 由定理 7.2.13(b) 可知

\[
\eta = \eta(f_1,\dots,f_m)\,\varphi^1\wedge\cdots\wedge\varphi^m,
\]

然后由 (a) 部分可得

\[
\begin{aligned}
A^*(\eta)
&= \eta(f_1,\dots,f_m)\,A^*(\varphi^1\wedge\cdots\wedge\varphi^m)\\
&= \eta(f_1,\dots,f_m)\det\!\left([A]_{\mathcal{H}}^{\mathcal{F}}\right)\theta^1\wedge\cdots\wedge\theta^m. \tag{7.3.6}
\end{aligned}
\]

将上述恒等式的两边同时作用于 \((h_1,\dots,h_m)\) 并使用定理 7.2.9 得到结果。

### 定理 7.3.4

设 \(V\) 为维数为 \(m\) 的向量空间，设 \(A:V\to V\) 为线性映射，设 \(\eta\) 为 \(\Lambda^m(V)\) 中的多协向量，设 \(v_1,\dots,v_m\) 为 \(V\) 中的向量。则：

- (a) \(A^*(\eta)=\det(A)\,\eta\)。
- (b) \(\eta(A(v_1),\dots,A(v_m))=\det(A)\,\eta(v_1,\dots,v_m)\)。

**证明。** (a) 取 \(W=V\) 且 \(\mathcal{F}=\mathcal{H}\) 于定理 7.3.3 中。则利用 (7.3.6)，

\[
A^*(\eta)=\det\left([A]_{\mathcal{H}}^{\mathcal{H}}\right)\eta(h_1,\dots,h_m)\,\theta^1\wedge\cdots\wedge\theta^m
=\det(A)\,\eta,
\]

其中最后一个等式由定理 7.2.13(b) 推出。

(b) 由 (a) 部分，

\[
\eta(A(v_1),\dots,A(v_m)) = A^*(\eta)(v_1,\dots,v_m) = \det(A)\,\eta(v_1,\dots,v_m).
\]

下一个结果让人联想到定理 2.4.9。

### 定理 7.3.5

设 \(V\) 为维数为 \(m\) 的向量空间，设 \(\eta\) 为 \(\Lambda^m(V)\) 中的非零多协向量，设 \(v_1,\dots,v_m\) 为 \(V\) 中的向量。则 \(v_1,\dots,v_m\) 线性无关当且仅当 \(\eta(v_1,\dots,v_m)\ne 0\)。

**证明。** 设 \(\mathcal{H}=(h_1,\dots,h_m)\) 为 \(V\) 的一组基，设 \((\theta^1,\dots,\theta^m)\) 为其对偶基。设 \(\mathcal{V}=(v_1,\dots,v_m)\)，设 \(A:V\to V\) 为由 \(A(\mathcal{H})=\mathcal{V}\) 定义的线性映射。由于 \(\eta\) 非零，由定理 7.2.13(b) 可得

\[
\eta(h_1,\dots,h_m)\ne 0, \tag{7.3.7}
\]

且由定理 7.3.4(b) 可得

\[
\eta(v_1,\dots,v_m) = \det(A)\,\eta(h_1,\dots,h_m). \tag{7.3.8}
\]

则

\[
\begin{aligned}
v_1,\dots,v_m \text{ 线性无关}
&\Longleftrightarrow \mathcal{V} \text{ 是 } V \text{ 的一组基}\\
&\Longleftrightarrow A \text{ 是线性同构} \quad [\mathrm{Th\ 1.1.13(a)}]\\
&\Longleftrightarrow \det(A)\ne 0 \quad [\mathrm{Th\ 2.5.3}]\\
&\Longleftrightarrow \eta(v_1,\dots,v_m)\ne 0. \quad [(7.3.7), (7.3.8)]
\end{aligned}
\]

顺便提及，\((\Longleftarrow)\) 由定理 7.1.2 推出。

### 定理 7.3.6（基变换）

设 \(V\) 为向量空间，设 \(\mathcal{H}\) 和 \(\mathcal{F}\) 为 \(V\) 的基，设 \((\theta^1,\dots,\theta^m)\) 和 \((\varphi^1,\dots,\varphi^m)\) 为相应的对偶基，设 \((\theta^I:I\in\mathcal{I}_{s,m})\) 和 \((\varphi^J:J\in\mathcal{I}_{s,m})\) 为 \(\Lambda^s(V)\) 的相应基。设 \(\eta\) 为 \(\Lambda^s(V)\) 中的多协向量，且

\[
\eta=\sum_{I\in\mathcal{I}_{s,m}} a_I\theta^I \qquad\text{and}\qquad \eta=\sum_{J\in\mathcal{I}_{s,m}} b_J\varphi^J.
\]

则

\[
a_I = \sum_{J\in\mathcal{I}_{s,m}} b_J \det\!\left(\left([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}\right)^J_I\right).
\]

**证明。** 将 \(W=V\) 且 \(A=\operatorname{id}_V\) 代入定理 7.3.2 得到

\[
\varphi^J = \sum_{I\in\mathcal{I}_{s,m}} \det\!\left(\left([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}\right)^J_I\right)\theta^I,
\]

故

\[
\begin{aligned}
\sum_{I\in\mathcal{I}_{s,m}} a_I\theta^I
&=\sum_{J\in\mathcal{I}_{s,m}} b_J\varphi^J\\
&=\sum_{J\in\mathcal{I}_{s,m}} b_J\left[\sum_{I\in\mathcal{I}_{s,m}} \det\!\left(\left([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}\right)^J_I\right)\theta^I\right]\\
&=\sum_{I\in\mathcal{I}_{s,m}}\left[\sum_{J\in\mathcal{I}_{s,m}} b_J \det\!\left(\left([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}\right)^J_I\right)\right]\theta^I,
\end{aligned}
\]

由此得到结果。

## 7.4 内乘

设 \(V\) 为维数为 \(m\) 的向量空间，设 \(v\) 为 \(V\) 中的向量。**由 \(v\) 的内乘** 是线性映射族

\[
i_v:\Lambda^s(V)\longrightarrow\Lambda^{s-1}(V)
\]

对 \(s\ge2\) 定义为

\[
i_v(\eta)(w_1,\dots,w_{s-1}) = \eta(v,w_1,\dots,w_{s-1})
\]

对于所有 \(\Lambda^s(V)\) 中的多协向量 \(\eta\) 和所有 \(V\) 中的向量 \(w_1,\dots,w_{s-1}\)。由于 \(V\) 中任意 \(m+1\) 个向量线性相关，由定理 7.3.5 可知 \(i_v\) 在 \(s>m\) 时为零映射。回顾由 (7.1.2) 和 (7.1.3) 可知 \(\Lambda^1(V)=V^*\) 且 \(\Lambda^0(V)=\mathbb R\)，我们将上述定义推广到 \(s=1\) 如下：

\[
i_v:\Lambda^1(V)\longrightarrow\Lambda^0(V)
\]

由

\[
i_v(\eta)=\eta(v)
\]

给出。





## 7.4 内乘

对所有余向量 η ∈ Λ¹(V)。当 s = 0 时，我们平凡地定义 iᵥ = 0。令我们记

\[
i_v \circ i_v \quad \text{by} \quad i_v^2.
\]

毫不奇怪，内乘对于基本代数结构表现良好，但它对楔积的处理更为复杂。

**定理 7.4.1.** 设 V 为向量空间，v, w 为 V 中的向量，η, ζ 为 Λˢ(V) 中的多向量，c 为实数。则：

(a) \(i_{v+w}(η)=i_v(η)+i_w(η)\)。

(b) \(i_{cv}(η)=c\,i_v(η)\)。

(c) \(i_v(η+ζ)=i_v(η)+i_v(ζ)\)。

(d) \(i_v(cη)=c\,i_v(η)\)。

*证明.* 显而易见。

**定理 7.4.2.** 设 V 为向量空间，v 为 V 中的向量，且 η¹, η², ⋯, ηˢ 为 V* 中的余向量。则

\[
i_v(η^1 \wedge \cdots \wedge η^s)=\sum_{j=1}^{s}(-1)^{j-1}η^j(v)\,η^1\wedge\cdots\wedge\widehat{η^j}\wedge\cdots\wedge η^s.
\]

其中 ^ 表示省略一个表达式。

*证明.* 对 V 中的向量 w₁, ⋯, wₛ₋₁，我们有

\[
\begin{aligned}
i_v(η^1\wedge\cdots\wedge η^s)(w_1,\ldots,w_{s-1})
&=(η^1\wedge\cdots\wedge η^s)(v,w_1,\ldots,w_{s-1})\\
&=\det\begin{pmatrix}
η^1(v)&η^1(w_1)&\cdots&η^1(w_{s-1})\\
\vdots&\vdots&&\vdots\\
η^s(v)&η^s(w_1)&\cdots&η^s(w_{s-1})
\end{pmatrix}\\
&=\sum_{j=1}^{s}(-1)^{j-1}η^j(v)\det\begin{pmatrix}
η^1(w_1)&\cdots&η^1(w_{s-1})\\
\vdots&&\vdots\\
η^s(w_1)&\cdots&η^s(w_{s-1})
\end{pmatrix}^{(j)}\\
&=\sum_{j=1}^{s}(-1)^{j-1}η^j(v)η^1\wedge\cdots\wedge\widehat{η^j}\wedge\cdots\wedge η^s(w_1,\ldots,w_{s-1}),
\end{aligned}
\]

其中第二个等式由定理 7.2.8 推出，第三个等式由沿第一列展开行列式得到。由于 w₁, ⋯, wₛ₋₁ 是任意的，结果得证。

下一个结果表明内乘满足一种新颖的乘积法则。

## 定理 7.4.3. 设 V 为向量空间，v 为 V 中的向量，且 η 和 ζ 分别为 Λ^s(V) 和 Λ^{s'}(V) 中的多余向量。则

i_v(η ∧ ζ) = i_v(η) ∧ ζ + (-1)^s η ∧ i_v(ζ).

# 证明.

由定理 7.3.1(b) 的证明所展示的原因，只需考虑形如 η = ξ^1 ∧ ⋯ ∧ ξ^s 和 ζ = ξ^{s+1} ∧ ⋯ ∧ ξ^{s+s'} 的多余向量，其中 ξ^j 为 V* 中的余向量，j = 1,2,…,s + s'。由定理 7.4.2，

i_v(η ∧ ζ) = i_v(ξ^1 ∧ ⋯ ∧ ξ^s ∧ ξ^{s+1} ∧ ⋯ ∧ ξ^{s+s'}) = Σ_{j=1}^s (-1)^{j-1} ξ^j(v) (ξ^1 ∧ ⋯ ∧ \widehat{ξ^j} ∧ ⋯ ∧ ξ^s) ∧ (ξ^{s+1} ∧ ⋯ ∧ ξ^{s+s'}) + Σ_{j=s+1}^{s+s'} (-1)^{j-1} ξ^j(v) (ξ^1 ∧ ⋯ ∧ ξ^s) ∧ (ξ^{s+1} ∧ ⋯ ∧ \widehat{ξ^j} ∧ ⋯ ∧ ξ^{s+s'}) = ( Σ_{j=1}^s (-1)^{j-1} ξ^j(v) ξ^1 ∧ ⋯ ∧ \widehat{ξ^j} ∧ ⋯ ∧ ξ^s ) ∧ ξ^{s+1} ∧ ⋯ ∧ ξ^{s+s'} + (-1)^s ξ^1 ∧ ⋯ ∧ ξ^s ∧ ( Σ_{j=s+1}^{s+s'} (-1)^{j-(s+1)} ξ^j(v) ξ^{s+1} ∧ ⋯ ∧ \widehat{ξ^j} ∧ ⋯ ∧ ξ^{s+s'} ) = i_v(η) ∧ ζ + (-1)^s η ∧ i_v(ζ).

## 7.5 多余向量数量积空间

在第 4.5 节中，我们展示了如何构造数量积空间的对偶。基于这一基础，我们现在将其推广到多向量。

设 (V, g) 为数量积空间，H 为 V 的基，Θ = (θ^1, …, θ^m) 为其对偶基。由定理 7.2.12(a)，(θ^I : I ∈ I_{s,m}) 为 Λ^s(V) 的基。我们定义一个双线性函数

g^Λ = ⟨·,·⟩^Λ : Λ^s(V) × Λ^s(V) ⟶ ℝ

如下。对于 Λ^s(V) 中的多余向量 η, ζ，其中

η = Σ_{I ∈ I_{s,m}} a_I θ^I    and    ζ = Σ_{J ∈ I_{s,m}} b_J θ^J,    (7.5.1)

令

⟨η, ζ⟩^Λ = Σ_{I,J ∈ I_{s,m}} det((g_Θ^*)_I^J) a_I b_J,    (7.5.2)

其中 𝔤* 为 V* 上的数量积，且

𝔤*_Θ = [ ⟨θ¹,θ¹⟩* … ⟨θ¹,θᵐ⟩* ; ⋮ ⋱ ⋮ ; ⟨θᵐ,θ¹⟩* … ⟨θᵐ,θᵐ⟩* ]

为 Θ 关于 𝔤* 的矩阵。

看起来 𝔤^∧ 的定义依赖于 V 的基的选择。令人惊讶的是，这并非如此。

## 定理 7.5.1. 在上述设定下，𝔤^∧ 与 V 基的选择无关。

证明. 设 ℱ 为 V 的另一基，Φ = (φ¹,…,φᵐ) 为其对偶基，且 (φᴶ : J ∈ ℐ_{s,m}) 为 Λˢ(V) 的相应基。设

η = Σ_{I∈ℐ_{s,m}} c_I φ^I   and   ζ = Σ_{J∈ℐ_{s,m}} d_J φ^J.

在 (7.5.1) 和 (7.5.2) 的记号下，我们需要证明

Σ_{I,J∈ℐ_{s,m}} det((𝔤*_Θ)_J^I) a_I b_J = Σ_{I,J∈ℐ_{s,m}} det((𝔤*_Φ)_J^I) c_I d_J.   (7.5.3)

设

P = [id_V]_{ℱ}^{ℋ}   and   Q = 𝔤*_Φ.   (7.5.4)

我们先进行几个初步计算。由于 Q 是对称的，我们由定理 2.1.6 得到

P^L (Q^I)^T = P^L (Q^T)_I = P^L Q_I = (P Q)_I^L   (7.5.5)

和

P^K ((P Q)^L)^T = P^K ((P Q)^T)_L = (P Q^T P^T)^K_L = (P Q P^T)^K_L.   (7.5.6)

我们也有

P Q P^T = [id_V]_{ℱ}^{ℋ} 𝔤*_Φ ([id_V]_{ℱ}^{ℋ})^T = ([id_V]_{ℋ}^{ℱ})^{-1} 𝔤_{ℱ}^{-1} (([id_V]_{ℋ}^{ℱ})^{-1})^T   [Th 2.2.5(b), Th 4.5.3(b)] = ((([id_V]_{ℋ}^{ℱ})^T) 𝔤_{ℱ} [id_V]_{ℋ}^{ℱ})^{-1}   [Th 2.1.3] = 𝔤_{ℋ}^{-1}   [Th 3.1.3] = 𝔤*_Θ,   [Th 4.5.3(b)]   (7.5.7)

且由定理 7.3.6 得
$c_I = \sum_{K \in \mathcal{I}_{s,m}} a_K \det(P_I^K) \quad \text{and} \quad d_J = \sum_{L \in \mathcal{I}_{s,m}} b_L \det(P_J^L). \tag{7.5.8}$
利用 (7.5.4) 和 (7.5.8)，(7.5.3) 的右边可表示为
$\sum_{I,J \in \mathcal{I}_{s,m}} \det\big((\mathfrak{g}_\Phi^*)_J^I\big) c_I d_J$
$= \sum_{I,J \in \mathcal{I}_{s,m}} \left[ \det(Q_J^I) \left( \sum_{K \in \mathcal{I}_{s,m}} a_K \det(P_I^K) \right) \left( \sum_{L \in \mathcal{I}_{s,m}} b_L \det(P_J^L) \right) \right] \tag{7.5.9}$
$= \sum_{K,L \in \mathcal{I}_{s,m}} \left[ \left( \sum_{I,J \in \mathcal{I}_{s,m}} \det(P_I^K) \det(P_J^L) \det(Q_J^I) \right) a_K b_L \right] .$

(7.5.9) 最后一行中的大括号项可表示为
$\sum_{I,J \in \mathcal{I}_{s,m}} \det(P_I^K) \det(P_J^L) \det(Q_J^I)$
$= \sum_{I \in \mathcal{I}_{s,m}} \left[ \det(P_I^K) \left( \sum_{J \in \mathcal{I}_{s,m}} \det(P_J^L) \det(Q_J^I) \right) \right]$
$= \sum_{I \in \mathcal{I}_{s,m}} \left( \det(P_I^K) \det\big(P^L (Q^I)^\mathsf{T}\big) \right) \quad \text{[Th 2.4.10(b)]}$
$= \sum_{I \in \mathcal{I}_{s,m}} \left( \det(P_I^K) \det\big((PQ)_I^L\big) \right) \quad \text{[(7.5.5)]}$
$= \det\left(P^K \big((PQ)^L\big)^\mathsf{T}\right) \quad \text{[Th 2.4.10(b)]}$
$= \det\left((PQ P^\mathsf{T})_L^K\right) \quad \text{[(7.5.6)]}$
$= \det\left((\mathfrak{g}_\Theta^*)_L^K\right). \quad \text{[(7.5.7)]}$

然后 (7.5.9) 和 (7.5.10) 给出
$\sum_{I,J \in \mathcal{I}_{s,m}} \det\big((\mathfrak{g}_\Phi^*)_J^I\big) c_I d_J = \sum_{K,L \in \mathcal{I}_{s,m}} \det\big((\mathfrak{g}_\Theta^*)_L^K\big) a_K b_L,$
这等价于 (7.5.3)。

## 定理 7.5.2.

设 $(V,\mathfrak{g})$ 为签名为 $(\varepsilon_1,\dots,\varepsilon_m)$ 的数量积空间，$\mathcal{E}$ 为 $V$ 的标准正交基，$(\xi^1,\dots,\xi^m)$ 为其对偶基，且 $I = (i_1,\dots,i_s)$ 和 $J$ 为 $\mathcal{I}_{s,m}$ 中的多指标。则
$\langle \xi^I, \xi^J \rangle^\Lambda = \begin{cases} \varepsilon_{i_1}\cdots\varepsilon_{i_s} & \text{if } I = J \\ 0 & \text{if } I \neq J, \end{cases}$
故每个 $\xi^I$ 都是单位向量。

证明. 设 Ξ = (ξ¹, …, ξᵐ)。由于 𝔤_ℰ = diag(ε₁, …, εₘ)，我们由定理 4.5.3(b) 得到

𝔤_Ξ* = 𝔤_ℰ⁻¹ = 𝔤_ℰ = diag(ε₁, …, εₘ),

然后由 (7.5.2) 得到

⟨ξ^I, ξ^J⟩^∧ = det(diag(ε₁, …, εₘ)^I_J).

若 I = J，则 diag(ε₁, …, εₘ)^I_J = diag(ε_{i₁}, …, ε_{i_s})。另一方面，若 I ≠ J，则 diag(ε₁, …, εₘ)^I_J 至少有一行（且至少有一列）为 0。结果得证。

我们现在处于定义第 4.5 节所述对偶数量积空间的多余向量对应物的位置。

## 定理 7.5.3. 设 (V, 𝔤) 为数量积空间，ℰ 为 V 的标准正交基，且 (ξ¹, …, ξᵐ) 为其对偶基。则：

+   (a) (Λ^s(V), 𝔤^∧) 是数量积空间，称为与 (V, 𝔤) 对应的多余向量数量积空间
+   (b) (ξ^I : I ∈ ℐ_{s,m}) 是 Λ^s(V) 的标准正交基。
+   (c) 若 (V, 𝔤) 为内积空间，则 (Λ^s(V), 𝔤^∧) 也是内积空间。

证明. (a): 我们需要证明 𝔤^∧ 是数量积。由构造，𝔤^∧ 是双线性的，且显然是对称的。根据定理 7.2.12(a)，(ξ^I : I ∈ ℐ_{s,m}) 是 Λ^s(V) 的基。设 η 为 Λ^s(V) 中的多余向量，使得对所有 Λ^s(V) 中的多余向量 ζ 都有 ⟨η, ζ⟩^∧ = 0，且令

η = Σ_{I ∈ ℐ_{s,m}} a_I ξ^I.   (7.5.11)

则定理 7.5.2 给出

0 = ⟨η, ξ^I⟩^∧ = ⟨ Σ_{J ∈ ℐ_{s,m}} a_J ξ^J, ξ^I ⟩^∧ = Σ_{J ∈ ℐ_{s,m}} a_J ⟨ξ^J, ξ^I⟩^∧ = ± a_I,

故 a_I = 0 对所有多指标 I ∈ ℐ_{s,m} 成立，所以 η = 0。于是，𝔤^∧ 是非退化的。

(b): 刚刚观察到 (ξ^I : I ∈ ℐ_{s,m}) 是 Λ^s(V) 的基。由定理 7.5.2，它是标准正交的。

(c): 由于 𝔤 是内积，我们由定理 7.5.2 得到

⟨ξ^I, ξ^J⟩^∧ = { 1 if I = J
                  0 if I ≠ J. }

以 (7.5.11) 中的 η 为例，

⟨η, η⟩^∧ = ⟨ Σ_{I ∈ ℐ_{s,m}} a_I ξ^I, Σ_{J ∈ ℐ_{s,m}} a_J ξ^J ⟩^∧ = Σ_{I,J ∈ ℐ_{s,m}} ⟨ξ^I, ξ^J⟩^∧ a_I a_J
          = Σ_{I ∈ ℐ_{s,m}} (a_I)² ≥ 0,

且等号成立当且仅当每个 a_I = 0，此时 η = 0。





现在我们已经有了标量积空间 $(\Lambda^s(V), \mathfrak{g}^\Lambda)$，第 4.5 节中的构造将产生相应的对偶标量积空间 $(\Lambda^s(V)^*, \mathfrak{g}^{\Lambda^*})$ 以及相关的平映射和尖映射：

```
F^\Lambda : \Lambda^s(V) \longrightarrow \Lambda^s(V)^* \quad \text{and} \quad S^\Lambda : \Lambda^s(V)^* \longrightarrow \Lambda^s(V).
```

根据定义，

```
\eta^{F^\Lambda}(\zeta) = \langle \eta, \zeta \rangle^\Lambda \tag{7.5.12}
```

对于 $\Lambda^s(V)$ 中的所有多重余向量 $\eta, \zeta$。
