
然后
\[
\mathcal{L}_X(d(x^i)) = d(\alpha^i)
\]
对 \(i=1,\ldots,m\)。

**证明。** 由 (16.4.1) 可知
\[
\mathcal{L}_X(x^i)=X(x^i)=\left(\sum_j\alpha^j\frac{\partial}{\partial x^j}\right)(x^i)=\sum_j\alpha^j\frac{\partial x^i}{\partial x^j}=\alpha^i,
\]
然后由定理 16.4.6 可知
\[
\mathcal{L}_X(d(x^i))=d(\mathcal{L}_X(x^i))=d(\alpha^i).
\]

### 定理 16.4.8（李导数的局部坐标表达式）。

设 \(M\) 为光滑 \(m\)-流形，设 \(X\) 为 \(\mathfrak{X}(M)\) 中的向量场，设 \(\mathcal{A}\) 为 \(T^r_s(M)\) 中的张量场。在局部坐标下，设
\[
X=\sum_{i=1}^{m}\alpha^i\frac{\partial}{\partial x^i},
\]
并且设 \(\mathcal{A}^{i_1\ldots i_r}_{j_1\ldots j_s}\) 为 \(\mathcal{A}\) 的分量。则 \(\mathcal{L}_X(\mathcal{A})\) 的分量为
\[
\mathcal{L}_X(\mathcal{A})^{i_1\ldots i_r}_{j_1\ldots j_s}
= \sum_{k=1}^{m}\alpha^k\frac{\partial\mathcal{A}^{i_1\ldots i_r}_{j_1\ldots j_s}}{\partial x^k}
-\sum_{l=1}^{r}\left(\sum_{k=1}^{m}\mathcal{A}^{i_1\ldots i_{l-1}\,k\,i_{l+1}\ldots i_r}_{j_1\ldots j_s}\frac{\partial\alpha^{i_l}}{\partial x^k}\right)
+\sum_{l=1}^{s}\left(\sum_{k=1}^{m}\mathcal{A}^{i_1\ldots i_r}_{j_1\ldots j_{l-1}\,k\,j_{l+1}\ldots j_s}\frac{\partial\alpha^k}{\partial x^{j_l}}\right).
\]

**证明。** 由定理 16.4.4，
\[
\mathcal{L}_X(\mathcal{A})^{i_1\ldots i_r}_{j_1\ldots j_s}
= \mathcal{L}_X(\mathcal{A})\left(d(x^{i_1}),\ldots,d(x^{i_r}),\frac{\partial}{\partial x^{j_1}},\ldots,\frac{\partial}{\partial x^{j_s}}\right)
\]
\[
= X\left(\mathcal{A}\left(d(x^{i_1}),\ldots,d(x^{i_r}),\frac{\partial}{\partial x^{j_1}},\ldots,\frac{\partial}{\partial x^{j_s}}\right)\right)
-\sum_{l=1}^{r}\mathcal{A}\left(d(x^{i_1}),\ldots,\mathcal{L}_X(d(x^{i_l})),\ldots,d(x^{i_r}),\frac{\partial}{\partial x^{j_1}},\ldots,\frac{\partial}{\partial x^{j_s}}\right)
-\sum_{l=1}^{s}\mathcal{A}\left(d(x^{i_1}),\ldots,d(x^{i_r}),\frac{\partial}{\partial x^{j_1}},\ldots,\left[X,\frac{\partial}{\partial x^{j_l}}\right],\ldots,\frac{\partial}{\partial x^{j_s}}\right). \tag{16.4.2}
\]

我们寻求 (16.4.2) 最后三行中各项的另一种表达式。对于第一项，
\[
X\left(\mathcal{A}\left(d(x^{i_1}),\ldots,d(x^{i_r}),\frac{\partial}{\partial x^{j_1}},\ldots,\frac{\partial}{\partial x^{j_s}}\right)\right)
= X\left(\mathcal{A}^{i_1\ldots i_r}_{j_1\ldots j_s}\right)
= \left(\sum_{k=1}^{m}\alpha^k\frac{\partial}{\partial x^k}\right)\left(\mathcal{A}^{i_1\ldots i_r}_{j_1\ldots j_s}\right)
= \sum_{k=1}^{m}\alpha^k\frac{\partial\mathcal{A}^{i_1\ldots i_r}_{j_1\ldots j_s}}{\partial x^k}. \tag{16.4.3}
\]

对于第二项，定理 15.4.7 和定理 16.4.7 给出
\[
\mathcal{L}_X\left(d(x^{i_l})\right)=d(\alpha^{i_l})=\sum_{k=1}^{m}\frac{\partial\alpha^{i_l}}{\partial x^k}\,d(x^k),
\]
故
\[
\sum_{l=1}^{r}\mathcal{A}\left(d(x^{i_1}),\ldots,\mathcal{L}_X(d(x^{i_l})),\ldots,d(x^{i_r}),\frac{\partial}{\partial x^{j_1}},\ldots,\frac{\partial}{\partial x^{j_s}}\right)
= \sum_{l=1}^{r}\mathcal{A}\left(d(x^{i_1}),\ldots,\sum_{k=1}^{m}\frac{\partial\alpha^{i_l}}{\partial x^k}\,d(x^k),\ldots,d(x^{i_r}),\frac{\partial}{\partial x^{j_1}},\ldots,\frac{\partial}{\partial x^{j_s}}\right)
\]
\[
= \sum_{l=1}^{r}\left\{\sum_{k=1}^{m}\frac{\partial\alpha^{i_l}}{\partial x^k}\,\mathcal{A}\left(d(x^{i_1}),\ldots,d(x^k),\ldots,d(x^{i_r}),\frac{\partial}{\partial x^{j_1}},\ldots,\frac{\partial}{\partial x^{j_s}}\right)\right\} \tag{16.4.4}
\]
\[
= \sum_{l=1}^{r}\left(\sum_{k=1}^{m}\mathcal{A}^{i_1\ldots i_{l-1}\,k\,i_{l+1}\ldots i_r}_{j_1\ldots j_s}\frac{\partial\alpha^{i_l}}{\partial x^k}\right).
\]

对于第三项，
\[
\left[X,\frac{\partial}{\partial x^{j_l}}\right]
= \sum_{k=1}^{m}\left[\alpha^k\frac{\partial}{\partial x^k},\frac{\partial}{\partial x^{j_l}}\right] \qquad[\text{Th 15.3.2(b)}]
\]
\[
= \sum_{k=1}^{m}\left(\alpha^k\left[\frac{\partial}{\partial x^k},\frac{\partial}{\partial x^{j_l}}\right]-\frac{\partial\alpha^k}{\partial x^{j_l}}\frac{\partial}{\partial x^k}\right) \qquad[\text{Th 15.3.2(d)}]
\]
\[
= -\sum_{k=1}^{m}\frac{\partial\alpha^k}{\partial x^{j_l}}\frac{\partial}{\partial x^k}, \qquad[\text{Th 15.3.3(b)}]
\]
故
\[
\sum_{l=1}^{s}\mathcal{A}\left(d(x^{i_1}),\ldots,d(x^{i_r}),\frac{\partial}{\partial x^{j_1}},\ldots,\left[X,\frac{\partial}{\partial x^{j_l}}\right],\ldots,\frac{\partial}{\partial x^{j_s}}\right)
= \sum_{l=1}^{s}\mathcal{A}\left(d(x^{i_1}),\ldots,d(x^{i_r}),\frac{\partial}{\partial x^{j_1}},\ldots,-\sum_{k=1}^{m}\frac{\partial\alpha^k}{\partial x^{j_l}}\frac{\partial}{\partial x^k},\ldots,\frac{\partial}{\partial x^{j_s}}\right)
\]
\[
= -\sum_{l=1}^{s}\left\{\sum_{k=1}^{m}\frac{\partial\alpha^k}{\partial x^{j_l}}\,\mathcal{A}\left(d(x^{i_1}),\ldots,d(x^{i_r}),\frac{\partial}{\partial x^{j_1}},\ldots,\frac{\partial}{\partial x^k},\ldots,\frac{\partial}{\partial x^{j_s}}\right)\right\}
\]
\[
= -\sum_{l=1}^{s}\left(\sum_{k=1}^{m}\mathcal{A}^{i_1\ldots i_r}_{j_1\ldots j_{l-1}\,k\,j_{l+1}\ldots j_s}\frac{\partial\alpha^k}{\partial x^{j_l}}\right). \tag{16.4.5}
\]

将 (16.4.3)–(16.4.5) 代入 (16.4.2) 即得结果。

### 例 16.4.9。

设 \(M\) 为光滑流形，设 \(X\) 为 \(\mathfrak{X}(M)\) 中的向量场，设 \(\omega\) 为 \(\mathfrak{X}^*(M)\) 中的余向量场，并且在局部坐标下，设
\[
X=\sum_{i=1}^{m}\alpha^i\frac{\partial}{\partial x^i}\quad\text{and}\quad \omega=\sum_{j=1}^{m}\beta_j\,d(x^j).
\]
由定理 16.4.8，余向量场 \(\mathcal{L}_X(\omega)\) 在 \(\mathfrak{X}^*(M)\) 中的分量为
\[
\mathcal{L}_X(\omega)_j=\sum_{k=1}^{m}\alpha^k\frac{\partial\beta_j}{\partial x^k}+\sum_{k=1}^{m}\beta_k\frac{\partial\alpha^k}{\partial x^j}.
\]

## 16.5 内乘

下面的定义是第 7.4 节中出现的流形版本。设 \(M\) 为光滑流形，设 \(X\) 为 \(\mathfrak{X}(M)\) 中的向量场。**由 \(X\) 的内乘** 是线性映射族
\[
i_X:\Lambda^s(M)\longrightarrow\Lambda^{s-1}(M)
\]
对 \(s\ge 2\) 定义为
\[
i_X(\omega)(Y_1,\ldots,Y_{s-1})=\omega(X,Y_1,\ldots,Y_{s-1})
\]
对所有 \(\Lambda^s(M)\) 中的形式 \(\omega\) 和所有 \(\mathfrak{X}(M)\) 中的向量场 \(Y_1,\ldots,Y_{s-1}\) 成立。回顾由 (15.8.1) 和 (15.8.2) 可知 \(\Lambda^1(M)=\mathfrak{X}^*(M)\) 且 \(\Lambda^0(M)=C^\infty(M)\)，我们现在将前述定义推广到 \(s=1\) 如下：
\[
i_X:\Lambda^1(M)\longrightarrow\Lambda^0(M)
\]
由
\[
i_X(\omega)=\omega(X)
\]
对所有 \(\mathfrak{X}^*(M)\) 中的余向量场 \(\omega\) 给出。特别地，对任意函数 \(f\) 在 \(C^\infty(M)\) 中，由定理 15.4.5(b) 和 (16.5.1) 可知
\[
i_X(df)=df(X)=X(f).
\]
对 \(s=0\)，我们平凡地定义 \(i_X=0\)；即
\[
i_X(f)=0
\]
对所有 \(C^\infty(M)\) 中的函数 \(f\) 成立。

### 定理 16.5.1。

设 \(M\) 为光滑流形，设 \(X,Y\) 为 \(\mathfrak{X}(M)\) 中的向量场，设 \(\omega,\xi\) 为 \(\Lambda^s(M)\) 中的形式，设 \(\zeta\) 为 \(\Lambda^{s'}(M)\) 中的形式，设 \(f\) 为 \(C^\infty(M)\) 中的函数。则：

- (a) \(i_{X+Y}(\omega)=i_X(\omega)+i_Y(\omega)\)。
- (b) \(i_{fX}(\omega)=f\,i_X(\omega)\)。
- (c) \(i_X(\omega+\xi)=i_X(\omega)+i_X(\xi)\)。
- (d) \(i_X(f\omega)=f\,i_X(\omega)\)。
- (e) \(i_X(\omega\wedge\zeta)=i_X(\omega)\wedge\zeta+(-1)^s\omega\wedge i_X(\zeta)\)。

**证明。** 这是定理 7.4.1 的流形版本结合定理 7.4.3 的流形版本。

### 例 16.5.2。

对 \(\mathfrak{X}(M)\) 中的向量场 \(X\) 和 \(\Lambda^1(M)=\mathfrak{X}^*(M)\) 中的余向量场 \(\omega,\xi\)，由 (15.8.3)、(16.5.1) 和定理 16.5.1(e) 可知
\[
i_X(\omega\wedge\xi)=i_X(\omega)\wedge\xi-\omega\wedge i_X(\xi)=\omega(X)\wedge\xi-\omega\wedge\xi(X)
=\omega(X)\xi-\xi(X)\omega.
\]

我们已在光滑流形上定义了两个微分算子——外微分和李导数。尽管它们在构造和性质上明显不同，但它们通过一个涉及内乘的 remarkable 恒等式相关联。

### 定理 16.5.3（Cartan 恒等式）。

若 \(M\) 为光滑流形且 \(X\) 为 \(\mathfrak{X}(M)\) 中的向量场，则
\[
\mathcal{L}_X=d\circ i_X+i_X\circ d;
\]
即
\[
\mathcal{L}_X(\omega)=d(i_X(\omega))+i_X(d\omega)
\]
对所有 \(\Lambda^s(M)\) 中的形式 \(\omega\) 成立。

**证明。** 设 \(\omega\) 和 \(\xi\) 分别为 \(\Lambda^s(M)\) 和 \(\Lambda^{s'}(M)\) 中的形式。我们有
\[
(d\circ i_X+i_X\circ d)(\omega\wedge\xi)=d(i_X(\omega\wedge\xi))+i_X(d(\omega\wedge\xi)),
\]
\[
\begin{aligned}
d(i_X(\omega\wedge\xi))
&=d(i_X(\omega)\wedge\xi+(-1)^s\omega\wedge i_X(\xi))\\
&=d(i_X(\omega)\wedge\xi)+(-1)^s d(\omega\wedge i_X(\xi))\\
&=d(i_X(\omega))\wedge\xi+(-1)^{s-1}i_X(\omega)\wedge d\xi\\
&\quad+(-1)^s\left[d(\omega)\wedge i_X(\xi)+(-1)^s\omega\wedge d(i_X(\xi))\right]\\
&=(d\circ i_X)(\omega)\wedge\xi-(-1)^s i_X(\omega)\wedge d\xi\\
&\quad+(-1)^s d(\omega)\wedge i_X(\xi)+\omega\wedge(d\circ i_X)(\xi),
\end{aligned}
\]
并且
\[
\begin{aligned}
i_X(d(\omega\wedge\xi))
&=i_X(d\omega\wedge\xi+(-1)^s\omega\wedge d\xi)\\
&=i_X(d\omega)\wedge\xi+(-1)^s i_X(\omega\wedge d\xi)\\
&=i_X(d\omega)\wedge\xi+(-1)^{s+1}d\omega\wedge i_X(\xi)\\
&\quad+(-1)^s\left[i_X(\omega)\wedge d\xi+(-1)^s\omega\wedge i_X(d\xi)\right]\\
&=(i_X\circ d)(\omega)\wedge\xi-(-1)^s d\omega\wedge i_X(\xi)\\
&\quad+(-1)^s i_X(\omega)\wedge d\xi+\omega\wedge(i_X\circ d)(\xi).
\end{aligned}
\]
结合上述恒等式得
\[
\begin{aligned}
(d\circ i_X+i_X\circ d)(\omega\wedge\xi)
&=(d\circ i_X)(\omega)\wedge\xi+(i_X\circ d)(\omega)\wedge\xi\\
&\quad+\omega\wedge(d\circ i_X)(\xi)+\omega\wedge(i_X\circ d)(\xi)\\
&=(d\circ i_X+i_X\circ d)(\omega)\wedge\xi+\omega\wedge(d\circ i_X+i_X\circ d)(\xi).
\end{aligned}
\]

