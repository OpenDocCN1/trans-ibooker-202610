# Step 3. Compute $\nabla^2_{X,Y}(Z)$.

By definition,

$$\nabla^2_{X,Y}(Z)=\nabla_X(\nabla_Y(Z))-\nabla_{\nabla_X(Y)}(Z).$$

We have from (12.4.16) and (12.4.19) that the $l$th component of $\nabla^2_{X,Y}(Z)\circ\varphi$ is

$$\begin{aligned}
(\nabla^2_{X,Y}(Z)\circ\varphi)^l
&=\left([\nabla_X(\nabla_Y(Z))-\nabla_{\nabla_X(Y)}(Z)]\circ\varphi\right)^l\\
&=\left(\sum^{(1)}+\sum^{(2)}+\sum^{(3)}+\sum^{(4)}+\sum^{(5)}+\sum^{(6)}+\sum^{(7)}\right)
-\left(\sum^{(1)}+\sum^{(2)}+\sum^{(8)}+\sum^{(9)}\right)\\
&=\sum_{ij}^{(3)}\alpha^i\beta^j\frac{\partial^2\gamma^l}{\partial r^i\partial r^j}+\sum_{ijk}^{(4)}\alpha^i\beta^j\frac{\partial\gamma^k}{\partial r^i}\Gamma^l_{jk}+\sum_{ijk}^{(5)}\alpha^i\beta^j\gamma^k\frac{\partial\Gamma^l_{jk}}{\partial r^i}\\
&\quad+\sum_{ijk}^{(6)}\alpha^i\beta^j\frac{\partial\gamma^k}{\partial r^j}\Gamma^l_{ik}+\sum_{ijkn}^{(7)}\alpha^i\beta^j\gamma^k\Gamma^n_{jk}\Gamma^l_{in}\\
&\quad-\sum_{ijk}^{(8)}\alpha^i\beta^j\frac{\partial\gamma^l}{\partial r^k}\Gamma^k_{ij}-\sum_{ijkn}^{(9)}\alpha^i\beta^j\gamma^k\Gamma^n_{ij}\Gamma^l_{nk}.
\end{aligned}$$

Since

$$\sum_{ijk}^{(4)}\alpha^i\beta^j\frac{\partial\gamma^k}{\partial r^i}\Gamma^l_{jk}+\sum_{ijk}^{(6)}\alpha^j\beta^i\frac{\partial\gamma^k}{\partial r^i}\Gamma^l_{jk}=\sum_{ijk}^{(4)+(6)}(\alpha^i\beta^j+\alpha^j\beta^i)\frac{\partial\gamma^k}{\partial r^i}\Gamma^l_{jk} \tag{12.4.21}$$

and

$$\begin{aligned}
&\sum_{ijk}^{(5)}\alpha^i\beta^j\gamma^k\frac{\partial\Gamma^l_{jk}}{\partial r^i}+\sum_{ijkn}^{(7)}\alpha^i\beta^j\gamma^k\Gamma^n_{jk}\Gamma^l_{in}-\sum_{ijkn}^{(9)}\alpha^i\beta^j\gamma^k\Gamma^n_{ij}\Gamma^l_{kn}\\
&=\sum_{ijk}^{(5)}\alpha^i\beta^j\gamma^k\frac{\partial\Gamma^l_{jk}}{\partial r^i}+\sum_{ijk}^{(7)}\left(\alpha^i\beta^j\gamma^k\sum_n\Gamma^n_{jk}\Gamma^l_{in}\right)-\sum_{ijk}^{(9)}\left(\alpha^i\beta^j\gamma^k\sum_n\Gamma^n_{ij}\Gamma^l_{kn}\right)\\
&=\sum_{ijk}^{(5)+(7)-(9)}\left[\alpha^i\beta^j\gamma^k\left(\frac{\partial\Gamma^l_{jk}}{\partial r^i}+\sum_n(\Gamma^l_{in}\Gamma^n_{jk}-\Gamma^l_{kn}\Gamma^n_{ij})\right)\right].
\end{aligned} \tag{12.4.22}$$