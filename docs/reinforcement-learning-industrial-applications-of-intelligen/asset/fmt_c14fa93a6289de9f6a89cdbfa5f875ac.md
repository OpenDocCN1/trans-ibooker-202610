
The gradient has an intuitive explanation. [Equation 5-7](#ch05.html#eq_logistic_policy_gradient_two_actions) is the difference between the observed state and the state expected by the policy.

## Softmax Policy

You can think of the softmax function as a multiclass version of the logistic function. This means you can use this to represent the probability of choosing from multiple actions. A softmax policy is defined in [Equation 5-8](#ch05.html#eq_softmax_policy).

### Equation 5-8. Softmax Policy

```
\pi_{\theta}\left( a \mid s \right) \doteq \frac{e^{\theta^{\intercal}s}}{\sum_{a}e^{\theta_{a}^{\intercal}s}}
```

In other words, [Equation 5-8](#ch05.html#eq_softmax_policy) is a logistic-like function normalized over all possible actions to ensure that all actions sum to one. The corresponding gradient is shown in [Equation 5-9](#ch05.html#eq_softmax_policy_gradient) and you can see the full derivation in [Appendix B](#app02.html#ch_derivative_of_softmax_policy).

### Equation 5-9. Softmax Policy Gradient

```
\nabla\ln\pi\left( a \mid s,\theta \right) = s - \sum\limits_{a}s\pi\left( \theta^{\intercal}s \right)
```

Like [“Logistic policy”](#ch05.html#sec_logistic_policy), the gradient shown in [Equation 5-9](#ch05.html#eq_softmax_policy_gradient) works out as the difference between the observed and expected feature vectors.

## Arbitrary Policies

Computing the derivatives of policies by hand has allowed me to show you precisely how I can improve a policy via iteration. But in general, this process is both tedious and obfuscating. Instead, you could compute a derivative in the following three ways: finite, automatic, and symbolic.

Finite differences calculate the difference in a policy over a small step in the parameter space, `∇f(x) = (f(x+h)−f(x))/2h`. This is simple but prone to numerical error because of the step size, `h`.

Automatic differentiation tools, like `JAX` or the back-propagation methods in `TensorFlow`/`PyTorch`/`MXNet`/others, attempt to track the usage of a parameter in an equation to build a directed acyclic graph (`DAG`). The `DAG` libraries map each step to the appropriate derivative and combine them through the chain rule.

Symbolic differentiation, using tools like `SymPy`, is effectively the same as automatic differentiation except it generally works in one direction (forward). Symbolic differentiation tools also place more emphasis on refactoring, which can result in derivatives that are easier to understand, as opposed to automatic methods that can result in chains of chains of chains, and so on.

In other words, if you can leverage a library, I recommend that you don’t do the differentiation by hand. Use a library that suits your use case.

# Basic Implementations

“Policy Functions” showed you how to provide the gradient function required by [Equation 5-4](#ch05.html#eq_policy_gradient_update_rule). The remaining detail is the use of `G`. Recall that this is the total reward from a point up until the end of an episode, if episodic, or forever more, if not (see [“Discounted Rewards”](#ch02.html#discounted_rewards)).

Chapters [2](#ch02.html#ch_mdp-dp-mc)–[4](#ch04.html#ch_deep_q_networks) show three different ways of calculating expected return, from Monte Carlo, full trajectory methods through to approximation, multistep projection methods. These implementation details are the only difference between several basic policy gradient implementations, including the `REINFORCE` and `actor-critic` algorithms.

## Monte Carlo (REINFORCE)

The first algorithm uses a Monte Carlo approach. It first collects a trajectory, based upon the current policy, for an entire episode. It then updates the policy via [Equation 5-4](#ch05.html#eq_policy_gradient_update_rule).

[Algorithm 5-1](#ch05.html#alg_reinforce) shows the `REINFORCE` algorithm. In step (6), `G` refers to the discounted reward from a time `t` up until the end of the episode, like in [Algorithm 2-3](#ch02.html#alg_on-policy-mc-agent). In step (7), I use the value of `G` to weight the differential of the policy. The differential calculates the direction in which you need to nudge the probability of choosing an action, defined by `θ`. The collective weighting of `αγ^t G` defines how much you need to nudge it by. I also reintroduce discounting in step (7), which is pulled out of the derivative because it does not depend on `θ`.

### Algorithm 5-1. REINFORCE Algorithm

```
\begin{aligned} {\text{1:}} & {\mathbf{i}\mathbf{n}\mathbf{p}\mathbf{u}\mathbf{t}\text{:}\text{a}\text{differentiable}\text{parameterized}\text{policy}\pi\left( a \mid s,\theta \right)\text{,}\text{step}\text{size}\alpha > 0} \\ {\text{2:}} & {\text{Initialize}\theta\text{arbitrarily}} \\ {\text{3:}} & {\mathbf{l}\mathbf{o}\mathbf{o}\mathbf{p}\text{for}\text{each}\text{episode:}} \\ {\text{4:}} & {\text{Generate}\text{trajectory}\text{following}\pi\left( \cdot \mid \cdot ,\theta \right)} \\ {\text{5:}} & {\mathbf{f}\mathbf{o}\mathbf{r}\text{each}\text{step}\text{in}\text{episode} t\leftarrow 0,1,\cdots,T\text{:}} \\ {\text{6:}} & \left. G\leftarrow\sum\limits_{k = t + 1}^{T}\gamma^{k - t - 1}r_{k} \right. \\ {\text{7:}} & \left. \theta\leftarrow\theta + \alpha\gamma^{t}G\nabla\pi\left( a \mid s,\theta \right) \right. \\ {\text{8:}} & {\mathbf{e}\mathbf{n}\mathbf{d}\mathbf{f}\mathbf{o}\mathbf{r}} \\ \end{aligned}
```

### Example: REINFORCE on the CartPole Environment

The beauty of using policy gradients is demonstrated by the low dimensionality of the policy. The `CartPole` environment has four features. The linear policy has four parameters that weight the importance of each of the features. The result is a probabilistic representation of the best course of action. If the angle of the pole is close to falling over, then the probability of choosing an action to attempt to correct that is very high. The rewards obtained during the training process are shown in [Figure 5-1](#ch05.html#fig_cartpole_reinforce_rewards). The policy should look far into the future to obtain good rewards, so I set `γ` ≐ 0.99. I chose `α` ≐ 0.01 because at higher values the learning was unstable.

Compare this result to the tabular implementation in [“Example: DQN on the CartPole Environment”](#ch04.html#sec_cartpole_dqn). There, I had to use a neural network to approximate the state space. Here, I have a linear mapping using just four parameters. The elegance, the sheer utility, is incredible.

![](assets/rele_0501.png)

## REINFORCE with Baseline

A more common implementation incorporates a baseline expected return, like the advantage function seen in [“Dueling Networks”](#ch04.html#sec_dueling_networks) (see [Equation 4-4](#ch04.html#eq_advantage_function)). This helps to improve the instability of the gradients due to large changes in the average reward.
