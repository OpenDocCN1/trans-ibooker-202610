Relabeling indices $(i_{n+1},\ldots,i_{r+1})$ as $(i_n,\ldots,i_r)$, and $(j_{k+1},\ldots,j_{s-1},q,j_l,\ldots,j_{s-1})$ as $(j_k,\ldots,j_{l-2},q,j_{l-1},\ldots,j_{s-2})$, gives the result.

**(b):** We have

$$C_k^n\circ S_k^n(\mathcal A)^{i_1\ldots\widehat{i_k}\ldots i_{r+1}}_{j_1\ldots\widehat{j_k}\ldots j_{s-1}}=\sum_p S_k^n(\mathcal A)^{i_1\ldots i_{n-1}\,p\,i_{n+1}\ldots i_{r+1}}_{j_1\ldots j_{k-1}\,p\,j_{k+1}\ldots j_{s-1}}$$

$$=\sum_p\left(\sum_q g^{pq}\mathcal A^{i_1\ldots\widehat{i_n}\ldots i_{r+1}}_{j_1\ldots j_{k-1}\,q\,j_{k+1}\ldots j_{s-1}}\right)$$

$$=\sum_{pq}g^{pq}\mathcal A^{i_1\ldots\widehat{i_n}\ldots i_{r+1}}_{j_1\ldots j_{k-1}\,q\,j_{k+1}\ldots j_{s-1}}$$

$$=\sum_{pq}g^{pq}\mathcal A^{i_1\ldots\widehat{i_n}\ldots i_{r+1}}_{j_1\ldots j_{k-1}\,p\,q\,j_{k+1}\ldots j_{s-1}}$$

and

$$C_k^n\circ S_{k+1}^n(\mathcal A)^{i_1\ldots\widehat{i_k}\ldots i_{r+1}}_{j_1\ldots\widehat{j_k}\ldots j_{s-1}}=\sum_p S_{k+1}^n(\mathcal A)^{i_1\ldots i_{n-1}\,p\,i_{n+1}\ldots i_{r+1}}_{j_1\ldots j_{k-1}\,p\,j_{k+1}\ldots j_{s-1}}$$

$$=\sum_p\left(\sum_q g^{pq}\mathcal A^{i_1\ldots i_{n-1}\,i_{n+1}}_{j_1\ldots j_{k-1}\,p\,q\,j_{k+1}\ldots j_{s-1}}\right)$$

$$=\sum_{pq}g^{pq}\mathcal A^{i_1\ldots i_{n-1}\,i_{n+1}}_{j_1\ldots j_{k-1}\,p\,q\,j_{k+1}\ldots j_{s-1}},$$

which gives the first equality. Relabeling indices $(i_{n+1},\ldots,i_{r+1})$ as $(i_n,\ldots,i_r)$, and $(j_{k+1},\ldots,j_{s-1})$ as $(j_k,\ldots,j_{s-2})$, gives the second equality. ■

We observe that $p$ and $q$ in (6.3.2) are in the $k$th and $l$th covariant positions of $\mathcal A$, respectively, and that $p$ and $q$ in (6.3.3) are in the $k$th and $(k+1)$th covariant positions of $\mathcal A$, respectively. We also note that the right hand side of (6.3.2) is independent of $n$. See the corresponding remarks following Theorem 6.2.5.

**Example 6.3.6.** For a tensor $\mathcal A$ in $\mathcal T^2_2(V)$, we have from Example 6.2.3 that

$$F_1^2(\mathcal A)^{ij}_{pkl}=\sum_q g_{pq}\mathcal A^{iq}_{kl}.$$

Substituting $\mathcal B=F_1^2(\mathcal A)$, as given by the above expression, into (6.3.1) yields

$$S_1^2\circ F_1^2(\mathcal A)^{ij}_{kl}=\sum_p g^{jp}F_1^2(\mathcal A)^i_{pkl}=\sum_p g^{jp}\left(\sum_q g_{pq}\mathcal A^{iq}_{kl}\right)$$
$$=\sum_q\left(\sum_p g^{jp}g_{pq}\right)\mathcal A^{iq}_{kl}=\sum_q\delta^j_q\mathcal A^{iq}_{kl}=\mathcal A^{ij}_{kl}.$$

Thus, $S_1^2\circ F_1^2(\mathcal A)=\mathcal A$. Likewise, $F_1^2\circ S_1^2(\mathcal A)=\mathcal A$. Since $\mathcal A$ was arbitrary, it follows from Theorem A.4 that $F_1^2$ and $S_1^2$ are inverses of each other, and are therefore linear isomorphisms. ◇