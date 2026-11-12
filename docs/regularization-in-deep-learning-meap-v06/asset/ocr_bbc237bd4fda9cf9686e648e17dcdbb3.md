```
>>> test = [1,2,3]
>>> transformed_test = softmax(test)
>>> print('softmax output:', transformed_test)
softmax output: [0.09003057 0.24472847 0.66524096]
```

Note that we did not add the softmax transformation to the predicted output at this stage for a reason. The specific cost function provided by PyTorch to be introduced in the next section performs the softmax transformation for us. As long as we provide the output in the raw numeric format, the cost function will handle the transformation as part of the calculation process under the hood.

# 4.3.3 Define the cost function

When the target output is numeric, the loss metric we used in the previous chapters is MSE (mean squared error). Since we are predicting the digits, this is a classification task. One of the most popular loss metrics used for the classification problem is the cross entropy loss (CEL). For the $i^{th}$ observation \(\mathbf{x_i}\), the prediction function would produce a probability output (i.e., the value is in bewteen 0 and 1), denoted as \(p_{i,k} = f(\mathbf{x_i};\mathbf{w})\) to indicate the likelihood of belonging to the \(k^{th}\) class. For a classification task with a total of \(C\) classes, the CEL for \(\mathbf{x_i}\) is defined as \(Q_i(\mathbf{w}) = -\sum^C_{k=1} y_{i,k}log(p_{i,k})\), where \(y_{i,k} = 1\) if the target label for the \(i^{th}\) observation belongs to the \(k^{th}\) class, and \(y_{i,k} = 0\) otherwise.

The summation means that the class-wise evaluation (i.e., the term \(y_{i,k}log(p_{i,k})\)) is performed for all classes and summed together to produce the total cost for the \(i^{th}\) observation. For each observation, there are a total of \(C\) predictions corresponding to the respective probability of belonging to each class. The cost thus aggregates the array of predicted probabilities by summing them into a single number. In addition, the result is negated to produce a positive number, since \(log(x)\) is negative if \(x\) is a probability between 0 and 1.

Note that the target label in the CEL calculation is one-hot encoded. The single label is converted to an array of binary numbers that contains one for the class label and zero otherwise. For example, for a digit image on number eight, instead of passing the original class as the target output, the resulting one-hot encoded target [0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0] would be used, where the eighth position is activated (i.e., hot) and the rest disabled. The target array also has the same length as the probability output array.

Intuitively, we would hope the predicted probability for the correct class to be close to 1 and for the incorrect class to be 0. That is, the loss should increase as the predicted probabilities diverges from the actual class label. The cross entropy loss is designed to follow this intuition. Specifically, we can look at the following four scenarios for the \(i^{th}\) observation:

+   * When the target label belongs to the \(k^{th}\) class (i.e., \(y_{i,k} = 1\)) and the predicted probability for the \(k^{th}\) class is very strong (i.e., \(p_{i,k} \approx 1\)), the cost should be low;
+   * When the target label belongs to the \(k^{th}\) class (i.e., \(y_{i,k} = 1\)) and the predicted probability for the \(k^{th}\) class is very weak (i.e., \(p_{i,k} \approx 0\)), the cost should be high;