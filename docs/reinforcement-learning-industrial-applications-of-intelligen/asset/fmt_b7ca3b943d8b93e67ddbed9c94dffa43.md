
Using DL in RL is more difficult than using it for ML. In supervised machine learning you improve a model by optimizing a measure of performance against the ground truth. In RL, the agent often has to wait a long time to receive any feedback at all. DL is already notorious because it takes a lot of data to train a good model, but combine this with the fact that the agent has to randomly stumble across a reward and it could take forever. For this and many more reasons, researchers have developed a bag of tricks to help DL learn in a reasonable amount of time.

# Deep Q-Learning

Before 2013, researchers realized that they needed function approximation to solve problems with large or complex state spaces. They had proven that using a linear approximation retained the same convergence guarantees as tabular methods. But they warned that using nonlinear function approximators may in fact diverge, rather than converge.^([1](#ch04.html#idm46428615774280)) So, much of the research focused on improving linear approximation methods.

Eventually the forbidden fruit that is DL proved too tempting for researchers and they designed a `deep Q-network` (`DQN`).^([2](#ch04.html#idm46428615771240)) They realized that the main problem was the moving goalpost. Imagine trying to swat a fly. The fly’s motion is so random and reactions so quick that only the Karate Kid could hope to catch it midair. If you wait until the fly is stationary, you have a much greater chance of predicting where the fly is going to be in 500 milliseconds and therefore have a better chance of hitting it. The agent has the same problem. Unless the data is stationary, it can find it very hard to converge. In some cases it can actually diverge because like in a destructive feedback loop, the next action might make matters worse.

In general, model optimizers like stochastic gradient descent assume that your data is independent and identically distributed (IID) random variables. When you sample close in time, the data is likely to be correlated. For example, the definition of the MDP says that the next observation is dependent on the previous state and the action taken. This violates the IID assumption and models can fail to converge.

## Experience Replay

The initial solution to this problem was to use `experience replay`. This is a buffer of observations, actions, rewards, and subsequent observations that can be used to train the DL model. This allows you to use old data to train your model, making training more sample efficient, much like in [“Eligibility Traces”](#ch03.html#sec_eligibility_traces). By taking a random subset when training, you break the correlation between consecutive observations.

The size of the `experience replay` buffer is important. Zhang and Sutton observed that a poorly tuned buffer size can have a large impact on performance.^([3](#ch04.html#idm46428615761928)) In the worst case, agents can “catastrophically forget” experiences when observations drop off the end of a `first in, first out` (`FIFO`) buffer, which can lead to a large change in policy. Ideally, the distribution of the training data should match that observed by the agent.^([4](#ch04.html#idm46428615759736))

## Q-Network Clones

In an update to the original `DQN` algorithm, the same researchers proposed cloning the `Q-network`. This leads to two `NNs`: one `online network` that produces actions and one `target network` that continues to learn. After some number of iterations the prediction network copies the current weights from the target network. The researchers found that this made the algorithm more stable and less likely to oscillate or diverge. The reason is the same as before. The data obtained by the agent is then stationary for a limited time. The trick is to set this period small enough so that you can improve the model quickly, but long enough to keep the data stationary.^([5](#ch04.html#idm46428615752136)) You will also notice that this is the same as the idea presented in [“Double Q-Learning”](#ch03.html#sec_double_q_learning); two estimates are more stable than one.

## Neural Network Architecture

Using randomly sampled `experience replay` and a `target Q-network`, `DQN` is stable enough to perform well in complex tasks. The primary driver is the choice of `NN` that models and maps the observations into actions. The original `DQN` paper played Atari games from video frames, so using a `CNN` is an obvious choice. But this does not prevent you from using other `NN` architectures. Choose an architecture that performs well in your domain.

Another interesting feature of `DQN` is the choice to predict the state-value function. Recall standard `Q-learning` (or `SARSA`) uses the action-value function; the `Q-values` hold both the state and the actions as parameters. The result is an expected value estimate for a state and a single action. You could train a single `NN` for each action. Instead, `NNs` can have several `heads` that predict the values of all actions at the same time. When training you can set the desired action to 1 and the undesired actions to 0, a `one-hot encoding` of the actions.

## Implementing DQN

The equation representing the update rule for `DQN` is like [“Q-Learning”](#ch03.html#sec_q_learning). The major difference is that the `Q-value` is approximated by a function, and that function has a set of parameters. For example, to choose the optimal action, pick the action that has the highest expected value like in [Equation 4-1](#ch04.html#eq_dqn_choosing_action).

### Equation 4-1. Choosing an action with DQN

```
$$\\left. a\\leftarrow\\operatorname{argmax}\\limits\_{a\_{s} \\in \\mathcal{A}{(s)}}Q\\left( {s,a\_{s};\\theta} \\right) \\right.$$  
```

In [Equation 4-1](#ch04.html#eq_dqn_choosing_action), `Q` represents the function used to predict the action values from the `NN` and `θ` represents the parameters of the function. All other symbols are the same as in [Equation 3-5](#ch03.html#eq_q_learning_update_function).

To train the `NN` you need to provide a loss function. Remember that the goal is to predict the action-value function. So a natural choice of loss function is the squared difference between the actual action-value function and the prediction, as shown in [Equation 4-2](#ch04.html#eq_dqn_loss_function). This is typically implemented as the `mean squared error` (MSE), but any loss function is possible.

### Equation 4-2. DQN loss function

```
L(θ) ≐ 𝔼[π][(y − Q(s, a; θ))²]
```

`NN` optimizers use gradient descent to update the estimates of the parameters of the `NN`. They use knowledge of the gradient of the loss function to nudge the parameters in a direction that minimizes the loss function. The underlying `DL` framework handles the calculation of the gradient for you. But you could write it out by differentiating [Equation 4-2](#ch04.html#eq_dqn_loss_function) with respect to `θ`.

The implementation of `DQN` is much `Q-learning` from [Algorithm 3-1](#ch03.html#alg_q_learning). The differences are that the action is delivered directly from the `NN`, the experiences need buffering, and occasionally you need to transfer or train the `target NN`’s parameters. The devil is in the detail. Precisely how you implement `experience replay`, what `NN` architecture and hyperparameters you choose, and when you do the training can make the difference between super-human performance and an algorithmic blowup.

For the rest of this chapter I have chosen to use a library called `Coach` by Intel’s Nervana Systems. I like it for three reasons: I find the abstractions intuitive, it implements many algorithms, and it supports lots of environments.

## Example: DQN on the CartPole Environment

To gain some experience with `DQN` I recommend you use a simple “toy” environment, like the `CartPole` environment from `OpenAI Gym`.
