
![](img/ee98e91e6845f41077b163bec833ee40_142_0.png)

Figure 4.22 Good and bad solution zones in the training and test error curves. There are multiple solutions where the weights can interpolate the training data and produce zero misclassification rate. However, solutions living within the wide and flat region tend to better generalize to the test set as represented by a lower misclassification rate.

### 4.4.2 Best versus worst global minimum

We have learned so far regarding the solution space (model weights upon convergence) that there are multiple global minima when the model is trained to interpolate the training data given a fixed model complexity and across different levels of model complexity. Out of these many equally good solutions, some tend to generalize well to the test set, for example, via implicit regularization when using SGD. Some tend to generalize poorly due to overfitting. Suppose we were to characterize the solution space of global minima for a fixed level of model complexity. In that case, we can obtain an upper bound (i.e., worst global minimum) and a lower bound (i.e., best global minimum). The upper bound would be the subject of study under the classical generalization theory that highlights a high test error due to overfitting. On the other hand, the lower bound reflects the modern generalization theory that highlights a low test error due to implicit or explicit regularization.

Let us look at this perspective more closely. Figure 4.23 illustrates the upper and lower bound of the test error as a function of the model complexity, plus the training error as a reference. The training error decreases as the model becomes complex under both classical and modern generalization views. However, the behaviors differ in the test error. The test error obtained under the classical generalization theory forms the upper bound of all possible test errors for a given level of model complexity due to overfitting. The increasing test error shows the importance of regularizing the (effective) model complexity to control for the excessive sensitivity during model fitting. The lower bound is the empirically observed best global minimum across different levels of model complexity. We can obtain the lower bound using implicit regularization (i.e., SGD), explicit regularization (i.e., penalizing the norm of the weights), or both. We also notice that the gap between the upper and lower bound of the possible test errors increases as the model becomes complex.

![](img/ee98e91e6845f41077b163bec833ee40_143_0.png)

Figure 4.23 The worst and best global minimum across different levels of model complexity. The upper bound is the worst global minima obtained under classical generalization theory when no regularization mechanism is used. The lower bound serves as the best obtainable global minima under the modern generalization theory. The gap between the upper bound and the lower bound increases as the model becomes complex.

When the model is less complex and small in size, we will obtain a unique or very similar global minimum in the test error as the model is trained to minimize the training error. However, the gap increases between the best and worst global minimum as the model becomes flexible and complex. It is thus essential to apply proper regularization for large and complex models such as deep neural networks to obtain the best possible global minimum and generalize well to the future unseen test set.

## 4.5 Summary

- Perceptrons as simple neural networks are powerful functional approximators based on embedded and repeated use of matrix multiplication and activation function
- Deep networks are more efficient than wide networks due to an exponentially growing flexibility in functional approximation
- Backward propagation is more computationally efficient than forward propagation and is a mainstream automatic differentiation mechanism offered in PyTorch
- A customized neural network class can be defined by specifying the `__init__()` function and the `forward()` function
- There are multiple global minima for a given level and across different levels of model complexity
- The best global minima is obtained via proper regularization (either explicit, implicit or a combination of both) under the modern generalization theory, while the worst global minima is obtained without regularization due to overfitting under the classical generalization theory
- The gap between best and worst global minima increases as the model becomes increasingly complex and flexible

## 5 Regularization via Data

This chapter covers

- Common challenges in the data and the need for data augmentation
- Different data augmentation techniques that contribute to regularized model training
- Applying data augmentation in image classification to boost training performance
- The deep bootstrap framework that connects offline generalization to online optimization

The data, apart from the training procedure, directly determine the generalization performance of the trained model. Providing a sufficient and representative dataset plays an essential role in training a good and generalizable model. Unfortunately, the limited training data is often all we have to work with, and acquiring new training data comes with an additional cost or is impossible in some cases.

One immediate challenge that arises due to limited data is that the available training data may come from an underlying data generating distribution different from that of the test data. Such distributional difference constitutes *nonstationarity* in the data. When comparing the training and test sets, a nonstationary dataset has different statistical characteristics such as the mean and variance of the design matrix or the target across different parts of the data. The mapping relationship in between may also shift. Training a classifier on data from one distribution and testing it on another thus does not guarantee a good generalization performance in the test set.

The distribution differences could exist in one or more of the following forms:

- *domain shift* (different mapping relationship between the input and the output when comparing the training set and the test set)
- *covariate shift* (change in the distribution of the input features only)
- *target shift* (change in the distribution of the output target only)

In addition, even if the training data comes from the same underlying distribution as the test data, the volume of the training data could be insufficient to support a proper training of a good classifier, making it challenging to capture the weak signal embedded in the data.

These challenges will likely compromise the quality of the trained model based on the existing data. We would need to transform and tweak the training data so that its distribution better approximates the test set.

In this chapter, we will start with common data-based methods that could help strengthen our training dataset, followed by practical examples of how to apply these techniques to improve generalization performance. We refer to these techniques as regularization via data, since they represent data-based transformations that regularize the final trained model in a way that improves its generalizability.

### 5.1 Data-based methods

Data-based methods refer to a series of transformations applied to the data, including the input design matrix, the output target, the hidden layer activations when using deep neural networks, and a combination of these components. Such transformations can be row-wise or column-wise. Since the model needs to fit a wide range of data after transformation, it is less likely for the model to become overfitting and overconfident about its predictions. Let us start with the design matrix to get a sense of how we could transform the data.

Imagine we have an input dataset **X** ∈ R<sup>n×p</sup>, i.e., a matrix with **n** rows and **p**. We could perform the horizontal treatment by adding more informative and predictive features, which effectively transforms and increases the original space of **p** features. This can be achieved by manual feature engineering, such as adding basis functions as additional features as we saw in chapter 2 or using automatic feature extraction through hidden layers in a deep neural network. The latter may involve a larger or smaller feature space at a specific hidden layer than the original input layer. When mapping/extracting the original feature space to a new representation in the hidden layer, the learned features tend to be more disentangled and thus more conducive to the current learning task.

We could also introduce vertical treatment by appending more artificial examples to **X**, forming a larger and possibly infinite augmented training dataset **X**<sup>∗</sup>. To enlarge the sample space, we could strategically sample the original dataset based on the specific type of problem at hand. For example, **oversampling** the minority group with replacement from **X** could often effectively combat the problem of class imbalance and lead to better models. By boosting the presence of the minority samples using repeated sampling, this technique provides an enlarged dataset that better fits training, despite the data distribution being deliberately different from the ground truth distribution. Note that **undersampling** could also reduce the presence of the majority group and strengthen the signal of the minority, thus playing a regularizing effect.

Alternatively, one can also use **bootstrap sampling** to repeatedly draw samples from the original dataset with replacement, creating multiple copies of the dataset, each containing a different number of repeated samples. These datasets can then be used to create multiple individual models, whose predictions would then get aggregated to produce the final prediction. This form of **ensemble learning** is called **bootstrap aggregation** (or **bagging**), which helps reduce overfitting and improve model stability.
