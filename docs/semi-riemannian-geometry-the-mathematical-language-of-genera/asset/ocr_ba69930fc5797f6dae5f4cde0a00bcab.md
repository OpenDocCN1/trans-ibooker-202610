# ### Theorem 7.1.2.

Let $V$ be a vector space and let $\eta$ be a tensor in $T^0_s(V)$. Then the following are equivalent:

+   (a) $\eta$ is a multivector in $\Lambda^s(V)$.
+   
+   (b) $\sigma(\eta)=\operatorname{sgn}(\sigma)\eta$ for all permutations $\sigma$ in $S_s$.
+   
+   (c) If $v_1,\ldots,v_s$ are vectors in $V$ and two (or more) of them are equal, then $\eta(v_1,\ldots,v_s)=0$.
+   
+   (d) If $v_1,\ldots,v_s$ are vectors in $V$ and $\eta(v_1,\ldots,v_s)\ne0$, then $v_1,\ldots,v_s$ are linearly independent.

**Proof.** The proof is similar to that of Theorem 2.4.2.

$(a)\Rightarrow(b)$: Let $\sigma=\tau_1\cdots\tau_k$ be a decomposition of $\sigma$ into transpositions. By Theorem 7.1.1,

$$\sigma(\eta)=(\tau_1\cdots\tau_k)(\eta)=(\tau_1\cdots\tau_{k-1})(\tau_k(\eta))$$
$$\qquad=(\tau_1\cdots\tau_{k-1})(-\eta)=-(\tau_1\cdots\tau_{k-1})(\eta).$$

Repeating the process $k-1$ more times gives $\sigma(\eta)=(-1)^k\eta$. By Theorem B.2.3, $\operatorname{sgn}(\sigma)=(-1)^k$.

$(b)\Rightarrow(a)$: If $\tau$ is a transposition in $S_s$, then, by Theorem B.2.3, $\tau(\eta)=\operatorname{sgn}(\tau)\eta=-\eta$.

$(a)\Rightarrow(c)$: If $v_i=v_j$ for some $1\le i<j\le s$, then

$$\eta(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s)=\eta(v_1,\ldots,v_j,\ldots,v_i,\ldots,v_s).$$

On the other hand, since $\eta$ is alternating,

$$\eta(v_1,\ldots,v_j,\ldots,v_i,\ldots,v_s)=-\eta(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s).$$

Thus,

$$\eta(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s)=-\eta(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s),$$

from which the result follows.

$(c)\Rightarrow(a)$: For all $1\le i<j\le s$, we have

$$0=\eta(v_1,\ldots,v_i+v_j,\ldots,v_i+v_j,\ldots,v_s)$$
$$\quad=\eta(v_1,\ldots,v_i,\ldots,v_i,\ldots,v_s)+\eta(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s)$$
$$\quad+\eta(v_1,\ldots,v_j,\ldots,v_i,\ldots,v_s)+\eta(v_1,\ldots,v_j,\ldots,v_j,\ldots,v_s)$$
$$\quad=\eta(v_1,\ldots,v_i,\ldots,v_j,\ldots,v_s)+\eta(v_1,\ldots,v_j,\ldots,v_i,\ldots,v_s),$$

from which the result follows.

To prove $(c)\Leftrightarrow(d)$, we replace the assertion in part (d) with the following logically equivalent assertion:

$(d')$: If $v_1,\ldots,v_s$ are linearly dependent vectors in $V$, then $\eta(v_1,\ldots,v_s)=0$.

$(c)\Rightarrow(d')$: Since $v_1,\ldots,v_s$ are linearly dependent, one of them can be expressed as a linear combination of the others, say, $v_1=\sum_{i=2}^s a^i v_i$. Then

$$\eta(v_1,\ldots,v_s)=\eta\left(\sum_{i=2}^s a^i v_i,v_2,\ldots,v_s\right)=\sum_{i=2}^s a^i\eta(v_i,v_2,\ldots,v_s).$$