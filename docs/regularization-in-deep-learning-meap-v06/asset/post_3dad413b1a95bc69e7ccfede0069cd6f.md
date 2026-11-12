
> Same linear regression model, but preferred in the face of extreme uncertainties

> This is still the true function we want to learn

Figure 6.1 Presenting the same scenario with only six data points available. A simple linear regression model is preferred over an overly complex model such as deep neural networks.

The example above is a good manifestation of introducing _inductive bias_, a set of implicit or explicit assumptions made by the model in the training procedure. By constraining the model to be a simple linear model, we end up building a less sophisticated model by stripping off unnecessary complexities such as polynomials of a higher degree. For example, if we are going to learn a model of the form $f(x) = w_0 + w_1x + w_2x^2 + w_3x^3$, an inductive bias that favors a linear straight-line fit in the face of extremely limited training set would allow us to set $w_2 = w_3 = 0$ directly. In other words, we specify a linear model in our design in the first place.

In addition to the assumption on the functional form of the model, a linear model also assumes normality on the errors. That is, the resulting errors from the linear regression fit are normally distributed with a zero mean and constant variance. Such assumption allows the linear regression model to be optimal, in the sense that it is provably unbiased of minimum variance.

Inductive bias also plays an important role in _Bayesian inference_, a particular field in statistics that studies the posterior distribution of a given parameter of interest based on a prior distribution and observed likelihood of the data. If we were to estimate a parameter $w$, Bayesian statistics put a distribution to $w$ as a form of probabilistic belief about its likely values, which is different from vanilla neural networks that try to estimate the single best value for w. Before observing any data, we would assume a particular prior distribution (along with its governing parameters) to w, which could be a simple standard normal distribution w~N(0,1), or other forms based on our prior knowledge or domain expertise about the system under study.

After assuming a specific prior distribution for w, we would then update this prior distribution to form a posterior distribution based on the observed data, in particular, the likelihood of seeing the available data we have. For example, using Bayes’ rule, the updated posterior distribution for w could be N(0.2,0.9) after observing some data. The prior distribution thus serves as a helpful inductive bias to warm-start the estimation process, which iteratively gets updated in the form of the posterior distribution calibrated by the observed data. Having a good prior distribution could thus greatly accelerate the process of locating the best posterior distribution. Figure 6.2 illustrates this example.

![](img/ee98e91e6845f41077b163bec833ee40_179_0.png)

Figure 6.2 Using Bayes' rule to update the prior distribution of the parameter and obtain the posterior distribution after observing some data. The prior distribution serves as an important inductive bias before any data is observed and could greatly accelerate the process of obtaining the best posterior distribution.

In this chapter, we will focus on different types of inductive bias used in a typical convolutional neural network. We will start by introducing properties such as translational invariance and weight sharing as a result of inductive bias introduced in the model architecture, followed by a detailed discussion on a commonly used regularization technique called dropout, which promotes a sparse representation. We also extend our discussion to multi-task learning in the second part of the chapter, showing detailed examples and implementation details on how regularization can be achieved from the model design perspective.

## 6.1 Inductive bias in convolutional neural networks

An inductive bias is an assumption, often based on domain expertise, to generalize the model's predictive performance on the test set. Multiple domain-specific applications require expert input in the model development process. For example, judges imbue judicial models (used to predict court sentencing) with fairness and justice, and doctors emphasize specific areas or patterns in an x-ray image when automating the diagnostics decisions using models.

This is also called _human-in-the-loop_ model development, where domain experts are closely involved in different modeling stages, such as pre-model preference or post-model adjustment. A recent work by Liu et al. in their paper named “Limousine Service Management: Capacity Planning with Predictive Analytics and Optimization” suggests that engaging domain experts in data-driven modeling could significantly improve the effectiveness of the final production model and lead to increased satisfaction level and operational efficiency.

Recall the old aphorism in statistics: all models are wrong, but some are useful. Since a model is essentially a hypothesis that we assume to approximate the underlying data generating function, the inductive bias exists in all models, expressed either explicitly or implicitly. For example, a linear model assumes linearity in the weights, while a non-linear model applies non-linear transformations depending on the model used. Inductive bias thus allows us to infuse the model with adequate external knowledge from the real world or based on past experience, particularly when the training data is insufficient to support a proper training process.

Let us revisit the structure of a fully-connected neural network, in particular the inductive bias we placed in the design of any neural network.

### 6.1.1 Revisiting the fully-connected network

Consider the same two-layer fully-connected network we discussed in chapter 4. As shown in figure 6.3, the network has one hidden layer of five nodes, each performing a weighted sum of previous input features, followed by a nonlinear transformation via a specified activation function. This is illustrated by zooming in the operation of the first node in the hidden layer, generating \( f^1_1 = \phi(b^1_1 + w^1_1 x) \).

![](img/ee98e91e6845f41077b163bec833ee40_181_0.png)

Figure 6.3 A two-layer fully-connected neural network that consists of one input node, five hidden nodes, and one output node. Each node in the hidden layer performs a weighted sum followed by a nonlinear transformation, as represented by zooming in the first node in the hidden layer.

As it turns out, a fully-connected network (FCN) has very little inductive bias in terms of how data flows between layers. When the input features go into a fully-connected network, we place little assumption on how they should interact with the weights other than a multiplication operation that traverses all elements. This leads to its flexible and expressive design and approximating capability, which is also backed by the universal approximation theorem introduced in chapter 4; by extending the number of nodes in the hidden layers to an arbitrary extent, a fully-connected neural network could approximate functions of any shape.

However, there are still some inductive biases we placed when designing the model architecture of a typical neural network in terms of its structure and optimization. Architecturally, the nodes within the same layer are assumed never to interact with each other. The feature maps in the hidden layers contain standalone features that only engage in the forward and backward propagations but never talk to their neighbors in the same hidden layer. Besides, a typical neural network takes the form of a directed acyclic graph, such as the one in figure 6.3, where inputs are processed from left to right until reaching the final layer for cost calculation. These architectural preferences constitute our implicit inductive bias in many types of neural networks used in day-to-day development.

From an optimization perspective, gradient descent has become the de facto optimization method when training a neural network, exhibiting an inductive bias for models that are easily trainable by gradient-based methods. In particular, the stochastic (mini-batch) gradient descent algorithm is a popular choice due to its simple and fast training time as well as the implicit regularization effect, inducing models that exhibit robustness in the location of optima across different batches (more on this in a later chapter on regularization via
