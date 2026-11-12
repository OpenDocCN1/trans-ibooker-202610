
The problem with this is that subsequent samples may experience large differences in the expected return. For example, imagine that a policy is reasonably efficient in the `CartPole` environment. The pole may, due to chance, accidentally fall over early in the episode. This will cause a large difference in the expected value and a large gradient update. Despite the fact that the policy was a good one, the agent forced a large update.

Another example is in the `Pong` environment from [“Example: Rainbow DQN on Atari Games”](#ch04.html#sec_rainbow_dqn_atari). Early episodes result in rewards close to –21. All actions result in a negative reward. Because all options are bad, the gradient update suppresses all actions. A better solution is to look at the change in the reward. The agent should use the relative difference of the actions compared to the average value.

I captured the gradient updates from a single run of the `REINFORCE` and `REINFORCE with baseline` algorithms. You can see the standard deviation of the gradients in [Figure 5-4](#ch05.html#fig_cartpole_baseline_gradient). Notice that after a period of learning, the standard deviation of the baseline algorithm drops dramatically, whereas the `REINFORCE` algorithm continues to produce large gradients that alter the policy, even after obtaining decent rewards. When the literature talks about “variance reduction,” this is what they mean: methods to improve the stability of the gradients without affecting learning performance.

![](assets/rele_0504.png)

## n-Step Actor-Critic and Advantage Actor-Critic (A2C)

The next improvement follows the path taken with tabular methods. `Monte Carlo` agents do not bootstrap, which means they have to wait until the episode is over before they can perform any learning. As in [Chapter 3](#ch03.html#td_nstep_qlearning), the solution is to work through temporal-difference methods up to `n`-step methods. However, policy gradient-based `n`-step algorithms have an interesting interpretation that you have not seen before.

Before I get there, let me describe the `n`-step implementation because there is one major change over [Algorithm 5-2](#ch05.html#alg_reinforce_baseline). In step (5) of [Algorithm 5-3](#ch05.html#alg_n_step_ac), the algorithm performs an update for every time step. This is no longer a `Monte Carlo` algorithm. Bootstrapping (see [“n-Step Algorithms”](#ch03.html#n_step_algorithms)) improves learning speed and step (8) implements the bootstrapping, to provide one-step updates `G[t:t+1] ≐ r + γV(s',w)`, like in [Equation 3-3](#ch03.html#eq_td_state_value_function).

### Algorithm 5-3: n-step actor-critic

```
\begin{array}{cl} {\text{1:}} & {\text{input: a differentiable parameterized policy } \pi(a \mid s,\theta),} \\ & {\text{a differentiable parameterized state-value function } V(s,w),} \\ & {\text{step sizes } \alpha_w \text{ and } \alpha_\theta > 0} \\ {\text{2:}} & {\text{Initialize } \theta \text{ and } w} \\ {\text{3:}} & {\text{loop for each episode:}} \\ {\text{4:}} & {\text{Initialize } I\leftarrow 1, t\leftarrow 0, \text{ and } s} \\ {\text{5:}} & {\text{while } s \text{ is not terminal:}} \\ {\text{6:}} & {\text{Choose } a \text{ from } s \text{ using } \pi, \text{ breaking ties randomly}} \\ {\text{7:}} & {\text{Take action } a, \text{ observe } s', r} \\ {\text{8:}} & {\delta \leftarrow G_{t:t+n} - V(s,\theta)} \\ {\text{9:}} & {w \leftarrow w + \alpha_w \delta \nabla V(s,w)} \\ {\text{10:}} & {\theta \leftarrow \theta + \alpha_\theta I \delta \nabla \ln \pi(a \mid s,\theta)} \\ {\text{11:}} & {I \leftarrow \gamma I} \\ {\text{12:}} & {s \leftarrow s'} \\ \end{array}
```

[Algorithm 5-3](#ch05.html#alg_n_step_ac) is an incremental improvement over [Algorithm 5-2](#ch05.html#alg_reinforce_baseline) that includes the temporal-difference return. But this algorithm is more than it seems. You have now derived an algorithm that can learn both a policy and its state-value function on every single step, as opposed to learning a baseline over entire trajectories (the `Monte Carlo`-inspired baseline algorithm).

This distinction is important. The policy learns and describes the best action to take. In RL parlance, this is known as the *actor*. The `n`-step baseline prediction criticizes the consequences of the action and is used to update the policy. This is the *critic*. Together they form the basis of *actor-critic* algorithms. Note that `Monte Carlo` algorithms are not considered to be actor-critic because they are not updated on every step.

This is more commonly called the advantage actor-critic (`A2C`) algorithm. I presented the algorithm in this way because you are familiar with development from [Chapter 3](#ch03.html#td_nstep_qlearning). But another way of formulating the problem is to consider the critic as a measure of the benefit of choosing an action, relative to a baseline. This is the exact definition of the advantage function from [Equation 4-4](#ch04.html#eq_advantage_function).

In the context of policy gradient algorithms, the only difference is that the agent follows the current optimal policy by definition, so you don’t need to calculate the value of each action. You only need the value for the states that the policy is going to visit. An approximation (which is proven in the reference) is presented in [Equation 5-10](#ch05.html#eq_advantage_function_approximation).^([2](#ch05.html#idm46428617079864))

### Equation 5-10: Advantage function approximation for policy gradient algorithms

```
\begin{array}{clr} {A^{\pi}(s,a)} & {\doteq Q(s,a) - V(s)} & \\ & {= r + Q(s',a') - V(s)} & {\text{(temporal-difference error)}} \\ & {\approx r + V_{\pi}(s') - V(s)} & {\text{(when the agent is following the optimal policy } \pi \text{)}} \\ \end{array}
```

It is possible to estimate the advantage function or the action-value function directly, but most implementations use the state-value version to reduce the number of models you need to train.

Another common question is the need for `I` in step (11) of [Algorithm 5-3](#ch05.html#alg_n_step_ac). When including discounting (see [“Discounted Rewards”](#ch02.html#discounted_rewards)) in the policy gradient theorem ([Equation 5-3](#ch05.html#eq_gradient_of_a_policy)), `γ` is a constant on the outside of the gradient calculation. But the discounting depends on the step number and in an online algorithm like this it makes sense to implement it in this way, although you could simply replace it with a `γ^t` if you like.

### Example: n-step actor-critic on the CartPole environment

The goal of this example is to demonstrate the differences between `Monte Carlo` (see [“REINFORCE with Baseline”](#ch05.html#sec_reinforce_with_baseline)) and temporal-difference policy gradient implementations produced by the use of bootstrapping.
