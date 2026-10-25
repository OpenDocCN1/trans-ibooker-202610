

+   (a) $G$ 是光滑的。
+   
+   (b) $\epsilon M = \langle G_q, G_q\rangle$ 对所有 $q$ 在 $U$ 中成立。
+   
+   (c) $G\circ\varphi^{-1}$ 是 $\mathbb{R}^3_\nu(\varphi(U))$ 中的单位法向量场。

**证明.** (a), (c): 由定理 12.2.7，$(\varphi(U),g|_{\varphi(U)})$ 是 $\mathbb{R}^3$ 中的正则曲面，所以 $g|_{\varphi(U)}$ 满足 [G1] 和 [G2]。令 $(V,g)=\mathbb{R}^3_\nu$，$U=T_p(M)$，且 $(u_1,u_2)=(H_1|_{\varphi^{-1}(p)},H_2|_{\varphi^{-1}(p)})$ 在定理 8.4.9 中，并利用定理 11.4.1(b)，由 [G1] 可得 $\|H_1|_{\varphi^{-1}(p)}\times H_2|_{\varphi^{-1}(p)}\|\ne 0$ 对所有 $p$ 在 $\varphi(U)$ 中成立。利用类似的推理，由定理 8.4.8，$H_1|_{\varphi^{-1}(p)}\times H_2|_{\varphi^{-1}(p)}\ne (0,0,0)$，并且由定理 8.4.4 和定理 11.4.1(b)，$H_1|_{\varphi^{-1}(p)}\times H_2|_{\varphi^{-1}(p)}$ 在所有 $p$ 在 $\varphi(U)$ 中属于 $T_p(\varphi(U))^\perp$。由此可得 $G\circ\varphi^{-1}$ 是沿 $\varphi(U)$ 的单位法向量场。我们由 [G2] 和定理 12.2.4 得到 $G\circ\varphi^{-1}$ 满足 [V2]。由于 $H_1$ 和 $H_2$ 在 $U$ 上光滑，由定理 10.1.17 和定理 11.2.8，$H_1\circ\varphi^{-1}\times H_2\circ\varphi^{-1}$ 在 $\varphi(U)$ 上光滑，因此，由定理 12.2.1(c)，$G\circ\varphi^{-1}$ 也是光滑的。我们随后由定理 10.1.17 得到 $G=(G\circ\varphi^{-1})\circ\varphi$ 在 $U$ 上光滑。

(b): 这由 (12.2.3) 和部分 (c) 得出。

继续定理 12.2.11 的设定，我们注意到，对 $M$ 上每个图表存在一个单位法光滑向量场，并不保证沿 $M$ 存在一个单位法光滑向量场。原因在于，对不同图表对应的单位法向量场可能在它们坐标域像的重叠上不一致。在第 12.7 节，我们对 $M$ 施加额外的结构来解决此问题。

现在让我们把注意力转向 $\mathbb{R}^3_\nu$ 中一类特殊的正则曲面。回顾第 3.1 节，对应于 $\mathbb{R}^3_\nu$ 的二次函数 $q$ 由 $q(\cdot)=\langle\cdot,\cdot\rangle$ 给出。我们考虑 $q$ 的三个水平集，其中第一个我们之前见过。对 $\nu=0$，**单位球面**为

$$S^2=\{p\in\mathbb{R}^3_0:q(p)=1\}=q^{-1}(1). \tag{12.2.6}$$

对 $\nu=1$，我们定义 **伪球面** 为

$$P^2=\{p\in\mathbb{R}^3_1:q(p)=1\}=q^{-1}(1), \tag{12.2.7}$$

以及 **双曲空间** 为

$$H^2=\{p\in\mathbb{R}^3_1:q(p)=-1\}=q^{-1}(-1). \tag{12.2.8}$$

因此，$S^2$ 是 $\mathbb{R}^3_0$ 中（类空）单位向量的集合，$P^2$ 是 $\mathbb{R}^3_1$ 中类空单位向量的集合，而 $H^2$ 是 $\mathbb{R}^3_1$ 中类时单位向量的集合。合起来，$S^2$、$P^2$ 和 $H^2$ 被称为 $\mathbb{R}^3_\nu$ 中的 **超二次曲面** 并统称为 $Q^2$。我们有以下表格：

| $Q^2$ | $\nu$ | 向量类型 |
|---|---|---:|---|
| $S^2$ | 0 | 类空 |
| $P^2$ | 1 | 类空 |
| $H^2$ | 1 | 类时 |

## 定理 12.2.12（超二次曲面）。设 $q$ 为对应于 $\mathbb{R}^3_\nu$ 的二次函数，设 $Q^2$ 为 $\mathbb{R}^3_\nu$ 中的超二次曲面，且 $p$ 为 $Q^2$ 中的一点。则：

+   (a) $Q^2$ 是 $\mathbb{R}^3_\nu$ 中的正则曲面；即 $S^2$ 是 $\mathbb{R}^3_0$ 中的正则曲面，且 $P^2$ 和 $H^2$ 是 $\mathbb{R}^3_1$ 中的正则曲面。

+   (b)

Grad(q)_p / ||Grad(q)_p|| = p.

+   (c) $p$ 是 $T_p(Q^2)^\perp$ 中的单位向量，其中第一个 $p$ 被视为 $\mathbb{R}^3_\nu$ 中的向量，第二个 $p$ 被视为 $Q^2$ 中的点。

+   (d) 超二次曲面具有以下特征：

| $Q^2$ | $\nu$ | 向量类型 | ind(g) | $\epsilon_{Q^2}$ |
|---|---|---|---|---|
| $S^2$ | 0 | 类空 | 0 | 1 |
| $P^2$ | 1 | 类空 | 1 | 1 |
| $H^2$ | 1 | 类时 | 0 | -1 |

证明. 设 $p = (x, y, z)$，为简便起见，令 $V = Grad(q)$。由于 $(1, 1, (-1)^\nu)$ 是 $\mathbb{R}^3_\nu$ 的符号，由定理 4.2.8，$q(x, y, z) = x^2 + y^2 + (-1)^\nu z^2$，故 $V_{(x,y,z)} = 2(x, y, z)$；即

V_p = 2p, (12.2.10)

所以

⟨V_p, V_p⟩ = 4⟨p, p⟩ = 4 q(p). (12.2.11)

(b): 如 (12.2.6)–(12.2.8) 所评论的那样，每个 $p$ 在 $Q^2$ 中都是单位向量。则 (12.2.11) 给出

||V_p|| = √|4⟨p, p⟩| = 2.

结果现在由 (12.2.10) 得出。

(a), (c): 我们由 (12.2.6)–(12.2.8) 得到 $Q^2$ 是 $q$ 的水平集，且由 (12.2.11) 得到 $V$ 满足 [V2]。由定理 12.2.10(b) 和部分 (b) 可得 $Q^2$ 是 $\mathbb{R}^3_\nu$ 中的正则曲面且 $p$ 是 $p$ 处的单位法向量。

(d): 第两列和第三列的条目直接来自 (12.2.9)。由 (12.2.3) 和部分 (c) 可得 $\epsilon_{Q^2} = \langle p, p\rangle$，所以第五列的条目由第三列的条目得出。利用 (12.2.4)，第四列的条目由第二列和第五列的条目得出 [见 (12.2.5)]。

有趣的是观察到，根据定理 12.2.12 部分 (d) 中的表格，$H^2$ 的指标为 0。因此，对每个 $p$ 在 $H^2$ 中，切平面 $T_p(H^2)$ 是一个内积空间，尽管 $T_p(H^2)$ 是洛伦兹向量空间 $\mathbb{R}^3_1$ 的子空间。

## 例 12.2.13（$\mathcal{S}^2$）。

在例 11.2.6 中以及再次在例 11.4.5 中已证明 $\mathcal{S}^2$ 是一个正则曲面。由定理 12.2.12(a)，$\mathcal{S}^2$ 是 $\mathbb{R}^3_0$ 中的正则曲面。

我们用一些后面会用到的定义来结束本节。设 $(M, \mathfrak{g})$ 是 $\mathbb{R}^3_\nu$ 中的正则曲面，设 $(U, \varphi)$ 是 $M$ 上的一个图表，且 $\mathcal{H} = (H_1, H_2)$ 为相应的坐标标架。我们定义 $C^\infty(U)$ 中的函数 $\mathfrak{g}_{ij}$ 为

```
𝔤ᵢⱼ(q) = ⟨Hᵢ|q, Hⱼ|q⟩
```

对所有 $q$ 在 $U$ 中，$i, j = 1, 2$。$\mathfrak{g}$ 关于 $\mathcal{H}$ 的矩阵记为 $\mathfrak{g}_{\mathcal{H}}$ 并定义为

```
𝔤_ℋ(q) = [ 𝔤₁₁(q)  𝔤₁₂(q) ; 𝔤₂₁(q)  𝔤₂₂(q) ]
```

对所有 $q$ 在 $U$ 中。令 $p = \varphi(q)$，我们回顾第 3.1 节，$\mathfrak{g}_p$ 关于 $\mathcal{H}_q$ 的矩阵为 $(\mathfrak{g}_p)_{\mathcal{H}_q} = [\mathfrak{g}_{ij}(q)]$。因此，作为记号，

```
𝔤_ℋ(q) = (𝔤_p)_ℋ_q.
```

$\mathfrak{g}$ 关于 $\mathcal{H}$ 的逆矩阵记为 $\mathfrak{g}_{\mathcal{H}}^{-1}$ 并定义为

```
𝔤_ℋ⁻¹(q) = 𝔤_ℋ(q)⁻¹
```

对所有 $q$ 在 $U$ 中。通常用上标表示 $\mathfrak{g}_{\mathcal{H}}(q)^{-1}$ 的条目：

```
𝔤_ℋ⁻¹(q) = [ 𝔤₁₁(q)  𝔤₁₂(q) ; 𝔤₂₁(q)  𝔤₂₂(q) ]⁻¹ = [ 𝔤¹¹(q)  𝔤¹²(q) ; 𝔤²¹(q)  𝔤²²(q) ].
```

映射 $q \mapsto \mathfrak{g}^{ij}(q)$ 定义了 $C^\infty(U)$ 中的函数 $\mathfrak{g}^{ij}$，$i, j = 1, 2$。由于 $[\mathfrak{g}_{ij}]$ 和 $[\mathfrak{g}^{ij}]$ 都是对称矩阵，函数 $\mathfrak{g}_{ij}$ 和 $\mathfrak{g}^{ij}$ 在 $i, j$ 上对称。

## 12.3 $\mathbb{R}^3_\nu$ 中的诱导欧几里得导数

设 $M$ 为正则曲面，且 $X$ 为 $\mathfrak{X}(M)$ 中的向量场。关于 $X$ 的诱导欧几里得导数由两个映射组成，均记为 $D_X$。第一个是

```
D_X : C^∞(M) ⟶ C^∞(M)
```

定义为

```
D_X(f)(p) = d_p(f)(X_p)   (12.3.1)
```

对所有函数 $f$ 在 $C^\infty(M)$ 中和所有 $p$ 在 $M$ 中。第二个是

```
D_X : 𝔛_ℝ³(M) ⟶ 𝔛_ℝ³(M)
```

定义为

```
D_X(V)_p = d_p(V)(X_p)   (12.3.2)
```

对所有向量场 $V$ 在 $\mathfrak{X}_{\mathbb{R}^3_\nu}(M)$ 中和所有 $p$ 在 $M$ 中。（从上下文中将清楚 $D_X$ 的记号是指关于 $X$ 的诱导欧几里得导数而不是第 10.3 节中讨论的关于 $X$ 的欧几里得导数。）

我们由 (11.5.1) 和 (11.7.1) 得到 $D_X(f)(p)$ 和 $D_X(V)_p$ 可以表示为

$$D_X(f)(p)=\frac{d(f\circ\lambda)}{dt}(t_0)$$  (12.3.3)

和

$$D_X(V)_p=\frac{d(V\circ\lambda)}{dt}(t_0),$$  (12.3.4)

其中 $\lambda(t):(a,b)\longrightarrow M$ 是任意光滑曲线使得 $\lambda(t_0)=p$ 且 $(d\lambda/dt)(t_0)=X_p$。设 $V=(\alpha^1,\alpha^2,\alpha^3)$。由定理 11.7.2 和 (12.3.2) 可得 $D_X(V)_p$ 也可以表示为

$$D_X(V)_p=\bigl(d_p(\alpha^1)(X_p),d_p(\alpha^2)(X_p),d_p(\alpha^3)(X_p)\bigr).$$  (12.3.5)

遵循 (12.2.2)，对向量场 $V,W$ 在 $\mathfrak{X}_{\mathbb{R}^3_\nu}(M)$ 中，我们定义函数

$$\langle V,W\rangle:M\longrightarrow\mathbb{R}$$

在 $C^\infty(M)$ 中，通过赋值

$$p\longmapsto\langle V_p,W_p\rangle$$

对所有 $p$ 在 $M$ 中。

下一个结果是定理 10.3.1 的对应物。

## **定理 12.3.1。**

设 $M$ 为正则曲面，设 $X,Y$ 和 $V,W$ 分别为 $\mathfrak{X}(M)$ 和 $\mathfrak{X}_{\mathbb{R}^3_\nu}(M)$ 中的向量场，且 $f$ 为 $C^\infty(M)$ 中的函数。则：

+   (a) $D_{X+Y}(V)=D_X(V)+D_Y(V)$。
+   
+   (b) $D_{fX}(V)=fD_X(V)$。
+   
+   (c) $D_X(V+W)=D_X(V)+D_X(W)$。
+   
+   (d) $D_X(fV)=D_X(f)V+fD_X(V)$。
+   
+   (e) $D_X(\langle V,W\rangle)=\langle D_X(V),W\rangle+\langle V,D_X(W)\rangle$。

**证明.** (a)–(d): 利用定理 11.5.3 和定理 11.7.2 得到结果。

(e): 设 $p$ 为 $M$ 中的一点，且 $\lambda(t):(a,b)\longrightarrow M$ 为光滑曲线使得 $\lambda(t_0)=p$ 且 $(d\lambda/dt)(t_0)=X_p$ 对某个 $t_0$ 在 $(a,b)$ 中成立。另设 $(\epsilon_1,\epsilon_2,\epsilon_3)=(1,1,(-1)^\nu)$ 为 $\mathbb{R}^3_\nu$ 的符号，且 $V=(\alpha^1,\alpha^2,\alpha^3)$ 和 $W=(\beta^1,\beta^2,\beta^3)$。由定理 4.2.8，

$$\langle V,W\rangle\circ\lambda=\langle V\circ\lambda,W\circ\lambda\rangle=\sum_i\epsilon_i(\alpha^i\circ\lambda)(\beta^i\circ\lambda),$$

