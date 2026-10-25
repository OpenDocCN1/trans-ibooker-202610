

连续。若存在点 $t_1\in(a,b)$ 使得 $(df/dt)(t_1)<0$，则由定理 9.1.19，存在点 $t_0\in(c,d)$ 使得 $(df/dt)(t_0)=0$，这与 $df/dt$ 处处非零的假设矛盾。因此，对所有 $t\in(a,b)$ 有 $(df/dt)(t)>0$；即 $f$ 严格递增。类似地，若 $(df/dt)(t_2)<0$，则 $f$ 严格递减。

$(c)$：这由（a）和（b）部分得出。

## 10.3 欧几里得导数与向量场

设 $U$ 是 $\mathbb{R}^m$ 中的开集，$X:U\to\mathbb{R}^m$ 是一个映射，这里我们注意到 $X$ 的定义域和陪域都是 $\mathbb{R}^m$ 的子集。在当前语境下，我们把 $X$ 称为**（$U$ 上的）向量场**。为了突出向量场的形式并将其与其他类型的映射区分开来，我们记

$X(p)\quad\text{用}\quad X_p$

表示，对所有 $p\in U$。我们说 $X$ 在 $p$ 处**为零**，若 $X_p=(0,\ldots,0)$；在 $p$ 处**非零**，若 $X_p\ne(0,\ldots,0)$；并且**在 $U$ 上处处非零**，若它在每个 $p\in U$ 处都非零。

我们用 $\mathfrak{X}(U)$ 表示 $U$ 上光滑向量场的集合。通过如下定义运算，我们使 $\mathfrak{X}(U)$ 同时成为 $\mathbb{R}$ 上的向量空间和 $C^\infty(U)$ 上的模：对所有向量场 $X,Y\in\mathfrak{X}(U)$、所有函数 $f\in C^\infty(U)$ 以及所有实数 $c$，令

$(X+Y)_p=X_p+Y_p,$

$cX_p=cX_p,$

以及

$(fX)_p=f(p)X_p$

对所有 $p\in U$ 成立。设

$X=(\alpha^1,\ldots,\alpha^m)\quad\text{且}\quad Y=(\beta^1,\ldots,\beta^m),$

其中 $\alpha^i,\beta^j$ 是 $C^\infty(U)$ 中的函数，$i,j=1,\ldots,m$。则

$(X+Y)_p=(\alpha^1(p)+\beta^1(p),\ldots,\alpha^m(p)+\beta^m(p))$

且

$(fX)_p=(f(p)\alpha^1(p),\ldots,f(p)\alpha^m(p)).$

$\alpha^i$ 称为 **$X$ 的分量**。设 $E_i$ 是由下式定义的 $\mathfrak{X}(U)$ 中的（常值）向量场：

$E_i=(0,\ldots,1,\ldots,0),$

其中对于 $i=1,\ldots,m$，第 $i$ 个位置为 1，其余位置为 0。

**关于 $X$ 的欧几里得导数**由两个映射组成，都记为 $D_X$。第一个是

$D_X:C^\infty(U)\longrightarrow C^\infty(U)$

由下式定义

$$D_X(f)(p)=d_p(f)(X_p)$$  (10.3.2)

对所有函数 $f\in C^\infty(U)$ 及所有 $p\in U$ 成立。第二个是

$$D_X:\mathfrak{X}(U)\longrightarrow\mathfrak{X}(U)$$

由下式定义

$$D_X(Y)_p=d_p(Y)(X_p)$$  (10.3.3)

对所有向量场 $Y\in\mathfrak{X}(U)$ 及所有 $p\in U$ 成立。由定理 10.1.14（a）可知，（10.3.2）和（10.3.3）可分别表示为

$$D_X(f)(p)=\frac{d(f\circ\lambda)}{dt}(t_0)$$

和

$$D_X(Y)_p=\frac{d(Y\circ\lambda)}{dt}(t_0),$$

其中 $\lambda(t):(a,b)\longrightarrow U$ 是任意光滑曲线，满足 $\lambda(t_0)=p$ 且 $(d\lambda/dt)(t_0)=X_p$（对某个 $t_0\in(a,b)$）。这样的光滑曲线的存在性由定理 10.1.13 保证。

显然，$f$ 和 $Y$ 关于 $X$ 的欧几里得导数在 $p$ 处的取值，与 $f$ 和 $Y$ 在 $p$ 处的微分作用于 $X_p$ 所得结果具有相同的数学内容。它们之间的差异，如果有的话，不过是记号的改变，用以强调 $X$ 在欧几里得导数中的作用。

对于 $\mathfrak{X}(U)$ 中的向量场 $X,Y$，定义函数

$$\langle X,Y\rangle:U\longrightarrow\mathbb{R}$$

且 $\langle X,Y\rangle\in C^\infty(U)$，通过赋值

$$p\longmapsto\langle X_p,Y_p\rangle$$

对所有 $p\in U$ 成立。

关于 $X$ 的欧几里得导数满足若干基本代数性质，这些性质的不同版本稍后会在多种场景中再次出现。

### 定理 10.3.1.

设 $U$ 是 $\mathbb{R}^m$ 中的开集，$X,Y,Z\in\mathfrak{X}(U)$ 是向量场，$f\in C^\infty(U)$ 是函数。则：

- (a) $D_{X+Y}(Z)=D_X(Z)+D_Y(Z)$.
- (b) $D_{fX}(Y)=fD_X(Y)$.
- (c) $D_X(Y+Z)=D_X(Y)+D_X(Z)$.
- (d) $D_X(fY)=D_X(f)Y+fD_X(Y)$.
- (e) $D_X(\langle Y,Z\rangle)=\langle D_X(Y),Z\rangle+\langle Y,D_X(Z)\rangle$.

*证明.* 利用定理 10.1.3 和定理 10.1.8(b) 即得结论。

### 定理 10.3.2.

设 $U$ 是 $\mathbb{R}^m$ 中的开集，$f\in C^\infty(U)$ 是函数，$X,Y\in\mathfrak{X}(U)$ 是向量场，且
\[
X=(\alpha^1,\ldots,\alpha^m) \qquad\text{且}\qquad Y=(\beta^1,\ldots,\beta^m).
\]
则：

- (a)

$$D_X(f)=\sum_i \alpha^i\frac{\partial f}{\partial x^i}.$$

- (b)

$$D_X(D_Y(f))=\sum_{ij}\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial f}{\partial x^j}+\alpha^i\beta^j\frac{\partial^2f}{\partial x^i\partial x^j}\right).$$

**证明.** (a)：对于 $p\in U$，我们有
\[
D_X(f)(p)=d_p(f)(X_p)
=d_p(f)(\alpha^1(p),\ldots,\alpha^m(p))
=\sum_i\alpha^i(p)\frac{\partial f}{\partial x^i}(p).
\]
由于 $p$ 是任意的，结论得证。

(b)：由 (a) 部分，
\[
D_X(D_Y(f))=D_X\left(\sum_j\beta^j\frac{\partial f}{\partial x^j}\right)=\sum_i\alpha^i\frac{\partial}{\partial x^i}\left(\sum_j\beta^j\frac{\partial f}{\partial x^j}\right)
\]
\[
=\sum_{ij}\alpha^i\frac{\partial}{\partial x^i}\left(\beta^j\frac{\partial f}{\partial x^j}\right)=\sum_{ij}\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial f}{\partial x^j}+\alpha^i\beta^j\frac{\partial^2f}{\partial x^i\partial x^j}\right).\quad\square
\]

### 定理 10.3.3.

设 $U$ 是 $\mathbb{R}^m$ 中的开集，$X,Y,Z\in\mathfrak{X}(U)$ 是向量场，且
\[
X=(\alpha^1,\ldots,\alpha^m),\qquad Y=(\beta^1,\ldots,\beta^m),\qquad Z=(\gamma^1,\ldots,\gamma^m).
\]
则：

- (a)

$$D_X(Y)=\sum_j\left(\sum_i\alpha^i\frac{\partial\beta^j}{\partial x^i}\right)E_j.$$

- (b)

$$D_X(D_Y(Z))=\sum_k\left[\sum_{ij}\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}+\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}\right)\right]E_k.$$

- (c)

$$D_{D_X(Y)}(Z)=\sum_k\left(\sum_{ij}\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}\right)E_k.$$

证明。我们将反复使用定理 10.3.1 和定理 10.3.2。设 $f\in C^{\infty}(U)$，并注意到

$$D_X(E_i)=0\quad\text{且}\quad D_{E_i}(f)=\frac{\partial f}{\partial x^i}$$

对 $i=1,\dots,m$ 成立。

(a)：我们有

$$D_X(Y)=D_{\sum_i\alpha^i E_i}\left(\sum_j\beta^j E_j\right)=\sum_{ij}\alpha^i D_{E_i}(\beta^j E_j)$$ $$=\sum_{ij}\alpha^i\left[D_{E_i}(\beta^j)E_j+\beta^j D_{E_i}(E_j)\right]$$ $$=\sum_{ij}\alpha^i\frac{\partial\beta^j}{\partial x^i}E_j=\sum_j\left(\sum_i\alpha^i\frac{\partial\beta^j}{\partial x^i}\right)E_j.$$

(b)：由 (a) 部分，

$$D_Y(Z)=\sum_k\left(\sum_j\beta^j\frac{\partial\gamma^k}{\partial x^j}\right)E_k,$$

因此

$$D_X(D_Y(Z))=D_{\sum_i\alpha^i E_i}\left(\sum_{jk}\beta^j\frac{\partial\gamma^k}{\partial x^j}E_k\right)=\sum_{ijk}\alpha^i D_{E_i}\left(\beta^j\frac{\partial\gamma^k}{\partial x^j}E_k\right)$$ $$=\sum_{ijk}\alpha^i\left[D_{E_i}(\beta^j)\frac{\partial\gamma^k}{\partial x^j}E_k+\beta^j D_{E_i}\left(\frac{\partial\gamma^k}{\partial x^j}\right)E_k+\beta^j\frac{\partial\gamma^k}{\partial x^j}D_{E_i}(E_k)\right]$$ $$=\sum_{ijk}\alpha^i\left(\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}E_k+\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}E_k\right)$$ $$=\sum_k\left[\left(\sum_{ij}\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}\right)+\sum_{ij}\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}\right]E_k.$$

(c)：由 (a) 部分，

$$D_{D_X(Y)}(Z)=D_{\sum_{ij}\alpha^i(\partial\beta^j/\partial x^i)E_j}\left(\sum_k\gamma^k E_k\right)$$ $$=\sum_{ijk}\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}\right)D_{E_j}(\gamma^k E_k)$$ $$=\sum_{ijk}\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}\right)\left[D_{E_j}(\gamma^k)E_k+\gamma^k D_{E_i}(E_k)\right]$$ $$=\sum_{ijk}\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}E_k=\sum_k\left(\sum_{ij}\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}\right)E_k.$$

设 $U$ 是 $\mathbb{R}^m$ 中的开集，$X,Y\in\mathfrak{X}(U)$ 是向量场。**关于 $X$ 和 $Y$ 的二阶欧几里得导数**由两个映射组成，都记为 $D^2_{X,Y}$。第一个是

$$D^2_{X,Y}:C^\infty(U)\longrightarrow C^\infty(U)$$

由下式定义

$$D^2_{X,Y}(f)=D_X(D_Y(f))-D_{D_X(Y)}(f)$$

对所有函数 $f\in C^\infty(U)$ 成立。第二个是

$$D^2_{X,Y}(Z):\mathfrak{X}(U)\longrightarrow\mathfrak{X}(U)$$

由下式定义

$$D^2_{X,Y}(Z)=D_X(D_Y(Z))-D_{D_X(Y)}(Z)$$

对所有向量场 $Z\in\mathfrak{X}(U)$ 成立。

**定理 10.3.4.** 设 $U$ 是 $\mathbb{R}^m$ 中的开集，$X,Y,Z\in\mathfrak{X}(U)$ 是向量场，且

$$X=(\alpha^1,\ldots,\alpha^m),\qquad Y=(\beta^1,\ldots,\beta^m),\qquad \text{且}\qquad Z=(\gamma^1,\ldots,\gamma^m).$$

则

- (a) $$D^2_{X,Y}(Z)=\sum_k\left(\sum_{ij}\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}\right)E_k.$$
- (b) $$D^2_{Y,X}(Z)=D^2_{X,Y}(Z).$$

*证明.* (a)：这由定理 10.3.3(b) 和定理 10.3.3(c) 得出。  
(b)：这由定理 10.1.6 和 (a) 部分得出。

## 10.4 李括号

设 $U$ 是 $\mathbb{R}^m$ 中的开集。**李括号**是映射

$$[\cdot,\cdot]:\mathfrak{X}(U)\times\mathfrak{X}(U)\longrightarrow\mathfrak{X}(U)$$

由下式定义

$$[X,Y]=D_X(Y)-D_Y(X)$$

对所有向量场 $X,Y\in\mathfrak{X}(U)$ 成立。我们称 $[X,Y]$ 为 **$X$ 与 $Y$ 的李括号**。

**定理 10.4.1.** 设 $U$ 是 $\mathbb{R}^m$ 中的开集，$X,Y\in\mathfrak{X}(U)$ 是向量场，且

$$X=(\alpha^1,\ldots,\alpha^m)\qquad\text{且}\qquad Y=(\beta^1,\ldots,\beta^m).$$

则

$$[X,Y]=\sum_j\left\{\sum_i\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}-\beta^i\frac{\partial\alpha^j}{\partial x^i}\right)\right\}E_j.$$

