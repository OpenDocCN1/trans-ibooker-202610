
The final way to represent the transition probabilities is with a matrix for each action, known as the `transition matrix`. You can construct the matrix in the same way as in [Table 2-1](#ch02.html#tab_simple_transition).

$$
\begin{aligned}
{p\left( {s^{'},a = \text{none}} \right)} & {= \begin{bmatrix} 1 & 0 & 0 \\ {p\left( \text{sale} \right)} & {1 - p\left( \text{sale} \right)} & 0 \\ 0 & {p\left( \text{sale} \right)} & {1 - p\left( \text{sale} \right)} \\ \end{bmatrix}} \\
{p\left( {s^{'},a = \text{restock}} \right)} & {= \begin{bmatrix} {p\left( \text{sale} \right)} & {1 - p\left( \text{sale} \right)} & 0 \\ 0 & {p\left( \text{sale} \right)} & {1 - p\left( \text{sale} \right)} \\ 0 & 0 & 1 \\ \end{bmatrix}}
\end{aligned}
$$

Each cell represents the probability of transitioning from the current state (0, 1, or 2 in rows) into the new state (0, 1, or 2 in columns). Note that each row must sum to 1. For example, if the agent chooses not to restock, action `a = none`, you use the top transition matrix. If the shop has one item in stock, so the state is 1 (the second row), you can see that there is a probability of `p(sale)` of transitioning into state 0, you’ve sold the item, and `1 - p(sale)` of staying in the same state after no sale. Also note that you need a separate function or matrix for the obtained rewards as well.

> **Tip**
>
> I prefer the transition graph because it allows you to trace the trajectory taken by the agent with your finger. But you can only visualize the graph up to a small number of states and actions. Computational capacity is the only thing that limits the transition matrix, but it is difficult to interpret by eye.

## Inventory Control Simulation

The previous sections presented all the ingredients required to build a simulation (the environment). But what about the agent? When should you restock?

You already placed a constraint that you cannot restock if the stockroom is full. That is one hard rule. But after that, what is the best action to take? If you look purely at the rewards, which represent the goal, then there is only a positive reward on making sales. Crucially, there is no negative reward for placing orders. In other words, according to the reward matrix, it doesn’t cost anything to keep stock and it is free to order new stock. Given this reward structure, the best strategy is to keep ordering as much as you can.

Even though the strategy is obvious in this case, you should always test your idea—the scientific method. It is always a good idea to compare your theory against other baselines.

[Figure 2-6](#ch02.html#fig_simple_inventory_policy_comparison) compares three different restocking strategies, or `policies`. Policies tell the agent which actions to take and in general the best ones maximize the total reward. The policies are keep restocking as much as possible, restock only when the stock levels drop to zero, or restock at random. The lines represent the cumulative rewards for each of the policies. The proportion of sales made is presented in brackets in the legend.

It is clear in this case that restocking at random is not a good idea, because quite often people wanted to buy but there was no stock. This was because the probability of a sale (0.7) was greater than the probability of restocking (0.5). The constant-restock policy managed to keep up with demand because you reordered at each time step. So the stock level (the state) bounced between 2 and 1. The reorder when zero stock policy bounced around the 0–1 level, because it only restocked when it hit zero. Effectively there is no performance difference between these two, but note that you might consider the keep buying policy to be wasteful, because you are using more space than you need, or safe, in case there’s a rush. It depends on your high-level perspective and goal.

![](assets/rele_0206.png)

> **Tip**
>
> This is an interesting example of how exactly the same data can mean different things to different people. Your boss might hate the waste, or they might love the buffer. Neither position is wrong.

You have just derived your first set of policies. And I haven’t yet discussed how to learn an optimal policy—that will take the rest of the book. But this is an important result to take in; an optimal policy is dependent on all components of an MDP.

Changing the environment to include more stock, or allowing people to buy more than one item at a time, affects the best restocking strategy. Altering the reward function by introducing a penalty for ordering—a charge for delivery, for example—also alters the best strategy. In these cases it might be better to wait until you have zero stock then order a full 2 units worth of product. The reward function defines what problem you want solved, not how to achieve it.

Standard MDPs require the state to be fully observable. If the agent cannot observe the true state of the system, then how can it pick an optimal action? There is an extension to the MDP model called partially observable MDPs (POMDPs). Here, you can introduce an extra mapping between the observations and the actual state. In other words, the agent would need to learn how to map actions to observations that are mapped to states that it cannot observe, a double mapping. As you can imagine, this is tricky to solve, because of more unknowns. In many applications, engineers trick the system back into a standard MDP by feeding more information from previous time steps into the current time step. Knowledge of the past can help the agent figure out how the actions altered the perceived states over a longer period of time.

I also want to stress that the important thing here is the observation of the state, not the environment. Agents do not concern themselves about what is happening behind the MDP interface. You can alter the simulation as much as you want, so long as you do not change the interface. In the inventory example, if you altered the customer’s buying behavior, it only affects the optimal policy. It does not change `how` the agent learns.

Bear these points in mind when you read the rest of this book.

# Policies and Value Functions

A policy is like a strategy. For example, you can’t be entirely sure what the other football team is going to do (the other team’s activities are stochastic), but you can approximate their actions and tell your players to act accordingly. A policy is a mapping from states to potential actions. Precisely how you do that mapping is the major difference between a large number of RL algorithms. But why should you build a mapping in the first place? And how can you evaluate different policies?

## Discounted Rewards

In many problems, including the inventory and website button examples, the reward at each step is straightforward to reason about. At each time step the agent may or may not receive a reward. Through multiple steps (in the inventory example), the agent may receive multiple rewards. In general the challenge is to maximize the total reward that you expect to receive.

The `return`, `G`, is the total reward from this current step up to the final time step (or infinity). The first dot-equals sign can be read as “I define G to be.” Note that the agent receives the reward after transitioning into the next step. Most practical implementations return the state and the reward at the same time, so they set the initial reward to zero.
