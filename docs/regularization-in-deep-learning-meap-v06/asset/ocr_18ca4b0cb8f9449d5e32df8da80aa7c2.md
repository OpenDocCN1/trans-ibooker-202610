# # 5 Regularization via Data

# This chapter covers

+   - Common challenges in the data and the need for data augmentation
+   - Different data augmentation techniques that contribute to regularized model training
+   - Applying data augmentation in image classification to boost training performance
+   - The deep bootstrap framework that connects offline generalization to online optimization

The data, apart from the training procedure, directly determine the generalization performance of the trained model. Providing a sufficient and representative dataset plays an essential role in training a good and generalizable model. Unfortunately, the limited training data is often all we have to work with, and acquiring new training data comes with an additional cost or is impossible in some cases.

One immediate challenge that arises due to limited data is that the available training data may come from an underlying data generating distribution different from that of the test data. Such distributional difference constitutes *nonstationarity* in the data. When comparing the training and test sets, a nonstationary dataset has different statistical characteristics such as the mean and variance of the design matrix or the target across different parts of the data. The mapping relationship in between may also shift. Training a classifier on data from one distribution and testing it on another thus does not guarantee a good generalization performance in the test set.

The distribution differences could exist in one or more of the following forms:

+   - *domain shift* (different mapping relationship between the input and the output when comparing the training set and the test set)
+   - *covariate shift* (change in the distribution of the input features only)
+   - *target shift* (change in the distribution of the output target only)

In addition, even if the training data comes from the same underlying distribution as the test data, the volume of the training data could be insufficient to support a proper training of a