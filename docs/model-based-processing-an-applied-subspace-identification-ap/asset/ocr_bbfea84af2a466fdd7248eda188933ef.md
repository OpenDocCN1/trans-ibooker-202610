```
P_X(x) = { 1-p   x = 0
          p     x = 1
          0     Otherwise }
```

![](bbox=[0.12, 0.24, 0.38, 0.48])

![](bbox=[0.55, 0.23, 0.85, 0.48])

# Figure A.1 Probability mass and distribution functions for coin tossing experiment.

we have

+   Sample space: Ω = {H, T}
+   Events: B = {0, {H}, {T}}
+   Probability: P_X(x₁ = H) = p
+   P_X(x₀ = T) = 1 - p
+   Random variable: X(ω₁ = H) = x₁ = 1
+   X(ω₂ = T) = x₂ = 0
+   Distribution: F_X(x_i) = { 1   x_i ≥ 1
+                            1 - p  0 ≤ x_i ≤ 1
+                            0   x_i < 0 }

the mass and distribution functions for this example are shown in Figure A.1. Note that the sum of the mass function value must be 1 and that the maximum value of the distribution function is 1 satisfying the properties mentioned previously.

If we extend the idea that a random variable is now a function of time as well, then we can define a stochastic process as discussed in Chapter 2. More formerly, a random or stochastic process is a two-dimensional function of t and ω:

```
X(t, ω),      ω∈Ω,   t∈T      (A.6)
```

where T is a set of index parameters (continuous or discrete) and Ω is the sample space.

We list some of the major theorems in probability theory and refer the reader to more detailed texts [1, 2].