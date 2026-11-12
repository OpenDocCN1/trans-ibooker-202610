
$$
\begin{aligned}
{\operatorname{maximize}\limits_{\theta} L^{TRPO}\left( \theta \right) = {\mathbb{E}}_{\pi_{\theta}}} & \left\lbrack {\frac{\pi_{\theta}\left( a \mid s \right)}{\pi_{\theta_{\text{old}}}\left( a \mid s \right)}A_{w}\left( s,a \right)} \right\rbrack \\
{\operatorname{subject}\operatorname{to}{\mathbb{E}}_{\pi_{\theta}}} & {\left\lbrack {\text{KL}\left\lbrack {\pi_{\theta_{\text{old}}}{\left( \cdot \mid s \right) \parallel}\pi_{\theta}\left( \cdot \mid s \right)} \right\rbrack} \right\rbrack \leq \delta}
\end{aligned}
$$

This is all well and good, but the major problem is that you can’t use stochastic gradient descent to optimize constrained nonlinear loss functions. Instead, you have to use conjugate gradient descent, which is similar to standard gradient descent but you can include a constraint.

`TRPO` adds a few bells and whistles like an extra linear projection to double-check that the step really does improve the objective.

The major problem, however, is that the implementation involves a complex calculation to find the conjugate gradients, which makes the algorithm hard to understand and computationally complex. There are other issues too, like the fact that you can’t use a neural network with a multihead structure, like when you predict action probabilities and a state-value estimate, because the `KL` divergence doesn’t tell you anything about how you should update the state-value prediction. In practice, then, it doesn’t work well for problems where good estimates of a complex state-value space are important, like the Atari games.

These issues prevent `TRPO` from being popular. You can read more about the derivation of `NPG` and `TRPO` in the resources suggested in [“Further Reading”](#ch06.html#sec_ch06_further_reading). Thankfully, there is a much simpler alternative.

## Proximal Policy Optimization

One of the main problems with `NPG` and `TRPO` is the use of conjugate gradient descent. Recall that the goal is to prevent large changes in the policy, or in other words, to prevent large changes in the action probability distribution. `NPG` and `TRPO` use a constraint on the optimization to achieve this, but another way is to penalize large steps.

[Equation 6-8](#ch06.html#eq_a2c_objective) is a maximization; it’s trying to promote the largest advantage estimates, so you could add a negative term inside the loss function that arbitrarily decreases the advantage value whenever policies change too much. [Equation 6-10](#ch06.html#eq_ppo_penalized) shows this in action, where the constraint is moved to a negation and an extra term, `β`, controls the proportion of penalization applied to the advantages.

The major benefit is that you can use standard gradient ascent optimization algorithms again, which dramatically simplifies the implementation. However, you would struggle to pick a value for `β` that performs consistently well. How big of a penalization should you apply? And should that change depending on what the agent is doing? These are the same problems presented at the start of this section, which makes this implementation no better than picking a step size for `A2C`.

#### Equation 6-10. Penalizing large changes in policy

$$
\begin{aligned}
{\operatorname{maximize}\limits_{\theta}{\mathbb{E}}_{\pi_{\theta}}} & \left\lbrack {\frac{\pi_{\theta}\left( a \mid s \right)}{\pi_{\theta_{\text{old}}}\left( a \mid s \right)}A_{w}\left( s,a \right) - \beta\text{KL}\left\lbrack {\pi_{\theta_{\text{old}}}{\left( \cdot \mid s \right) \parallel}\pi_{\theta}\left( \cdot \mid s \right)} \right\rbrack} \right\rbrack \\
\end{aligned}
$$

But do not despair; [Equation 6-10](#ch06.html#eq_ppo_penalized) highlights an important idea. It shows that you can add arbitrary functions to influence the size and direction of the step in an optimization algorithm. This opens a Pandora’s box of possibilities, where you could imagine adding things like (simpler) functions to prevent large steps sizes, functions to increase step sizes when you want more exploration, and even changes in direction to attempt to explore different regions in policy space. Proximal policy optimization (`PPO`) is one such implementation that adds a simpler penalization, an integrated function to allow for multihead networks, and an entropy-based addition to improve exploration.^([20](#ch06.html#idm46428616551224))

### PPO’s clipped objective

Schulman et al. proposed that although `TRPO` was technically more correct, it wasn’t necessary to be so fancy. You can use the importance sampling ratio as an estimate of the size of the change in the policy. Values not close to 1 are indicative that the agent wants to make a big change to the policy. A simple solution is to `clip` this value, which constrains the policy to change by a small amount. This results in the a new clipped loss function shown in [Equation 6-11](#ch06.html#eq_ppo_clip).

#### Equation 6-11. Clipping large changes in policy according to the importance sampling ratio

```
L^CLIP(θ) ≐ 𝔼[min(r(θ)A(s,a), clip(r(θ), 1-ϵ, 1+ϵ) â(s,a))]
r(θ) ≐ π[θ](a|s) / π[θ_old](a|s)
```

is the importance sampling ratio. Inside the expectation operator there is a `min` function. The righthand side of `min` contains an importance sampling ratio that is clipped whenever the value veers more than `ϵ` away from 1. The lefthand side of the `min` function is the normal unclipped objective. The reason for using `min` is so that the objective results in providing the worst possible loss for these parameters, which is called a *lower bound* or *pessimistic bound*.

At first this sounds counterintuitive. In situations like this I find it useful to imagine what happens when you test the function with different inputs. Remember that the advantage function can have positive and negative values. [Figure 6-5](#ch06.html#fig_ppo_clipped_advantage) shows the clipped loss when the advantages are positive (left) and negative (right).

![](assets/rele_0605.png)

A positive advantage (left) indicates that the policy has a better than expected effect on the outcome. Positive loss values will nudge the policy parameters toward these actions. As the importance sampling ratio `r(θ)` increases, the righthand side of the `min` function will clip at the 1 + `ϵ` point and produce the lowest loss. This has the effect of limiting maximum loss value and therefore the maximum change in parameters. You don’t want to change parameters too quickly when improving, because a jump too big might push you back into poor performance. Most of the time the ratio will be near 1. The `ϵ` allows a little room to improve.

A negative advantage suggests that the policy has a worse than expected effect on the outcome. Negative loss values will push the policy parameters away from these actions. At high ratios, the lefthand side of the `min` function will dominate. Large negative advantages will push the agent back toward more promising actions. At low ratios, the righthand side of the `min` function dominates again and is clipped at 1 − `ϵ`. In other words, if the distribution has a low ratio (meaning there is a significant difference in the old and new policies), then the policy parameters will continue to change at the same rate.

But why? Why do this at all? Why not just use the raw advantage? Schulman et al. set out to establish and improve a lower bound on performance, a worst-case scenario. If you can design a loss function that guarantees improvement (they did) then you don’t need to care so much about the step size.
