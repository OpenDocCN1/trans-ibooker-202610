or

$P(1 < X < 2) = \left(1 - e^{-\left(\frac{2}{5}\right)^{1.5}}\right) - \left(1 - e^{-\left(\frac{1}{5}\right)^{1.5}}\right) = e^{-0.2^{1.5}} - e^{-0.4^{1.5}}$ 
$= 0.9144 - 0.7765 \approx 0.1379$

# 1.2.3 Probability Density Function

The probability density function (PDF) of $X$ is the derivative of $F(t)$:

$f(t) = F'(t)$  (1.16)

+   1. *Exponential PDF:*
+      It is easy to see that in the exponential case
+      $f(t) = \lambda e^{-\lambda t}$  (1.17)
+   2. *Weibull PDF:*
+      $f(t) = \frac{\beta}{\eta^{\beta}} t^{\beta-1} e^{-\left(\frac{t}{\eta}\right)^{\beta}}$  (1.18)
+   3. *Gamma PDF:*
+      The gamma PDF is the integrand of Eq. (1.4):
+      $f(t) = \frac{t^{k-1} e^{-\frac{t}{\theta}}}{\theta^{k} \Gamma(k)}$  (1.19)
+   4. *Normal PDF:*
+      The normal PDF has a closed form representation:
+      $f(t) = \frac{1}{\sigma \sqrt{2\pi}} e^{-\frac{(t-\mu)^2}{2\sigma^2}}$  (1.20)
+   5. *Lognormal PDF:*
+      $f(t) = \frac{1}{t \sigma \sqrt{2\pi}} e^{-\frac{(\ln(t)-\mu)^2}{2\sigma^2}}$  (1.21)
+   6. *Logistic PDF:*
+      $f(t) = \frac{e^{z}}{\sigma (1+e^{z})^2}$ with $z = \frac{t - \mu}{\sigma}$  (1.22)
+   7. *Gumbel PDF:*
+      $f(t) = \frac{1}{\sigma} e^{z - e^{z}}$ with $z = \frac{t - \mu}{\sigma}$  (1.23)
+   8. *Log-logistic PDF:*
+      $f(t) = \frac{e^{x}}{\sigma t (1 + e^{x})^2}$ with $x = \frac{\ln(t) - \mu}{\sigma}$  (1.24)
+   9. *Log-Gumbel PDF:*
+      $f(t) = e^{x - e^{x}} \cdot \frac{1}{\sigma t}$ with $x = \frac{\ln(t) - \mu}{\sigma}$  (1.25)
+   10. *Log-gamma PDF:*
+       $f(t) = \frac{\lambda (\lambda (\ln(t))^{\alpha-1} e^{-\lambda \ln(t)}}{\Gamma(\alpha) t}$  (1.26)