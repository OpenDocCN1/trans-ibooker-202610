# 1.2.1  **Time-to-Failure Distributions**

The time to failure (TTF) of a particular object is usually modeled using probability theory, which represents the average behavior of a large set of identical objects and not the behavior of a particular object. It is considered as a random variable, denoted by $X$. The cumulative distribution function (CDF) of $X$ is the probability that failure occurs at or before time $t$:

$$F(t)=P(X\leq t) \tag{1.1}$$

It is usually assumed that the object starts its operation at $t=0$, so $F(t)=0$ as $t\leq 0$. Furthermore, $F(t)$ tends to unity as $t\to\infty$: the function is non-decreasing. In reliability engineering, the most frequently used distribution types are as follows:

+   1. *Exponential* distribution:
+   
+      $$F(t)=1-e^{-\lambda t},\qquad t\geq 0 \tag{1.2}$$
+   
+      where the parameter $\lambda>0$ determines the lifetime.
+   
+   2. *Weibull* distribution:
+   
+      $$F(t)=1-e^{-(t/\eta)^\beta},\qquad t\geq 0 \tag{1.3}$$
+   
+      where $\eta>0$ is referred to as the *scale* and $\beta>0$ is referred to as the *shape*.
+   
+   3. *Gamma* distribution:
+   
+      $$F(t)=\int_0^t\frac{x^{k-1}e^{-x/\theta}}{\theta^k\Gamma(k)}\,dx,\qquad t\geq 0 \tag{1.4}$$
+   
+      where $k$ represents a shape parameter and $\theta$ represents a scale parameter. Furthermore, $\Gamma(k)$ is the gamma function value at $k$. Very often, variable $\lambda=\frac{1}{\theta}$ is used in applications.
+   
+   4. *Normal* distribution:
+   
+      $$F(t)=\phi\left(\frac{t-\mu}{\sigma}\right),\qquad -\infty<t<\infty \tag{1.5}$$
+   
+      where $\mu$ is a real parameter and $\sigma>0$.
+   
+   5. *Lognormal* distribution:
+   
+      $$F(t)=\phi\left(\frac{\ln(t)-\mu}{\sigma}\right),\qquad t>0 \tag{1.6}$$
+   
+      where $\mu$ is a real value and $\sigma>0$. Here $\phi(t)$ is the standard normal distribution function defined in Eq. (1.7):
+   
+      $$\phi(t)=\int_{-\infty}^{t}\frac{1}{\sqrt{2\pi}}e^{-x^2/2}\,dx \tag{1.7}$$
+   
+      This does not have an exact simple formula; only function approximations are available, or the value of $\phi(t)$ for any particular value of $t$ can be obtained from the corresponding function table.
+   
+   6. *Logistic* distribution:
+   
+      $$F(t)=\frac{1}{1+e^{-z}}=\frac{e^z}{1+e^z},\qquad z=\frac{t-\mu}{\sigma},\qquad -\infty<t<\infty \tag{1.8}$$
+   
+      where $\mu$ and $\sigma>0$ are the model parameters.