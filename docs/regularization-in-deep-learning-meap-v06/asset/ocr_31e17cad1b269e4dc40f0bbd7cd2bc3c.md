# # Generalization: A Modern View

# ## This chapter covers

+   - The modern view on generalization in machine learning and deep learning
+   - The “double descent” phenomenon of over-parameterized models and its connection with the classical bias-variance tradeoff
+   - The implementation of a smoothing spline model as an extension of the polynomial regression model with smoothness penalty
+   - The extension of the “double descent” phenomenon to both model iterations and sample size

One of the biggest takeaways in chapter 2 is the bias-variance tradeoff. According to the classical view on generalization, a model with excessive complexity will incur high variance and overfit the data, thus not generalizing to the future test data. A properly trained model should balance underfitting and overfitting: It should be complex enough to learn the underlying relationship and simple enough to not fall into a potential spurious pattern due to random noise in the data. This best model, based on the classical statistics, is identified at the “sweet spot” in the inverted U-shaped risk curve for the test set, as shown in figure 1.7 in chapter 1.

Interestingly, machine learning practitioners found that over-parameterized models such as deep neural networks tend to perform well in the test set, even when they fit the training set perfectly, i.e., the training error is zero. Under such circumstance, the model essentially interpolates and passes through all training data points when perfect fitting occurs, leading to zero training error. Nevertheless, these overly complex models can generalize well to the unseen test set. Such a phenomenon is an apparent contradiction to the classical view on generalization. How could an overly complex model manage to generalize even when it overfits the training data?