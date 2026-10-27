The derivative of the bracketed expression is

```
R(T)ρ(T) + ∫₀ᵀ R(t)dt ρ'(T) − f(T) = ρ'(T) ∫₀ᵀ R(t)dt > 0
```

if ρ(T) is increasing. So Eq. (1.125) is an increasing function, and its value at T = 0 is −c_p < 0. As T → ∞, it converges to infinity if ρ(T) → ∞ as T → ∞; otherwise it converges to a positive limit if

```
(c_f − c_p) ∫₀^∞ R(t)dt ρ(∞) − c_f > 0
```

Therefore, in these cases there is a unique T̄ > 0 that makes Eq. (1.125) 0, and this is the unique optimum. If the limit of Eq. (1.125) is non-positive at infinity, then there is no finite optimum, and the equipment must be replaced when it fails.

The derivative of Eq. (1.124) has the form

```
− c_p/T² R(T) − c_p/T f(T) + c_f/T f(T)
```

with the same sign as

```
(c_f − c_p)ρ(T) − c_p/T   (1.126)
```

If ρ(T) increases, then this function also increases; at T = 0, its value converges to −∞; and as T → ∞, its limit is positive. So, there is again a unique solution that is the unique optimizer. The solution of monotonic equations can be obtained by standard computer methods (Yakowitz and Szidarovszky 1989).

# Example 1.22  Assume a Weibull distribution with parameters β = 1.5 and η = 5, and c_f = 200 and c_p = 100. We showed earlier that the failure rate of a Weibull distribution is

```
ρ(t) = β/η^β t^{β−1}
```

and the first-order condition from Eq. (1.126) can be written as

```
(c_f − c_p) β/η^β T^{β−1} − c_p/T = 0
```

implying that

```
T = { c_p η^β / [β(c_f − c_p)] }^{1/β}
```

In this case

```
T = { 100(5)^{1.5} / [1.5(100)] }^{1/1.5} ≈ 3.816
```