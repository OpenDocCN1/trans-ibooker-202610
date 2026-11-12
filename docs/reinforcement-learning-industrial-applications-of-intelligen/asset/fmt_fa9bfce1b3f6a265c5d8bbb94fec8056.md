
Step (13) updates a variable that points to the state-action pair that was `n` steps ago. Initially, this will be before `t` = 0, so it checks to prevent index-out-of-bound errors in step (14). If `τ >= 0` then [Algorithm 3-3](#ch03.html#alg_n_step_sarsa) begins to update the action-value functions with the expected return.

Step (15) calculates the reward part of the `n`-step expected return defined in [Equation 3-7](#ch03.html#eq_2_step_expected_return). Step (16) checks to see if the algorithm is past the end of the episode, plus `n`. If not, add the prediction of the next state to the expected return, as defined on the righthand side of [Equation 3-7](#ch03.html#eq_2_step_expected_return).

Step (18) is the same as before; it updates the action-value function using the `n`-step expected value estimate. Finally, step (19) increments the step counter. The algorithm returns to the main loop in step (20) after updating all steps up to termination.

## n-Step Algorithms on Grid Environments

I implemented an `n`-step SARSA agent on the grid environment and set the number of steps to 2 and 4. All other settings are the same as the standard SARSA implementation. You can see the difference between standard and `n`-step SARSA in [Figure 3-7](#ch03.html#fig_n_step_sarsa_comparison).

The one thing that is obvious from [Figure 3-7](#ch03.html#fig_n_step_sarsa_comparison) is that the `n`-step algorithms are capable of learning optimal policies faster than their `1`-step counterparts. This intuitively makes sense. If you are able to observe into the “future” (in quotes because the reality is that you are delaying updates) then you can learn optimal trajectories earlier. The optimal value for `n` depends on the problem and should be treated as a hyper-parameter that requires tuning. The main drawback of these methods is the extra computational and memory burden.

This section focused upon on-policy SARSA. Of course, you can implement off-policy and non-SARSA implementations of `n`-step TD learning, too. You can read more about this in the books listed in [“Further Reading”](#ch03.html#ch03-further-reading).

![](assets/rele_0307.png)

# Eligibility Traces

[“n-Step Algorithms”](#ch03.html#n_step_algorithms) developed a method to provide the benefits of both bootstrapping and forward planning. An extension to this idea is to average over different values of `n`, an average of `2`-step and `4`-step SARSA, for example. You can take this idea to the extreme and average over all values of `n`.

Of course, iterating over different `n`-step updates ([Equation 3-9](#ch03.html#eq_n_step_td_update_rule)) adds a significant amount of computational complexity. Also, as you increase `n` the agent will have to wait for longer and longer before it can start updating the first time step. In the extreme this will be as bad as waiting until the end of the episode before the agent can make any updates (like MC methods).

The problem with the algorithms seen so far is that they attempt to look forward in time, which is impossible, so instead they delay updates and pretend like they are looking forward. One solution is to look backward in time, rather than forward. If you attempt to hike up a mountain in the fastest time possible, you can try several routes, then contemplate your actions to find the best trajectory.

In other words, the agent requires a new mechanism to mark a state-action pair as tainted so that it can come back and update that state with future updates. You can use *tracers* to mark the location of something of interest. Radioactive tracers help to diagnose illnesses and direct medicine. Geophysicists inject them into rock during hydraulic fracturing to provide evidence of the size and location of cracks. You can use a virtual tracer to remind the agent of which state-action estimates need updating in the future.

Next, you need to update state-action function with an average of all the `n`-step returns. A one-step update is the TD error update ([Equation 3-6](#ch03.html#eq_td_action_value_function)). You can approximate an average using an exponentially weighted moving average, which is the online equivalent of an average (as shown in [“Policy Evaluation: The Value Function”](#ch02.html#value-function)). Interested readers can find a proof that this is equivalent to averaging many `n`-step returns and that it converges in [“Further Reading”](#ch03.html#ch03-further-reading).

Combining the one-step update with the moving average, coupled with the idea of a tracer, provides an online SARSA that has zero delay (no direct `n`-step lookahead) but all the benefits of forward planning and bootstrapping. Since you have seen the mathematics before, let me dive into the algorithm in [Algorithm 3-4](#ch03.html#alg_lambda_sarsa).

### Algorithm 3-4. SARSA(λ)

```
\begin{array}{cl} {\text{1:}} & {\mathbf{input: a policy that uses the action-value function, } \pi(a \mid s, Q(s,a)), \text{ step size } 0 < \alpha < 1, \text{ a tracer decay rate } 0 \leq \lambda \leq 1} \\ {\text{2:}} & \left. \text{Initialize } Q(s,a) \leftarrow 0, \text{ for all } s \in \mathcal{S}, a \in \mathcal{A}(s) \right. \\ {\text{3:}} & {\mathbf{loop for each episode:}} \\ {\text{4:}} & \left. \text{Initialize } z(s,a) \leftarrow 0, \text{ for all } s \in \mathcal{S}, a \in \mathcal{A}(s), \text{ and default state } s \right. \\ {\text{5:}} & {\text{Choose } a \text{ from } s \text{ using } \pi, \text{ breaking ties randomly}} \\ {\text{6:}} & {\mathbf{do:}} \\ {\text{7:}} & {\text{Take action, } a, \text{ and observe } r, s'} \\ {\text{8:}} & {\text{Choose } a' \text{ from } s' \text{ using } \pi, \text{ breaking ties randomly}} \\ {\text{10:}} & \left. \delta \leftarrow r + \gamma Q(s',a') - Q(s,a) \right. \\ {\text{11:}} & \left. z(s,a) \leftarrow z(s,a) + 1 \right. \\ {\text{12:}} & {\mathbf{for each } s \in \mathcal{S}:} \\ {\text{13:}} & {\mathbf{for each } a \in \mathcal{A}(s):} \\ {\text{14:}} & \left. Q(s,a) \leftarrow Q(s,a) + \alpha \delta z(s,a) \right. \\ {\text{15:}} & \left. z(s,a) \leftarrow \gamma \lambda z(s,a) \right. \\ {\text{16:}} & \left. s \leftarrow s', a \leftarrow a' \right. \\ {\text{17:}} & {\mathbf{while } s \text{ is not terminal}} \\ \end{array}
```

Most of the code in [Algorithm 3-4](#ch03.html#alg_lambda_sarsa) is repeating [Algorithm 3-2](#ch03.html#alg_sarsa) so I won’t repeat myself. The differences begin in step (10), where it temporarily calculates the one-step TD error for use later. Step (11) implements a tracer by incrementing a cell in a table that represents the current state-action pair. This is how the algorithm “remembers” which states and actions it touched during the trajectory.

Steps (12) and (13) loop over all states and actions to update the action-value function in step (14). Here, the addition is that it weights the TD error by the current tracer value, which exponentially decays in step (15). In other words, if the agent touched the state-action pair a long time ago there is a negligible update. If it touched it in the last time step, there is a large update.
