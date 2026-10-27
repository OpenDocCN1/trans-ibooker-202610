
## Acknowledgments

The authors would like to express our thanks for the understanding, patience, and support given to us by our families, especially our wives, and by our colleagues. We would also like to thank the numerous government agencies and technical points of contact behind the grants that supported the research results reflected in this book: Department of Defense – NAVAIR, NAVSEA, Air Force, and Army, NASA, and the US Department of Energy. We also acknowledge the support, understanding, and encouragement we received from the staff of Wiley books. Last, but not least, we acknowledge the support and assistance of our staff editor, Richard Thompson, provided in the way of logistics, editing, coordination, and liaison—especially with the Wiley staff.

## CORE PROGNOSTIC FRAMEWORKS for PROGNOSIS in a PHM SYSTEM

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_31_0.png)

Figure 1.1 Core prognostic frameworks in a PHM system. Source: based on IEEE (2017).

## PHM: FRAMEWORK DIAGRAM

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_31_1.png)

Figure 1.2 Framework diagram for a PHM system. Source: based on CAVE3 (2015).

classical approach based on reliability theory and statistics: the fundamental ideas and methods of those classical approaches will be presented and discussed in this chapter. First, the basic ideas and methods of reliability engineering will be outlined, followed by brief discussions of the main ideas of the different elements of prognostic health management. This chapter includes some cost-minimizing and cost-benefit models.

The primary objective of this chapter is to provide a general overview of this complex scientific field. Detailed descriptions and model development for a heuristic approach to modeling condition-based data (CBD) to support prognosis in a PHM system are presented in the later chapters.

### 1.1.2 Chapter Organization

The remainder of this chapter is organized to present and discuss a heuristic-based approach to modeling CBD signatures as follows:

- 1.2 Foundation of Reliability Theory
This section presents the foundation upon which reliability is built: failure distributions, probability and reliability, probability density functions and relationships, failure rate, and expected value and variance.

- 1.3 Failure Distributions Under Extreme Stress Levels
This section presents models for failure and extreme stress, cumulative damage, and exponential and Weibull failures and distribution.

- 1.4 Uncertainty Measures in Parameter Estimation
This section presents an introduction to measurement metrics dealing with uncertainty, such as likelihood, variance, and covariance.

- 1.5 Expected Number of Failures
This section presents the effects and interaction of repair, replacement, and partial repair activities on assessing the number of failures and decrease in effective age due to partial repairs.

- 1.6 System Reliability and Prognosis and Health Management
This section presents a framework for a PHM system based on condition-based maintenance (CBM) and introduces the reader to such a framework, CBD and signatures, and CBM, which can transform signatures into curves useful for processing to produce prognostic information.

- 1.7 Prognostic Information
This section introduces basic concepts and measures that are applicable to prognostic information: RUL, SoH, prognostic horizon (PH), prognostic distance (PD), and convergence.

- 1.8 Decisions on Cost and Benefits
This section presents a brief overview of decision problems and their mathematical modeling during different stages of the lifetime of equipment, with some illustrative mathematical models.

- 1.9 Introduction to PHM: Summary
This section summarizes the material presented in this chapter.

## 1.2 Foundation of Reliability Theory

Engineering systems and their components and elements are subject to degradation that sooner or later results in failure. A component that fails sooner compared to other components is said to be less reliable. Failures of like objects, such as components and assemblies, do not all occur at the same point in time; failures are distributed over time.

### 1.2.1 Time-to-Failure Distributions

The time to failure (TTF) of a particular object is usually modeled using probability theory, which represents the average behavior of a large set of identical objects and not the behavior of a particular object. It is considered as a random variable, denoted by $X$. The cumulative distribution function (CDF) of $X$ is the probability that failure occurs at or before time $t$:

$$F(t)=P(X\leq t) \tag{1.1}$$

It is usually assumed that the object starts its operation at $t=0$, so $F(t)=0$ as $t\leq 0$. Furthermore, $F(t)$ tends to unity as $t\to\infty$: the function is non-decreasing. In reliability engineering, the most frequently used distribution types are as follows:

- 1. *Exponential* distribution:
  
      $$F(t)=1-e^{-\lambda t},\qquad t\geq 0 \tag{1.2}$$
  
      where the parameter $\lambda>0$ determines the lifetime.
  
- 2. *Weibull* distribution:
  
      $$F(t)=1-e^{-(t/\eta)^\beta},\qquad t\geq 0 \tag{1.3}$$
  
      where $\eta>0$ is referred to as the *scale* and $\beta>0$ is referred to as the *shape*.
  
- 3. *Gamma* distribution:
  
      $$F(t)=\int_0^t\frac{x^{k-1}e^{-x/\theta}}{\theta^k\Gamma(k)}\,dx,\qquad t\geq 0 \tag{1.4}$$
  
      where $k$ represents a shape parameter and $\theta$ represents a scale parameter. Furthermore, $\Gamma(k)$ is the gamma function value at $k$. Very often, variable $\lambda=\frac{1}{\theta}$ is used in applications.
  
- 4. *Normal* distribution:
  
      $$F(t)=\phi\left(\frac{t-\mu}{\sigma}\right),\qquad -\infty<t<\infty \tag{1.5}$$
  
      where $\mu$ is a real parameter and $\sigma>0$.
  
- 5. *Lognormal* distribution:
  
      $$F(t)=\phi\left(\frac{\ln(t)-\mu}{\sigma}\right),\qquad t>0 \tag{1.6}$$
  
      where $\mu$ is a real value and $\sigma>0$. Here $\phi(t)$ is the standard normal distribution function defined in Eq. (1.7):
  
      $$\phi(t)=\int_{-\infty}^{t}\frac{1}{\sqrt{2\pi}}e^{-x^2/2}\,dx \tag{1.7}$$
  
      This does not have an exact simple formula; only function approximations are available, or the value of $\phi(t)$ for any particular value of $t$ can be obtained from the corresponding function table.
  
- 6. *Logistic* distribution:
  
      $$F(t)=\frac{1}{1+e^{-z}}=\frac{e^z}{1+e^z},\qquad z=\frac{t-\mu}{\sigma},\qquad -\infty<t<\infty \tag{1.8}$$
  
      where $\mu$ and $\sigma>0$ are the model parameters.

7. Gumbel distribution:

$$F(t) = 1 - e^{-e^{z}} \text{ with } z = \frac{t - \mu}{\sigma} \qquad -\infty < t < \infty \tag{1.9}$$

where the meanings of μ and σ are the same as before. This distribution is often called the log-Weibull distribution.

8. Log-logistic distribution:

$$F(t) = \frac{e^{x}}{1 + e^{x}} \text{ with } x = \frac{\ln(t) - \mu}{\sigma} \qquad t > 0 \tag{1.10}$$

where Eq. (1.10) also has an alternative formulation:

$$F(t) = \frac{t^{\beta}}{\alpha^{\beta} + t^{\beta}} \tag{1.11}$$

with α, β > 0 and α = e^{μ} and β = 1/σ

9. Log-Gumbel distribution:

$$F(t) = 1 - e^{-e^{x}} \text{ with } x = \frac{\ln(t) - \mu}{\sigma} \qquad t > 0 \tag{1.12}$$

10. Log-gamma distribution:

$$F(t) = \int_{0}^{t} \frac{\lambda (\lambda \ln(x))^{\alpha -1} e^{-\lambda \ln(x)}}{\Gamma(\alpha) x} dx \qquad t > 0 \tag{1.13}$$

where λ and α are positive parameters.

The CDFs of exponential and Weibull distributions are illustrated in Figures 1.3 and 1.4.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_34_0.png)

Figure 1.3 Graph of the exponential CDF with λ = 3.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_35_0.png)

Figure 1.4 Graph of the Weibull CDF with β = 1.2 and η = 5.

### 1.2.2 Probability and Reliability

Most probability values can be determined by using the CDF of X. The probability that failure occurs at or before time t is given as F(t), and the probability that the object is still working at time t is the complement of F(t),

```
P(X > t) = 1 − F(t) = R(t)    (1.14)
```

which is called the reliability function of X. Sometimes it is also denoted by F̄(t). The probability that failure occurs during time interval [t₁, t₂] is given as

```
P(t₁ < X ≤ t₂) = F(t₂) − F(t₁)    (1.15)
```

# Example 1.1 Assume the random variable X is exponential with parameter λ = 1. Then

```
F(t) = 1 − e^{−t}
```

```
P(X > 2) = 1 − (1 − e^{−2}) = 0.1353
```

```
P(1 < X < 2) = (1 − e^{−2}) − (1 − e^{−1}) = e^{−1} − e^{−2} = 0.3679 − 0.1353 = 0.2326
```

# Example 1.2 Assume next that random variable X is Weibull with β = 1.5 and η = 5.

Then F(t) = 1 − e^{−(t/5)^{1.5}}, and therefore

```
P(X > 2) = 1 − (1 − e^{−(2/5)^{1.5}}) = e^{−0.2530} ≈ 0.7765
```
