![](bbox=[0.185, 0.135, 0.905, 0.36])

Figure 5.16 Comparing soft classification curves of training and test sets using LeNet and LeNet_FC trained on CIFAR-5m.

# 5.4 Summary

+   - Regularization via data refers to a series of data-based methods that help build a regularized model
+   - Transformations on data can be applied to the input matrix, the output, the hidden layer activations in deep neural networks, and a combination of these components
+   - When transforming an input matrix for regularization purpose, horizontal treatments include manual feature engineering and automatic feature extraction, and vertical treatments include repeated sampling (bootstrap and oversampling) and data augmentation
+   - Label smoothing is a technique that smooths the “hard” one-hot encoded target to a “soft” target perturbed by a uniformly distributed noise that is controlled by a trade-off hyperparameter
+   - Data augmentation aims at generating artificial samples by applying slight transformations to the original data; it is the de facto data pre-processing step in modern computer vision tasks
+   - Data augmentation helps regularize the model training process and build a more generalized model in image classification using CIFAR10
+   - The deep bootstrap framework provides an alternative perspective on generalization by connecting the offline generalization performance in the real world with online optimization speed in the ideal world