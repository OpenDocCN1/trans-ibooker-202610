integer. To aid the reading of sample CCM, Tiao and Box (1981) device a simplified approach to inspect the features of sample CCM. Specifically, consider \(\hat{\boldsymbol{\mathcal{R}}}(\ell)\) for \(\ell > 0\). Tiao and Box define a corresponding simplified matrix \(\boldsymbol{\mathcal{S}}(\ell) = [S_{ij}(\ell)]\), where

```
S_{ij}(\ell) = \begin{cases} + & \text{if } \hat{\rho}_{ij}(\ell) \ge 2/\sqrt{T}, \\ . & \text{if } |\hat{\rho}_{ij}(\ell)| < 2/\sqrt{T}, \\ - & \text{if } \hat{\rho}_{ij}(\ell) \le -2/\sqrt{T}, \end{cases}
```

where it is understood that \(1/\sqrt{T}\) is the sample standard error of elements of \(\hat{\boldsymbol{\mathcal{R}}}(\ell)\) provided that \(z_t\) is a Gaussian white noise. It is much easier to comprehend the dynamic dependence of \(z_t\) by inspecting the simplified CCM \(\boldsymbol{\mathcal{S}}(\ell)\) for \(\ell = 1, ..., m\). We demonstrate the effectiveness of the simplified CCM via an example.

# Example 1.1

Consider the three temperature series of Figure 1.1. The sample means of the three series are 0.15, 0.14, and 0.12, respectively, for Europe, North America, and South America. The sample standard deviations are 1.02, 1.06, and 0.47, respectively. The average temperatures of November for South America appear to be lower and relatively less variable compared with those of Europe and North America. Turn to sample CCMs. In this particular instance, \(k=3\) so that each CCM contains 9 real numbers. Suppose we like to examine the dynamic dependence of the three series using the first 12 lags of sample CCM. This would require us to decipher 108 numbers simultaneously. On the other hand, the dynamic dependence pattern is relatively easy to comprehend by examining the simplified CCMs, which are given below.

```
Simplified matrix:
CCM at lag: 1
. . +
. . +
. . +
CCM at lag: 2
. . +
. . +
+ . +
CCM at lag: 3
. . +
. . +
. + +
CCM at lag: 4
. . .
. . .
. . +
CCM at lag: 5
. . .
. . +
. + +
```