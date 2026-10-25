which, since $M$ is Riemannian, is equivalent to

$$\left\|\frac{d\lambda}{ds}(s)\right\|^2=\left\|\frac{dr}{ds}(s)\right\|^2+\left\|\frac{\partial\sigma}{\partial s}(r(s),s)\right\|^2.$$

Thus,

$$\left\|\frac{d\lambda}{ds}(s)\right\|\geq\left\|\frac{dr}{ds}(s)\right\| \tag{21.1.9}$$

for all $s$ in $(0,1]$. For a real number $0<\varepsilon<1$,

$$\begin{aligned}
L(\lambda)&\geq\int_{\varepsilon}^{1}\left\|\frac{d\lambda}{ds}(s)\right\|\,ds &&[19.2.1]\\
&\geq\int_{\varepsilon}^{1}\left\|\frac{dr}{ds}(s)\right\|\,ds &&[21.1.9]\\
&\geq\left|\int_{\varepsilon}^{1}\frac{dr}{ds}(s)\,ds\right|\\
&=|r(1)-r(\varepsilon)|, &&[\mathrm{FTC}]
\end{aligned} \tag{21.1.10}$$

where FTC stands for the fundamental theorem of calculus. It follows from (21.1.2) and (21.1.10) that

$$L(\lambda)\geq\lim_{\varepsilon\to0^+}|r(1)-r(\varepsilon)|=|r(1)|=L(\rho_{p,v}).$$

An observation on Theorem 21.1.1 is that, in geometric terms, (21.1.8) says that the latitude curve $\sigma_r$ and longitude curve $\sigma_s$ intersect orthogonally along the radial geodesic $\sigma_s$, something that is intuitively clear in certain cases, such as the sphere.

# 21.2  Classical Vector Calculus Theorems

Section 10.6 presents some of the classical theorems of differential vector calculus. At various points in the book, we encountered their modern counterparts, as summarized in the following table:

| Classical | Modern |
|---|---|
| Theorem 10.6.1(a) | Theorem 20.3.3 |
| Theorem 10.6.1(b) | Theorem 20.4.4 |
| Theorem 10.6.1(c) | Example 16.1.7 |
| Theorem 10.6.1(d) | Example 16.1.8 |
| Theorem 10.6.2 | Theorems 16.9.2, 16.9.3, 16.9.8 |