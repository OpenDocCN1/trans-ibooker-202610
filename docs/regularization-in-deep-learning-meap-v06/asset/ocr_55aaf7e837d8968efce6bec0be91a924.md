# # 7

Regularization via Objective Function

# ### This chapter covers

+   - Norm-based penalties and their use in regularizing a model estimation process
+   - L2-norm-based penalty in ridge regression and its implementation
+   - L1-norm-based penalty in LASSO regression and its implementation
+   - Derivation of closed-form solutions for both ridge and LASSO regression
+   - Empirical analysis on the effect of different regularization schemes in training feed-forward neural networks

The objective function is the final stop of the forward pass when training a neural network. Also referred to as the cost function, the objective function determines the quality of prediction based on the current parameter configuration of the network. It guides the weight updates in the next iteration via gradient descent. By summarizing the prediction quality for all observations, the objective function offers a single metric that summarizes the amount of error for downstream optimization. That single metric is small in value if the predictions are close to the target values (on average) and large otherwise.

The directionality of the objective function applies to both regression and classification settings. We know that mean squared error (MSE) and cross-entropy loss (CEL) are popular choices of metric for regression and classification, respectively. MSE is lowered bounded by zero, and we get a zero MSE when the algorithm perfectly interpolates all training data points. However, getting an exact zero CEL is impossible since the SoftMax operation is used. Minimizing these empirical loss functions in both situations keeps the optimization iterations running. When using big neural networks, we often run the optimization until the model