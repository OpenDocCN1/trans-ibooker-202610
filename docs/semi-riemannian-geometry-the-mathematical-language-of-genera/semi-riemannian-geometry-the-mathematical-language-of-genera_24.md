## 附录 B
## 抽象代数
### B.1 群

群是一个对 $(G, \circ)$，其中 $G$ 是一个集合且 $\circ: G \times G \to G$ 是一个映射，称为 **乘法**，使得对 $G$ 中所有 $f,g,h$。

- [G1] 乘法是结合的:
  
  $$f \circ (g \circ h) = (f \circ g) \circ h.$$

- [G2] $G$ 中存在一个元素 $\operatorname{id}_G$，称为 **单位元**，使得
  
  $$\operatorname{id}_G \circ g = g \circ \operatorname{id}_G = g.$$

- [G3] $G$ 中存在一个元素 $g^{-1}$，称为 $g$ 的 **逆元**，使得
  
  $$g \circ g^{-1} = g^{-1} \circ g = \operatorname{id}_G.$$

通常 $g \circ h$ 简写为 $gh$。容易证明单位元和元素的逆元是唯一的。标准做法是将 $G$ 称为群，乘法映射被理解。我们说 $G$ 是 **交换的** 如果乘法是可交换的；即对所有 $g,h$ 在 $G$ 中 $gh = hg$。在这种情况下，通常采用加法记号，群记为 $(G,+)$，单位元记为 $0_G$ 或 $0$，$g$ 的逆元记为 $-g$。

$G$ 的子集 $G'$ 被称为 $G$ 的 **子群** 如果：(i) $G'$ 本身是一个群（当乘法在 $G$ 上限制到 $G'$ 时），且 (ii) $G'$ 与 $G$ 具有相同的单位元。我们说一个群是 **有限的** 如果它作为集合是有限的。

> **定理 B.1.1.** 如果 $\{G_\alpha : \alpha \in A\}$ 是 $G$ 的子群的集合，则 $\bigcap_{\alpha\in A} G_\alpha$ 是 $G$ 的子群。

*证明.* 显而易见。

设 $G$ 和 $H$ 为群。映射 $F : G ⟶ H$ 被称为群同态，如果对所有 $g, h$ 在 $G$ 中 $F(gh) = F(g)F(h)$。我们说 $F$ 是群同构，且 $G$ 和 $H$ 同构，如果 $F$ 是双射。

### 定理 B.1.2. 如果 $G$ 和 $H$ 是群且 $F : G ⟶ H$ 是群同构，则 $F^{-1} : H ⟶ G$ 是群同构。

证明。证明类似于定理 1.1.9 的证明。

### 例 B.1.3. 显然，$(\mathbb{R}, +)$ 是一个交换群，$(\mathbb{R}\{0}, \times)$ 也是如此。在第 2.4 节中，我们引入了一般线性群 GL(m)，它由所有可逆实 $m\times m$ 矩阵组成。对 $m \ge 2$，GL(m) 是一个无限非交换群，其中群乘法由通常的矩阵乘法给出。在第 B.2 节中，我们讨论置换群 $Sₘ$。对 $m \ge 3$，$Sₘ$ 是一个有限非交换群。

### B.2 置换群

设 $m$ 为正整数，且 $Sₘ$ 为 $\{1,2,\ldots,m\}$ 上的双射函数集合。在当前上下文中，$Sₘ$ 的每个元素称为置换。我们通过定义群乘法为函数的复合，使 $Sₘ$ 成为一个群，称为 $\{1,2,\ldots,m\}$ 上的对称群。$Sₘ$ 的单位元记为 id。设 $\sigma$ 和 $\rho$ 为 $Sₘ$ 中的置换。为简洁起见，我们用 $\sigma\rho$ 表示 $\sigma \circ \rho$，并用 $\sigma\cdots\sigma$ [k 份] 表示 $\sigmaᵏ$。$Sₘ$ 中的每个置换可如下显示：

```
σ = ( 1   2   …   m−1   m )
      (σ(1) σ(2) … σ(m−1) σ(m))
```

这使得清楚 $\operatorname{card}(Sₘ) = m!$。

设 $a$ 为 $\{1,2,\ldots,m\}$ 中的整数。包含 $a$ 的轮换是置换定义为

被称为 **不相交**。$ \mathcal{S}_m$ 中的每个置换都可以写成不相交轮换的乘积，且唯一（在项的顺序上）。长度为 2 的轮换称为 **对换**。长度为 3 或更大的任何轮换都可以写成对换的乘积的几种方式。例如，

(a₁ a₂ a₃ a₄ … aₖ₋₁ aₖ) = (a₁ a₂)(a₂ a₃)(a₃ a₄)⋯(aₖ₋₁ aₖ)  (B.2.2)

和

(a₁ a₂ a₃ a₄ … aₖ₋₁ aₖ) = (a₁ aₖ)(a₁ aₖ₋₁)⋯(a₁ a₃)(a₁ a₂).  (B.2.3)

轮换的逆很容易得到：

(a₁ a₂ a₃ a₄ … aₖ₋₁ aₖ)⁻¹ = (a₁ aₖ aₖ₋₁ … a₄ a₃ a₂).  (B.2.4)

需要强调的是，由于函数的复合是从右到左进行的，因此置换的乘法，包括对换的乘法，也是从右到左进行的。例如，(1 2)(2 3) 将 1 送到 2，2 送到 3，3 送到 2 再到 1，所以 (1 2)(2 3) = (1 2 3)。

### **例 B.2.1.** 考虑置换

$\sigma = \begin{pmatrix} 1 & 2 & 3 & 4 & 5 \\ 4 & 5 & 1 & 3 & 2 \end{pmatrix}$  (B.2.5)

在 $\mathcal{S}_5$ 中，其轮换分解为 $\sigma = (1\ 4\ 3)(2\ 5)$。尽管此例中没有长度为 1 的轮换，但在轮换分解中通常会抑制此类轮换。由于轮换分解中的轮换是不相交的，它们可交换，所以我们同样可以写 $\sigma = (2\ 5)(1\ 4\ 3)$。$\sigma$ 的逆只需交换 (B.2.5) 的行并重新排序列即可得到

$\sigma^{-1} = \begin{pmatrix} 1 & 2 & 3 & 4 & 5 \\ 3 & 5 & 4 & 1 & 2 \end{pmatrix}$

其轮换分解为 $\sigma^{-1} = (1\ 3\ 4)(2\ 5)$。$\sigma^{-1}$ 的轮换分解可以直接从 $\sigma$ 的轮换分解使用 (B.2.4) 得到：

$\sigma^{-1} = (1\ 4\ 3)^{-1}(2\ 5)^{-1} = (1\ 3\ 4)(2\ 5)$。

借助 (B.2.2) 和 (B.2.3)，我们得到 $\sigma^{-1}$ 的替代轮换分解：

$\sigma^{-1} = (1\ 3)(3\ 4)(2\ 5) = (1\ 4)(1\ 3)(2\ 5)$。

设 $\tau = (2\ 4)$，并考虑置换

$\tau\sigma = \begin{pmatrix} 1 & 2 & 3 & 4 & 5 \\ 2 & 5 & 1 & 3 & 4 \end{pmatrix}$,  (B.2.6)

其轮换分解为 $\tau\sigma = (1\ 2\ 5\ 4\ 3)$。我们注意到 (B.2.6) 只是简单地将 (B.2.5) 的第二行中的条目 2 和 4 互换得到。

这一观察背后的一般原理提示了计算置换逆的另一种方法。例如，对应于 $\sigma$，考虑表示 (B.2.5) 第二行的 5 元组 $(4,5,1,3,2)$。我们寻求一系列对换将 $(4,5,1,3,2)$ 重排为 $(1,2,3,4,5)$。根据前述的说明，它们的乘积（按正确顺序）给出 $\sigma^{-1}$ 的轮换分解。在一个明显的记号下，我们有

```
$(1\ 5)(4,5,1,3,2)=(4,1,5,3,2)$, 然后  
$(3\ 5)(4,1,5,3,2)=(4,1,3,5,2)$, 然后  
$(2\ 5)(4,1,3,5,2)=(4,1,3,2,5)$, 然后  
$(4\ 1)(4,1,3,2,5)=(1,4,3,2,5)$, 然后  
$(3\ 4)(1,4,3,2,5)=(1,3,4,2,5)$, 然后  
$(2\ 4)(1,3,4,2,5)=(1,3,2,4,5)$, 然后  
$(2\ 3)(1,3,2,4,5)=(1,2,3,4,5)$.
```

收集对换在一起得到

$(2\ 3)(2\ 4)(3\ 4)(4\ 1)(2\ 5)(3\ 5)(1\ 5)=(1\ 3\ 4)(2\ 5)=\sigma^{-1}$。

设 $x_1,\ldots,x_m$ 为不定元，并考虑多项式

```
$$\Delta=\prod_{1\le i<j\le m}(x_i-x_j).$$
```

对每个 $\sigma\in S_m$，令

```
$$\sigma(\Delta)=\prod_{1\le i<j\le m}(x_{\sigma(i)}-x_{\sigma(j)}).$$
```

$\sigma$ 的 **符号** 定义为

```
$$\operatorname{sgn}(\sigma)=\frac{\sigma(\Delta)}{\Delta}=\prod_{1\le i<j\le m}\frac{x_{\sigma(i)}-x_{\sigma(j)}}{x_i-x_j}.$$
```

显然，$\operatorname{sgn}(\sigma)$ 等于 $1$ 或 $-1$。

### **定理 B.2.2.** 如果 $\sigma,\rho$ 是 $S_m$ 中的置换，则：

- (a) $\operatorname{sgn}(\sigma\rho)=\operatorname{sgn}(\sigma)\operatorname{sgn}(\rho)$。  
- (b) $\operatorname{sgn}(\sigma)=\operatorname{sgn}(\sigma^{-1})$。  
- (c) $\operatorname{sgn}(\sigma\rho\sigma^{-1})=\operatorname{sgn}(\rho)$。

*证明.* (a): 由于 $\rho(\Delta)=\pm\Delta$，我们有

```
$$\frac{(\sigma\rho)(\Delta)}{\rho(\Delta)}=\frac{\sigma(\rho(\Delta))}{\rho(\Delta)}=\frac{\sigma(\pm\Delta)}{\pm\Delta}=\frac{\pm\sigma(\Delta)}{\pm\Delta}=\frac{\sigma(\Delta)}{\Delta}=\operatorname{sgn}(\sigma),$$
```

故

```
$$\operatorname{sgn}(\sigma\rho)=\frac{(\sigma\rho)(\Delta)}{\Delta}=\frac{(\sigma\rho)(\Delta)}{\rho(\Delta)}\frac{\rho(\Delta)}{\Delta}=\operatorname{sgn}(\sigma)\operatorname{sgn}(\rho).$$
```





证明. 我们使用例B.2.1中说明的算法，基于将\((m-k+1,\ldots,m;\;1,\ldots,m-k)\)置换为\((1,\ldots,k;\;k+1,\ldots,m)\)所需的换位次数来计算\(\operatorname{sgn}(\sigma)\)。要将\(m\)移动到\(m-k\)之后的位置，需要\(m-k\)次换位；也就是说，将\((m-k+1,\ldots,m;\;1,\ldots,m-k)\)置换为\((m-k+1,\ldots,m-1;\;1,\ldots,m-k,m)\)。然后要将\(m-1\)移动到\(m-k\)之后的位置，又需要\(m-k\)次换位；即，将\((m-k+1,\ldots,m-1;\;1,\ldots,m-k,m)\)置换为\((m-k+1,\ldots,m-2;\;1,\ldots,m-k,m-1,m)\)。这个过程总共持续\(k\)步。因此，\(k(m-k)\)次换位得到所需的置换。

回顾附录A中属于\(\mathcal I_{k,m}\)的多重指标\(I=(i_1,\ldots,i_k)\)及其属于\(\mathcal I_{m-k,m}\)的补指标\(I^c=(i_{k+1},\ldots,i_m)\)。令

\[
(I,I^c)=(i_1,\ldots,i_k,i_{k+1},\ldots,i_m)
\qquad\text{和}\qquad
(I^c,I)=(i_{k+1},\ldots,i_m,i_1,\ldots,i_k).
\]

对应于\((I,I^c)\)和\((I^c,I)\)，我们定义\(S_m\)中的置换如下：

\[
\sigma_{(I,I^c)}=
\begin{pmatrix}
1 & \cdots & k & k+1 & \cdots & m\\
i_1 & \cdots & i_k & i_{k+1} & \cdots & i_m
\end{pmatrix}
\]

以及

\[
\sigma_{(I^c,I)}=
\begin{pmatrix}
1 & \cdots & m-k & m-k+1 & \cdots & m\\
i_{k+1} & \cdots & i_m & i_1 & \cdots & i_k
\end{pmatrix}.
\]

**定理B.2.5.** 在上述设定下，

\[
\operatorname{sgn}(\sigma_{(I,I^c)})\operatorname{sgn}(\sigma_{(I^c,I)})=(-1)^{k(m-k)}.
\]

**证明.** 我们有

\[
(\sigma_{(I^c,I)})^{-1}=
\begin{pmatrix}
i_1 & \cdots & i_k & i_{k+1} & \cdots & i_m\\
m-k+1 & \cdots & m & 1 & \cdots & m-k
\end{pmatrix},
\]

因此

\[
(\sigma_{(I^c,I)})^{-1}\sigma_{(I,I^c)}=
\begin{pmatrix}
1 & \cdots & k & k+1 & \cdots & m\\
m-k+1 & \cdots & m & 1 & \cdots & m-k
\end{pmatrix}.
\]

结果由定理B.2.2和定理B.2.4得出。

**例B.2.6.** 设\(m=6\)，\(k=3\)，令\(I=(2,4,6)\)。则\(I^c=(1,3,5)\)，并且

\[
(I,I^c)=(2,4,6,1,3,5)
\qquad\text{和}\qquad
(I^c,I)=(1,3,5,2,4,6),
\]

因此

\[
\sigma_{(I,I^c)}=
\begin{pmatrix}
1 & 2 & 3 & 4 & 5 & 6\\
2 & 4 & 6 & 1 & 3 & 5
\end{pmatrix}
\qquad\text{和}\qquad
\sigma_{(I^c,I)}=
\begin{pmatrix}
1 & 2 & 3 & 4 & 5 & 6\\
1 & 3 & 5 & 2 & 4 & 6
\end{pmatrix}.
\]

用循环记号表示，

\[
\sigma_{(I,I^c)}=(1\,2\,4)(3\,6\,5)=(1\,2)(2\,4)(3\,6)(6\,5).
\]

## B.3 环

一个**环**是一个三元组\((R,+,\circ)\)，其中\(R\)是一个集合，\(+:R\times R\longrightarrow R\)和\(\circ:R\times R\longrightarrow R\)是映射，分别称为**加法**和**乘法**，满足对所有\(R\)中的\(a,b,c\)：

- [R1] \((R,+)\)是交换群。

- [R2] 乘法是**结合的**：
  \[
  a\circ(b\circ c)=(a\circ b)\circ c.
  \]

- [R3] 加法和乘法满足**分配律**：
  \[
  \begin{aligned}
  a\circ(b+c)&=(a\circ b)+(a\circ c),\\
  (a+b)\circ c&=(a\circ c)+(b\circ c).
  \end{aligned}
  \]

通常将\(a\circ b\)简写为\(ab\)。如果乘法是交换的，即对\(R\)中所有\(a,b\)有\(ab=ba\)，则称\(R\)是**交换的**。此外，如果\(R\)中存在一个元素\(\mathrm{id}_R\)，称为（乘法）**单位元**，使得对\(R\)中所有\(a\)有

\[
\mathrm{id}_R\,a=a\,\mathrm{id}_R=a
\]

则称\(R\)为**含单位元的环**。例如，整数集\(\{\ldots,-2,-1,0,1,2,\ldots\}\)是含单位元的交换环。

## B.4 域

一个**域**是含单位元的交换环\((F,+,\circ)\)，且\((F\setminus\{0\},\circ)\)是一个群。即，对于\(F\)中每个非零元素\(a\)，在\(F\)中存在一个元素\(a^{-1}\)，称为\(a\)的（乘法）**逆元**，使得

\[
a a^{-1}=a^{-1}a=\mathrm{id}_F.
\]

例如，实数集是一个域。

## B.5 多重线性映射

设\(V_1,\ldots,V_m\)为\(R\)-模。我们通过如下定义加法和数乘，使\(V_1\times\cdots\times V_m\)成为\(R\)-模：对所有\(V_1\times\cdots\times V_m\)中的\((v_1,\ldots,v_m)\)、\((w_1,\ldots,w_m)\)以及所有\(c\in R\)，令

\[
(v_1,\ldots,v_m)+(w_1,\ldots,w_m)=(v_1+w_1,\ldots,v_m+w_m)
\]

且

\[
c(v_1,\ldots,v_m)=(cv_1,\ldots,cv_m).
\]

如果\(V\)和\(W\)是\(R\)-模，且\(V_1=\cdots=V_r=V\)、\(V_{r+1}=\cdots=V_s=W\)，其中\(r+s=m\)，我们记

\[
V\times\cdots\times V\times W\times\cdots\times W
\qquad\text{为}\qquad
V^r\times W^s.
\]

对于\(R\)-模\(V_1,\ldots,V_m,W\)，如果映射

\[
\mathcal A:V_1\times\cdots\times V_m\longrightarrow W
\]

在每一个自变量上都是\(R\)-线性的，则称它是**\(R\)-多重线性的**。即，对所有\(V_i\)中的\(v_i,w_i\)和所有\(c\in R\)，

\[
\mathcal A(v_1,\ldots,cv_i+w_i,\ldots,v_m)=
c\mathcal A(v_1,\ldots,v_i,\ldots,v_m)+\mathcal A(v_1,\ldots,w_i,\ldots,v_m)
\]

其中\(i=1,\ldots,m\)。从\(V_1\times\cdots\times V_m\)到\(W\)的\(R\)-多重线性映射的集合记为

\[
\operatorname{Mult}_R(V_1\times\cdots\times V_m,W).
\]

我们通过如下定义加法和数乘，使\(\operatorname{Mult}_R(V_1\times\cdots\times V_m,W)\)成为\(R\)-模：对所有\(\operatorname{Mult}_R(V_1\times\cdots\times V_m,W)\)中的映射\(\mathcal A,\mathcal B\)和所有\(c\in R\)，令

\[
(\mathcal A+\mathcal B)(v_1,\ldots,v_m)=
\mathcal A(v_1,\ldots,v_m)+\mathcal B(v_1,\ldots,v_m)
\]

且

\[
(c\mathcal A)(v_1,\ldots,v_m)=c\mathcal A(v_1,\ldots,v_m)
\]

对所有\((v_1,\ldots,v_m)\in V_1\times\cdots\times V_m\)成立。\(\operatorname{Mult}_R(V_1\times\cdots\times V_m,W)\)的**零元素**记为\(0\)，即零映射，也就是将\(V_1\times\cdots\times V_m\)的所有元素映至\(W\)的零元素的映射。当\(m=1\)时，\(R\)-多重线性映射就是\(R\)-线性映射。在这种情况下，我们使用记号\(\operatorname{Lin}_R(V,W)\)。由于\(R\)是自身的\(R\)-模，上述考虑也涵盖了\(R\)-模\(\operatorname{Mult}_R(V_1\times\cdots\times V_m,R)\)、\(\operatorname{Mult}_R(V^m,R)\)和\(\operatorname{Lin}_R(V,R)\)。

## B.6 向量空间

域\(F\)（视为含单位元的交换环）上的模\(V\)称为**\(F\)上的向量空间**（或**\(F\)-向量空间**）。\(V\)的\(F\)-子模称为\(V\)的**子空间**，\(F\)-模同态（同构）称为**\(F\)-线性映射（\(F\)-线性同构）**。

设\(W\)是\(F\)上的向量空间。在B.5节的记号下，我们有\(F\)-向量空间\(\mathrm{Mult}_F(V_1\times\cdots\times V_m,W)\)、\(\mathrm{Mult}_F(V^m,W)\)和\(\mathrm{Lin}_F(V,W)\)。为简洁起见，当\(F=\mathbb R\)时，我们在记号中省略下标\(\mathbb R\)，记作\(\mathrm{Mult}(V_1\times\cdots\times V_m,W)\)、\(\mathrm{Mult}(V^m,W)\)和\(\mathrm{Lin}(V,W)\)。特别地，当\(F=W=\mathbb R\)时，我们得到\(\mathrm{Mult}(V_1\times\cdots\times V_m,\mathbb R)\)、\(\mathrm{Mult}(V^m,\mathbb R)\)和\(\mathrm{Lin}(V,\mathbb R)\)。

## B.7 李代数

设\(R\)是含单位元的交换环。一个**\(R\)上的李代数**是一个四元组\((V,+,\circ,[\cdot,\cdot])\)，其中\((V,+,\circ)\)是一个\(R\)-模，\([\cdot,\cdot]:V\times V\longrightarrow V\)是一个映射，称为**李括号**，满足对所有\(V\)中的\(u,v,w\)和所有\(c\in R\)：

- [L1] 李括号是**\(R\)-双线性的**：
  \[
  [cu+v,w]=c[u,w]+[v,w],
  \qquad
  [u,cv+w]=c[u,v]+[u,w].
  \]

- [L2] 李括号是**交错的**：
  \[
  [v,w]=-[w,v].
  \]

- [L3] 李括号满足**雅可比恒等式**：
  \[
  [u,[v,w]]+[v,[w,u]]+[w,[u,v]]=0.
  \]

例如，设\(R\)是含单位元的（不一定交换的）环，定义映射\([\cdot,\cdot]:R\times R\longrightarrow R\)为对\(R\)中所有\(a,b\)有\([a,b]=ab-ba\)。则\((R,+,\circ,[\cdot,\cdot])\)是\(R\)上的李代数。
