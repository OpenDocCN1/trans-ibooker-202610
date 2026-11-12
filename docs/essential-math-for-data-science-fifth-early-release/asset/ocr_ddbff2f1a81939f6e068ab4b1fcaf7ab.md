# LASSO AND RIDGE REGRESSION

Two somewhat popular variants of linear regression are lasso regression and ridge regression. Ridge regression adds a further bias to a linear regression in the form of a penalty, and therefore causing it to fit less to the data. Lasso regression will attempt to marginalize variables that are noisy, making it useful when you want to automatically remove variables that might be irrelevant.

Still we cannot just apply a linear regression to some data, make some predictions with it, and assume all is okay. A linear regression can still overfit even with the bias of a straight line. Therefore, we need to check and mitigate for both overfitting and underfitting to find the sweet spot between the two. That is if there is one at all, in which case you should abandon the model altogether.

# Stochastic Gradient Descent

In a machine learning context, you are unlikely to do gradient descent in practice like we did earlier, where we trained on all training data (called batch gradient descent). In practice you are more likely to perform stochastic gradient descent, which will only train on one sample of the dataset on each iteration. In mini-batch gradient descent, multiple samples of the dataset are used (e.g. 10 or 100 data points) on each iteration.

Why only use part of the data on each iteration? Machine learning practitioners cite a few benefits. First it reduces computation significantly, as each iteration does not have to traverse the entire training dataset but only part of it. The second benefit is it reduces overfitting. By exposing the training algorithm to only part of the data on each iteration, it keeps changing the loss landscape so it does not settle in the loss minimum. After all, minimizing the loss is what causes overfitting and so we introduce some randomness to create a little bit of underfitting (but hopefully not too much).

Of course, our approximation becomes loose so we have to be careful. This is why we will talk about training/testing splits shortly, as well as other