
Trust region methods attempt to solve this problem by attempting to specify a region in parameter space which gradient steps can be trusted. You can use this information to dynamically alter the step size so that the parameters change quickly when gradients are trusted and slowly when they are not. Then you can find ways to quantify that trust, the first of which is based upon using the Kullback–Leibler divergence to measure the difference between two policies.

## Kullback–Leibler Divergence

The Kullback–Leibler (KL) divergence is often described as a measure of “surprise” when comparing two distributions. When the distributions are the same, then the KL divergence is zero. When the distributions are dramatically different, the KL divergence is large.

It can also be used to calculate the extra number of bits required to describe a new distribution given another. If the distributions are the same, then you require no extra bits to differentiate the new distribution. If the distributions are different, then you would need many new bits.

For example, if you had a special coin that only ever landed on heads, then you would need zero bits to represent the coin’s states. There is only one state—it is always going to be heads—so you need zero bits, just the value zero, to encode that state. If you compared that to a fair coin, you would now have two states, so you would need one full bit to represent those states. In this hypothetical example, the KL divergence between these two distributions is 1 (hypothetical because as one state approaches zero, the division in the KL divergence algorithm will produce a `nan`).

[Equation 6-6](#ch06.html#eq_kl_divergence) presents the divergence calculation. ∥ represents the divergence operator, which tells you to calculate the divergence between this and that. You can use any base for the logarithm, but most people use a base of 2 (`log2`), which represents the number of bits.

### Equation 6-6. Kullback–Leibler Divergence

```
D_{KL}\left( P\left. \parallel Q \right) \doteq \right.\sum\limits_{x \in \mathcal{X}}P\left( x \right)\log\left( \frac{P\left( x \right)}{Q\left( x \right)} \right)
```

### KL Divergence Experiments

Consider the following two experiments where I flip coins and roll dice. KL divergence is a measure of how different distributions are, so flipping two fair dice should result in a divergence of zero. Then I simulate a biased coin that is weighted toward landing on tails.

In the second experiment I simulate three six-sided dice. The first is a standard, fair die. The second is a fair die but produces double the value of a standard six-sided die. In other words, the probabilities are similar but the distributions do not align. The third die produces standard values but is biased to produce more sixes than you would expect.

[Figure 6-3](#ch06.html#fig_kl_divergence_coins_dice) shows histograms of these experiments where the KL divergence is shown in brackets. As you would expect, the KL divergence between two fair coins is near zero. However, when comparing with the biased coin, the divergence is nonzero.

Comparing fair and biased dice produces the same nonzero result. But note the result from the comparison between the fair die and the die that produces values that lie outside the range of the fair die. The KL divergence is near zero. This is because where it does overlap, it has the same uniform distribution. This is because KL divergence does not measure the spread between distributions; if you’re interested in that, use the total variance divergence instead.

![](assets/rele_0603.png)

## Natural Policy Gradients and Trust Region Policy Optimization

Natural policy gradients (NPG) introduced the idea that you can quantify step size not in terms of policy parameter space, but in terms of a metric based upon the distance between the current policy and the policy after updating by a gradient step.^([17](#ch06.html#idm46428616599256))

[Figure 6-4](#ch06.html#fig_natural_policy_gradient) shows an example of the difference between standard policy gradients and natural policy gradients. The contours represent the value of a particular state and the arrows show the direction of the gradient as defined by the type of gradient calculation. It demonstrates that the gradients change direction when you compute the differential with respect to a different feature.

You can estimate the distance between two policies by comparing the distributions of the generated trajectories using one of many statistical measures to compare two distributions. But the trust region policy optimization (TRPO) algorithm uses KL divergence.^([18](#ch06.html#idm46428616595240)) Recall that optimization algorithms measure step sizes by the Euclidian distance in parameter space; steps have units of `δθ`. TRPO suggests that you can constrain, or limit this step size according to the change in policy. You can tell the optimization algorithm that you don’t want the probability distribution to change by a large amount. Remember, this is possible because policies are action-probability distributions.

![](assets/rele_0604.png)

[Equation 6-7](#ch06.html#eq_reformulated_a2c) presents a slight reformulation of the A2C policy gradient. If you look at the derivation of the policy gradient theorem in [Equation 5-3](#ch05.html#eq_gradient_of_a_policy), at the end I introduced a logarithm to simplify the mathematics. Technically, this gradient is computed using trajectories sampled from an old policy. So if you undo the last step (chain rule), you end up with the same equation, but the use of the old policy parameters is more explicit.

### Equation 6-7. Reformulated Advantage Actor-Critic Loss Function

```
\begin{array}{cl} {\nabla J\left( \theta \right)} & {= {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla\ln\pi_{\theta}\left( a \mid s \right)A_{w}\left( s,a \right)} \right\rbrack} \\ & {= {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\frac{\nabla\pi_{\theta}\left( a \mid s \right)}{\pi_{\theta_{\text{old}}}\left( a \mid s \right)}A_{w}\left( s,a \right)} \right\rbrack} \\ \end{array}
```

Next, because you can delegate the gradient calculation, you can rewrite [Equation 6-7](#ch06.html#eq_reformulated_a2c) in terms of the objective, which is to maximize the expected advantages by altering the policy parameters, `θ`. You can see the result in [Equation 6-8](#ch06.html#eq_a2c_objective).

### Equation 6-8. Presenting A2C in Terms of Maximization of the Objective

```
\begin{aligned} {\operatorname{maximize}\limits_{\theta} L^{PG}\left( \theta \right)} & {= {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\frac{\pi_{\theta}\left( a \mid s \right)}{\pi_{\theta_{\text{old}}}\left( a \mid s \right)}A_{w}\left( s,a \right)} \right\rbrack} \\ \end{aligned}
```

An alternative interpretation of [Equation 6-8](#ch06.html#eq_a2c_objective) is that this is importance sampling of the advantages (not the gradient, like in [“Off-Policy Actor-Critics”](#ch06.html#sec_off_pac)). The expected advantage is equal to the advantage of the trajectories sampled by the old policy, adjusted by importance sampling, to yield the results that would be obtained if you used the new policy.

[Equation 6-9](#ch06.html#eq_npg) shows the loss function for the NPG algorithm, which as you can see is the same as [Equation 6-8](#ch06.html#eq_a2c_objective), but with a *constraint* (a mathematical tool that limits the value of the equation above it). NPG uses a constraint based upon the KL divergence of the new and old policies. This constraint prevents the divergence from being greater than `δ`.

### Equation 6-9. Natural Policy Gradients Optimization Loss Function
