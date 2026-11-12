
When features are unbounded, like `IP` or email addresses, you can forcibly constrain the features into a limited set of bins without loss of information by `hashing` (there will still be a loss in accuracy due to `binning`).

## Supervised methods

In some domains you may have labeled data that is important to the RL problem. You might want to consider building a classification model and pass the result into the RL algorithm. There are a wide variety of algorithms suitable for this purpose.^([28](#ch09.html#idm46428619115192))

## Unsupervised methods

If you don’t have labels you could use unsupervised machine learning methods, like `k-means`, to position bins in better locations.^([29](#ch09.html#idm46428619111480))

## Mixed state spaces

If you have a mixed state space then you have three options:

1.  Unify the state space through conversion
2.  Cast the discrete space to a continuous one and continue using continuous techniques
3.  Split the state space into separate problems

Options 1 and 2 are self explanatory. The majority of `functional approximators` will happily work with `discretized values`, even though it may not be optimal or efficient.

Option 3, however, is interesting. If you can split the state into different components that match bounded contexts in the problem domain, that might indicate that it is more efficient to use `submodels` or `subpolicies`. For example, a crude approach could be to separate the continuous and discrete states, feed them into two separate `RL algorithms`, then combine them into an `ensemble`.^([30](#ch09.html#idm46428619101768)) As you can see this is touching upon `hierarchical RL`, so be sure to leverage ideas from there.

## Mapping Policies to Action Spaces

Before you attempt to define how a policy specifies actions, take a step back and consider what type of actions would be most suitable for your problem. Ask yourself the following questions:

*   *What value types can your actions take?* Are they binary? Are they discrete? Are they bounded or unbounded? Are they continuous? Are they mixed?
*   *Are simultaneous actions possible?* Is it possible to select multiple actions at the same time? Can you iterate faster and interleave single actions to approach selecting multiple actions?
*   *When do actions need to be made?* Is there a temporal element to action selection?
*   *Are there any meta-actions?* Can you think of any high-level actions that might help learning or handle the temporal elements?
*   *Can you split the actions into subproblems?* Are there obvious bounded contexts in your actions? Can these be split into separate problems? Is this a hierarchical problem?
*   *Can you view your actions in another way?* For example, rather than moving to a continuous-valued position, could you consider a “move forward” action? Or vice versa?

All of these decisions have grave impacts on your work, so be careful when making them. For example, switching an action from continuous to discrete might force you to change algorithms. Or realizing that your agent requires simultaneous actions might make your problem much harder to solve.

### Binary actions

Binary actions are possibly the simplest to work with, because they are easy to optimize, visualize, debug, and implement. Policies are expected to output a binary value mapping to the action that they want to choose. In problems where only a single action is allowed, picking the maximum expected return (for `value methods`) or the action with the highest probability (for `policy gradient methods`) is a simple implementation.

As seen in [“How Does the Temperature Parameter Alter Exploration?”](#ch07.html#sec_entropy_exploration), you can encourage broad exploration by making the policy output probabilities, or values that can be interpreted as probabilities. The agent can then randomly sample from this action space according to the assigned probabilities. For `value methods`, you can assign the highest probabilities to the highest expected values. I would recommend using `advantages`, rather than `values` directly, to avoid bias. An `exponential softmax function` is recommended to convert values to probabilities. In `policy gradient methods` you can train the models to predict probabilities directly.

### Continuous actions

Continuous actions are very common, like determining the position of a servo or specifying the amount to bid on an ad. In these cases it is most natural to use some kind of approximation function to output a continuous prediction for a given state (just like regression); `deterministic policy gradients` do this. But researchers have found that modeling the output as a `random variable` can help exploration. So rather than directly predicting the output value, they predict the `mean` of a distribution, like a `Gaussian`. Then a value is sampled from distribution. Other parameters of the distribution, like the `standard deviation` for the `Gaussian`, can also be predicted. Then you can set the initial `standard deviation` at the beginning high, to encourage exploration, and allow the algorithm to shrink it as it learns. I like to think of this as a crude attempt to incorporate probabilistic modeling. Of course this assumes that your actions are normally distributed, which may not be accurate, so think carefully about what distribution you should choose for your actions.

An alternative is to leverage newer methods that are based upon `Q-learning`, but have been adapted to approximate continuous functions: `continuous action Q-learning`.^([31](#ch09.html#idm46428619071784)) This is by no means as common as using `policy gradient methods` but claims to be more efficient.

One final trick that I want to mention, which is helpful in simple examples, is called the `dimension scaling trick`. When using `linear approximators` that need to predict multiple outputs, like the `mean` and `standard deviation`, you can have distinct parameters for each action and update the parameters for the chosen action individually. This is effectively a `dictionary of parameters`, where the `key` is the action and the values are the parameters for the continuous mapping, like the `mean` and `standard deviation`.

### Hybrid action spaces

Spaces with both discrete and continuous actions are relatively common, like deciding to buy and how much to spend, and can be tricky to deal with. First consider if you can split the problem. If it’s a hierarchical problem, go down that route first. Next consider if you can get away with only continuous or only discrete. You’ll probably lose a little bit of granularity, but it might make it easier to get something up and running.

New algorithms are emerging that learn when to select either a continuous algorithm or a discrete one, depending on the required action space. `Parameterized DQN` is one such example, but the idea could be applied to any algorithm.^([32](#ch09.html#idm46428619064152)) Another approach called `action branching` simply creates more heads with different activations and training goals on a `neural network`, which seems like a nice engineering solution to the problem.^([33](#ch09.html#idm46428619061880)) This `multihead architecture` also enables `simultaneous actions`. In a sense this just a handcrafted form of `hierarchical RL`.

### When to perform actions

Here’s a curveball: what about when you want to output no action? That’s actually a lot harder than it sounds, because everything from the `Bellman equation` to `neural network` training is set up to expect an action on every step. The `Atari` games implement `frame skipping` without really considering how many frames to skip; 4 are just taken as given. But researchers have found that you can optimize the the size of number of `skipped frames` to improve performance.^([34](#ch09.html#idm46428619058152))
