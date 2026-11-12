
To transfer the model I copied the policy and value estimate weights learned when training on random customers and pasted them into another agent with only a single customer. Also, these results are for a single run. You can imagine a smoother set of results if I were to repeat this 50 times. If you compare this result to [Figure 5-9](#ch05.html#fig_simple_shopping_single_customer), then you can see there is no initial ramp-up in performance because the pretrained weights are already very good.

Another observation is that it takes a long time (approximately 200 epochs) for the single customer agent to obtain similar performance to the result in [Figure 5-9](#ch05.html#fig_simple_shopping_single_customer). This is because the agent has learned a policy that works best on average across all customers. When I transfer the policy across to a new agent on the single customer data, it needs to retrain those weights. But the average policy appears to have lowered the probability of exploring the states that are most important to that single customer.

In other words, it seems that in [Figure 5-9](#ch05.html#fig_simple_shopping_single_customer), the agent has overfit for that particular customer. Typically overfitting is a bad thing, since the model will not generalize to other customers. But in this example, that is exactly the point. I want the agent to fit around the preferences of a single customer.

Here there is a trade-off. You want to start with a reasonable policy, so the customer receives good recommendations quickly. But you don't want to have to wait for 200 orders before they are in positive reward territory. One solution to this problem is to treat the number of pretraining epochs as a hyperparameter and tune it to obtain a satisfactory result.

## Summary

Prior to this chapter, I focused on value-based methods, methods that quantify the value of being in a state. The policy is derived as a maximization of the expected value in each state. But another way is to parameterize the policy directly and alter the weights of the parameterization to produce a policy that maximizes the expected return. You can use policy gradient methods to find the updates necessary to optimize a policy.

Policy gradient methods have a range of advantages. They can learn to output probabilities, which negates the need for an explicit exploration algorithm like `ϵ`-greedy. They can output continuous actions. And sometimes it is easier to model the policy, rather than having to model the state-action value.

On the other hand, policy gradient methods tend to be more sensitive to the choice of model. With value methods, it is fairly easy to alter the complexity of the model to predict the expected value function, whereas the policy model needs to be chosen carefully.

This chapter followed the same derivation as before, from Monte Carlo through to eligibility traces. Even though policy-based methods are optimizing for a slightly different solution, expected value estimates are still incredibly important, as you saw in the actor-critic algorithms.

I presented a simplified industrial example where the goal was to automatically order products for users. You can imagine a large number of improvements to this and the generalization of this idea, recommenders, is an active area of RL research. I know of several organizations that are using RL for recommendation purposes.

## Further Reading

*   Gradient descent:
    *   Ruder, Sebastian. 2017. [“An Overview of Gradient Descent Optimization Algorithms.”](https://oreil.ly/iAYEs) ArXiv:1609.04747, June.
*   Policy gradient derivation:
    *   A succinct mathematical introduction to on-policy gradients: Kämmerer, Mattis Manfred. 2019. [“On Policy Gradients.”](https://oreil.ly/1EAUY) ArXiv:1911.04817, November.
    *   Sutton, Richard S., and Andrew G. Barto. 2018. *Reinforcement Learning: An Introduction*. MIT Press.
*   Building generative models:
    *   Foster, David. 2019. *Generative Deep Learning: Teaching Machines to Paint, Write, Compose, and Play*. O’Reilly.

^([1](#ch05.html#idm46428617259448-marker)) Houthooft, Rein, Richard Y. Chen, Phillip Isola, Bradly C. Stadie, Filip Wolski, Jonathan Ho, and Pieter Abbeel. 2018. [“Evolved Policy Gradients”](https://oreil.ly/IQPmY). ArXiv:1802.04821, April.

^([2](#ch05.html#idm46428617079864-marker)) Bhatnagar, Shalabh, Mohammad Ghavamzadeh, Mark Lee, and Richard S Sutton. 2008. [“Incremental Natural Actor-Critic Algorithms”](https://oreil.ly/9UYvm). In *Advances in Neural Information Processing Systems* 20, edited by J. C. Platt, D. Koller, Y. Singer, and S. T. Roweis, 105–112. Curran Associates, Inc.

^([3](#ch05.html#idm46428617021336-marker)) Wolpert, D. H., and W. G. Macready. 1997. [“No Free Lunch Theorems for Optimization”](https://oreil.ly/lpi8x). *IEEE Transactions on Evolutionary Computation* 1(1): 67–82.

# Chapter 6. Beyond Policy Gradients

Policy gradient (PG) algorithms are widely used in reinforcement learning (RL) problems with continuous action spaces. [Chapter 5](#ch05.html#ch_policy_gradient_methods) presented the basic idea: represent the policy by a parametric probability distribution `π[θ] ≐ ℙ[a|s;θ]`, then adjust the policy parameters, `θ`, in the direction of greater cumulative reward. They are popular because an alternative, like the greedy maximization of the action-value function performed in Q-learning, becomes problematic if actions are continuous because this would involve a global maximization over an infinite number of actions. Nudging the policy parameters in the direction of higher expected return can be simpler and computationally attractive.

Until recently, practical online PG algorithms with convergence guarantees have been restricted to the *on-policy* setting, in which the agent learns from the policy it is executing, the *behavior* policy. In an *off-policy* setting, an agent learns policies different to the behavior policy. This idea presents new research opportunities and applications; for example, learning from an offline log, learning an optimal policy while executing an exploratory policy, learning from demonstration, and learning multiple tasks in parallel from a single environment.

This chapter investigates state-of-the-art off-policy PG algorithms, starting with an in-depth look at some of problems that arise when trying to learn off-policy.

## Off-Policy Algorithms

The PG algorithms presented in [Chapter 5](#ch05.html#ch_policy_gradient_methods) were on-policy; you generate new observations by following the current policy—the same policy that you want to improve. You have already seen that this approach, this feedback loop, can lead to overoptimistic expected returns (see [“Double Q-Learning”](#ch03.html#sec_double_q_learning)). Off-policy algorithms decouple exploration and exploitation by using two policies: a target policy and a behavior policy.

All of the RL algorithms presented up to this point have performed a least-squares-like optimization, where estimates of a value approximation are compared to an observed result; if you observe a surprising result, then you update the estimate accordingly. In an off-policy setting, the target policy never has the opportunity to test its estimate, so how can you perform optimization?

### Importance Sampling

Before I continue, I want you to understand something called importance sampling, which is the result of some mathematical trickery that allows you to estimate a quantity of interest without directly sampling it.

I’ve mentioned a few times so far that the expectation of a random variable, `X`, is the integral of an individual value, `x`, multipled by the probability of obtaining that value, `f(x)`, for all possible values of `x`. In a continuous probability distribution this means `𝔼[f][x] ≐ ∫[x] x f(x)`. In the discrete case this is `𝔼[f][x] ≐ ∑[x] x f(x)`. This is called the *population mean*.
