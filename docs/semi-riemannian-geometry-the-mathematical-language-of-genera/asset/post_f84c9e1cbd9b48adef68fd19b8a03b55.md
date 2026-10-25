
It follows from Theorem 10.1.17 and Theorem 11.2.8 that $\beta^j \circ \varphi^{-1}$ is smooth. We have

$D_{H_i \circ \varphi^{-1}}(\beta^j \circ \varphi^{-1})(p) = \frac{d(\beta^j \circ \varphi^{-1} \circ \lambda)}{dt}(0) \quad [(12.3.3)]$
$= \frac{d(\beta^j \circ \zeta)}{dt}(0)$
$= \frac{\partial \beta^j}{\partial r^i}(q) \quad [(12.3.12)]$
$= \frac{\partial \beta^j}{\partial r^i}(\varphi^{-1}(p)) = \left(\frac{\partial \beta^j}{\partial r^i} \circ \varphi^{-1}\right)(p).$

Since $p$ was arbitrary,

$D_{H_i \circ \varphi^{-1}}(\beta^j \circ \varphi^{-1}) = \frac{\partial \beta^j}{\partial r^i} \circ \varphi^{-1}. \qquad (12.3.13)$

Then

$D_{H_i \circ \varphi^{-1}}(Y) = D_{H_i \circ \varphi^{-1}}\left(\left[\sum_j \beta^j H_j\right] \circ \varphi^{-1}\right)$
$= D_{H_i \circ \varphi^{-1}}\left(\sum_j (\beta^j \circ \varphi^{-1})(H_j \circ \varphi^{-1})\right)$
$= \sum_j D_{H_i \circ \varphi^{-1}}\left((\beta^j \circ \varphi^{-1})(H_j \circ \varphi^{-1})\right)$
$= \sum_j \left[D_{H_i \circ \varphi^{-1}}(\beta^j \circ \varphi^{-1})(H_j \circ \varphi^{-1}) + (\beta^j \circ \varphi^{-1}) D_{H_i \circ \varphi^{-1}}(H_j \circ \varphi^{-1})\right]$
$= \sum_j \left(\frac{\partial \beta^j}{\partial r^i} \circ \varphi^{-1}\right)(H_j \circ \varphi^{-1}) + \sum_j (\beta^j \circ \varphi^{-1})\left[\left(\sum_k \Gamma^k_{ij} H_k + \vartheta_{ij} G\right) \circ \varphi^{-1}\right]$
$= \left[\sum_j \frac{\partial \beta^j}{\partial r^i} H_j + \sum_j \beta^j \left(\sum_k \Gamma^k_{ij} H_k + \vartheta_{ij} G\right)\right] \circ \varphi^{-1}, $

where the fifth equality follows from (12.3.13) and part (a). Thus,

$$D_{H_i\circ\varphi^{-1}}(Y)\circ\varphi=\sum_j\frac{\partial\beta^j}{\partial r^i}H_j+\sum_j\beta^j\left(\sum_k\Gamma^k_{ij}H_k+\vartheta_{ij}G\right)$$
$$=\sum_j\frac{\partial\beta^j}{\partial r^i}H_j+\sum_k\beta^k\left(\sum_j\Gamma^j_{ik}H_j+\vartheta_{ik}G\right)$$
$$=\sum_j\left(\frac{\partial\beta^j}{\partial r^i}+\sum_k\beta^k\Gamma^j_{ik}\right)H_j+\left(\sum_k\beta^k\vartheta_{ik}\right)G$$
$$=\sum_j\beta^j_{;i}H_j+\left(\sum_j\beta^j\vartheta_{ij}\right)G. \tag{12.3.9}$$

(c): We have
$$D_X(Y)=D_{\sum_i(\alpha^i\circ\varphi^{-1})(H_i\circ\varphi^{-1})}(Y)=\sum_i(\alpha^i\circ\varphi^{-1})D_{H_i\circ\varphi^{-1}}(Y)$$
$$=\left(\sum_i\alpha^i[D_{H_i\circ\varphi^{-1}}(Y)\circ\varphi]\right)\circ\varphi^{-1},$$
hence
$$D_X(Y)\circ\varphi=\sum_i\alpha^i[D_{H_i\circ\varphi^{-1}}(Y)\circ\varphi]$$
$$=\sum_i\alpha^i\left[\sum_j\beta^j_{;i}H_j+\left(\sum_j\beta^j\vartheta_{ij}\right)G\right]$$
$$=\sum_j\left(\sum_i\alpha^i\beta^j_{;i}\right)H_j+\left(\sum_{ij}\alpha^i\beta^j\vartheta_{ij}\right)G.$$

## 12.4 Covariant Derivative on Regular Surfaces in $\mathbb{R}^3_\nu$

Let $(M,g)$ be a regular surface in $\mathbb{R}^3_\nu$, and let $X,Y$ be vector fields in $\mathfrak{X}(M)$. As remarked in conjunction with Example 12.3.2, although the vector field $D_X(Y)$ is in $\mathfrak{X}_{\mathbb{R}^3_\nu}(M)$, it may not be in $\mathfrak{X}(M)$. In other words, even though $Y_p$ is a vector in $T_p(M)$ for all $p$ in $M$, the same might not be true of $D_X(Y)_p$. We need a definition of “derivative” that sends vector fields in $\mathfrak{X}(M)$ to vector fields in $\mathfrak{X}(M)$, thereby avoiding this problem. Our approach is pragmatic: we modify the induced Euclidean derivative, discussed in Section 12.3, by eliminating the part that is not tangential to $M$.

For each point $p$ in $M$, we have by definition that $g_p$ is nondegenerate on the subspace $T_p(M)$ of $\mathbb{R}^3_\nu$. It follows from Theorem 4.1.3 that $\mathbb{R}^3_\nu$ is the direct sum $\mathbb{R}^3_\nu=T_p(M)\oplus T_p(M)^\perp$. For brevity, let us denote the projection maps $\mathcal{P}_{T_p(M)}$

and 𝒫_{T_p(M)}^⊥ by tan_p and nor_p, respectively, so that

tan_p : ℝ³_ν ⟶ T_p(M)      and      nor_p : ℝ³_ν ⟶ T_p(M)⊥.

The covariant derivative with respect to X consists of two maps, both denoted by ∇_X. The first is

∇_X : C^∞(M) ⟶ C^∞(M)

defined by

∇_X(f)(p) = d_p(f)(X_p)      (12.4.1)

for all functions f in C^∞(M) and all p in M. The second is

∇_X : 𝔛(M) ⟶ 𝔛(M)

defined by

∇_X(Y)_p = tan_p(D_X(Y)_p)      (12.4.2)

for all vector fields Y in 𝔛(M) and all p in M, where D_X(Y)_p is given by (12.3.2). Observe that in the definition of the covariant derivative, all vector fields reside in 𝔛(M). This is in contrast to the definition in Section 12.3 of the induced Euclidean derivative where vector fields in 𝔛_{ℝ³_ν}(M) also appear.

For vector fields X, Y in 𝔛(M), we define a function

⟨X, Y⟩ : M ⟶ ℝ

in C^∞(M) by the assignment

p ↦ ⟨X_p, Y_p⟩

for all p in M.

### Theorem 12.4.1. Let (M, 𝔤) be a regular surface in ℝ³_ν, let X, Y, Z be vector fields in 𝔛(M), and let f be a function in C^∞(M). Then:

+   (a) ∇_{X+Y}(Z) = ∇_X(Z) + ∇_Y(Z).
+   (b) ∇_{fX}(Y) = f∇_X(Y).
+   (c) ∇_X(Y + Z) = ∇_X(Y) + ∇_X(Z).
+   (d) ∇_X(fY) = ∇_X(f)Y + f∇_X(Z).
+   (e) ∇_X(⟨Y, Z⟩) = ⟨∇_X(Y), Z⟩ + ⟨Y, ∇_X(Z)⟩.

Proof. (a)–(d): This follows from parts (a)–(d) of Theorem 12.3.1.

(e): Let p be a point in M. We have

∇_X(⟨Y, Z⟩)(p) = D_X(⟨Y, Z⟩)(p)      [(12.3.1), (12.4.1)]
= ⟨D_X(Y)_p, Z_p⟩ + ⟨Y_p, D_X(Z)_p⟩.      [Th 12.3.1(e)]

We also have

D_X(Y)_p = tan_p(D_X(Y)_p) + nor_p(D_X(Y)_p)      [Th 4.1.4]
= ∇_X(Y)_p + nor_p(D_X(Y)_p),      [(12.4.2)]

where

γ_{i;j}^l = ∂γ^l/∂r^j + Σ_n γ^n Γ^l_{jn}. (12.4.8)

We seek alternative expressions for the two terms in the second row of (12.4.7).

It follows from (12.4.5) that

∂μ^l/∂r^i = Σ_j ∂β^j/∂r^i γ_{i;j}^l + Σ_j β^j ∂γ_{i;j}^l/∂r^i, (12.4.9)

and from (12.4.8) that

Σ_j ∂β^j/∂r^i γ_{i;j}^l = Σ_j ∂β^j/∂r^i ∂γ^l/∂r^j + Σ_{jn} ∂β^j/∂r^i γ^n Γ^l_{jn} (12.4.10)

and

∂γ_{i;j}^l/∂r^i = ∂²γ^l/∂r^i∂r^j + Σ_n ∂γ^n/∂r^i Γ^l_{jn} + Σ_n γ^n ∂Γ^l_{jn}/∂r^i. (12.4.11)

Then (12.4.11) gives

Σ_j β^j ∂γ_{i;j}^l/∂r^i = Σ_j β^j ∂²γ^l/∂r^i∂r^j + Σ_{jn} β^j ∂γ^n/∂r^i Γ^l_{jn} + Σ_{jn} β^j γ^n ∂Γ^l_{jn}/∂r^i. (12.4.12)

Combining (12.4.9), (12.4.10), and (12.4.12) yields

∂μ^l/∂r^i = Σ_j ∂β^j/∂r^i ∂γ^l/∂r^j + Σ_{jn} ∂β^j/∂r^i γ^n Γ^l_{jn} + Σ_j β^j ∂²γ^l/∂r^i∂r^j + Σ_{jn} β^j ∂γ^n/∂r^i Γ^l_{jn} + Σ_{jn} β^j γ^n ∂Γ^l_{jn}/∂r^i, (12.4.13)

which is the desired expression for the first term in the second row of (12.4.7). We have from (12.4.8) that

Σ_{jk} β^j γ_{i;j}^k Γ^l_{ik} = Σ_{jk} β^j (∂γ^k/∂r^j + Σ_n γ^n Γ^k_{jn}) Γ^l_{ik} = Σ_{jk} β^j ∂γ^k/∂r^j Γ^l_{ik} + Σ_{jkn} β^j γ^n Γ^k_{jn} Γ^l_{ik}, (12.4.14)

which is the desired expression for the second term in the second row of (12.4.7). Substituting (12.4.13) and (12.4.14) into (12.4.7) gives

```
μ^l_{i;} = Σ_j ∂β^j/∂r^i ∂γ^l/∂r^j + Σ_{jn} ∂β^j/∂r^i γ^n Γ^l_{jn} + Σ_j β^j ∂²γ^l/∂r^i∂r^j + Σ_{jn} β^j ∂γ^n/∂r^i Γ^l_{jn} + Σ_{jn} β^j γ^n ∂Γ^l_{jn}/∂r^i + Σ_{jk} β^j ∂γ^k/∂r^j Γ^l_{ik} + Σ_{jkn} β^j γ^n Γ^k_{jn} Γ^l_{ik}. (12.4.15)
```

It follows from (12.4.6) and (12.4.15) that the lth component of ∇_X(∇_Y(Z)) ∘ φ is

```
(∇_X(∇_Y(Z)) ∘ φ)^l = Σ_i α^i μ^l_{i;} = Σ_{ij} α^i ∂β^j/∂r^i ∂γ^l/∂r^j + Σ_{ijn} α^i ∂β^j/∂r^i γ^n Γ^l_{jn} + Σ_{ij} α^i β^j ∂²γ^l/∂r^i∂r^j + Σ_{ijn} α^i β^j ∂γ^n/∂r^i Γ^l_{jn} + Σ_{ijn} α^i β^j γ^n ∂Γ^l_{jn}/∂r^i + Σ_{ijk} α^i β^j ∂γ^k/∂r^j Γ^l_{ik} + Σ_{ijkn} α^i β^j γ^n Γ^k_{jn} Γ^l_{ik} (12.4.16) = Σ_{ij}^{(1)} α^i ∂β^j/∂r^i ∂γ^l/∂r^j + Σ_{ijk}^{(2)} α^i ∂β^j/∂r^i γ^k Γ^l_{jk} + Σ_{ij}^{(3)} α^i β^j ∂²γ^l/∂r^i∂r^j + Σ_{ijk}^{(4)} α^i β^j ∂γ^k/∂r^i Γ^l_{jk} + Σ_{ijk}^{(5)} α^i β^j γ^k ∂Γ^l_{jk}/∂r^i + Σ_{ijk}^{(6)} α^i β^j ∂γ^k/∂r^j Γ^l_{ik} + Σ_{ijkn}^{(7)} α^i β^j γ^k Γ^n_{jk} Γ^l_{in},
```

where the summations have been numbered for easy reference.

## Step 2. Compute ∇_{∇_X(Y)}(Z).

By Theorem 12.4.2(c),

```
∇_X(Y) = Σ_j ( Σ_i (α^i ∘ φ^{-1})(β^j_{i;} ∘ φ^{-1}) ) H_j ∘ φ^{-1},
```

so

$$\nabla_{\nabla_X(Y)}(Z)=\nabla\sum_j(\Sigma_i(\alpha^i\circ\varphi^{-1})(\beta^j_{;i}\circ\varphi^{-1})H_j\circ\varphi^{-1}(Z))$$

$$=\sum_{ij}(\alpha^i\circ\varphi^{-1})(\beta^j_{;i}\circ\varphi^{-1})\nabla_{H_j\circ\varphi^{-1}}(Z)$$

$$=\sum_{ij}(\alpha^i\circ\varphi^{-1})(\beta^j_{;i}\circ\varphi^{-1})\left(\sum_l(\gamma^l_{;j}\circ\varphi^{-1})H_l\circ\varphi^{-1}\right)$$

$$=\sum_l\left(\sum_{ij}(\alpha^i\circ\varphi^{-1})(\beta^j_{;i}\circ\varphi^{-1})(\gamma^l_{;j}\circ\varphi^{-1})\right)H_l\circ\varphi^{-1}$$

$$=\left(\sum_l\left[\sum_{ij}\alpha^i\beta^j_{;i}\gamma^l_{;j}\right]H_l\right)\circ\varphi^{-1},$$

where the third equality follows from Theorem 12.4.2(b). Then

$$\nabla_{\nabla_X(Y)}(Z)\circ\varphi=\sum_l\left(\sum_{ij}\alpha^i\beta^j_{;i}\gamma^l_{;j}\right)H_l,$$ (12.4.17)

where

$$\beta^j_{;i}=\frac{\partial\beta^j}{\partial r^i}+\sum_k\beta^k\Gamma^j_{ik}\qquad\text{and}\qquad\gamma^l_{;j}=\frac{\partial\gamma^l}{\partial r^j}+\sum_n\gamma^n\Gamma^l_{jn}.$$ (12.4.18)

From (12.4.17) and (12.4.18), the $l$th component of $\nabla_{\nabla_X(Y)}(Z)\circ\varphi$ is

$$(\nabla_{\nabla_X(Y)}(Z)\circ\varphi)^l$$
$$=\sum_{ij}\alpha^i\beta^j_{;i}\gamma^l_{;j}$$
$$=\sum_{ij}\alpha^i\left(\frac{\partial\beta^j}{\partial r^i}+\sum_k\beta^k\Gamma^j_{ik}\right)\left(\frac{\partial\gamma^l}{\partial r^j}+\sum_n\gamma^n\Gamma^l_{jn}\right)$$
$$=\sum_{ij}\alpha^i\frac{\partial\beta^j}{\partial r^i}\frac{\partial\gamma^l}{\partial r^j}+\sum_{ijn}\alpha^i\frac{\partial\beta^j}{\partial r^i}\gamma^n\Gamma^l_{jn}$$
$$+\sum_{ijk}\alpha^i\beta^k\frac{\partial\gamma^l}{\partial r^j}\Gamma^j_{ik}+\sum_{ijkn}\alpha^i\beta^k\gamma^n\Gamma^j_{ik}\Gamma^l_{jn}$$
$$=\sum_{ij}^{(1)}\alpha^i\frac{\partial\beta^j}{\partial r^i}\frac{\partial\gamma^l}{\partial r^j}+\sum_{ijk}^{(2)}\alpha^i\frac{\partial\beta^j}{\partial r^i}\gamma^k\Gamma^l_{jk}$$
$$+\sum_{ijk}^{(8)}\alpha^i\beta^j\frac{\partial\gamma^l}{\partial r^k}\Gamma^k_{ij}+\sum_{ijkn}^{(9)}\alpha^i\beta^j\gamma^k\Gamma^n_{ij}\Gamma^l_{kn}.$$
