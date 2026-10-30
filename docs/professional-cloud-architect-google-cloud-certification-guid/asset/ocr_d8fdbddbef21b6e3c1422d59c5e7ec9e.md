# Training

Now, we'll get to the heart of ML: the actual training. Let's look at the model we have chosen for training. Here, we have X = W x Y + b, where W is weight and b is bias, while X and Y are the features. During training, we will manipulate the W and b values and use the training data to verify whether we are getting correct predictions. Each of these manipulations is referred to as a training step:

![](bbox=[0.28, 0.25, 0.68, 0.42])

Figure 14.3 – ML training

While at the outset we may obtain poor results, gradually, we will get accurate predictions for the W and b values. If we are not getting the results we anticipated, we may need to choose a more complex model:

![](bbox=[0.12, 0.55, 0.85, 0.72])

Figure 14.4 – Adjusting weight and bias

In the preceding plots, we can see how the line changes when we manipulate W and b. In the beginning, we may get results that do not appear to be fit for purpose, but eventually, we achieve values with satisfying results.