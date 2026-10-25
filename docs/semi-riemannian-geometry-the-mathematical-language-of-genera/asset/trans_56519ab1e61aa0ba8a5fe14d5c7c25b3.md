
由定理 10.1.17 和定理 11.2.8 可知，$\beta^j \circ \varphi^{-1}$ 是光滑的。我们有

$D_{H_i \circ \varphi^{-1}}(\beta^j \circ \varphi^{-1})(p) = \frac{d(\beta^j \circ \varphi^{-1} \circ \lambda)}{dt}(0) \quad [(12.3.3)]$
$= \frac{d(\beta^j \circ \zeta)}{dt}(0)$
$= \frac{\partial \beta^j}{\partial r^i}(q) \quad [(12.3.12)]$
$= \frac{\partial \beta^j}{\partial r^i}(\varphi^{-1}(p)) = \left(\frac{\partial \beta^j}{\partial r^i} \circ \varphi^{-1}\right)(p).$

由于 $p$ 是任意的，

$D_{H_i \circ \varphi^{-1}}(\beta^j \circ \varphi^{-1}) = \frac{\partial \beta^j}{\partial r^i} \circ \varphi^{-1}. \qquad (12.3.13)$

于是

$D_{H_i \circ \varphi^{-1}}(Y) = D_{H_i \circ \varphi^{-1}}\left(\left[\sum_j \beta^j H_j\right] \circ \varphi^{-1}\right)$
$= D_{H_i \circ \varphi^{-1}}\left(\sum_j (\beta^j \circ \varphi^{-1})(H_j \circ \varphi^{-1})\right)$
$= \sum_j D_{H_i \circ \varphi^{-1}}\left((\beta^j \circ \varphi^{-1})(H_j \circ \varphi^{-1})\right)$
$= \sum_j \left[D_{H_i \circ \varphi^{-1}}(\beta^j \circ \varphi^{-1})(H_j \circ \varphi^{-1}) + (\beta^j \circ \varphi^{-1}) D_{H_i \circ \varphi^{-1}}(H_j \circ \varphi^{-1})\right]$
$= \sum_j \left(\frac{\partial \beta^j}{\partial r^i} \circ \varphi^{-1}\right)(H_j \circ \varphi^{-1}) + \sum_j (\beta^j \circ \varphi^{-1})\left[\left(\sum_k \Gamma^k_{ij} H_k + \vartheta_{ij} G\right) \circ \varphi^{-1}\right]$
$= \left[\sum_j \frac{\partial \beta^j}{\partial r^i} H_j + \sum_j \beta^j \left(\sum_k \Gamma^k_{ij} H_k + \vartheta_{ij} G\right)\right] \circ \varphi^{-1}, $

其中第五个等号由 (12.3.13) 和第 (a) 部分得到。因此，

$$D_{H_i\circ\varphi^{-1}}(Y)\circ\varphi=\sum_j\frac{\partial\beta^j}{\partial r^i}H_j+\sum_j\beta^j\left(\sum_k\Gamma^k_{ij}H_k+\vartheta_{ij}G\right)$$
$$=\sum_j\frac{\partial\beta^j}{\partial r^i}H_j+\sum_k\beta^k\left(\sum_j\Gamma^j_{ik}H_j+\vartheta_{ik}G\right)$$
$$=\sum_j\left(\frac{\partial\beta^j}{\partial r^i}+\sum_k\beta^k\Gamma^j_{ik}\right)H_j+\left(\sum_k\beta^k\vartheta_{ik}\right)G$$
$$=\sum_j\beta^j_{;i}H_j+\left(\sum_j\beta^j\vartheta_{ij}\right)G. \tag{12.3.9}$$

(c)：我们有
$$D_X(Y)=D_{\sum_i(\alpha^i\circ\varphi^{-1})(H_i\circ\varphi^{-1})}(Y)=\sum_i(\alpha^i\circ\varphi^{-1})D_{H_i\circ\varphi^{-1}}(Y)$$
$$=\left(\sum_i\alpha^i[D_{H_i\circ\varphi^{-1}}(Y)\circ\varphi]\right)\circ\varphi^{-1},$$
因此
$$D_X(Y)\circ\varphi=\sum_i\alpha^i[D_{H_i\circ\varphi^{-1}}(Y)\circ\varphi]$$
$$=\sum_i\alpha^i\left[\sum_j\beta^j_{;i}H_j+\left(\sum_j\beta^j\vartheta_{ij}\right)G\right]$$
$$=\sum_j\left(\sum_i\alpha^i\beta^j_{;i}\right)H_j+\left(\sum_{ij}\alpha^i\beta^j\vartheta_{ij}\right)G.$$

## 12.4 $\mathbb{R}^3_\nu$ 中正则曲面上的协变导数

设 $(M,g)$ 是 $\mathbb{R}^3_\nu$ 中的正则曲面，并设 $X,Y$ 是 $\mathfrak{X}(M)$ 中的向量场。正如结合示例 12.3.2 所述，尽管向量场 $D_X(Y)$ 属于 $\mathfrak{X}_{\mathbb{R}^3_\nu}(M)$，但它不一定属于 $\mathfrak{X}(M)$。换言之，即使对所有 $p\in M$，$Y_p$ 都是 $T_p(M)$ 中的向量，$D_X(Y)_p$ 却未必如此。我们需要一种“导数”的定义，把 $\mathfrak{X}(M)$ 中的向量场映到 $\mathfrak{X}(M)$ 中的向量场，从而避免这个问题。我们的做法是实用性的：通过消去与 $M$ 不相切的部分，来修改第 12.3 节讨论的诱导欧几里得导数。

对 $M$ 中的每一点 $p$，按定义 $g_p$ 在 $\mathbb{R}^3_\nu$ 的子空间 $T_p(M)$ 上非退化。由定理 4.1.3 可知，$\mathbb{R}^3_\nu$ 是直和 $\mathbb{R}^3_\nu=T_p(M)\oplus T_p(M)^\perp$。为简洁起见，我们将投影映射 $\mathcal{P}_{T_p(M)}$ 和 $\mathcal{P}_{T_p(M)}^\perp$ 分别记为 $\tan_p$ 和 $\operatorname{nor}_p$，于是

tan_p : ℝ³_ν ⟶ T_p(M)      and      nor_p : ℝ³_ν ⟶ T_p(M)⊥.

关于 $X$ 的协变导数由两个映射组成，二者都记作 $\nabla_X$。第一个是

∇_X : C^∞(M) ⟶ C^∞(M)

由

∇_X(f)(p) = d_p(f)(X_p)      (12.4.1)

定义，其中 $f$ 是 $C^\infty(M)$ 中的任意函数，$p$ 是 $M$ 中的任意一点。第二个是

∇_X : 𝔛(M) ⟶ 𝔛(M)

由

∇_X(Y)_p = tan_p(D_X(Y)_p)      (12.4.2)

定义，其中 $Y$ 是 $\mathfrak{X}(M)$ 中的任意向量场，$p$ 是 $M$ 中的任意一点，而 $D_X(Y)_p$ 由 (12.3.2) 给出。注意，在协变导数的定义中，所有向量场都位于 $\mathfrak{X}(M)$ 中。这与第 12.3 节中诱导欧几里得导数的定义形成对比，后者的定义中还出现了 $\mathfrak{X}_{\mathbb{R}^3_\nu}(M)$ 中的向量场。

对于 $\mathfrak{X}(M)$ 中的向量场 $X,Y$，我们通过如下赋值定义一个 $C^\infty(M)$ 中的函数 $\langle X,Y\rangle:M\longrightarrow\mathbb{R}$：

p ↦ ⟨X_p, Y_p⟩

对所有 $p\in M$ 成立。

### 定理 12.4.1. 设 $(M,g)$ 是 $\mathbb{R}^3_\nu$ 中的正则曲面，$X,Y,Z$ 是 $\mathfrak{X}(M)$ 中的向量场，$f$ 是 $C^\infty(M)$ 中的函数。则：

+   (a) $\nabla_{X+Y}(Z)=\nabla_X(Z)+\nabla_Y(Z)$.
+   (b) $\nabla_{fX}(Y)=f\nabla_X(Y)$.
+   (c) $\nabla_X(Y+Z)=\nabla_X(Y)+\nabla_X(Z)$.
+   (d) $\nabla_X(fY)=\nabla_X(f)Y+f\nabla_X(Z)$.
+   (e) $\nabla_X(\langle Y,Z\rangle)=\langle\nabla_X(Y),Z\rangle+\langle Y,\nabla_X(Z)\rangle$.

证明。(a)–(d)：这由定理 12.3.1 的 (a)–(d) 部分得到。

(e)：设 $p$ 是 $M$ 中的一点。我们有

∇_X(⟨Y, Z⟩)(p) = D_X(⟨Y, Z⟩)(p)      [(12.3.1), (12.4.1)]
= ⟨D_X(Y)_p, Z_p⟩ + ⟨Y_p, D_X(Z)_p⟩.      [Th 12.3.1(e)]

我们还有

D_X(Y)_p = tan_p(D_X(Y)_p) + nor_p(D_X(Y)_p)      [Th 4.1.4]
= ∇_X(Y)_p + nor_p(D_X(Y)_p),      [(12.4.2)]

其中

γ_{i;j}^l = ∂γ^l/∂r^j + Σ_n γ^n Γ^l_{jn}. (12.4.8)

我们寻求 (12.4.7) 第二行中两项的另一些表达式。

由 (12.4.5) 可得

∂μ^l/∂r^i = Σ_j ∂β^j/∂r^i γ_{i;j}^l + Σ_j β^j ∂γ_{i;j}^l/∂r^i, (12.4.9)

并由 (12.4.8) 可得

Σ_j ∂β^j/∂r^i γ_{i;j}^l = Σ_j ∂β^j/∂r^i ∂γ^l/∂r^j + Σ_{jn} ∂β^j/∂r^i γ^n Γ^l_{jn} (12.4.10)

以及

∂γ_{i;j}^l/∂r^i = ∂²γ^l/∂r^i∂r^j + Σ_n ∂γ^n/∂r^i Γ^l_{jn} + Σ_n γ^n ∂Γ^l_{jn}/∂r^i. (12.4.11)

于是由 (12.4.11) 可得

Σ_j β^j ∂γ_{i;j}^l/∂r^i = Σ_j β^j ∂²γ^l/∂r^i∂r^j + Σ_{jn} β^j ∂γ^n/∂r^i Γ^l_{jn} + Σ_{jn} β^j γ^n ∂Γ^l_{jn}/∂r^i. (12.4.12)

结合 (12.4.9)、(12.4.10) 和 (12.4.12) 得出

∂μ^l/∂r^i = Σ_j ∂β^j/∂r^i ∂γ^l/∂r^j + Σ_{jn} ∂β^j/∂r^i γ^n Γ^l_{jn} + Σ_j β^j ∂²γ^l/∂r^i∂r^j + Σ_{jn} β^j ∂γ^n/∂r^i Γ^l_{jn} + Σ_{jn} β^j γ^n ∂Γ^l_{jn}/∂r^i, (12.4.13)

这就是 (12.4.7) 第二行中第一项所需的表达式。由 (12.4.8) 可得

Σ_{jk} β^j γ_{i;j}^k Γ^l_{ik} = Σ_{jk} β^j (∂γ^k/∂r^j + Σ_n γ^n Γ^k_{jn}) Γ^l_{ik} = Σ_{jk} β^j ∂γ^k/∂r^j Γ^l_{ik} + Σ_{jkn} β^j γ^n Γ^k_{jn} Γ^l_{ik}, (12.4.14)

这就是 (12.4.7) 第二行中第二项所需的表达式。将 (12.4.13) 和 (12.4.14) 代入 (12.4.7)，得到

```
μ^l_{i;} = Σ_j ∂β^j/∂r^i ∂γ^l/∂r^j + Σ_{jn} ∂β^j/∂r^i γ^n Γ^l_{jn} + Σ_j β^j ∂²γ^l/∂r^i∂r^j + Σ_{jn} β^j ∂γ^n/∂r^i Γ^l_{jn} + Σ_{jn} β^j γ^n ∂Γ^l_{jn}/∂r^i + Σ_{jk} β^j ∂γ^k/∂r^j Γ^l_{ik} + Σ_{jkn} β^j γ^n Γ^k_{jn} Γ^l_{ik}. (12.4.15)
```

由 (12.4.6) 和 (12.4.15) 可知，$\nabla_X(\nabla_Y(Z))\circ\varphi$ 的第 $l$ 个分量为

```
(∇_X(∇_Y(Z)) ∘ φ)^l = Σ_i α^i μ^l_{i;} = Σ_{ij} α^i ∂β^j/∂r^i ∂γ^l/∂r^j + Σ_{ijn} α^i ∂β^j/∂r^i γ^n Γ^l_{jn} + Σ_{ij} α^i β^j ∂²γ^l/∂r^i∂r^j + Σ_{ijn} α^i β^j ∂γ^n/∂r^i Γ^l_{jn} + Σ_{ijn} α^i β^j γ^n ∂Γ^l_{jn}/∂r^i + Σ_{ijk} α^i β^j ∂γ^k/∂r^j Γ^l_{ik} + Σ_{ijkn} α^i β^j γ^n Γ^k_{jn} Γ^l_{ik} (12.4.16) = Σ_{ij}^{(1)} α^i ∂β^j/∂r^i ∂γ^l/∂r^j + Σ_{ijk}^{(2)} α^i ∂β^j/∂r^i γ^k Γ^l_{jk} + Σ_{ij}^{(3)} α^i β^j ∂²γ^l/∂r^i∂r^j + Σ_{ijk}^{(4)} α^i β^j ∂γ^k/∂r^i Γ^l_{jk} + Σ_{ijk}^{(5)} α^i β^j γ^k ∂Γ^l_{jk}/∂r^i + Σ_{ijk}^{(6)} α^i β^j ∂γ^k/∂r^j Γ^l_{ik} + Σ_{ijkn}^{(7)} α^i β^j γ^k Γ^n_{jk} Γ^l_{in},
```

其中求和已被编号以便参考。

## 步骤 2：计算 $\nabla_{\nabla_X(Y)}(Z)$

由定理 12.4.2(c)，

```
∇_X(Y) = Σ_j ( Σ_i (α^i ∘ φ^{-1})(β^j_{i;} ∘ φ^{-1}) ) H_j ∘ φ^{-1},
```

所以

$$\nabla_{\nabla_X(Y)}(Z)=\nabla\sum_j(\Sigma_i(\alpha^i\circ\varphi^{-1})(\beta^j_{;i}\circ\varphi^{-1})H_j\circ\varphi^{-1}(Z))$$

$$=\sum_{ij}(\alpha^i\circ\varphi^{-1})(\beta^j_{;i}\circ\varphi^{-1})\nabla_{H_j\circ\varphi^{-1}}(Z)$$

$$=\sum_{ij}(\alpha^i\circ\varphi^{-1})(\beta^j_{;i}\circ\varphi^{-1})\left(\sum_l(\gamma^l_{;j}\circ\varphi^{-1})H_l\circ\varphi^{-1}\right)$$

$$=\sum_l\left(\sum_{ij}(\alpha^i\circ\varphi^{-1})(\beta^j_{;i}\circ\varphi^{-1})(\gamma^l_{;j}\circ\varphi^{-1})\right)H_l\circ\varphi^{-1}$$

$$=\left(\sum_l\left[\sum_{ij}\alpha^i\beta^j_{;i}\gamma^l_{;j}\right]H_l\right)\circ\varphi^{-1},$$

其中第三个等号由定理 12.4.2(b) 得到。于是

$$\nabla_{\nabla_X(Y)}(Z)\circ\varphi=\sum_l\left(\sum_{ij}\alpha^i\beta^j_{;i}\gamma^l_{;j}\right)H_l,$$ (12.4.17)

其中

$$\beta^j_{;i}=\frac{\partial\beta^j}{\partial r^i}+\sum_k\beta^k\Gamma^j_{ik}\qquad\text{and}\qquad\gamma^l_{;j}=\frac{\partial\gamma^l}{\partial r^j}+\sum_n\gamma^n\Gamma^l_{jn}.$$ (12.4.18)

由 (12.4.17) 和 (12.4.18) 可知，$\nabla_{\nabla_X(Y)}(Z)\circ\varphi$ 的第 $l$ 个分量为

$$(\nabla_{\nabla_X(Y)}(Z)\circ\varphi)^l$$
$$=\sum_{ij}\alpha^i\beta^j_{;i}\gamma^l_{;j}$$
$$=\sum_{ij}\alpha^i\left(\frac{\partial\beta^j}{\partial r^i}+\sum_k\beta^k\Gamma^j_{ik}\right)\left(\frac{\partial\gamma^l}{\partial r^j}+\sum_n\gamma^n\Gamma^l_{jn}\right)$$
$$=\sum_{ij}\alpha^i\frac{\partial\beta^j}{\partial r^i}\frac{\partial\gamma^l}{\partial r^j}+\sum_{ijn}\alpha^i\frac{\partial\beta^j}{\partial r^i}\gamma^n\Gamma^l_{jn}$$
$$+\sum_{ijk}\alpha^i\beta^k\frac{\partial\gamma^l}{\partial r^j}\Gamma^j_{ik}+\sum_{ijkn}\alpha^i\beta^k\gamma^n\Gamma^j_{ik}\Gamma^l_{jn}$$
$$=\sum_{ij}^{(1)}\alpha^i\frac{\partial\beta^j}{\partial r^i}\frac{\partial\gamma^l}{\partial r^j}+\sum_{ijk}^{(2)}\alpha^i\frac{\partial\beta^j}{\partial r^i}\gamma^k\Gamma^l_{jk}$$
$$+\sum_{ijk}^{(8)}\alpha^i\beta^j\frac{\partial\gamma^l}{\partial r^k}\Gamma^k_{ij}+\sum_{ijkn}^{(9)}\alpha^i\beta^j\gamma^k\Gamma^n_{ij}\Gamma^l_{kn}.$$

