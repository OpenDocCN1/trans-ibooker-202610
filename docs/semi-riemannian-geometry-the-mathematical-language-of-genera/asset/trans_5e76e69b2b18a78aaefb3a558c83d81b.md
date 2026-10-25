

证明。由定理 10.3.3(a)，

$$D_X(Y)=\sum_{j}\left(\sum_{i}\alpha^{i}\frac{\partial\beta^{j}}{\partial x^{i}}\right)E_{j}\qquad\text{and}\qquad D_Y(X)=\sum_{j}\left(\sum_{i}\beta^{i}\frac{\partial\alpha^{j}}{\partial x^{i}}\right)E_{j},$$

由此可得结果。

## 定理 10.4.2. 设 $U$ 是 $\mathbb{R}^m$ 中的一个开集，设 $X,Y,Z$ 是 $\mathfrak{X}(U)$ 中的向量场，且 $f,g$ 是 $C^{\infty}(U)$ 中的函数。则：

- (a) $[Y,X]=-[X,Y]$。
- (b) $[X+Y,Z]=[X,Z]+[Y,Z]$。
- (c) $[X,Y+Z]=[X,Y]+[X,Z]$。
- (d) $[fX,gY]=fg[X,Y]+fD_X(g)Y-gD_Y(f)X$。
- (e) $[X,[Y,Z]]+[Y,[Z,X]]+[Z,[X,Y]]=(0,\dots,0).\qquad\text{(Jacobi's identity)}$
- (f) $[E_i,E_j]=(0,\dots,0)$ 对于 $i,j=1,\dots,m$。

证明。 (a)：显而易见。

(b), (c)：这由定理 10.3.1 的部分 (a) 和 (c) 可得。

(d)：设

$$X=(\alpha^1,\dots,\alpha^m),\qquad Y=(\beta^1,\dots,\beta^m),\qquad\text{and}\qquad Z=(\gamma^1,\dots,\gamma^m),$$

使得 $fX=(f\alpha^1,\dots,f\alpha^m)$ 且 $gY=(g\beta^1,\dots,g\beta^m)$。由定理 10.3.2 和定理 10.4.1，

$$[fX,gY]=\sum_{j}\left\{\sum_{i}\left(f\alpha^{i}\frac{\partial(g\beta^{j})}{\partial x^{i}}-g\beta^{i}\frac{\partial(f\alpha^{j})}{\partial x^{i}}\right)\right\}E_{j}$$

对于 $j,k=1,\dots,m$。我们有

$[X,[Y,Z]]=[\sum_i\alpha^iE_i,[\sum_j\beta^jE_j,\sum_k\gamma^kE_k]]=\sum_{ijk}[\alpha^iE_i,[\beta^jE_j,\gamma^kE_k]]=\sum_{ijk}[\alpha^iE_i,\beta^j\frac{\partial\gamma^k}{\partial x^j}E_k-\gamma^k\frac{\partial\beta^j}{\partial x^k}E_j]=\sum_{ijk}^{(1)}[\alpha^iE_i,\beta^j\frac{\partial\gamma^k}{\partial x^j}E_k]-\sum_{ijk}^{(2)}[\alpha^iE_i,\gamma^k\frac{\partial\beta^j}{\partial x^k}E_j],\quad(10.4.1)$

其中求和按后续引用编号。我们也有

$[\alpha^iE_i,\beta^j\frac{\partial\gamma^k}{\partial x^j}E_k]=\alpha^i\beta^j[E_i,E_k]+\alpha^iD_{E_i}(\beta^j\frac{\partial\gamma^k}{\partial x^j})E_k-\beta^j\frac{\partial\gamma^k}{\partial x^j}D_{E_k}(\alpha^i)E_i=\alpha^i\frac{\partial}{\partial x^i}(\beta^j\frac{\partial\gamma^k}{\partial x^j})E_k-\beta^j\frac{\partial\gamma^k}{\partial x^j}\frac{\partial\alpha^i}{\partial x^k}E_i=\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}E_k+\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}E_k-\beta^j\frac{\partial\gamma^k}{\partial x^j}\frac{\partial\alpha^i}{\partial x^k}E_i,$

故

$\sum_{ijk}^{(1)}[\alpha^iE_i,\beta^j\frac{\partial\gamma^k}{\partial x^j}E_k]=\sum_{ijk}\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}E_k+\sum_{ijk}\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}E_k-\sum_{ijk}\beta^j\frac{\partial\gamma^k}{\partial x^j}\frac{\partial\alpha^i}{\partial x^k}E_i=\sum_{ijk}\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}E_k+\sum_{ijk}\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}E_k-\sum_{ijk}\beta^i\frac{\partial\gamma^j}{\partial x^i}\frac{\partial\alpha^k}{\partial x^j}E_k=\sum_k\{\sum_{ij}(\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}+\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}-\frac{\partial\alpha^k}{\partial x^j}\beta^i\frac{\partial\gamma^j}{\partial x^i})\}E_k,\quad(10.4.2)$

将 $(\alpha,\beta,\gamma)$ 在 $\sum^{(1)}$ 中替换为 $(\alpha,\gamma,\beta)$ 得

$\sum_{ijk}^{(2)}[\alpha^iE_i,\gamma^j\frac{\partial\beta^k}{\partial x^j}E_k]=\sum_k\{\sum_{ij}(\alpha^i\gamma^j\frac{\partial^2\beta^k}{\partial x^i\partial x^j}+\alpha^i\frac{\partial\gamma^j}{\partial x^i}\frac{\partial\beta^k}{\partial x^j}-\frac{\partial\alpha^k}{\partial x^j}\gamma^i\frac{\partial\beta^j}{\partial x^i})\}E_k,$

所以

$\sum_{ijk}^{(2)}[\alpha^iE_i,\gamma^k\frac{\partial\beta^j}{\partial x^k}E_j]=\sum_k\{\sum_{ij}(\alpha^i\gamma^j\frac{\partial^2\beta^k}{\partial x^i\partial x^j}+\alpha^i\frac{\partial\gamma^j}{\partial x^i}\frac{\partial\beta^k}{\partial x^j}-\frac{\partial\alpha^k}{\partial x^j}\gamma^i\frac{\partial\beta^j}{\partial x^i})\}E_k\quad(10.4.3)=\sum_k\{\sum_{ij}(\alpha^i\frac{\partial^2\beta^k}{\partial x^i\partial x^j}\gamma^j+\alpha^i\frac{\partial\beta^k}{\partial x^j}\frac{\partial\gamma^j}{\partial x^i}-\frac{\partial\alpha^k}{\partial x^j}\frac{\partial\beta^j}{\partial x^i}\gamma^i)\}E_k.$

由 $(10.4.1)$–$(10.4.3)$ 可得 $[X,[Y,Z]]$ 的第 $k$ 个分量为 $[X,[Y,Z]]^k=\sum_{ij}^{(3)}(\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}+\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}-\frac{\partial\alpha^k}{\partial x^j}\beta^i\frac{\partial\gamma^j}{\partial x^i}-\alpha^i\frac{\partial^2\beta^k}{\partial x^i\partial x^j}\gamma^j-\alpha^i\frac{\partial\beta^k}{\partial x^j}\frac{\partial\gamma^j}{\partial x^i}+\frac{\partial\alpha^k}{\partial x^j}\frac{\partial\beta^j}{\partial x^i}\gamma^i).\quad(10.4.4)$

将 $(\alpha,\beta,\gamma)$ 在 $\sum^{(3)}$ 中替换为 $(\beta,\gamma,\alpha)$ 得 $[Y,[Z,X]]^k=\sum_{ij}(\beta^i\gamma^j\frac{\partial^2\alpha^k}{\partial x^i\partial x^j}+\beta^i\frac{\partial\gamma^j}{\partial x^i}\frac{\partial\alpha^k}{\partial x^j}-\frac{\partial\beta^k}{\partial x^j}\gamma^i\frac{\partial\alpha^j}{\partial x^i}-\beta^i\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}\alpha^j-\beta^i\frac{\partial\gamma^k}{\partial x^j}\frac{\partial\alpha^j}{\partial x^i}+\frac{\partial\beta^k}{\partial x^j}\frac{\partial\gamma^j}{\partial x^i}\alpha^i)=\sum_{ij}(\frac{\partial^2\alpha^k}{\partial x^i\partial x^j}\beta^i\gamma^j+\frac{\partial\alpha^k}{\partial x^j}\beta^i\frac{\partial\gamma^j}{\partial x^i}-\frac{\partial\alpha^j}{\partial x^i}\frac{\partial\beta^k}{\partial x^j}\gamma^i-\alpha^j\beta^i\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}-\frac{\partial\alpha^j}{\partial x^i}\beta^i\frac{\partial\gamma^k}{\partial x^j}+\alpha^i\frac{\partial\beta^k}{\partial x^j}\frac{\partial\gamma^j}{\partial x^i}).\quad(10.4.5)$

将 $(\alpha,\beta,\gamma)$ 在 $\sum^{(3)}$ 中替换为 $(\gamma,\alpha,\beta)$ 得 $[Z,[X,Y]]^k=\sum_{ij}(\gamma^i\alpha^j\frac{\partial^2\beta^k}{\partial x^i\partial x^j}+\gamma^i\frac{\partial\alpha^j}{\partial x^i}\frac{\partial\beta^k}{\partial x^j}-\frac{\partial\gamma^k}{\partial x^j}\alpha^i\frac{\partial\beta^j}{\partial x^i}-\gamma^i\frac{\partial^2\alpha^k}{\partial x^i\partial x^j}\beta^j-\gamma^i\frac{\partial\alpha^k}{\partial x^j}\frac{\partial\beta^j}{\partial x^i}+\frac{\partial\gamma^k}{\partial x^j}\frac{\partial\alpha^j}{\partial x^i}\beta^i)=\sum_{ij}(\alpha^j\frac{\partial^2\beta^k}{\partial x^i\partial x^j}\gamma^i+\frac{\partial\alpha^j}{\partial x^i}\frac{\partial\beta^k}{\partial x^j}\gamma^i-\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}-\frac{\partial^2\alpha^k}{\partial x^i\partial x^j}\beta^j\gamma^i-\frac{\partial\alpha^k}{\partial x^j}\frac{\partial\beta^j}{\partial x^i}\gamma^i+\frac{\partial\alpha^j}{\partial x^i}\beta^i\frac{\partial\gamma^k}{\partial x^j}).\quad(10.4.6)$

合并 $(10.4.4)$–$(10.4.6)$ 并利用定理 10.1.6 得结果。

## 定理 10.4.3. 若 $U$ 是 $\mathbb{R}^m$ 中的一个开集且 $X,Y,Z$ 是 $\mathfrak{X}(U)$ 中的向量场，则：

$D_{D_X(Y)}(Z)-D_{D_Y(X)}(Z)=D_{[X,Y]}(Z)=D_X(D_Y(Z))-D_Y(D_X(Z)).$

证明。设

$X=(\alpha^1,\dots,\alpha^m),\quad Y=(\beta^1,\dots,\beta^m),\quad\text{and}\quad Z=(\gamma^1,\dots,\gamma^m).$

(a)：由定理 10.3.3(c)，

$D_{D_X(Y)}(Z)=\sum_k(\sum_{ij}\alpha^i\frac{\partial\beta^j}{\partial x^i})\frac{\partial\gamma^k}{\partial x^j}E_k$

且

$D_{D_Y(X)}(Z)=\sum_k(\sum_{ij}\beta^i\frac{\partial\alpha^j}{\partial x^i})\frac{\partial\gamma^k}{\partial x^j}E_k,$

故

$D_{D_X(Y)}(Z)-D_{D_Y(X)}(Z)=\sum_{kj}\{\sum_i(\alpha^i\frac{\partial\beta^j}{\partial x^i}-\beta^i\frac{\partial\alpha^j}{\partial x^i})\frac{\partial\gamma^k}{\partial x^j}\}E_k=\sum_k(\sum_j[X,Y]^j\frac{\partial\gamma^k}{\partial x^j})E_k\quad[\text{Th 10.4.1}]=D_{[X,Y]}(Z),\quad[\text{Th 10.3.3(a)}]$

这证明了第一个等式。

由定理 10.3.3(b) 可知 $D_X(D_Y(Z))$ 的第 $k$ 个分量为

$(D_X(D_Y(Z)))^k=\sum_{ij}(\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}+\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}).\quad(10.4.7)$

将 $(\alpha,\beta,\gamma)$ 替换为 $(\beta,\alpha,\gamma)$ 在前述恒等式中得

$(D_Y(D_X(Z)))^k=\sum_{ij}(\beta^i\frac{\partial\alpha^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}+\beta^i\alpha^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}).\quad(10.4.8)$

$D_X(D_Y(Z))-D_Y(D_X(Z))$ 与 $D_{[X,Y]}(Z)$ 的第 $k$ 个分量相等：

$$
\begin{aligned}
\left(D_X(D_Y(Z))-D_Y(D_X(Z))\right)^k
&=\sum_{ij}\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}-\beta^i\frac{\partial\alpha^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}\right)\\
&=\sum_j\left\{\sum_i\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}-\beta^i\frac{\partial\alpha^j}{\partial x^i}\right)\right\}\frac{\partial\gamma^k}{\partial x^j}\\
&=\sum_j[X,Y]^j\frac{\partial\gamma^k}{\partial x^j}\\
&=\left(D_{[X,Y]}(Z)\right)^k,
\end{aligned}
$$

这证明了第二个等式。

## 10.5 积分

在第 10.1–10.4 节讨论了导数之后，我们现在简要地将注意力转向积分。

设 $[a_i,b_i]$ 是 $\mathbb{R}$ 中的闭区间，其中 $a_i\le b_i$ 对于 $i=1,\ldots,m$。集合

$$C=[a_1,b_1]\times\cdots\times[a_m,b_m]$$

称为 $\mathbb{R}^m$ 中的一个 **闭胞**。$C$ 的 **含量** 定义为

$$\operatorname{cont}(C)=(b_1-a_1)\cdots(b_m-a_m).$$

我们注意到 $\operatorname{cont}(C)=0$ 当且仅当存在某个 $1\le i\le m$ 使得 $a_i=b_i$。当 $m=1,2,3$ 时，“含量”分别称为“长度”、“面积”、“体积”。例如，考虑 $\mathbb{R}$ 的子集 $[0,1]$ 和 $\mathbb{R}^2$ 的“几何等价”子集 $[0,1]\times[0,1]$。则 $[0,1]$ 的长度为 $1$，而 $[0,1]\times[0,1]$ 的面积为 $1$。$\mathbb{R}^m$ 的子集 $S$ 称为具有 **含量零**，如果对于每一个实数 $\epsilon>0$，存在有限个闭胞的集合 $\{C_i:1=1,\ldots,k\}$ 使得 $S\subseteq\bigcup_{i=1}^k C_i$ 且 $\sum_{i=1}^k\operatorname{cont}(C_i)<\epsilon$。例如，$[0,1]\times[0,1]$ 具有含量零。

设 $C$ 是 $\mathbb{R}^m$ 中的一个闭胞，且 $f:C\to\mathbb{R}$ 是一个有界函数。我们说 $\mathbb{R}^m$ 中闭胞的有限集合 $\{C_i:i=1,\ldots,k\}$ 是 $C$ 的一个 **划分**，如果每个 $C_i$ 是 $C$ 的子集，$C_i$ 仅在它们的边界上相交，且 $C=\bigcup_{i=1}^k C_i$。例如，$\{[0,1]\times[0,1],[1,2]\times[0,1]\}$ 是 $[0,2]\times[0,1]$ 的一个划分。给定 $C_i$ 中的点 $p_i$ 对于 $i=1,\ldots,k$，和 $\sum_i f(p_i)\operatorname{cont}(C_i)$ 是我们直观认为的“$f$ 图形下方含量”的一个近似。对于 $C$ 的每个划分的选择和 $p_i$ 的每个选择，我们得到一个相应的和。采用与一元实变积分演算中类似的方法，其中使用内接和外接矩形来近似一元实值函数图形下的面积，我们定义 $f$ 在 $C$ 上的下积分和上积分，分别记为 $\int_C f$ 和 $\overline{\int_C} f$，为上述近似和的极限。可以证明由于 $f$ 有界，$\int_C f$ 和 $\overline{\int_C} f$ 都是有限的。我们说 $f$ 是 **(Riemann) 可积的** 如果 $\int_C f$ 和 $\overline{\int_C} f$ 相等。在这种情况下，它们的共同值，称为 $f$ 在 $C$ 上的 **(Riemann) 积分**，记为

$$\int_C f\,dx^1\cdots dx^m \qquad\text{or}\qquad \int_C f(x^1,\ldots,x^m)\,dx^1\cdots dx^m.$$

我们需要能够对比闭胞更宽松的集合上的函数进行积分。设 $S$ 是 $\mathbb{R}^m$ 中的有界集，且 $f:S\to\mathbb{R}$ 是有界函数。由于 $S$ 有界，存在包含 $S$ 的闭胞 $C$。设我们定义函数 $f_C:C\to\mathbb{R}$ 为

$$f_C(p)=\begin{cases}f(p)&\text{if }p\in S,\\0&\text{if }p\in C\setminus S.\end{cases}$$

我们说 $f$ 是 **(Riemann) 可积的** 如果 $f_C$ 可积。在这种情况下，$f$ 在 $S$ 上的 **(Riemann) 积分** 记为 $\int_S f\,dx^1\cdots dx^m$ 并定义为

$$\int_S f\,dx^1\cdots dx^m=\int_C f_C\,dx^1\cdots dx^m. \tag{10.5.2}$$

可以证明 $f$ 的可积性以及 $\int_S f\,dx^1\cdots dx^m$ 的值与包含 $S$ 的闭胞的选择无关。

我们现在引入 $\mathbb{R}^m$ 中一类有界集，在其上某类函数总是可积的。$\mathbb{R}^m$ 的子集 $D$ 称为一个 **积分域**，如果它是 有界的 且其在 $\mathbb{R}^m$ 中的边界具有含量零。例如，$\mathbb{R}^m$ 中的闭胞是一个积分域。

**定理 10.5.1 (可积性准则).** 如果 $D$ 是 $\mathbb{R}^m$ 中的一个积分域且 $f:D\to\mathbb{R}$ 是有界连续函数，则 $f$ 是可积的。

**定理 10.5.2.** 设 $D$ 是 $\mathbb{R}^m$ 中的一个积分域，设 $f,g:D\to\mathbb{R}$ 是有界连续函数，且 $c$ 是实数。则 $cf+g$ 是可积的且

$$\int_D(cf+g)\,dx^1\cdots dx^m=c\int_D f\,dx^1\cdots dx^m+\int_D g\,dx^1\cdots dx^m.$$

设 $U$ 是 $\mathbb{R}^m$ 中的一个开集，且 $f:U\to\mathbb{R}$ 是一个具有紧支集的连续函数；即 $\operatorname{supp}(f)$ 在 $U$ 中是紧的。由定理 9.2.2，$U$ 是 $\mathbb{R}^m$ 中一族开球的并。利用定理 9.1.5，可以很容易地证明由于 $\operatorname{supp}(f)$ 在 $U$ 中是紧的，它包含于开球的有限子族的并 $D$ 中。因此，$\operatorname{supp}(f)\subseteq D\subseteq U$。此外，还可以证明 $D$ 是一个积分域。