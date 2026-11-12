

## Generalization: A Modern View

### This chapter covers

- The modern view on generalization in machine learning and deep learning
- The "double descent" phenomenon of over-parameterized models and its connection with the classical bias-variance tradeoff
- The implementation of a smoothing spline model as an extension of the polynomial regression model with smoothness penalty
- The extension of the "double descent" phenomenon to both model iterations and sample size

One of the biggest takeaways in chapter 2 is the bias-variance tradeoff. According to the classical view on generalization, a model with excessive complexity will incur high variance and overfit the data, thus not generalizing to the future test data. A properly trained model should balance underfitting and overfitting: It should be complex enough to learn the underlying relationship and simple enough to not fall into a potential spurious pattern due to random noise in the data. This best model, based on the classical statistics, is identified at the "sweet spot" in the inverted U-shaped risk curve for the test set, as shown in figure 1.7 in chapter 1.

Interestingly, machine learning practitioners found that over-parameterized models such as deep neural networks tend to perform well in the test set, even when they fit the training set perfectly, i.e., the training error is zero. Under such circumstance, the model essentially interpolates and passes through all training data points when perfect fitting occurs, leading to zero training error. Nevertheless, these overly complex models can generalize well to the unseen test set. Such a phenomenon is an apparent contradiction to the classical view on generalization. How could an overly complex model manage to generalize even when it overfits the training data?

Many attempts have been made to understand why the empirically witnessed behavior in training modern machine learning models is at odds with the classical view on generalization. To better understand the phenomenon and possible drivers, we will first introduce the modern view on generalization and its connection to the classical view via the "double descent" curve. The "double descent" curve essentially describes a twofold descent risk curve. As the model becomes complex, we would initially observe a decreasing risk, which attains a minimum risk at the "sweet spot" and increases until the number of features equals the number of observations. However, as the model complexity increases, the test risk surprisingly decreases again for the second and final time.

This twofold descent performance curve, however, reconciles the classical view and the modern view on generalization. In the following, we will first provide the modern view on generalization and discuss the "double descent" phenomenon and the reasons behind its occurrence. We will then accompany these discussions with a concrete example on training an over-capacitated polynomial regression model with a smoothness penalty, a continuation of the example from the previous chapter.

### 3.1 A modern view on generalization

The primary goal of model training is to seek a model \( f_w \) out of the total space of model class \( F_w \) that minimizes the empirical risk \( \frac{1}{n} \sum_{i=1}^n L(y_i, f_w(\mathbf{x}_i)) \) on the test subset of the available dataset \( (\mathbf{X}, \mathbf{y}) \). By minimizing the empirical test risk as a proxy of the true unobservable risk, we hope to minimize the true risk \( \mathbb{E}(L(y, f_w(\mathbf{X}))) \). Under the framework of empirical risk minimization, classical statistics suggests controlling the model complexity by balancing underfitting and overfitting via the bias-variance tradeoff philosophy: a model \( f_w \) that has a larger empirical risk at the test subset will generalize poorly to the new data when it has either limited or excessive representational capacity.

The classical view on generalization is characterized by a U-shaped curve that signifies the bias-variance tradeoff. The curve usually represents the model’s test set performance as the empirical test risk as a function of model complexity. The training set’s performance curve is also plotted to verify the increased predictive power of the model. As shown in figure 3.1, the resulting figure is essentially an inverted version of figure 1.7, where the "sweet spot" indicates the model that reaches an optimal tradeoff between the bias and the variance.

![](img/ee98e91e6845f41077b163bec833ee40_72_0.png)

Figure 3.1 The typical U-shaped training and test risk curves that indicate the classical view on the bias-variance tradeoff. As the model becomes complex, both training and test risks will go down. However, when the model becomes overly complex and overfits the training data, the test risk will start to increase, despite continuously decreasing training risk.

Based on the classical view on generalization, the best model is the one that has the lowest empirical test risk, i.e., the model at the “sweet spot”. The region on the left of the “sweet spot” underfits the training data and fails to capture the true relationship due to an excessively simplifying assumption. This is the case when the (squared) bias term dominates the test risk. The region on the right of the “sweet spot” overfits the training data and generates unstable predictions due to an overly complex model capacity. This is the case when the variance term dominates the test risk.

Note that the model complexity can be controlled by regularization via data, model architecture, cost, optimization algorithm, or any combination of the four components. When the model complexity is properly controlled to strike a balance between underfitting and overfitting, it is expected to generalize well to future unseen data sampled from the total population. The classical U-shaped risk curve is thus widely used to guide model selection. In contrast, a model that gives zero training error at the interpolation threshold is said to interpolate the training data and would generalize poorly to the new data. Thus the conventional wisdom from classical statistics argues that larger models suffer in predictive performance once they overfit.

### 3.1.1 Beyond perfect interpolation

Complex models such as neural networks and nonlinear class of models tend to have very low, or even zero training cost. The zero training cost occurs when the model becomes sufficiently complex to interpolate the training data, as shown by the rightmost dot in figure 3.1. Perfect prediction in the training data happens when the number of weights p used by the model is precisely equal to the number of observations n in the training data. In the case of linear regression, when p = n, we solve a system of linear equations where the number of free variables, whose optimal values we are solving for, is equal to the number of linear equations in the system.

A simple example is a two-dimensional training set with two observation points on a coordinate system. When only one parameter is available, the model is reflected as a straight horizontal line in the coordinate system. This univariate model is too simple to fit these two points unless they happen to live along the fitted line. The resulting system of equations is said to be overdetermined and under-parameterized since we have more linear equations than the unknown variables in the system.

When we have two parameters to fit the model, the problem becomes solvable with a exact solution: two linear equations and two unknowns, a straightforward exercise in high school. We can fit a line anywhere in the coordinate system by adjusting its intercept and slope. The problem thus becomes fitting a line that passes through the two points. We can train a model that passes through the two points and produces zero training error, thus interpolating between these two points.

When there are more than two unknown variables, say we have three weights in the model, the problem becomes underdetermined and over-parameterized. The solution, when solvable, will be infinite. Having more than two parameters corresponds to fitting two points with a curved line. Any curve that passes through the two points produces zero training error, yet the curve can be arbitrarily wiggly and complex depending on the number of parameters used. Figure 3.2 summarizes these three scenarios.

