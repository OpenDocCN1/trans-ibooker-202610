# ### Theorem 6.3.7.

If $(V,\mathfrak g)$ is a scalar product space, and $1\leq k\leq r$ and $1\leq l\leq s$ are integers, then $F_l^k$ and $S_l^k$ are linear isomorphisms that are inverses.

**Proof.** The proof is an elaboration of Example 6.3.6. Let $\mathcal H$ be a basis for $V$, let $\mathcal A$ and $\mathcal B$ be tensors in $\mathcal T_s^r(V)$ and $\mathcal T_{s+1}^{r-1}(V)$, respectively, and let $\mathfrak g_\mathcal H=[g_{ij}]$. By Theorem 6.2.4, $F_l^k(\mathcal A)$ is a tensor in $\mathcal T_{s+1}^{r-1}(V)$ that has the components

$$F_l^k(\mathcal A)^{i_1\ldots i_{k-1}i_k}_{j_1\ldots j_{s+1}}=\sum_q g_{j_1q}\mathcal A^{i_1\ldots i_{k-1}q i_{k+1}\ldots i_r}_{j_2\ldots j_{s+1}}$$

with respect to $\mathcal H$. Relabeling indices $(i_k,\ldots,i_{r-1})$ as $(i_{k+1},\ldots,i_r)$, and $(j_1,\ldots,j_{s+1})$ as $(p,j_1,\ldots,j_s)$, yields

$$F_l^k(\mathcal A)^{i_1\ldots i_k\ldots i_r}_{j_1\ldots j_{l-1}p j_{l+1}\ldots j_s}=\sum_q g_{pq}\mathcal A^{i_1\ldots i_{k-1}q i_{k+1}\ldots i_r}_{j_1\ldots j_s}. \tag{6.3.5}$$

By Theorem 6.3.4, the tensor $S_l^k(\mathcal B)$ in $\mathcal T_s^r(V)$ has the components

$$S_l^k(\mathcal B)^{i_1\ldots i_r}_{j_1\ldots j_s}=\sum_p g^{i_kp}\mathcal B^{i_1\ldots i_{k-1} \widehat{i_k} i_{k+1}\ldots i_r}_{j_1\ldots j_{l-1}p j_{l+1}\ldots j_s}. \tag{6.3.6}$$

with respect to $\mathcal H$. Substituting $\mathcal B=F_l^k(\mathcal A)$, as given by (6.3.5), into (6.3.6) gives

$$\begin{aligned}
S_l^k\circ F_l^k(\mathcal A)^{i_1\ldots i_r}_{j_1\ldots j_s}
&=\sum_p g^{i_kp}F_l^k(\mathcal A)^{i_1\ldots \widehat{i_k}\ldots i_r}_{j_1\ldots j_{l-1}p j_{l+1}\ldots j_s}\\
&=\sum_p g^{i_kp}\left(\sum_q g_{pq}\mathcal A^{i_1\ldots i_{k-1}q i_{k+1}\ldots i_r}_{j_1\ldots j_s}\right)\\
&=\sum_q\left(\sum_p g^{i_kp}g_{pq}\right)\mathcal A^{i_1\ldots i_{k-1}q i_{k+1}\ldots i_r}_{j_1\ldots j_s}\\
&=\sum_q\delta_q^{i_k}\mathcal A^{i_1\ldots i_{k-1}q i_{k+1}\ldots i_r}_{j_1\ldots j_s}\\
&=\mathcal A^{i_1\ldots i_{k-1}i_k i_{k+1}\ldots i_r}_{j_1\ldots j_s}.
\end{aligned}$$

The rest of the proof uses the argument presented in Example 6.3.6.

# ## 6.4 Representation of Tensors

Section 5.3 was devoted to the topic of representation of tensors over vector spaces. We now extend that discussion to tensors over scalar product spaces.

Let $(V,\mathfrak g)$ be a scalar product space, let $\mathcal H=(h_1,\ldots,h_m)$ be a basis for $V$, let $(\theta^1,\ldots,\theta^m)$ be its dual basis, and let $\mathfrak g_\mathcal H=[g_{ij}]$. Following Section B.5 and Section B.6, we denote by $\operatorname{Mult}(V^s,V)$ the vector space of $\mathbb R$-multilinear maps from $V^s$ to $V$. Let us define a map

$$\widetilde S_s:\operatorname{Mult}(V^s,V)\longrightarrow\mathcal T^0_{s+1}(V),$$