

证明。设 ℰ = (e₁, …, eₘ)，且 V 的符号为 (ε₁, …, εₘ)。

(a)：设 u = Σᵢ bⁱ eᵢ，于是 [u]ℰ = [b¹ … bᵐ]ᵀ。我们由定理 2.4.6 和 (8.4.1) 得

⟨u, v₁ × ⋯ × vₘ₋₁⟩
= ⟨ Σᵢ bⁱ eᵢ, Σⱼ εⱼ (−1)ʲ⁻¹ det( [[v₁]ℰ … [vₘ₋₁]ℰ]⁽ʲ⁾ᶜ ) eⱼ ⟩
= Σᵢⱼ bⁱ ⟨eᵢ, eⱼ⟩ εⱼ (−1)ʲ⁻¹ det( [[v₁]ℰ … [vₘ₋₁]ℰ]⁽ʲ⁾ᶜ )
= Σᵢ (−1)ⁱ⁻¹ bⁱ det( [[v₁]ℰ … [vₘ₋₁]ℰ]⁽ⁱ⁾ᶜ )
= det( [[u]ℰ [v₁]ℰ … [vₘ₋₁]ℰ] ),

这证明了第一个等式。由定理 1.2.1(d)，u = Σᵢ ξⁱ(u) eᵢ，故

[u]ℰ = [ξ¹(u) … ξᵐ(u)]ᵀ,

[v₁]ℰ, …, [vₘ₋₁]ℰ 有相应的表达式。于是定理 7.2.8 给出

ξ¹ ∧ ⋯ ∧ ξᵐ(u, v₁, …, vₘ₋₁) = det( [ ξ¹(u) ξ¹(v₁) … ξ¹(vₘ₋₁) ; ⋮ ⋮ ⋱ ⋮ ; ξᵐ(u) ξᵐ(v₁) … ξᵐ(vₘ₋₁) ] )
= det( [[u]ℰ [v₁]ℰ … [vₘ₋₁]ℰ] ),

这证明了第二个等式。

(b)：设

P = [[u]ℰ [v₁]ℰ … [vₘ₋₁]ℰ],

并注意到 𝔮ℰ = diag(ε₁, …, εₘ)。于是

det( [ ⟨u,u⟩ ⟨u,v₁⟩ … ⟨u,vₘ₋₁⟩ ; ⟨v₁,u⟩ ⟨v₁,v₁⟩ … ⟨v₁,vₘ₋₁⟩ ; ⋮ ⋮ ⋱ ⋮ ; ⟨vₘ₋₁,u⟩ ⟨vₘ₋₁,v₁⟩ … ⟨vₘ₋₁,vₘ₋₁⟩ ] )
= det(Pᵀ 𝔮ℰ P) [Th 3.1.2(b)]
= det(𝔮ℰ) det(P)²
= (−1)ᵛ det(P)² [(4.2.3)]
= (−1)ᵛ ⟨u, v₁ × ⋯ × vₘ₋₁⟩². [part (a)]

## 定理 8.4.6。设 (V, 𝔤) 为维数 m ≥ 2 且指标为 ν 的数量积空间，设 ℰ 为 V 的标准正交基，且 v₁, …, vₘ₋₁, w₁, …, wₘ₋₁ 为 V 中的向量。则：

由于 [[v_j]ℰ [v₂]ℰ … [vₘ₋₁]ℰ] 在 (至少) 两个位置含有 [v_j]ℰ，故由定理 2.4.2 可知 det([[v_j]ℰ [v₂]ℰ … [vₘ₋₁]ℰ]^{(i)^c}) = 0 对 j = 2, …, m − 1 成立。于是，

det([[v₁]ℰ [v₂]ℰ … [vₘ₋₁]ℰ]^{(i)^c}) = 0

对 i = 1, …, m 成立。由 (8.4.1) 可知 v₁ × ⋯ × vₘ₋₁ = 0。

(⇐)：由于 span({v₁, …, vₘ₋₁}) 的维数至多为 m − 1，故存在向量 v₀ ∈ V \ span({v₁, …, vₘ₋₁})。于是

⟨v₀, v₁ × ⋯ × vₘ₋₁⟩ = 0
⇔ det([[v₀]ℰ [v₁]ℰ … [vₘ₋₁]ℰ]) = 0 [Th 8.4.5(a)]
⇔ [v₀]ℰ, [v₁]ℰ, …, [vₘ₋₁]ℰ 线性相关 [Th 2.4.9]
⇔ v₀, v₁, …, vₘ₋₁ 线性相关 [Th 2.2.1]
⇔ v₁, …, vₘ₋₁ 线性相关。 [Th 1.1.2]

因此，若 v₁ × ⋯ × vₘ₋₁ = 0，则 v₁, …, vₘ₋₁ 线性相关。

向量在向量空间中是否线性无关或线性相关与是否存在数量积无关。这意味着在定理 8.4.8 中，线性无关性可使用任意方便的数量积和标准正交基来检验。

## 定理 8.4.9。设 (V, g) 为维数 m ≥ 2 的数量积空间，设 ℰ 为 V 的标准正交基，设 U 为 V 的维数为 m − 1 的子空间，且 (u₁, …, uₘ₋₁) 为 U 的基。则 g 在 U 上非退化当且仅当 ||u₁ × ⋯ × uₘ₋₁|| ≠ 0。

证明。g|_U 关于 (u₁, …, uₘ₋₁) 的矩阵为 [⟨u_i, u_j⟩]，于是

- g 在 U 上非退化
- ⇔ g|_U 在 U 上非退化 [按定义]
- ⇔ det([⟨u_i, u_j⟩]) ≠ 0 [Th 3.3.3]
- ⇔ ⟨u₁ × ⋯ × uₘ₋₁, u₁ × ⋯ × uₘ₋₁⟩ ≠ 0 [Th 8.4.6(a)]
- ⇔ ||u₁ × ⋯ × uₘ₋₁|| ≠ 0.

## 定理 8.4.10。设 (V, g) 为维数 m ≥ 2 的数量积空间，且 ℰ 为 V 的标准正交基。设 U 为 V 的维数为 m − 1 且 g 在其上非退化的子空间，设 (u₁, …, uₘ₋₁) 为 U 的基，且 ε 为 ⟨u₁ × ⋯ × uₘ₋₁, u₁ × ⋯ × uₘ₋₁⟩ 的符号，于是

ε = ⟨ u₁ × ⋯ × uₘ₋₁ / ||u₁ × ⋯ × uₘ₋₁|| , u₁ × ⋯ × uₘ₋₁ / ||u₁ × ⋯ × uₘ₋₁|| ⟩ . (8.4.5)

则：

- (a) $U^\perp = \mathbb{R}(u_1 \times \cdots \times u_{m-1})$ 是 $V$ 的 1 维子空间。

- (b) $(\epsilon u_1 \times \cdots \times u_{m-1}, u_1, \ldots, u_{m-1})$ 是与 $\mathcal{E}$ 相容的 $V$ 的基。

- (c) 若 $(u_1, \ldots, u_{m-1})$ 是 $U$ 的标准正交基，则 $(\epsilon u_1 \times \cdots \times u_{m-1}, u_1, \ldots, u_{m-1})$ 是 $V$ 的标准正交基。

*注。* 由定理 8.4.9，$\langle u_1 \times \cdots \times u_{m-1}, u_1 \times \cdots \times u_{m-1} \rangle \neq 0$，故 ε 有定义且 (8.4.5) 中的分母非零。

*证明。* (a)：由定理 4.1.2(b) 可知 $U^\perp$ 是 $V$ 的 1 维子空间，由定理 8.4.3(a) 可知 $u_1 \times \cdots \times u_{m-1}$ 在 $U^\perp$ 中，且由定理 8.4.8 可知 $u_1 \times \cdots \times u_{m-1} \neq 0$。结论得证。

(b)：由定理 4.1.3，$V = U \oplus U^\perp$。由于 $(u_1, \ldots, u_{m-1})$ 是 $U$ 的基，由定理 1.1.2 和 (a) 可知 $\mathcal{U} = (\epsilon u_1 \times \cdots \times u_{m-1}, u_1, \ldots, u_{m-1})$ 是 $V$ 的基。我们由 (2.2.7) 和定理 8.4.5(a) 得

$\det\big([\mathrm{id}_V]^\mathcal{E}_\mathcal{U}\big) = \det\big([\epsilon u_1 \times \cdots \times u_{m-1}]_\mathcal{E}\ [u_1]_\mathcal{E}\ \cdots\ [u_{m-1}]_\mathcal{E}\big)$

$= \epsilon \langle u_1 \times \cdots \times u_{m-1}, u_1 \times \cdots \times u_{m-1} \rangle$

$= |\langle u_1 \times \cdots \times u_{m-1}, u_1 \times \cdots \times u_{m-1} \rangle| > 0,$

故 $\mathcal{U}$ 与 $\mathcal{E}$ 相容。

(c)：由于 $(u_1, \ldots, u_{m-1})$ 正交，由定理 8.4.6(b) 可知 $||\epsilon u_1 \times \cdots \times u_{m-1}|| = 1$，故 $\epsilon u_1 \times \cdots \times u_{m-1}$ 为单位向量。由定理 8.4.3(a)，$\langle \epsilon u_1 \times \cdots \times u_{m-1}, u_j \rangle = 0$ 对 $j = 1, \ldots, m-1$ 成立。结论得证。$\square$

## 定理 8.4.11。设 $(V, \mathfrak{g})$ 为维数 $m \ge 2$ 的数量积空间，设 $\mathcal{E}$ 为 $V$ 的标准正交基，设 $U$ 为 $V$ 的维数为 $m-1$ 且 $\mathfrak{g}$ 在其上非退化的子空间，且 $\mathcal{U} = (u_1, \ldots, u_{m-1})$ 和 $\mathcal{V} = (v_1, \ldots, v_{m-1})$ 为 $U$ 的基。则：

- (a)

$u_1 \times \cdots \times u_{m-1} = \det\big([\mathrm{id}_U]^\mathcal{V}_\mathcal{U}\big)\, v_1 \times \cdots \times v_{m-1}.$

- (b)

$\frac{u_1 \times \cdots \times u_{m-1}}{||u_1 \times \cdots \times u_{m-1}||} = \pm \frac{v_1 \times \cdots \times v_{m-1}}{||v_1 \times \cdots \times v_{m-1}||},$

其中正号（负号）是在 $\mathcal{U}$ 和 $\mathcal{V}$ 相容（不相容）时选取。

*注。* 由定理 8.4.9，(b) 部分的分母非零。

*证明。* (a)：设 $P = [\mathrm{id}_U]^\mathcal{V}_\mathcal{U}$ 且设 $A : U \longrightarrow U$ 为线性同构，定义为 $A(\mathcal{U}) = \mathcal{V}$。由定理 8.4.7，

$v_1 \times \cdots \times v_{m-1} = \det(A)\, u_1 \times \cdots \times u_{m-1},$

故 det(A) = det(P)^{-1}。结论得证。

(b)：由定理 2.4.12 和定理 8.4.9 可知 det(P)、||u₁ × ⋯ × uₘ₋₁|| 和 ||v₁ × ⋯ × vₘ₋₁|| 均非零。于是 (a) 给出

u₁ × ⋯ × uₘ₋₁ / ||u₁ × ⋯ × uₘ₋₁|| = det(P)/|det(P)| · v₁ × ⋯ × vₘ₋₁ / ||v₁ × ⋯ × vₘ₋₁|| = sgn(det(P)) · v₁ × ⋯ × vₘ₋₁ / ||v₁ × ⋯ × vₘ₋₁||.

## 定理 8.4.12。设 (V, g) 为维数 m ≥ 2 的数量积空间，设 ℰ = (e₁, …, e_m) 和 ℱ 为 V 的标准正交基，且 ⊠ 表示对应于 ℱ 的向量积运算。则

e₁ ⊠ ⋯ ⊠ ê_i ⊠ ⋯ ⊠ e_m = ε_i (−1)^{i−1} det([id_V]_ℰ^ℱ) e_i

对 i = 1, …, m 成立，其中 ^ 表示省略该表达式。

证明。设 U_i 为由 e₁, …, ê_i, …, e_m 张成的 V 的子空间。由于 g 在 ℝe_i 上非退化，由定理 4.1.3 可知 V = ℝe_i ⊕ (ℝe_i)⊥ 且 g 在 (ℝe_i)⊥ 上非退化。按定义，每个 e₁, …, ê_i, …, e_m 都与 e_i 正交，故 U_i ⊆ (ℝe_i)⊥。由定理 1.1.18，

m = dim(V) = dim(ℝe_i) + dim((ℝe_i)⊥) = 1 + dim((ℝe_i)⊥),

故 dim((ℝe_i)⊥) = m − 1 = dim(U_i)。

由定理 1.1.7(b)，U_i = (ℝe_i)⊥，故 g 在 U_i 上非退化。由定理 4.1.2(c) 和定理 8.4.10(a) 可知 ℝe_i = U_i⊥ = ℝ(e₁ ⊠ ⋯ ⊠ ê_i ⊠ ⋯ ⊠ e_m)，

故 e₁ ⊠ ⋯ ⊠ ê_i ⊠ ⋯ ⊠ e_m = c_i e_i 对某个实数 c_i 成立。我们有

c_i ε_i = ⟨e_i, c_i e_i⟩ = ⟨e_i, e₁ ⊠ ⋯ ⊠ ê_i ⊠ ⋯ ⊠ e_m⟩ = det([[e_i]_ℱ [e₁]_ℱ … [ê_i]_ℱ … [e_m]_ℱ]) [Th 8.4.5(a)] = (−1)^{i−1} det([[e₁]_ℱ … [e_i]_ℱ … [e_m]_ℱ]) [Th 8.4.3(d)] = (−1)^{i−1} det([id_V]_ℰ^ℱ), [(2.2.7)]

故 c_i = ε_i (−1)^{i−1} det([id_V]_ℰ^ℱ)。

结论得证。

