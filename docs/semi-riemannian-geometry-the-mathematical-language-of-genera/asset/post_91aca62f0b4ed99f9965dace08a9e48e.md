
Proof. By Theorem 10.3.3(a),

$$D_X(Y)=\sum_{j}\left(\sum_{i}\alpha^{i}\frac{\partial\beta^{j}}{\partial x^{i}}\right)E_{j}\qquad\text{and}\qquad D_Y(X)=\sum_{j}\left(\sum_{i}\beta^{i}\frac{\partial\alpha^{j}}{\partial x^{i}}\right)E_{j},$$

from which the result follows.

## Theorem 10.4.2. Let $U$ be an open set in $\mathbb{R}^m$, let $X,Y,Z$ be vector fields in $\mathfrak{X}(U)$, and let $f,g$ be functions in $C^{\infty}(U)$. Then:

- (a) $[Y,X]=-[X,Y]$.
- (b) $[X+Y,Z]=[X,Z]+[Y,Z]$.
- (c) $[X,Y+Z]=[X,Y]+[X,Z]$.
- (d) $[fX,gY]=fg[X,Y]+fD_X(g)Y-gD_Y(f)X$.
- (e) $[X,[Y,Z]]+[Y,[Z,X]]+[Z,[X,Y]]=(0,\dots,0).\qquad\text{(Jacobi's identity)}$
- (f) $[E_i,E_j]=(0,\dots,0)$ for $i,j=1,\dots,m$.

Proof. (a): Straightforward.

(b), (c): This follows from parts (a) and (c) of Theorem 10.3.1.

(d): Let

$$X=(\alpha^1,\dots,\alpha^m),\qquad Y=(\beta^1,\dots,\beta^m),\qquad\text{and}\qquad Z=(\gamma^1,\dots,\gamma^m),$$

so that $fX=(f\alpha^1,\dots,f\alpha^m)$ and $gY=(g\beta^1,\dots,g\beta^m)$. By Theorem 10.3.2 and Theorem 10.4.1,

$$[fX,gY]=\sum_{j}\left\{\sum_{i}\left(f\alpha^{i}\frac{\partial(g\beta^{j})}{\partial x^{i}}-g\beta^{i}\frac{\partial(f\alpha^{j})}{\partial x^{i}}\right)\right\}E_{j}$$

for $j,k=1,\dots,m$. We have

$[X,[Y,Z]]=[\sum_i\alpha^iE_i,[\sum_j\beta^jE_j,\sum_k\gamma^kE_k]]=\sum_{ijk}[\alpha^iE_i,[\beta^jE_j,\gamma^kE_k]]=\sum_{ijk}[\alpha^iE_i,\beta^j\frac{\partial\gamma^k}{\partial x^j}E_k-\gamma^k\frac{\partial\beta^j}{\partial x^k}E_j]=\sum_{ijk}^{(1)}[\alpha^iE_i,\beta^j\frac{\partial\gamma^k}{\partial x^j}E_k]-\sum_{ijk}^{(2)}[\alpha^iE_i,\gamma^k\frac{\partial\beta^j}{\partial x^k}E_j],\quad(10.4.1)$

where summations are numbered for later reference. We also have

$[\alpha^iE_i,\beta^j\frac{\partial\gamma^k}{\partial x^j}E_k]=\alpha^i\beta^j[E_i,E_k]+\alpha^iD_{E_i}(\beta^j\frac{\partial\gamma^k}{\partial x^j})E_k-\beta^j\frac{\partial\gamma^k}{\partial x^j}D_{E_k}(\alpha^i)E_i=\alpha^i\frac{\partial}{\partial x^i}(\beta^j\frac{\partial\gamma^k}{\partial x^j})E_k-\beta^j\frac{\partial\gamma^k}{\partial x^j}\frac{\partial\alpha^i}{\partial x^k}E_i=\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}E_k+\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}E_k-\beta^j\frac{\partial\gamma^k}{\partial x^j}\frac{\partial\alpha^i}{\partial x^k}E_i,$

hence

$\sum_{ijk}^{(1)}[\alpha^iE_i,\beta^j\frac{\partial\gamma^k}{\partial x^j}E_k]=\sum_{ijk}\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}E_k+\sum_{ijk}\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}E_k-\sum_{ijk}\beta^j\frac{\partial\gamma^k}{\partial x^j}\frac{\partial\alpha^i}{\partial x^k}E_i=\sum_{ijk}\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}E_k+\sum_{ijk}\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}E_k-\sum_{ijk}\beta^i\frac{\partial\gamma^j}{\partial x^i}\frac{\partial\alpha^k}{\partial x^j}E_k=\sum_k\{\sum_{ij}(\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}+\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}-\frac{\partial\alpha^k}{\partial x^j}\beta^i\frac{\partial\gamma^j}{\partial x^i})\}E_k,\quad(10.4.2)$

Replacing $(\alpha,\beta,\gamma)$ with $(\alpha,\gamma,\beta)$ in $\sum^{(1)}$ gives

$\sum_{ijk}^{(2)}[\alpha^iE_i,\gamma^j\frac{\partial\beta^k}{\partial x^j}E_k]=\sum_k\{\sum_{ij}(\alpha^i\gamma^j\frac{\partial^2\beta^k}{\partial x^i\partial x^j}+\alpha^i\frac{\partial\gamma^j}{\partial x^i}\frac{\partial\beta^k}{\partial x^j}-\frac{\partial\alpha^k}{\partial x^j}\gamma^i\frac{\partial\beta^j}{\partial x^i})\}E_k,$

so

$\sum_{ijk}^{(2)}[\alpha^iE_i,\gamma^k\frac{\partial\beta^j}{\partial x^k}E_j]=\sum_k\{\sum_{ij}(\alpha^i\gamma^j\frac{\partial^2\beta^k}{\partial x^i\partial x^j}+\alpha^i\frac{\partial\gamma^j}{\partial x^i}\frac{\partial\beta^k}{\partial x^j}-\frac{\partial\alpha^k}{\partial x^j}\gamma^i\frac{\partial\beta^j}{\partial x^i})\}E_k\quad(10.4.3)=\sum_k\{\sum_{ij}(\alpha^i\frac{\partial^2\beta^k}{\partial x^i\partial x^j}\gamma^j+\alpha^i\frac{\partial\beta^k}{\partial x^j}\frac{\partial\gamma^j}{\partial x^i}-\frac{\partial\alpha^k}{\partial x^j}\frac{\partial\beta^j}{\partial x^i}\gamma^i)\}E_k.$

It follows from $(10.4.1)$–$(10.4.3)$ that the $k$-th component of $[X,[Y,Z]]$ is $[X,[Y,Z]]^k=\sum_{ij}^{(3)}(\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}+\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}-\frac{\partial\alpha^k}{\partial x^j}\beta^i\frac{\partial\gamma^j}{\partial x^i}-\alpha^i\frac{\partial^2\beta^k}{\partial x^i\partial x^j}\gamma^j-\alpha^i\frac{\partial\beta^k}{\partial x^j}\frac{\partial\gamma^j}{\partial x^i}+\frac{\partial\alpha^k}{\partial x^j}\frac{\partial\beta^j}{\partial x^i}\gamma^i).\quad(10.4.4)$

Replacing $(\alpha,\beta,\gamma)$ with $(\beta,\gamma,\alpha)$ in $\sum^{(3)}$ gives $[Y,[Z,X]]^k=\sum_{ij}(\beta^i\gamma^j\frac{\partial^2\alpha^k}{\partial x^i\partial x^j}+\beta^i\frac{\partial\gamma^j}{\partial x^i}\frac{\partial\alpha^k}{\partial x^j}-\frac{\partial\beta^k}{\partial x^j}\gamma^i\frac{\partial\alpha^j}{\partial x^i}-\beta^i\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}\alpha^j-\beta^i\frac{\partial\gamma^k}{\partial x^j}\frac{\partial\alpha^j}{\partial x^i}+\frac{\partial\beta^k}{\partial x^j}\frac{\partial\gamma^j}{\partial x^i}\alpha^i)=\sum_{ij}(\frac{\partial^2\alpha^k}{\partial x^i\partial x^j}\beta^i\gamma^j+\frac{\partial\alpha^k}{\partial x^j}\beta^i\frac{\partial\gamma^j}{\partial x^i}-\frac{\partial\alpha^j}{\partial x^i}\frac{\partial\beta^k}{\partial x^j}\gamma^i-\alpha^j\beta^i\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}-\frac{\partial\alpha^j}{\partial x^i}\beta^i\frac{\partial\gamma^k}{\partial x^j}+\alpha^i\frac{\partial\beta^k}{\partial x^j}\frac{\partial\gamma^j}{\partial x^i}).\quad(10.4.5)$

Replacing $(\alpha,\beta,\gamma)$ with $(\gamma,\alpha,\beta)$ in $\sum^{(3)}$ yields $[Z,[X,Y]]^k=\sum_{ij}(\gamma^i\alpha^j\frac{\partial^2\beta^k}{\partial x^i\partial x^j}+\gamma^i\frac{\partial\alpha^j}{\partial x^i}\frac{\partial\beta^k}{\partial x^j}-\frac{\partial\gamma^k}{\partial x^j}\alpha^i\frac{\partial\beta^j}{\partial x^i}-\gamma^i\frac{\partial^2\alpha^k}{\partial x^i\partial x^j}\beta^j-\gamma^i\frac{\partial\alpha^k}{\partial x^j}\frac{\partial\beta^j}{\partial x^i}+\frac{\partial\gamma^k}{\partial x^j}\frac{\partial\alpha^j}{\partial x^i}\beta^i)=\sum_{ij}(\alpha^j\frac{\partial^2\beta^k}{\partial x^i\partial x^j}\gamma^i+\frac{\partial\alpha^j}{\partial x^i}\frac{\partial\beta^k}{\partial x^j}\gamma^i-\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}-\frac{\partial^2\alpha^k}{\partial x^i\partial x^j}\beta^j\gamma^i-\frac{\partial\alpha^k}{\partial x^j}\frac{\partial\beta^j}{\partial x^i}\gamma^i+\frac{\partial\alpha^j}{\partial x^i}\beta^i\frac{\partial\gamma^k}{\partial x^j}).\quad(10.4.6)$

Combining $(10.4.4)$–$(10.4.6)$ and using of Theorem 10.1.6 gives the result.

## Theorem 10.4.3. If $U$ is an open set in $\mathbb{R}^m$ and $X,Y,Z$ are vector fields in $\mathfrak{X}(U)$, then:

$D_{D_X(Y)}(Z)-D_{D_Y(X)}(Z)=D_{[X,Y]}(Z)=D_X(D_Y(Z))-D_Y(D_X(Z)).$

Proof. Let

$X=(\alpha^1,\dots,\alpha^m),\quad Y=(\beta^1,\dots,\beta^m),\quad\text{and}\quad Z=(\gamma^1,\dots,\gamma^m).$

(a): By Theorem 10.3.3(c),

$D_{D_X(Y)}(Z)=\sum_k(\sum_{ij}\alpha^i\frac{\partial\beta^j}{\partial x^i})\frac{\partial\gamma^k}{\partial x^j}E_k$

and

$D_{D_Y(X)}(Z)=\sum_k(\sum_{ij}\beta^i\frac{\partial\alpha^j}{\partial x^i})\frac{\partial\gamma^k}{\partial x^j}E_k,$

hence

$D_{D_X(Y)}(Z)-D_{D_Y(X)}(Z)=\sum_{kj}\{\sum_i(\alpha^i\frac{\partial\beta^j}{\partial x^i}-\beta^i\frac{\partial\alpha^j}{\partial x^i})\frac{\partial\gamma^k}{\partial x^j}\}E_k=\sum_k(\sum_j[X,Y]^j\frac{\partial\gamma^k}{\partial x^j})E_k\quad[\text{Th 10.4.1}]=D_{[X,Y]}(Z),\quad[\text{Th 10.3.3(a)}]$

which proves the first equality.

It follows from Theorem 10.3.3(b) that the $k$-th component of $D_X(D_Y(Z))$ is

$(D_X(D_Y(Z)))^k=\sum_{ij}(\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}+\alpha^i\beta^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}).\quad(10.4.7)$

Replacing $(\alpha,\beta,\gamma)$ with $(\beta,\alpha,\gamma)$ in the preceding identity gives

$(D_Y(D_X(Z)))^k=\sum_{ij}(\beta^i\frac{\partial\alpha^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}+\beta^i\alpha^j\frac{\partial^2\gamma^k}{\partial x^i\partial x^j}).\quad(10.4.8)$

The $k$th components of $D_X(D_Y(Z))-D_Y(D_X(Z))$ and $D_{[X,Y]}(Z)$ are equal:

$$
\begin{aligned}
\left(D_X(D_Y(Z))-D_Y(D_X(Z))\right)^k
&=\sum_{ij}\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}-\beta^i\frac{\partial\alpha^j}{\partial x^i}\frac{\partial\gamma^k}{\partial x^j}\right)\\
&=\sum_j\left\{\sum_i\left(\alpha^i\frac{\partial\beta^j}{\partial x^i}-\beta^i\frac{\partial\alpha^j}{\partial x^i}\right)\right\}\frac{\partial\gamma^k}{\partial x^j}\\
&=\sum_j[X,Y]^j\frac{\partial\gamma^k}{\partial x^j}\\
&=\left(D_{[X,Y]}(Z)\right)^k,
\end{aligned}
$$

which proves the second equality.

## 10.5 Integrals

Having discussed derivatives in Sections 10.1–10.4, we now briefly turn our attention to integrals.

Let $[a_i,b_i]$ be a closed interval in $\mathbb{R}$, where $a_i\le b_i$ for $i=1,\ldots,m$. The set

$$C=[a_1,b_1]\times\cdots\times[a_m,b_m]$$

is called a **closed cell** in $\mathbb{R}^m$. The **content of $C$** is defined by

$$\operatorname{cont}(C)=(b_1-a_1)\cdots(b_m-a_m).$$

We observe that $\operatorname{cont}(C)=0$ if and only if $a_i=b_i$ for some $1\le i\le m$. When $m=1,2,3$, “content” is referred to as “length”, “area”, “volume”, respectively. To illustrate, consider the subset $[0,1]$ of $\mathbb{R}$ and the “geometrically equivalent” subset $[0,1]\times[0,1]$ of $\mathbb{R}^2$. Then $[0,1]$ has a length of $1$, while $[0,1]\times[0,1]$ has an area of $1$. A subset $S$ of $\mathbb{R}^m$ is said to have **content zero** if for every real number $\epsilon>0$, there is a finite collection $\{C_i:1=1,\ldots,k\}$ of closed cells such that $S\subseteq\bigcup_{i=1}^k C_i$ and $\sum_{i=1}^k\operatorname{cont}(C_i)<\epsilon$. For example, $[0,1]\times[0,1]$ has content zero.

Let $C$ be a closed cell in $\mathbb{R}^m$, and let $f:C\to\mathbb{R}$ be a bounded function. We say that a finite collection $\{C_i:i=1,\ldots,k\}$ of closed cells in $\mathbb{R}^m$ is a **partition** of $C$ if each $C_i$ is a subset of $C$, the $C_i$ intersect only along their boundaries, and $C=\bigcup_{i=1}^k C_i$. For example, $\{[0,1]\times[0,1],[1,2]\times[0,1]\}$ is a partition of $[0,2]\times[0,1]$. Given a point $p_i$ in $C_i$ for $i=1,\ldots,k$, the sum $\sum_i f(p_i)\operatorname{cont}(C_i)$ is an approximation to what we intuitively think of as the “content under the graph of $f$”. For each choice of partition of $C$ and each choice of the $p_i$, we get a corresponding sum. Using an approach analogous to that adopted in the integral calculus of one real variable, where inscribed and superscribed rectangles are used to approximate the area under the graph of a

real-valued function of one real variable, we define lower and upper integrals of $f$ over $C$, denoted by $\int_C f$ and $\overline{\int_C} f$, respectively, to be limits of the preceding approximating sums. It can be shown that since $f$ is bounded, both $\int_C f$ and $\overline{\int_C} f$ are finite. We say that $f$ is **(Riemann) integrable** if $\int_C f$ and $\overline{\int_C} f$ are equal. In that case, their common value, called the **(Riemann) integral of $f$ over $C$**, is denoted by

$$\int_C f\,dx^1\cdots dx^m \qquad\text{or}\qquad \int_C f(x^1,\ldots,x^m)\,dx^1\cdots dx^m.$$

We need to be able to integrate functions over sets less restrictive than closed cells. Let $S$ be a bounded set in $\mathbb{R}^m$, and let $f:S\to\mathbb{R}$ be a bounded function. Since $S$ is bounded, there is a closed cell $C$ containing $S$. Let us define a function $f_C:C\to\mathbb{R}$ by

$$f_C(p)=\begin{cases}f(p)&\text{if }p\in S,\\0&\text{if }p\in C\setminus S.\end{cases}$$

We say that $f$ is **(Riemann) integrable** if $f_C$ is integrable. In that case, the **(Riemann) integral of $f$ over $S$** is denoted by $\int_S f\,dx^1\cdots dx^m$ and defined by

$$\int_S f\,dx^1\cdots dx^m=\int_C f_C\,dx^1\cdots dx^m. \tag{10.5.2}$$

It can be shown that the integrability of $f$ and the value of $\int_S f\,dx^1\cdots dx^m$ are independent of the choice of closed cell containing $S$.

We now introduce a type of bounded set in $\mathbb{R}^m$ over which a certain type of function is always integrable. A subset $D$ of $\mathbb{R}^m$ is called a **domain of integration** if it is bounded and its boundary in $\mathbb{R}^m$ has content zero. For example, a closed cell in $\mathbb{R}^m$ is a domain of integration.

**Theorem 10.5.1 (Criterion for Integrability).** If $D$ is a domain of integration in $\mathbb{R}^m$ and $f:D\to\mathbb{R}$ is a bounded continuous function, then $f$ is integrable.

**Theorem 10.5.2.** Let $D$ be a domain of integration in $\mathbb{R}^m$, let $f,g:D\to\mathbb{R}$ be bounded continuous functions, and let $c$ be a real number. Then $cf+g$ is integrable and

$$\int_D(cf+g)\,dx^1\cdots dx^m=c\int_D f\,dx^1\cdots dx^m+\int_D g\,dx^1\cdots dx^m.$$

Let $U$ be an open set in $\mathbb{R}^m$, and let $f:U\to\mathbb{R}$ be a continuous function that has compact support; that is, $\operatorname{supp}(f)$ is compact in $U$. By Theorem 9.2.2, $U$ is the union of a collection of open balls in $\mathbb{R}^m$. Using Theorem 9.1.5, it is easily shown that since $\operatorname{supp}(f)$ is compact in $U$, it is contained in the union $D$ of a finite subcollection of the open balls. Thus, $\operatorname{supp}(f)\subseteq D\subseteq U$. Furthermore, it can also be shown that $D$ is a domain of integration. The
