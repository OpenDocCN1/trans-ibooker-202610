

## 2.4 矩阵的行列式

### 定理 2.4.5

若 \(P\) 和 \(Q\) 是 \(\operatorname{Mat}_{m\times m}\) 中的矩阵，则：

- (a) \(\det(PQ)=\det(P)\det(Q)\)。
- (b) 若 \(P\) 可逆，则 \(\det(P^{-1})=\det(P)^{-1}\)。
- (c) \(\det(P^{\mathsf T})=\det(P)\)。

**Proof.** (a): 设我们定义一个函数 \(\Delta:(\operatorname{Mat}_{m\times 1})^m\to\mathbb R\) 为

\[
\Delta(R_1,\ldots,R_m)=\det([PR_1\ \cdots\ PR_m])
\]

对所有 \(\operatorname{Mat}_{m\times 1}\) 中的矩阵 \(R_1,\ldots,R_m\)。容易证明 \(\Delta\) 是一个行列式函数。我们有

\[
\begin{aligned}
\det([PQ_{(1)}\ \cdots\ PQ_{(m)}])
&=\Delta(Q_{(1)},\ldots,Q_{(m)})\\
&=\Delta(E_1,\ldots,E_m)\det([Q_{(1)}\ \cdots\ Q_{(m)}]) \qquad [(2.4.1),\text{ Th 2.4.3}]\\
&=\det(P)\det(Q).
\end{aligned}
\]

(b): 我们由 (2.4.1) 和部分 (a) 得到

\[
1=\det(I_m)=\det(PP^{-1})=\det(P)\det(P^{-1}),
\]

由此结果得出。

(c): 设 \(P^{\mathsf T}=[p_j^i]^{\mathsf T}=[q_j^i]\)。则

\[
\det(P^{\mathsf T})
=\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma)\,q_{\sigma(1)}^1\cdots q_{\sigma(m)}^m
=\sum_{\sigma\in S_m}\operatorname{sgn}(\sigma)\,p_1^{\sigma(1)}\cdots p_m^{\sigma(m)}
=\det(P),
\]

其中第一个和最后一个等式由 (2.4.1) 得出，而第二个等式由观察 \(q_{\sigma(i)}^i=p_i^{\sigma(i)}\) 对所有 \(S_m\) 中的置换 \(\sigma\) 成立得出。

设 \(P\) 是 \(\operatorname{Mat}_{m\times m}\) 中的矩阵。\(P\) 的 \(ij\)-余子式定义为

\[
c_j^i=(-1)^{i+j}\det\left(P^{(1,\ldots,\hat i,\ldots,m)}_{(1,\ldots,\hat j,\ldots,m)}\right),
\]

其中帽子表示省略一个表达式。于是，\(P^{(1,\ldots,\hat i,\ldots,m)}_{(1,\ldots,\hat j,\ldots,m)}\) 是通过删除 \(P\) 的第 \(i\) 行和第 \(j\) 列得到的 \(\operatorname{Mat}_{(m-1)\times(m-1)}\) 中的矩阵。\(P\) 的伴随矩阵是 \(\operatorname{Mat}_{m\times m}\) 中定义的矩阵

\[
\operatorname{adj}(P)=[c_j^i]^{\mathsf T}.
\]

### 定理 2.4.6 (行列式的列展开)

若 \(P=[p_j^i]\) 是 \(\operatorname{Mat}_{m\times m}\) 中的矩阵且 \(1\le j\le m\)，则

\[
\det(P)=\sum_i c_j^i p_j^i,
\]

这称为 \(\det(P)\) 沿 \(P\) 的第 \(j\) 列的展开。

**Proof.** 我们有

\[
P=\begin{bmatrix} P_{(1,\ldots,j-1)} & \sum_i p_j^i E_i & P_{(j+1,\ldots,m)} \end{bmatrix}
\]

故

\[
\det(P)=\sum_i p_j^i\det\left(\begin{bmatrix} P_{(1,\ldots,j-1)} & E_i & P_{(j+1,\ldots,m)} \end{bmatrix}\right).
\]

我们也有

\[
\begin{aligned}
\det\left(\begin{bmatrix} P_{(1,\ldots,j-1)} & E_i & P_{(j+1,\ldots,m)} \end{bmatrix}\right)
&=\det\left(\begin{bmatrix}
P^{(1,\ldots,i-1)}_{(1,\ldots,j-1)} & O_{(i-1)\times1} & P^{(1,\ldots,i-1)}_{(j+1,\ldots,m)}\\
P^{(i)}_{(1,\ldots,j-1)} & 1 & P^{(i)}_{(j+1,\ldots,m)}\\
P^{(i+1,\ldots,m)}_{(1,\ldots,j-1)} & O_{(m-i)\times1} & P^{(i+1,\ldots,m)}_{(j+1,\ldots,m)}
\end{bmatrix}\right)\\
&=(-1)^{(i-1)+(j-1)}\det\left(\begin{bmatrix}
1 & P^{(i)}_{(1,\ldots,j-1)} & P^{(i)}_{(j+1,\ldots,m)}\\
O_{(i-1)\times1} & P^{(1,\ldots,i-1)}_{(1,\ldots,j-1)} & P^{(1,\ldots,i-1)}_{(j+1,\ldots,m)}\\
O_{(m-i)\times1} & P^{(i+1,\ldots,m)}_{(1,\ldots,j-1)} & P^{(i+1,\ldots,m)}_{(j+1,\ldots,m)}
\end{bmatrix}\right)\\
&=(-1)^{i+j}\det\left(\begin{bmatrix}
1 & P^{(i)}_{(1,\ldots,\hat j,\ldots,m)}\\
O_{(m-1)\times1} & P^{(1,\ldots,\hat i,\ldots,m)}_{(1,\ldots,\hat j,\ldots,m)}
\end{bmatrix}\right)\\
&=(-1)^{i+j}\det\left(P^{(1,\ldots,\hat i,\ldots,m)}_{(1,\ldots,\hat j,\ldots,m)}\right)=c_j^i,
\end{aligned}
\]

其中第二个等式是通过交换 \(i-1\) 个相邻行，然后交换 \(j-1\) 个相邻列得到的，其中第四个等式由 (2.4.1) 得出。结合上述恒等式得到结果。

### 定理 2.4.7

若 \(P\) 是 \(\operatorname{Mat}_{m\times m}\) 中的矩阵，则

\[
P\operatorname{adj}(P)=\det(P)I_m.
\]

**Proof.** 设 \(P=[p_j^i]\)。对于给定的整数 \(1\le k,l\le m\)，设 \(Q=[q_j^i]\) 是通过用 \(P\) 的第 \(l\) 列替换 \(P\) 的第 \(k\) 列得到的 \(\operatorname{Mat}_{m\times m}\) 中的矩阵；即

\[
Q=[P_{(1)}\ \cdots\ P_{(k-1)}\ P_{(l)}\ P_{(k+1)}\ \cdots\ P_{(m)}].
\]

设 \(\operatorname{adj}(P)=[c_j^i]^{\mathsf T}\) 且 \(\operatorname{adj}(Q)=[d_j^i]^{\mathsf T}\)。由定理 2.4.6，

\[
\sum_i c_k^i p_k^i=\det(P).
\]

若 \(k\ne l\)，则 \(Q\) 的两列相等且

\[
Q^{(1,\ldots,\hat i,\ldots,m)}_{(1,\ldots,\hat k,\ldots,m)}=P^{(1,\ldots,\hat i,\ldots,m)}_{(1,\ldots,\hat k,\ldots,m)}.
\]

由定理 2.4.2 和 2.4.6 可得

\[
\sum_i c_k^i p_l^i=\sum_i d_k^i q_l^i=\det(Q)=0.
\]

于是，

\[
\sum_i c_k^i p_l^i=\det(P)\,\delta_{kl}
\]

对 \(k,l=1,\ldots,m\) 成立，其中 \(\delta_{kl}\) 是克罗内克 delta。则 \([c_j^i][p_j^i]^{\mathsf T}=\det(P)I_m\)。取转置得到结果。

### 定理 2.4.8

若 \(P\) 是 \(\operatorname{Mat}_{m\times m}\) 中的矩阵，则以下条件等价：

- (a) \(P\) 可逆。
- (b) \(\det(P)\ne 0\)。
- (c) \(\operatorname{rank}(P)=m\)。

若上述等价条件之一成立，则

\[
P^{-1}=\frac{1}{\det(P)}\operatorname{adj}(P).
\]

**Proof.** (a) \(\Rightarrow\) (b): 由定理 2.4.5(b)，\(\det(P)\det(P^{-1})=1\)，故 \(\det(P)\ne 0\)。

(b) \(\Rightarrow\) (c): 由于 \(\det([P_{(1)}\ \cdots\ P_{(m)}])=\det(P)\ne0\)，由定理 2.4.2 可知 \(P_{(1)},\ldots,P_{(m)}\) 线性无关，所以

\[
\operatorname{rank}(P)=\operatorname{rank}([P_{(1)}\ \cdots\ P_{(m)}])=m.
\]

(c) \(\Rightarrow\) (a): 由于 \(\operatorname{rank}(P)=m\)，\(\mathscr P=(P_{(1)},\ldots,P_{(m)})\) 是 \(\operatorname{Mat}_{m\times1}\) 的基。设 \(Q=[[E_1]_{\mathscr P}\ \cdots\ [E_m]_{\mathscr P}]\)。根据定义，\(E_j=P[E_j]_{\mathscr P}\) 对 \(j=1,\ldots,m\) 成立，故

\[
I_m=[E_1\ \cdots\ E_m]=PQ,
\]

所以 \(Q=P^{-1}\)。最后的断言由定理 2.4.7 得出。

### 定理 2.4.9

\(\operatorname{Mat}_{m\times1}\) 中的矩阵 \(P_1,\ldots,P_m\) 线性无关当且仅当 \(\det([P_1\ \cdots\ P_m])\ne0\)。

**Proof.** 我们有

\[
\begin{aligned}
P_1,\ldots,P_m\text{ 线性无关}
&\Longleftrightarrow \operatorname{rank}([P_1\ \cdots\ P_m])=m\\
&\Longleftrightarrow \det([P_1\ \cdots\ P_m])\ne0. \qquad [\text{Th 2.4.8}]
\end{aligned}
\]

下一个结果通常不包含在行列式的概述中，但它在以后将证明非常有价值。

### 定理 2.4.10 (Cauchy–Binet 恒等式)

设 \(1\le k\le m\) 为整数。

- (a) 若 \(P\) 和 \(Q\) 是 \(\operatorname{Mat}_{m\times k}\) 中的矩阵，则

\[
\det(P^{\mathsf T}Q)=\sum_{I\in\mathcal I_{k,m}}\det(P^I)\det(Q^I).
\]

- (b) 若 \(R\) 和 \(S\) 是 \(\operatorname{Mat}_{k\times m}\) 中的矩阵，则

\[
\det(RS^{\mathsf T})=\sum_{J\in\mathcal I_{k,m}}\det(R_J)\det(S_J).
\]

**Proof.** (a): 设 \(Q=[q_j^i]\)，于是

\[
Q=[Q_{(1)}\ \cdots\ Q_{(k)}],\qquad Q_{(j)}=[q_j^1\ \cdots\ q_j^m]^{\mathsf T},
\]

且

\[
P^{\mathsf T}Q_{(j)}=\sum_i q_j^i(P^{\mathsf T})_{(i)}
\]

对 \(j=1,\ldots,k\)。我们有

\[
\begin{aligned}
\det(P^{\mathsf T}Q)
&=\det\left([P^{\mathsf T}Q_{(1)}\ \cdots\ P^{\mathsf T}Q_{(k)}]\right)\\
&=\det\left(\left[\sum_{i_1}q_1^{i_1}(P^{\mathsf T})_{(i_1)}\ \cdots\ \sum_{i_k}q_k^{i_k}(P^{\mathsf T})_{(i_k)}\right]\right)\\
&=\sum_{1\le i_1,\ldots,i_k\le m}q_1^{i_1}\cdots q_k^{i_k}\det\left([(P^{\mathsf T})_{(i_1)}\ \cdots\ (P^{\mathsf T})_{(i_k)}]\right)\\
&=\sum_{\substack{1\le i_1,\ldots,i_k\le m\\ i_1,\ldots,i_k\text{ 互不相同}}}q_1^{i_1}\cdots q_k^{i_k}\det\left([(P^{\mathsf T})_{(i_1)}\ \cdots\ (P^{\mathsf T})_{(i_k)}]\right)\\
&=\sum_{1\le i_1<\cdots<i_k\le m}\left(\sum_{\sigma\in S_k}q_1^{i_{\sigma(1)}}\cdots q_k^{i_{\sigma(k)}}\det\left([(P^{\mathsf T})_{(i_{\sigma(1)})}\ \cdots\ (P^{\mathsf T})_{(i_{\sigma(k)})}]\right)\right).
\end{aligned}
\]

其中第三个等式由定理 2.4.2 得出。前一显示中最后一行的带大括号的项可以表示为

\[
\begin{aligned}
&\sum_{\sigma\in S_k}q_1^{i_{\sigma(1)}}\cdots q_k^{i_{\sigma(k)}}\det\left([(P^{\mathsf T})_{(i_{\sigma(1)})}\ \cdots\ (P^{\mathsf T})_{(i_{\sigma(k)})}]\right)\\
&=\sum_{\sigma\in S_k}q_1^{i_{\sigma(1)}}\cdots q_k^{i_{\sigma(k)}}\operatorname{sgn}(\sigma)\det\left([(P^{\mathsf T})_{(i_1)}\ \cdots\ (P^{\mathsf T})_{(i_k)}]\right)\\
&=\det\left([(P^{\mathsf T})_{(i_1)}\ \cdots\ (P^{\mathsf T})_{(i_k)}]\right)\left(\sum_{\sigma\in S_k}\operatorname{sgn}(\sigma)q_1^{i_{\sigma(1)}}\cdots q_k^{i_{\sigma(k)}}\right)\\
&=\det\left(P^{\mathsf T}_{(i_1,\ldots,i_k)}\right)\det\left(Q^{(i_1,\ldots,i_k)}\right).
\end{aligned}
\]

于是，

\[
\det(P^{\mathsf T}Q)=\sum_{1\le i_1<\cdots<i_k\le m}\det\left(P^{\mathsf T}_{(i_1,\ldots,i_k)}\right)\det\left(Q^{(i_1,\ldots,i_k)}\right),
\]

这就是 Cauchy–Binet 恒等式的经典版本。

### 定理 2.4.12

若 \(V\) 是向量空间，且 \(\mathcal H\) 和 \(\mathcal F\) 是 \(V\) 的基，则

\[
\det([\operatorname{id}_V]_{\mathcal H}^{\mathcal F})\ne 0.
\]

**Proof.** 我们有

\[
1=\det([\operatorname{id}_V]_{\mathcal H}^{\mathcal H}) \qquad [(2.4.2)]
\]

\[
=\det([\operatorname{id}_V]_{\mathcal F}^{\mathcal H})\det([\operatorname{id}_V]_{\mathcal H}^{\mathcal F}) \qquad [\text{Th 2.2.2(c), Th 2.4.5(a)}],
\]

由此结果得出。

在与定理 2.2.3 相关的讨论中提到，\(\operatorname{Mat}_{m\times m}\) 在通常的矩阵加法和矩阵乘法运算下是一个环，故它在矩阵加法下是一个群。但 \(\operatorname{Mat}_{m\times m}\) 显然不是在矩阵乘法下是一个群，因为并非所有矩阵都有逆。然而，\(\operatorname{Mat}_{m\times m}\) 包含若干矩阵群。其中最大的是由所有可逆矩阵组成的一般线性群：

\[
GL(m)=\{P\in\operatorname{Mat}_{m\times m}:\det(P)\ne0\},
\]

其中使用行列式的刻画由定理 2.4.8 得出。

我们说 \(\operatorname{Mat}_{m\times m}\) 中的矩阵 \(P\) 是正交的，如果 \(P^{\mathsf T}P=I_m\)。如果是，则 \(P^{\mathsf T}=P^{-1}\)，故 \(PP^{\mathsf T}=PP^{-1}=I_m\)。于是，\(P^{\mathsf T}P=I_m\) 当且仅当 \(PP^{\mathsf T}=I_m\)。正交群是 \(GL(m)\) 的子群，由所有正交矩阵组成：

\[
O(m)=\{P\in GL(m):P^{\mathsf T}P=I_m\}.
\]

对于 \(O(m)\) 中的矩阵 \(P\)，我们有

\[
1=\det(I_m)=\det(P^{\mathsf T}P)=\det(P^{\mathsf T})\det(P)=\det(P)^2,
\]

故 \(\det(P)=\pm1\)。特殊正交群是 \(O(m)\) 的子群，由所有行列式等于 \(1\) 的矩阵组成：

\[
SO(m)=\{P\in O(m):\det(P)=1\}.
\]

## 2.5 线性映射的迹与行列式

矩阵的迹在第 2.1 节中定义，行列式在第 2.4 节中定义。我们现在将这些概念推广到线性映射。

设 \(V\) 是向量空间，\(\mathcal H\) 是 \(V\) 的基，\(A:V\longrightarrow V\) 是线性映射。\(A\) 的迹定义为

\[
\operatorname{tr}(A)=\operatorname{tr}([A]_{\mathcal H}^{\mathcal H}), \qquad (2.5.1)
\]

\(A\) 的行列式定义为

\[
\det(A)=\det([A]_{\mathcal H}^{\mathcal H}). \qquad (2.5.2)
\]

