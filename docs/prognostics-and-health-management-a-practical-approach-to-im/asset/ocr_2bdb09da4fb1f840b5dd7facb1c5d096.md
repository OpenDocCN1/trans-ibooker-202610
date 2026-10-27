# Maximum Likelihood Method

The maximum likelihood method (Ross 1987) is a common procedure to estimate unknown parameters of probability distributions. As in Section 1.4, let f(t|θ) denote the PDF where θ is unknown. Assume we have a random sample t1, t2, ..., tN from this distribution. The likelihood function is defined as

L(θ) = f(t1|θ) f(t2|θ) ... f(tN|θ) (2.16)

which represents the probability of the sampling event that actually occurred. Since the logarithmic function strictly increases, instead of L(θ), its logarithm is maximized:

Maximize ln(L(θ)) (2.17)

and the optional θ value is accepted as the estimate of the unknown parameter.

# Example 2.7 Assume that the distribution is Weibull with a known value of β and an unknown value of η. We discussed in Chapter 1 that

f(t | β, η) = β/η^β t^{β-1} e^{-(t/η)^β}

and therefore

L(η) = β^N / η^{Nβ} (∏_{i=1}^N t_i)^{β-1} e^{-∑_{i=1}^N (t_i)^β / η^β}

so

l(η) = ln(L(η)) = N ln(β) - Nβ ln(η) + (β - 1) ln(∏_{i=1}^N t_i) - η^{-β} ∑_{i=1}^N t_i^β

By differentiation,

∂/∂η l(η) = - Nβ/η + β η^{-β-1} ∑_{i=1}^N t_i^β

So, the first-order condition can be written as

- Nβ/η + β/η^{β+1} ∑_{i=1}^N t_i^β = 0

showing that

η = (1/N ∑_{i=1}^N t_i^β)^{1/β} (2.18)

# Likelihood Ratio Test

The likelihood ratio test (Casella and Berger 2002) is usually used to determine the validity of an estimate. For example, let θ0 be an estimate of an unknown parameter of a PDF f(t|θ). The likelihood ratio test is based on the likelihood ratio

r = L(θ0) / max_θ L(θ) (2.19)