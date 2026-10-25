Let $g(u):[c,d]\to[a,b]$ be a diffeomorphism, and let $c_i=g^{-1}(a_i)$ for $i=1,\ldots,k+1$. According to Theorem 10.2.4(c), $g$ is either strictly increasing or strictly decreasing, so we have either $c=c_1<c_2<\cdots<c_{k+1}=d$ or $c=c_{k+1}<c_k<\cdots<c_1=d$. From (16.8.1), the line integral of $\omega$ over $\lambda\circ g$ is

$$\int_{\lambda\circ g}\omega=\sum_{i=1}^{k}\int_{g^{-1}([a_i,a_{i+1}])}(\lambda\circ g)^*(\omega). \tag{16.8.5}$$

where

$$g^{-1}([a_i,a_{i+1}])=\begin{cases}[c_i,c_{i+1}]&\text{if }g\text{ is strictly increasing},\\[2pt][c_{k+2-i},c_{k+1-i}]&\text{if }g\text{ is strictly decreasing},\end{cases}$$

for $i=1,\ldots,k$. When $\lambda$ is smooth, (16.8.5) simplifies to

$$\int_{\lambda\circ g}\omega=\int_{[c,d]}(\lambda\circ g)^*(\omega). \tag{16.8.6}$$

The next result is the line integral counterpart of Theorem 16.7.1.

# **Theorem 16.8.1 (Diffeomorphism Invariance of Line Integral).**

Let $M$ be a smooth manifold, let $\omega$ be a covector field in $\mathfrak{X}^*(M)$, let $\lambda(t):[a,b]\to M$ be a piecewise smooth curve, and let $g(u):[c,d]\to[a,b]$ be a diffeomorphism. Then

$$\int_{\lambda\circ g}\omega=\pm\int_{\lambda}\omega,$$

where the positive (negative) sign is chosen if $g$ is strictly increasing (strictly decreasing).

*Proof.* We consider only the case where $\lambda$ is smooth. Then

$$\begin{aligned}\int_{\lambda}\omega&=\int_{[a,b]}\lambda^*(\omega) &&[(16.8.2)]\\ &=\pm\int_{[c,d]}g^*(\lambda^*(\omega)) &&[\text{Th 16.7.1}]\\ &=\pm\int_{[c,d]}(\lambda\circ g)^*(\omega) &&[\text{Th 15.9.2, (15.12.3)}]\\ &=\pm\int_{\lambda\circ g}\omega. &&[(16.8.6)]\end{aligned}$$

Theorem 16.8.1 says that, up to a sign determined by the “direction” of the reparametrizing diffeomorphism, the line integral of $\omega$ is independent of the way $\lambda$ is parametrized.

The next result can be viewed as a generalization of the fundamental theorem of calculus.