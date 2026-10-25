continuous. If there is a point $t_1$ in $(a,b)$ such that $(df/dt)(t_1)<0$, then by Theorem 9.1.19, there is a point $t_0$ in $(c,d)$ such that $(df/dt)(t_0)=0$, which contradicts the nowhere-vanishing assumption on $df/dt$. Thus, $(df/dt)(t)>0$ for all $t$ in $(a,b)$; that is, $f$ is strictly increasing. Similarly, if $(df/dt)(t_2)<0$, then $f$ is strictly decreasing.

$(c)$: This follows from parts (a) and (b).

# 10.3 Euclidean Derivative and Vector Fields

Let $U$ be an open set in $\mathbb{R}^m$, and let $X:U\to\mathbb{R}^m$ be a map, where we note that both the domain and codomain of $X$ are subsets of $\mathbb{R}^m$. In the present context, we refer to $X$ as a **vector field (on $U$)**. To highlight the appearance of vector fields and to distinguish them from other types of maps, let us denote

$X(p)\quad\text{by}\quad X_p$

for all $p$ in $U$. We say that $X$ **vanishes at** $p$ if $X_p=(0,\ldots,0)$, is **nonvanishing at** $p$ if $X_p\ne(0,\ldots,0)$, and is **nowhere-vanishing (on $U$)** if it is nonvanishing at every $p$ in $U$.

Let us denote the set of smooth vector fields on $U$ by $\mathfrak{X}(U)$. We make $\mathfrak{X}(U)$ into both a vector space over $\mathbb{R}$ and a module over $C^\infty(U)$ by defining operations as follows: for all vector fields $X,Y$ in $\mathfrak{X}(U)$, all functions $f$ in $C^\infty(U)$, and all real numbers $c$, let

$(X+Y)_p=X_p+Y_p,$

$cX_p=cX_p,$

and

$(fX)_p=f(p)X_p$

for all $p$ in $U$. Let

$X=(\alpha^1,\ldots,\alpha^m)\quad\text{and}\quad Y=(\beta^1,\ldots,\beta^m),$

where $\alpha^i,\beta^j$ are functions in $C^\infty(U)$ for $i,j=1,\ldots,m$. Then

$(X+Y)_p=(\alpha^1(p)+\beta^1(p),\ldots,\alpha^m(p)+\beta^m(p))$

and

$(fX)_p=(f(p)\alpha^1(p),\ldots,f(p)\alpha^m(p)).$

The $\alpha^i$ are called the **components of $X$**. Let $E_i$ be the (constant) vector field in $\mathfrak{X}(U)$ defined by

$E_i=(0,\ldots,1,\ldots,0),$

where 1 is in the $i$th position and 0s are elsewhere for $i=1,\ldots,m$.

The **Euclidean derivative with respect to $X$** consists of two maps, both denoted by $D_X$. The first is

$D_X:C^\infty(U)\longrightarrow C^\infty(U)$