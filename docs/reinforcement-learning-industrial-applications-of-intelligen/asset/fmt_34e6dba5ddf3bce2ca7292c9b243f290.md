
In 1972 Robert Rescorla and Allan Wagner found another interesting phenomenon that Pavlovian conditioning couldn’t explain. They first blew a puff of air into a rabbit’s eye, which caused it to blink. They then trained the rabbit to associate an external stimulus, a sound, with the puff of air. The rabbit blinked when it heard the sound, even if there was no puff of air. They then retrained the rabbit to blink when exposed to both a sound and a light. Again, when the rabbit heard the sound and saw the light with no puff of air, it blinked. But next, when the researchers only flashed the light, the rabbit did not blink.^([31](#ch01.html#idm46428621058888))

The rabbit had developed a hierarchy of expectations; sound and light equals blink. When the rabbit did not observe the base expectation (the sound), this *blocked* all subsequent conditioning. You may have experienced this sensation yourself. Occasionally you learn something so incredible, so fundamental, you might feel that any derivative beliefs are also invalid. Your lower-level conditioning has been violated and your higher-order expectations have been blocked. The result of this work was the Rescorla–Wagner model. Their research was never presented this way, but this describes a method called *value estimation*.

## Value Estimation

Imagine yourself trying to simulate the Rescorla–Wagner rabbit trial. The goal is to predict when the rabbit blinks. You can create a model of that experiment by describing the inputs and the expected output. The inputs represent the actions under your control and the output is a prediction of whether the rabbit blinks.

You can represent the inputs as a vector `s = (s[0], s[1], ..., s[n-1])` where `s[i] = 1` if the `i`th stimulus is present in a trial and zero otherwise. These are binary actions.

For example, imagine that the feature `s[0]` represents the sound and `s[1]` represents the light. Then the vector `s = [0,1]` represents the situation where the sound is not present and the light is.

You can write the estimated output as `V`, which represents the prediction of whether the rabbit blinks or not. The states are then mapped to the correct prediction using a function, `V(s)`.

### Note

I have made an active decision to simplify the mathematics as much as possible, to improve readability and understanding. This means that it loses mathematical formality, like denoting an estimate with a *hat*, to improve readability. See the academic papers for full mathematical rigor.

Now comes the hard part: defining the mapping function. One common solution is to multiply the inputs by *parameters* under your control. You can alter these parameters to produce an output that depends on the inputs. These parameters are called *weights* and the model you have just built is *linear*.

The weights are defined by another vector, `w`, which is the same shape as the features. For example, the data may show that the light did not cause the rabbit to blink; it blinked only when it heard the sound. This results in a model with a weight of value 1 for the sound feature and a weight of value 0 for the light.

Formally, the function is a sum of the inputs multiplied by the weights. This operation is called a *dot product*. The result, which is shown in [Equation 1-1](#ch01.html#eq_psychology-value-estimate), is a prediction of whether the rabbit blinks.

### Equation 1-1. Value Estimate

$$
\begin{array}{cl}
V(s,w) & \doteq w_0 s_0 + w_1 s_1 + ... + w_n s_n \\
       & = w \cdot s = w^{\intercal} s
\end{array}
$$

But, in general, how do you decide on the values for the weights?

## Prediction Error

You can use an optimization method to find the optimal parameters. The most common method is to quantify how wrong your estimate is compared to the ground truth (the correct answer). Then you can try lots of different weights to minimize the error.

The error in your prediction, `δ`, is the difference between the actual outcome of the experiment, `E`, and the prediction (the value estimate). The prediction is based upon the current state of the environment (an observation of the rabbit), `s`, and the current weights. All variables change over time and are often denoted with the subscript `t`, but I ignore that to reduce the amount of notation. You can see this defined in [Equation 1-2](#ch01.html#eq_psychology-prediction-error).

### Equation 1-2. Prediction Error

$$ \delta \doteq E - V(s,w) $$

One way to think of [Equation 1-2](#ch01.html#eq_psychology-prediction-error) is that `δ` is a form of surprise. If you make a prediction that the rabbit will definitely blink and it does not, then the difference between what you predicted and what happened will be large; you’d be surprised by the result.

Given a quantitative definition of surprise, how should the weights be altered? One way is to nudge the weights in proportion to the prediction error. For example, consider the situation where the expectation was that the sound (index zero) and the light (index one) had equal importance. The set of weights is `w = [1,1]`.

This time the experiment is to shine only the light, not sound. So the observed state of the experiment is `x(s) = [0,1]`. To make a prediction, `V(s,w)`, calculate the result of performing a dot product on the previous vectors. The result is 1 (blink).

When you ran your simulation, the rabbit did not blink. The value was actually 0. The value of [Equation 1-2](#ch01.html#eq_psychology-prediction-error), `δ`, is equal to −1.

Knowledge of the previous state and the prediction error helps alter the weights. Multiplying these together, the result is `δ * x(s) = [0,1]`. Adding this to the current weights yields `w = [1,0]`.

Note how the new weights would have correctly predicted the actual outcome, 0 (not blink) (`w^⊺ * x(s) = [1,0]^⊺ * [0,1] = 0`).

## Weight Update Rule

If you have any experience with ML, you know that you should not try to jump straight to the correct, *optimal* result. One reason is that the experimental outcome might be noisy. Always move toward the correct result gradually and repeat the experiment multiple times. Then, due to the *law of large numbers*, you converge toward the best average answer, which is also the optimal one. One caveat is that this is only true if the underlying mathematics proves that convergence is inevitable; in many algorithms, like those with nonlinear approximators, it is not guaranteed.

This is formalized as an *update rule* for the weights. In [Equation 1-3](#ch01.html#eq_psychology-update-rule), you can control the speed at which the weights update using the hyperparameter `α`, which must be between zero and one. [Equation 1-3](#ch01.html#eq_psychology-update-rule) *updates* the weights inline, which means that the weights in the next time step derive from the weights in the current time step. Again, I ignore the subscripts to simplify the notation.

### Equation 1-3. Weight Update Rule

$$ w \leftarrow w - \alpha \delta x(s) $$

## Is RL the Same as ML?

The mathematics presented in [Equation 1-1](#ch01.html#eq_psychology-value-estimate) and [Equation 1-3](#ch01.html#eq_psychology-update-rule) provide one psychological model of how animals learn. You could easily apply the same approach to software-controlled agents. But the real reason for going into such depth was to provide a mathematical warm-up exercise for the rest of the book. Here I used mathematical symbols that are used in later chapters (the original paper used different symbols and terminology from psychology).
