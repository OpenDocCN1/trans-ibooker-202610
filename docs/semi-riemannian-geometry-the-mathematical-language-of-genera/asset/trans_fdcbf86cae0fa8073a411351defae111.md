

## 19.7 等距映射

其中假设 $r\psi(s)$ 在 $\mathcal{E}_p$ 中对所有 $(r,s)$ 在 $[0,1]\times(-\varepsilon,\varepsilon)$ 成立。则，对所有 $(r,s)$ 在 $[0,1]\times(-\varepsilon,\varepsilon)$：

-   (a)

$$\left\langle \frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial r}(r,s)\right\rangle=\langle\psi(s),\psi(s)\rangle.$$

-   (b)

$$\left\langle\frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial s}(r,s)\right\rangle=r\left\langle\frac{d\psi}{ds}(s),\psi(s)\right\rangle.$$

**证明。** (a): 回顾第 14.9 节，对于 $(-\varepsilon,\varepsilon)$ 中的给定点 $s$，经度曲线 $\sigma_s(r):[0,1]\longrightarrow M$ 定义为 $\sigma_s(r)=\sigma(r,s)$。根据定理 18.11.4(a)，$\sigma_s$ 是以 $\psi(s)$ 为初速度的测地线；即，$(d\sigma_s/dr)(0)=\psi(s)$。由定理 19.9.1(a) 可知，函数 $\langle d\sigma_s/dr,d\sigma_s/dr\rangle:[0,1]\longrightarrow M$ 为常数，故

$$\left\langle\frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial r}(r,s)\right\rangle=\left\langle\frac{d\sigma_s}{dr}(r),\frac{d\sigma_s}{dr}(r)\right\rangle=\left\langle\frac{d\sigma_s}{dr}(0),\frac{d\sigma_s}{dr}(0)\right\rangle=\langle\psi(s),\psi(s)\rangle.$$

(b): 根据定理 19.3.1，

$$\frac{\partial}{\partial r}\left(\left\langle\frac{\partial\sigma}{\partial r},\frac{\partial\sigma}{\partial s}\right\rangle\right)=\left\langle\nabla_{\frac{\partial}{\partial r}}\left(\frac{\partial\sigma}{\partial r}\right),\frac{\partial\sigma}{\partial s}\right\rangle+\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial r}}\left(\frac{\partial\sigma}{\partial s}\right)\right\rangle.$$

由于 $\sigma_s$ 是测地线，

$$\nabla_{\frac{\partial}{\partial r}}\left(\frac{\partial\sigma}{\partial r}\right)=0,$$

并且根据定理 18.6.4，

$$\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial r}}\left(\frac{\partial\sigma}{\partial s}\right)\right\rangle=\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial s}}\left(\frac{\partial\sigma}{\partial r}\right)\right\rangle.$$

将上述三个恒等式合并得到

$$\frac{\partial}{\partial r}\left(\left\langle\frac{\partial\sigma}{\partial r},\frac{\partial\sigma}{\partial s}\right\rangle\right)=\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial s}}\left(\frac{\partial\sigma}{\partial r}\right)\right\rangle.$$

我们有

$$2\left\langle\frac{\partial\sigma}{\partial r},\nabla_{\frac{\partial}{\partial s}}\left(\frac{\partial\sigma}{\partial r}\right)\right\rangle=\frac{\partial}{\partial s}\left(\left\langle\frac{\partial\sigma}{\partial r},\frac{\partial\sigma}{\partial r}\right\rangle\right)\qquad[\text{Th 19.3.1}]$$

$$=\frac{d}{ds}(\langle\psi,\psi\rangle)\qquad[\text{part (a)}]$$

$$=2\left\langle\frac{d\psi}{ds},\psi\right\rangle.\qquad[\text{Th 19.3.1}]$$

于是前两个恒等式给出

$$\frac{\partial}{\partial r}\left\langle\frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial s}(r,s)\right\rangle=\left\langle\frac{d\psi}{ds}(s),\psi(s)\right\rangle,$$

其中我们注意到右端与 $r$ 无关。由一元实变量微分学，

$$\left\langle\frac{\partial\sigma}{\partial r}(r,s),\frac{\partial\sigma}{\partial s}(r,s)\right\rangle=c+r\left\langle\frac{d\psi}{ds}(s),\psi(s)\right\rangle \tag{19.7.3}$$

对于某个实数 $c$。由定理 18.9.4 和定理 18.11.4(c) 可知

$$\frac{\partial\sigma}{\partial r}(0,s)=d_0(\exp_p)(\psi(s))=\psi(s),$$

并且由定理 18.11.4(d) 可知

$$\frac{\partial\sigma}{\partial s}(0,s)=0.$$

在 (19.7.3) 中令 $r=0$ 并利用前述两个恒等式得到 $c=0$。

设 $(M,\mathfrak g)$ 为半黎曼流形，$p$ 为 $M$ 中的一点。设 $v$ 和 $w$ 为 $\mathcal E_p$ 中的向量，并回顾由 (18.9.4) 给出的微分映射

$$d_v(\exp_p):T_v(T_p(M))\longrightarrow T_{\exp_p(v)}(M).$$

下一个结果表明 $d_v(\exp_p)$ 可以被视为沿径向测地线的“线性等距映射”。

### **定理 19.7.5（高斯引理）。** *在上述设定下，*

$$\langle v,w\rangle=\left\langle d_v(\exp_p)(v),d_v(\exp_p)(w)\right\rangle.$$

*证明。* 设 $\psi(s):(-\varepsilon,\varepsilon)\longrightarrow T_p(M)$ 为光滑曲线，使得 $\psi(0)=v$ 且 $(d\psi/ds)(0)=w$，例如 $\psi(s)=v+sw$。考虑参数化曲面 $\sigma(r,s):[0,1]\times(-\varepsilon,\varepsilon)\longrightarrow M$ 定义为 $\sigma(r,s)=\exp_p(r\psi(s))$，其中我们假设 $r\psi(s)$ 在所有 $(r,s)$ 在 $[0,1]\times(-\varepsilon,\varepsilon)$ 时属于 $\mathcal E_p$。由定理 18.11.4 的 (c) 和 (d) 部分可知

$$\frac{\partial\sigma}{\partial r}(1,0)=d_v(\exp_p)(v)\quad\text{and}\quad\frac{\partial\sigma}{\partial s}(1,0)=d_v(\exp_p)(w),$$

然后由定理 19.7.4(b) 可知

$$\left\langle d_v(\exp_p)(v),d_v(\exp_p)(w)\right\rangle=\left\langle\frac{d\psi}{ds}(0),\psi(0)\right\rangle=\langle w,v\rangle.$$

## 19.8 黎曼曲率张量

设 $(M,\mathfrak{g})$ 为半黎曼流形。在此设定下，曲率张量（场）$R$，最初在具有联络的光滑流形背景下引入，被称为 **黎曼曲率张量（场）**。我们定义映射

```
\mathcal{R}: \mathfrak{X}(M)^4 \longrightarrow C^{\infty}(M),
```

也称为黎曼曲率张量（场）由

```
\mathcal{R}(X, Y, Z, W) = \langle R(X, Y) Z, W \rangle
```

对所有向量场 $X, Y, Z, W$ 在 $\mathfrak{X}(M)$ 中成立。

### **定理 19.8.1（$\mathcal{R}$ 的表示）。** 若 $(M,\mathfrak{g})$ 为半黎曼流形，则 $\mathcal{R}$ 为 $\mathrm{Mult}_{C^{\infty}(M)}(\mathfrak{X}(M)^4, C^{\infty}(M))$ 中的映射并且可以等同于 $\mathcal{T}_4^0(M)$ 中的张量场。

*证明。* 由定理 18.7.1(b) 可知 $\mathcal{R}$ 在其前三个自变量上为 $C^{\infty}(M)$-线性，显然它在其第四个自变量上也为 $C^{\infty}(M)$-线性。这证明了第一个断言。第二个断言由定理 15.7.2 得出。

通过详细研究图 19.5.1、定理 19.5.5(b) 和定理 19.8.1，我们发现 $\mathcal{R}$ 可以与 $R$ 等同。通常认为 $\mathcal{R}$ 是通过“降低一个指标”从 $R$ 得到。这解释了将 $\mathcal{R}$ 和 $R$ 都称为黎曼曲率张量的做法。

### **定理 19.8.2。** 若 $(M,\mathfrak{g})$ 为具有联络的半黎曼 $m$-流形，则在局部坐标下，对 $i,j,k,l = 1,\dots,m$：
(a) $\mathcal{R}$ 关于坐标标架的分量为

```
\mathcal{R}_{ijkl} = \sum_{n} \mathfrak{g}_{ln} R^{n}_{ijk}.
```

(b)

```
R^{l}_{ijk} = \sum_{n} \mathfrak{g}^{ln} \mathcal{R}_{ijkn}.
```

*证明。* (a): 由 (18.7.2) 可知

```
\mathcal{R}_{ijkl} = \mathcal{R}\left(\frac{\partial}{\partial x^{i}}, \frac{\partial}{\partial x^{j}}, \frac{\partial}{\partial x^{k}}, \frac{\partial}{\partial x^{l}}\right) = \left\langle R\left(\frac{\partial}{\partial x^{i}}, \frac{\partial}{\partial x^{j}}\right)\frac{\partial}{\partial x^{k}}, \frac{\partial}{\partial x^{l}}\right\rangle
```

```
= \left\langle \sum_{n} R^{n}_{ijk} \frac{\partial}{\partial x^{n}}, \frac{\partial}{\partial x^{l}}\right\rangle = \sum \mathfrak{g}_{ln} R^{n}_{ijk}.
```

(b): 由 (a) 部分，

```
\sum_{n} \mathfrak{g}^{ln} \mathcal{R}_{ijkn} = \sum_{n} \mathfrak{g}^{ln} \left(\sum_{p} \mathfrak{g}_{np} R^{p}_{ijk}\right) = \sum_{p} \left(\sum_{n} \mathfrak{g}^{ln} \mathfrak{g}_{np}\right) R^{p}_{ijk}
```

```
= \sum_{p} \delta^{l}_{p} R^{p}_{ijk} = R^{l}_{ijk}.
```

### 定理 19.8.3（第一 Bianchi 恒等式）。

设 $(M,g,\nabla)$ 为具有联络的半黎曼流形，设 $X,Y,Z,W$ 为 $\mathfrak{X}(M)$ 中的向量场。则：

-   (a)

$$\sum_{(X,Y,Z)} \mathcal{R}(X,Y,Z,W)=\sum_{(X,Y,Z)}\langle\nabla_X(T(Y,Z)),W\rangle+\sum_{(X,Y,Z)}\langle T(X,[Y,Z]),W\rangle,$$

其中 $T$ 为挠率张量。

-   (b) 若 $\nabla$ 为对称的（尤其是若 $\nabla$ 为 Levi-Civita 联络），则

$$\sum_{(X,Y,Z)}\mathcal{R}(X,Y,Z,W)=0.$$

*证明。* 这由定理 18.7.3 得出。

### 定理 19.8.4（第二 Bianchi 恒等式）。

设 $(M,g,\nabla)$ 为具有联络的半黎曼流形，设 $X,Y,Z,V,W$ 为 $\mathfrak{X}(M)$ 中的向量场。则：

-   (a)

$$\sum_{(X,Y,Z)}\nabla_X(\mathcal{R})(Y,Z,V,W)=-\sum_{(X,Y,Z)}\mathcal{R}(T(X,Y),Z,V,W),\tag{19.8.1}$$

其中 $T$ 为挠率张量。

-   (b) 若 $\nabla$ 为对称的（尤其是若 $\nabla$ 为 Levi-Civita 联络），则

$$\sum_{(X,Y,Z)}\nabla_X(\mathcal{R})(Y,Z,V,W)=0.$$

*证明。* (a): 我们反复使用 (18.7.3)。由定理 18.1.4，

$$\nabla_X(\mathcal{R})(Y,Z,V,W)=X(\mathcal{R}(Y,Z,V,W))-\mathcal{R}(\nabla_X(Y),Z,V,W)$$
$$\qquad-\mathcal{R}(Y,\nabla_X(Z),V,W)-\mathcal{R}(Y,Z,\nabla_X(V),W)$$
$$\qquad-\mathcal{R}(Y,Z,V,\nabla_X(W)).$$

