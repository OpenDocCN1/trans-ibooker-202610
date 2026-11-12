
## Eligibility Trace Actor-Critic Algorithm

```latex
\begin{array}{cl} 
{\text{1:}} & {\mathbf{i}\mathbf{n}\mathbf{p}\mathbf{u}\mathbf{t}\text{:}\text{a}\text{differentiable}\text{parameterized}\text{policy}\pi\left( a \mid s,\theta \right),} \\ 
& {\text{a}\text{differentiable}\text{parameterized}\text{state-value}\text{function} V\left( s,w \right),} \\ 
& {\text{step}\text{sizes} 0 < \alpha_{w},\alpha_{\theta} < 1,\text{and}\text{trace-decay}\text{rates} 0 \leq \lambda_{\theta},\lambda_{w} \leq 1} \\ 
{\text{2:}} & {\text{Initialize}\theta\text{and} w\text{arbitrarily}} \\ 
{\text{3:}} & {\mathbf{l}\mathbf{o}\mathbf{o}\mathbf{p}\text{for}\text{each}\text{episode:}} \\ 
{\text{4:}} & \left. \text{Initialize} t\leftarrow 0,I\leftarrow 1\text{and} s, \right. \\ 
& \left. z_{w}\leftarrow 0\text{(same}\text{dimensionality}\text{as} w\text{)},\text{and} \right. \\ 
& \left. z_{\theta}\leftarrow 0\text{(same}\text{dimensionality}\text{as}\theta\text{)} \right. \\ 
{\text{5:}} & {\mathbf{w}\mathbf{h}\mathbf{i}\mathbf{l}\mathbf{e} s\text{is}\text{not}\text{terminal:}} \\ 
{\text{6:}} & {\text{Choose} a\text{from} s\text{using}\pi\text{,}\text{breaking}\text{ties}\text{randomly}} \\ 
{\text{7:}} & {\text{Take}\text{action} a\text{,}\text{observe} s^{'},r} \\ 
{\text{8:}} & \left. \delta\leftarrow r + \gamma V\left( s^{'},w \right) - V\left( s,\theta \right) \right. \\ 
{\text{9:}} & \left. z_{w}\leftarrow\gamma\lambda_{w}z_{w} + \nabla V\left( s,w \right) \right. \\ 
{\text{10:}} & \left. z_{\theta}\leftarrow\gamma\lambda_{\theta}z_{\theta} + I\nabla\ln\pi\left( a \mid s,\theta \right) \right. \\ 
{\text{11:}} & \left. w\leftarrow w + \alpha_{w}\delta z_{w} \right. \\ 
{\text{12:}} & \left. \theta\leftarrow\theta + \alpha_{\theta}I\delta z_{\theta} \right. \\ 
{\text{13:}} & \left. I\leftarrow\gamma I \right. \\ 
{\text{14:}} & \left. s\leftarrow s' \right. \\ 
\end{array}
```

### Example: Eligibility Trace Actor-Critic on the CartPole Environment

I made minor modifications to the example in ["Example: n-step actor-critic on the CartPole environment"](#ch05.html#sec_example_actor_critic_cartpole) to include the decaying eligibility traces. This experiment uses `α_θ` = `α_w` = `λ_θ` = `λ_w` ≐ 0.1 and you can observe the results in [Figure 5-8](#ch05.html#fig_cartpole_eligibility).

![](assets/rele_0508.png)

The performance is largely unchanged. This demonstrates that a successful agent does not require foresight in the `CartPole` environment. Observing the potential downfall at the next step is adequate.

Changing the value of `λ` has little effect except for values of 0, where there is no temporal-difference update, and 1, where gradients explode. In my experiments, all values in between led to similar results.

In environments where rewards are infrequent or a significant amount of planning is required, multistep temporal-difference estimates excel.

## A Comparison of Basic Policy Gradient Algorithms

This chapter has presented four different implementations of the policy gradient algorithm. All use gradient descent to iteratively improve the policy, but each have different value estimate implementations. A better value estimate reduces unnecessarily large updates; it reduces the variance of the gradients.

You can see the different forms in [Equation 5-11](#ch05.html#eq_pg_equivalent_forms). The gradient of the policy is equal to the expectation of the gradient of the parameters multiplied by the return. The only thing that differs between these algorithms is how you quantify the return. I find this high-level interpretation of policy gradient algorithms insightful because it clarifies the subtle changes between distinct implementations.

### Equation 5-11. Equivalent Implementations of the Policy Gradient Algorithm

```latex
\begin{array}{clr} 
{\nabla_{\theta}J\left( \theta \right)} & {\doteq {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla_{\theta}\ln\pi_{\theta}\left( a \mid s \right)G} \right\rbrack} & \text{(REINFORCE)} \\ 
& {\doteq {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla_{\theta}\ln\pi_{\theta}{\left( a \mid s \right)\left( G - \right.}V_{w}\left( s \right)} \right\rbrack} & {\text{(REINFORCE}\text{with}\text{Baseline)}} \\ 
& {\doteq {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla_{\theta}\ln\pi_{\theta}\left( a \mid s \right)Q_{w}\left( s,a \right)} \right\rbrack} & \text{(Actor-Critic)} \\ 
& {\doteq {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla_{\theta}\ln\pi_{\theta}\left( a \mid s \right)A_{w}\left( s,a \right)} \right\rbrack} & {\text{(Advantage}\text{Actor-Critic)}} \\ 
& {\doteq {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla_{\theta}\ln\pi_{\theta}\left( a \mid s \right)\delta} \right\rbrack} & {\text{(Temporal-Difference}\text{Actor-Critic)}} \\ 
& {\doteq {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla_{\theta}\ln\pi_{\theta}\left( a \mid s \right)\delta\left( \lambda \right)} \right\rbrack} & {\text{(Eligibility}\text{Actor-Critic)}} \\ 
\end{array}
```

# Industrial Example: Automatically Purchasing Products for Customers

A recent business model trend is to automatically deliver products to customers and allow them to return those that they are not interested in. Stitch Fix is one of the most famous of the subscription box model businesses. After submitting a smorgasbord of initial information about body type and personal preferences, a Stitch Fix customer will be sent a package full of new clothes. The customer is then free to send back any clothes they don’t like and they are charged for the items they keep (plus a service fee).

This idea might sound novel, but companies have been doing this for a long time via “no-commitment” subscription models. For example, Hewlett Packard will happily deliver ink for your printer at a prescribed rate. You can subscribe to certain products on Amazon. Hello Fresh will deliver you food. The difference between no-commitment and subscription box models is that if the customer is on the wrong plan (for example, they are receiving too much ink), they are likely to cancel. It is in HP’s interest to deliver ink at the right rate to maximize the customer lifetime value.

I want to stress how impressive this challenge is. Here, an agent with no human intervention can automatically send you products and iteratively learn your preferences. You will receive items when you need them without you ordering them. Even better, the agent knows what items you might like, even though you have never thought of buying them yourself. From the perspective of a marketplace, this demonstrates why RL could be so lucrative. Vendors can sell you more stuff and you will be genuinely happy that they have!

## The Environment: `Gym-Shopping-Cart`

This example embodies the subscription box model using publicly available data. I have created [an environment](https://gitlab.com/winderresearch/gym-shopping-cart) that uses data from Instacart to simulate purchasing behavior.
