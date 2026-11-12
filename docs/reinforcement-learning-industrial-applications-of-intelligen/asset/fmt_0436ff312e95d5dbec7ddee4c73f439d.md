
# Instacart Shopping Cart Environment

Instacart [open sourced three million orders](https://oreil.ly/-g1ix) from its shopping platform. I performed a significant amount of data cleaning that resulted in a state consisting of `one-hot` encoded day of the week and hour of the day. It also includes the number of days since the last order normalized to a value of 1. The actions are a list of 50,000 `one-hot`, binary encoded products. To order a product you set the product's column to 1. I keep the reward simple. You will receive a reward of +1 for every product that matches what the customer actually ordered. You will receive a reward –1 for every incorrectly ordered product. The goal is to order what the customer desires and nothing else. Each order is a step and all orders for a single customer is an epoch.

This is a simplification over what happens in the real world. The actual rewards are unlikely to be as simple as +/– 1. Offering a new product to a customer might cause them to buy more in the future, whereas a mistake could cost a lot if the products are perishable and can't be resold. I am limited by the features offered in the dataset. Other features are likely to be more informative. For example, Stitch Fix has a library of your preferences. You should build a process to gather user preferences about what food they like.

But the greatest issue is that the simulation uses static data. Ideally the agent should explore which products the customer desires. The customer might not even know it yet. But with fixed data, you are optimizing to solve a static solution. In other words, you can use standard machine learning techniques. Actionable exploration is at the heart of `RL` and in this example there is none. `RL` is more than just prediction.

One potential solution to this is to model customer purchases. For example, you can build a `generative adversarial network` to model what customers purchase in a simulated, but dynamic environment. Then you could transfer the learned policy to a live environment (see [“Further Reading”](#ch05.html#sec_ch05-further-reading)). For the sake of simplicity, I use the static data.

## Expectations

The state and actions in this environment are binary, so you could use a tabular technique like `Q-learning`. But to continue the thread of this chapter I will use the `eligibility actor-critic policy gradient algorithm` (see [“Eligibility Traces Actor-Critic”](#ch05.html#sec_eligibility_actor_critic)). The agent must generate the same number of binary actions as products. I use a `linear policy` with a `logistic function` for every action. Note that `softmax` is not appropriate because that normalizes all actions to have a total value of 1. I also model the `value function` as a `linear function`. Both are for the sake of simplicity. You could use more complex models.

Let me stop for a moment to set some expectations. The propensity to buy a product is a function of the customer's profile. You can imagine that if the customer had babies then they might be more likely to buy baby products. If they are vegan then they are unlikely to order meat. If they are young they will probably buy different products compared to someone who is old. All of these are latent features. They are not directly observable from the actions that they take. It is for this reason that it is unlikely that the linear model will perform very well in a real-life test. The linear model will learn how often customers buy products, ignoring *why* they buy them.

### Tip

Simplify the problem by reducing the number of latent features. Create explicit features.

In a real application then, it will pay dividends if you invest in building a more sophisticated model that represents the customers, not just the products. Given enough data, you should also look at deep learning to represent these higher-level features.

Another issue is that the linear model has no memory. When I bulk buy washing powder, I rarely buy more in the next shop. The exception is when the kids decide to build a bed-sheet den in the mud. But the linear weights see that I bought lots of washing powder and probably offer more. To learn that people buy products at different rates you have to introduce some memory. One of the simplest ways to achieve this is to include the previous orders in the state. This is how `DQN` works so well on the Atari environment, for example. The researchers found that four frames worth of pixels were enough to provide a proxy for memory. More advanced solutions include using stateful neural networks like `recurrent` and `long short-term memory` neurons (see [“Common Neural Network Architectures”](#ch04.html#sec_nn_architectures)).

## Results from the Shopping Cart Environment

Again, for the sake of simplicity, I stick with a linear implementation. To help with the scale of the problem—50,000 actions—I allow the agent to buy only the most popular products.

You can see the first results in [Figure 5-9](#ch05.html#fig_simple_shopping_single_customer). In this initial experiment I restricted the dataset to a single customer. I chose to keep the 15 most popular products for this customer and trained the agent over 50 epochs. You can also see an agent that randomly orders products, for reference. In this example the customer had 358 purchases of popular products over 71 orders. Hence the highest reward possible is 358.

![](assets/rele_0509.png)

The highest reward for the agent was 57, indicating that the agent was buying products that the customer actually wanted. But this is much lower than the highest possible reward of 358 products. The random agent performs consistently poorly because it repeatedly orders products the customer does not want.

In [Figure 5-10](#ch05.html#fig_simple_shopping_random_customers) I repeat the same experiment but this time with 50 random customers that have different numbers of orders. In other words, the agent cannot overfit a single customer. It must attempt to generalize to all customers. This time the agent achieves a maximum reward of –1. A little underwhelming compared to the previous experiment. This is saying that the agent is capable of just about breaking even for any given random customer. But you must remember how simple the agent is. The agent has only a handful of linear parameters and it is, to a limited extent, able to capture the complexity of the shopping process. I find this remarkable.

[Figure 5-11](#ch05.html#fig_simple_shopping_number_of_products) shows the performance of the same agent over random customers when altering the number of products that the agent can choose from. In other words, I am altering the number of actions. When the number of products increases the agent receives less reward. However, it does appear that performance is asymptotic. Performance may converge with more training time.

![](assets/rele_0510.png)![](assets/rele_0511.png)

Finally, I want to demonstrate something called `transfer learning`. The goal is to train a model to solve one problem then apply the same model to a different, but related problem. This paradigm works really well with `RL` because you can potentially learn from offline data or models and fine-tune using online data.

For example, I can use the shopping cart model trained upon random customers ([Figure 5-10](#ch05.html#fig_simple_shopping_random_customers)) then transfer the learned model to a new customer. [Figure 5-12](#ch05.html#fig_simple_shopping_transfer_learning) shows the results of such an experiment.

![](assets/rele_0512.png)
