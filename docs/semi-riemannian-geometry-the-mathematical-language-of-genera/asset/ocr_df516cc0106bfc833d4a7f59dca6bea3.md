![](bbox=[0.143, 0.09, 0.848, 0.399])

**Figure 11.6.1.** Differential map

By Theorem 11.3.1(d), $d_q(\varphi)$ is invertible, so

$$\frac{d\mu}{dt}(t_0)=d_q(\varphi)^{-1}(v).$$

We have from Theorem 11.6.1 that $G=\psi^{-1}\circ F\circ\varphi$ is (Euclidean) smooth, and then from Theorem 10.1.12 that so is

$$F\circ\lambda=F\circ\varphi\circ\mu=\psi\circ G\circ\mu.$$

Then

$$\begin{aligned}
d_p(F)(v)&=\frac{d(F\circ\lambda)}{dt}(t_0)\\
&=\frac{d(\psi\circ G\circ\mu)}{dt}(t_0)\\
&=d_{\mu(t_0)}(\psi\circ G)\left(\frac{d\mu}{dt}(t_0)\right)\\
&=d_q(\psi\circ G)\circ d_q(\varphi)^{-1}(v).
\end{aligned}$$

(a): This follows from (11.6.3).

(b): Since $v$ was arbitrary, we have from (11.6.3) that

$$d_p(F)=d_q(\psi\circ G)\circ d_q(\varphi)^{-1}.$$

Thus, $d_p(F)$ is the composition of linear maps, so it too is linear.

# **Theorem 11.6.4 (Chain Rule).** Let $M$, $N$, and $P$ be regular surfaces, let $F:M\to N$ and $G:N\to P$ be smooth maps, and let $p$ be a point in $M$. Then

$$d_p(G\circ F)=d_{F(p)}(G)\circ d_p(F).$$