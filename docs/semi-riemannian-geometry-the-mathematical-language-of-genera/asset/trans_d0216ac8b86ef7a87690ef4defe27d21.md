

## 定理 19.1.3.

设 $(M,\mathfrak{g})$ 是一个 $m$ 维半黎曼流形，设 $p$ 是 $M$ 中的一点，设 $(U,(x^i))$ 与 $(\widetilde{U},(\widetilde{x}^i))$ 是 $p$ 处的坐标卡，并设 $\mathcal{X}_p$ 与 $\widetilde{\mathcal{X}}_p$ 是 $p$ 处相应的坐标基。则

$$\mathfrak{g}_{\widetilde{\mathcal{X}}}(p)=\left(\left[\mathrm{id}_{T_p(M)}\right]_{\mathcal{X}_p}^{\widetilde{\mathcal{X}}_p}\right)^{\mathrm{T}}\,\mathfrak{g}_{\mathcal{X}}(p)\left[\mathrm{id}_{T_p(M)}\right]_{\mathcal{X}_p}^{\widetilde{\mathcal{X}}_p},$$

其中

$$\left[\mathrm{id}_{T_p(M)}\right]_{\mathcal{X}_p}^{\widetilde{\mathcal{X}}_p}=\begin{bmatrix}\frac{\partial x^1}{\partial \widetilde{x}^1}(p)&\cdots&\frac{\partial x^1}{\partial \widetilde{x}^m}(p)\\\vdots&\ddots&\vdots\\\frac{\partial x^m}{\partial \widetilde{x}^1}(p)&\cdots&\frac{\partial x^m}{\partial \widetilde{x}^m}(p)\end{bmatrix}.$$

证明：这由定理 3.1.3 和定理 14.3.5(b) 得出。

设 $(\overline{M},\overline{\mathfrak{g}})$ 是一个半黎曼流形，并设 $M$ 是 $\overline{M}$ 的子流形。若 $\overline{\mathfrak{g}}|_M$ 是 $M$ 上的度量，则称 $(M,\overline{\mathfrak{g}}|_M)$ 是 $(\overline{M},\overline{\mathfrak{g}})$ 的半黎曼子流形。在这种情况下，根据定义：(i) 对 $M$ 中所有 $p$，$(\overline{\mathfrak{g}}|_M)_p$ 在 $T_p(M)$ 上非退化；(ii) $\mathrm{ind}((\overline{\mathfrak{g}}|_M)_p)$ 与 $M$ 中的 $p$ 无关。$\mathrm{ind}((\overline{\mathfrak{g}}|_M)_p)$ 的公共值记为 $\mathrm{ind}(\overline{\mathfrak{g}}|_M)$，称为 $\overline{\mathfrak{g}}|_M$ 的指标或 $M$ 的指标。$(\overline{\mathfrak{g}}|_M)_p$ 的公共符号差称为 $\overline{\mathfrak{g}}|_M$ 的符号差或 $M$ 的符号差。若 $(M,\overline{\mathfrak{g}}|_M)$ 在本身作为半黎曼流形来看时是黎曼（洛伦兹）流形，则称 $(M,\overline{\mathfrak{g}}|_M)$ 是黎曼（洛伦兹）子流形。当 $(M,\overline{\mathfrak{g}}|_M)$ 是 $\overline{M}$ 的黎曼（洛伦兹）子流形且同时是超曲面时，我们称 $(M,\overline{\mathfrak{g}}|_M)$ 为 $\overline{M}$ 的黎曼（洛伦兹）超曲面。注意到，若 $(\overline{M},\overline{\mathfrak{g}})$ 是黎曼流形，则 $(M,\overline{\mathfrak{g}}|_M)$ 自动是黎曼子流形。另一方面，若 $(\overline{M},\overline{\mathfrak{g}})$ 是洛伦兹流形，且 $(M,\overline{\mathfrak{g}}|_M)$ 是半黎曼子流形，则后者既可能是黎曼子流形，也可能是洛伦兹子流形。举例说明，由定理 12.2.12(d) 和定理 14.8.2 可知，$\mathcal{S}^2$ 是黎曼 3-流形 $\mathbb{R}^3$ 的黎曼超曲面，而 $\mathcal{H}^2$ 是洛伦兹 3-流形 $\mathbb{R}^3_1$ 的黎曼超曲面，$\mathcal{P}^2$ 是 $\mathbb{R}^3_1$ 的洛伦兹超曲面。

设 $(M,\overline{\mathfrak{g}}|_M)$ 是 $(\overline{M},\overline{\mathfrak{g}})$ 的半黎曼超曲面，并设 $p$ 是 $M$ 中的一点。根据 (14.8.1)，$T_p(M)$ 可以看作 $T_p(\overline{M})$ 的向量子空间。由定理 4.1.3 可得 $T_p(\overline{M})=T_p(M)\oplus T_p(M)^\perp$，再由定理 1.1.18 可得 $T_p(M)^\perp$ 是 1 维的。若 $T_p(\overline{M})$ 中的向量 $v$ 属于 $T_p(M)^\perp$，则称 $v$ 在 $p$ 处为法向量。若 $v$ 同时是单位向量，则称它为 $p$ 处的单位法向量。此时，由于 $v$ 非零，我们有 $T_p(M)^\perp=\mathbb{R}v$。设 $V$ 是沿 $M$ 的（未必光滑的）向量场。若对 $M$ 中所有 $p$，$V_p$ 都在 $p$ 处为法向量，则称 $V$ 是法向量场。当 $V$ 既是单位向量场又是法向量场时，称它为单位法向量场。

定理 12.2.2–12.2.5 可以直接推广到半黎曼流形的半黎曼超曲面的情形。由定理 12.2.5 的推广可知，存在沿 M 的（未必光滑的）单位法向量场 V，使得 ⟨V_p, V_p⟩ 与 M 中的 p 无关。⟨V_p, V_p⟩ 的公共值记为 ε_M，称为 M 的符号。因此，对所有 p ∈ M，ε_M = ⟨V_p, V_p⟩。由定理 12.2.2(b) 的推广可得 ε_M = (−1)^{μ−ind(𝔤|_M)}。

## 19.2 曲线

设 (M, 𝔤) 是一个半黎曼流形，λ(t) : (a, b) ⟶ M 是一条光滑曲线，g(u) : (c, d) ⟶ (a, b) 是一个微分同胚。光滑曲线 λ ∘ g(u) : (c, d) ⟶ ℝ³ 称为 λ 的光滑重参数化。回顾第 15.1 节，λ 的速度是光滑曲线 (dλ/dt)(t) : (a, b) ⟶ M，并且若 λ 的速度处处非零，则称 λ 是正则的。λ 的速率是（未必光滑的）曲线 ‖(dλ/dt)(t)‖ : (a, b) ⟶ M。当 ν = 0 时，λ 正则当且仅当其速率处处非零。若存在实数 c，使得对所有 t ∈ (a, b) 都有 ‖(dλ/dt)(t)‖ = c，则称 λ 具有常速率。若对所有 t ∈ (a, b)，(dλ/dt)(t) 都是类空（相应地，类时、类光）的，则称 λ 是类空（相应地，类时、类光）的。另外，若对所有 t ∈ (a, b)，(dλ/dt)(t) 都是未来指向（过去指向）的，则称 λ 是未来指向（过去指向）的。

设 λ(t) : [a, b] ⟶ M 是闭区间上的光滑曲线。λ 的长度定义为 L(λ) = ∫_a^b ‖ dλ/dt (t) ‖ dt。 (19.2.1)

## 定理 19.2.1（长度的微分同胚不变性）

设 (M, 𝔤) 是一个半黎曼流形，λ(t) : [a, b] ⟶ M 是一条光滑曲线，g(u) : [c, d] ⟶ [a, b] 是一个微分同胚。则 L(λ) = L(λ ∘ g)。

证明：该证明与定理 12.1.1 的证明相同。

## 19.3 半黎曼流形的基本定理

设 (M, 𝔤, ∇) 是带联络的半黎曼流形。如果没有进一步假设，就没有理由期望 𝔤 和 ∇ 之间存在联系。若对所有向量场 X, Y, Z ∈ 𝔛(M)，都有 X(⟨Y, Z⟩) = ⟨∇_X Y, Z⟩ + ⟨Y, ∇_X Z⟩ = ∇_X(⟨Y, Z⟩) (19.3.1)，则称 ∇ 与 𝔤 相容，其中第二个等号由 (18.1.1) 得出。根据第 18.4 节给出的定义，∇(𝔤) 是 T_3^0(M) 中的张量场；如果 ∇(𝔤) = 0，则 𝔤 是（关于 ∇）平行的。

## 定理 19.3.1.

设 $(M,\mathfrak{g},\nabla)$ 是带联络的半黎曼流形。则以下条件等价：

- (a) $\nabla$ 与 $\mathfrak{g}$ 相容。
- (b) $\mathfrak{g}$ 关于 $\nabla$ 平行。
- (c) 若 $\lambda(t): (a,b)\longrightarrow M$ 是正则光滑曲线，且 $J,K$ 是 $\mathfrak{X}_M(\lambda)$ 中的向量场，则

$$\frac{d\langle J,K\rangle}{dt}(t)=\left\langle\frac{\nabla J}{dt}(t),K(t)\right\rangle+\left\langle J(t),\frac{\nabla K}{dt}(t)\right\rangle$$

对所有 $t\in(a,b)$ 成立。
- (d) 若 $\lambda(t): (a,b)\longrightarrow M$ 是光滑曲线，且 $J,K$ 是 $\mathfrak{X}_M(\lambda)$ 中的平行向量场，则函数 $\langle J,K\rangle(t): (a,b)\longrightarrow\mathbb{R}$ 是常值函数。

证明：(a) ⇔ (b)：由定理 18.4.6，对所有向量场 $X,Y,Z\in\mathfrak{X}(M)$，有

$$\nabla(\mathfrak{g})(X,Y,Z)=X\langle Y,Z\rangle-\langle\nabla_X(Y),Z\rangle-\langle Y,\nabla_X(Z)\rangle$$

由此即得结论。

(a) ⇒ (c)：设 $\lambda(t): (a,b)\longrightarrow M$ 是正则光滑曲线，设 $J,K$ 是 $\mathfrak{X}_M(\lambda)$ 中的向量场，设 $t_0$ 是 $(a,b)$ 中的一点，并设 $p=\lambda(t_0)$。由定理 15.1.2，存在 $\mathfrak{X}(M)$ 中的向量场 $X$，使得 $X_p=(d\lambda/dt)(t_0)$。由于 $\lambda$ 是正则的，可以证明存在包含 $t_0$ 的区间 $(c,d)\subseteq(a,b)$ 以及 $\mathfrak{X}(M)$ 中的向量场 $Y,Z$，使得 $J|_{(c,d)}=Y\circ\lambda|_{(c,d)}$，$K|_{(c,d)}=Z\circ\lambda|_{(c,d)}$。于是

$$\frac{d\langle J,K\rangle}{dt}(t_0)=\frac{d\langle Y\circ\lambda,Z\circ\lambda\rangle}{dt}(t_0)=\frac{d\langle Y,Z\rangle\circ\lambda}{dt}(t_0)$$
$$=\frac{d\lambda}{dt}(t_0)\langle Y,Z\rangle=X_p\langle Y,Z\rangle=X\langle Y,Z\rangle(p)$$
$$=\langle\nabla_X(Y)_p,Z_p\rangle+\langle Y_p,\nabla_X(Z)_p\rangle$$
$$=\langle\nabla_{X_p}(Y),Z_p\rangle+\langle Y_p,\nabla_{X_p}(Z)\rangle$$
$$=\langle\nabla_{(d\lambda/dt)(t_0)}(Y),Z_p\rangle+\langle Y_p,\nabla_{(d\lambda/dt)(t_0)}(Z)\rangle$$
$$=\left\langle\frac{\nabla(Y\circ\lambda)}{dt}(t_0),Z_p\right\rangle+\left\langle Y_p,\frac{\nabla(Z\circ\lambda)}{dt}(t_0)\right\rangle$$
$$=\left\langle\frac{\nabla J}{dt}(t_0),K(t_0)\right\rangle+\left\langle J(t_0),\frac{\nabla K}{dt}(t_0)\right\rangle,$$

其中第三个等号由 (14.7.5) 得出，第五个等号由 (15.1.2) 得出，第六个等号根据假设，第七个等号由 (18.1.5) 得出，第九个等号由定理 18.3.1(c) 得出。由于 $t_0$ 是任意的，结论得证。

(c) ⇒ (a)：设 $X,Y,Z$ 是 $\mathfrak{X}(M)$ 中的向量场，设 $p$ 是 $M$ 中的一点，并设 $\lambda(t): (a,b)\longrightarrow M$ 是光滑曲线，使得 $\lambda(t_0)=p$ 且 $(d\lambda/dt)(t_0)=X_p$

