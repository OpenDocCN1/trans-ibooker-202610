# Theorem 15.12.2 (Pullback of Covariant Tensor Field).

Let $M$ be a smooth $m$-manifold, let $N$ be a smooth $n$-manifold, let $F:M\to N$ be a smooth map, and let $(U,(x^j))$ and $(V,(y^i))$ be charts on $M$ and $N$, respectively, such $U\cap F^{-1}(V)$ is nonempty. Let $\mathcal A$ be a tensor field in $T^0_s(N)$, and, in local coordinates, let

$$\mathcal A=\sum_{1\le i_1,\ldots,i_s\le n}A_{i_1\ldots i_s}\,d(y^{i_1})\otimes\cdots\otimes d(y^{i_s}).$$

Then

$$F^*(\mathcal A)=\sum_{1\le j_1,\ldots,j_s\le m}\left(\sum_{1\le i_1,\ldots,i_s\le n}(A_{i_1\ldots i_s}\circ F)\frac{\partial(y^{i_1}\circ F)}{\partial x^{j_1}}\cdots\frac{\partial(y^{i_s}\circ F)}{\partial x^{j_s}}\right)d(x^{j_1})\otimes\cdots\otimes d(x^{j_s}).$$

*Proof.* We have from Theorem 15.12.1 that

$$F^*(\mathcal A)=F^*\left(\sum_{1\le i_1,\ldots,i_s\le n}A_{i_1\ldots i_s}\,d(y^{i_1})\otimes\cdots\otimes d(y^{i_s})\right)$$
$$=\sum_{1\le i_1,\ldots,i_s\le n}F^*(A_{i_1\ldots i_s})F^*\left(d(y^{i_1})\otimes\cdots\otimes d(y^{i_s})\right),$$

$$F^*(A_{i_1\ldots i_s})=A_{i_1\ldots i_s}\circ F.$$

We also have

$$F^*\left(d(y^{i_1})\otimes\cdots\otimes d(y^{i_s})\right)$$
$$=F^*(d(y^{i_1}))\otimes\cdots\otimes F^*(d(y^{i_s})) \qquad [\mathrm{Th\ 15.12.1(c)}]$$
$$=d(F^*(y^{i_1}))\otimes\cdots\otimes d(F^*(y^{i_s})) \qquad [\mathrm{Th\ 15.11.2,\ (15.12.3)}]$$
$$=d(y^{i_1}\circ F)\otimes\cdots\otimes d(y^{i_s}\circ F). \qquad [(15.9.2),\ (15.12.3)]$$

and from Theorem 15.4.7 that

$$d(y^{i_k}\circ F)=\sum_{j_k}\frac{\partial(y^{i_k}\circ F)}{\partial x^{j_k}}\,d(x^{j_k})$$

for $k=1,\ldots,s$. Thus,

$$F^*\left(d(y^{i_1})\otimes\cdots\otimes d(y^{i_s})\right)$$
$$=\left(\sum_{j_1}\frac{\partial(y^{i_1}\circ F)}{\partial x^{j_1}}d(x^{j_1})\right)\otimes\cdots\otimes\left(\sum_{j_s}\frac{\partial(y^{i_s}\circ F)}{\partial x^{j_s}}d(x^{j_s})\right)$$
$$=\sum_{1\le j_1,\ldots,j_s\le m}\frac{\partial(y^{i_1}\circ F)}{\partial x^{j_1}}\cdots\frac{\partial(y^{i_s}\circ F)}{\partial x^{j_s}}d(x^{j_1})\otimes\cdots\otimes d(x^{j_s}).$$

Substituting (15.12.5) and (15.12.6) into (15.12.4) and reversing summations gives the result. □