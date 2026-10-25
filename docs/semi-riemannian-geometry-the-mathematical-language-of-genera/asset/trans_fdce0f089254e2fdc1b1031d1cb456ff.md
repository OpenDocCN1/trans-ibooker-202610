

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

