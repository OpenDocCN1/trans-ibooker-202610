![](bbox=[0.28, 0.12, 0.72, 0.42])

# Figure 13.6.1. One, $\mathcal{P}^2$

In the notation of Theorem 11.4.2, let

```
f(x, y) = \sqrt{x^2 + y^2 - 1}
\varphi(x, y) = (x, y, \sqrt{x^2 + y^2 - 1})
U = \{(x, y) \in \mathbb{R}^2 : x^2 + y^2 > 1\}.
```

By Theorem 11.4.2, One is a regular surface and $(U, \varphi)$ is a chart. Viewing One as a regular surface in $\mathbb{R}_0^3$, Theorem 12.10.1 gives:

```
H_1 = \left(1, 0, \frac{x}{\sqrt{x^2 + y^2 - 1}}\right) \quad H_2 = \left(0, 1, \frac{y}{\sqrt{x^2 + y^2 - 1}}\right)
\mathcal{N} \circ \varphi = \frac{1}{\sqrt{2x^2 + 2y^2 - 1}} \left(-x, -y, \sqrt{x^2 + y^2 - 1}\right)
\mathfrak{g}_\mathcal{H} = \frac{1}{x^2 + y^2 - 1} \begin{bmatrix} 2x^2 + y^2 - 1 & xy \\ xy & x^2 + 2y^2 - 1 \end{bmatrix}
\mathfrak{h}_\mathcal{H} = \frac{1}{(x^2 + y^2 - 1)\sqrt{2x^2 + 2y^2 - 1}} \begin{bmatrix} y^2 - 1 & -xy \\ -xy & x^2 - 1 \end{bmatrix}
\mathcal{K} \circ \varphi = -\frac{1}{(2x^2 + 2y^2 - 1)^2}.
```

We observe that the Gauss curvature is nonconstant and strictly negative.

# 13.7 Hyperboloid of Two Sheets in $\mathbb{R}_0^3$

The set

```
Two = \{(x, y, z) \in \mathbb{R}^3 : x^2 + y^2 - z^2 = -1, z > 0\}
```