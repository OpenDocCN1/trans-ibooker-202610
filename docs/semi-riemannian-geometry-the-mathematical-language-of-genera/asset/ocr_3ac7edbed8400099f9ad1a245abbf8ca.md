**Proof.** (c): We have

$\eta$ is in $\ker(A^*)$  
$\Leftrightarrow\quad A^*(\eta)=0$  
$\Leftrightarrow\quad A^*(\eta)(v)=0$ for all $v$ in $V$  
$\Leftrightarrow\quad \eta(A(v))=0$ for all $v$ in $V$ \quad [(1.3.1)]  
$\Leftrightarrow\quad \eta$ is in $\operatorname{im}(A)^0$.

(d): It follows from Theorem 1.3.1(c) and part (c) that

$$\ker(A)=\ker(A^{**})=\operatorname{im}(A^*)^0,$$

and then from Theorem 1.4.2 that

$$\ker(A)^0=\operatorname{im}(A^*)^{00}=\operatorname{im}(A^*).$$

(a): We have

$$\begin{aligned}
\operatorname{rank}(A^*) &= \dim(\operatorname{im}(A^*)) \\
&= \dim(\ker(A)^0) && [\text{part (d)}]\\
&= \dim(V)-\dim(\ker(A)) && [\text{Th 1.4.1}]\\
&= \operatorname{rank}(A). && [\text{Th 1.1.11}]
\end{aligned}$$

(b): We have

$$\begin{aligned}
\operatorname{rank}(A^*)+\operatorname{null}(A^*) &= \dim(W^*) && [\text{Th 1.1.11}]\\
&= \dim(W) && [\text{Th 1.2.1(c)}]\\
&= \dim(V) && [\text{assumption}]\\
&= \operatorname{rank}(A)+\operatorname{null}(A). && [\text{Th 1.1.11}]
\end{aligned}$$

The result now follows from part (a).