# Classifying images with scikit-learn (HOG and logistic regression)

In this recipe, you are going to implement a feature-based image classifier using the scikit-image and scikit-learn library functions. A multiclass logistic regression (softmax regression) classifier will be trained on the histogram of oriented gradients (HOG) descriptors extracted from the training images. The following equations show how the parameters for a K-class softmax regression classifier are estimated in the training phase (for example, with stochastic gradient descent) and then the model that is learned is used to predict the probability of a class label given an input image in the testing phase:

> **Multinomial logit Classifier (Softmax Regression)**
> 
> **Training (parameter estimation)**
> 
> \(J(\theta) = - \left[ \sum_{i=1}^m \sum_{k=1}^K 1 \{y^{(i)} = k\} \log \frac{\exp(\theta^{(k) \top} x^{(i)})}{\sum_{j=1}^K \exp(\theta^{(j) \top} x^{(i)})} \right]\)
> 
> \(\nabla_{\theta^{(k)}} J(\theta) = - \sum_{i=1}^m \left[ x^{(i)} \left( 1\{y^{(i)} = k\} - P(y^{(i)} = k | x^{(i)}; \theta) \right) \right]\)
> 
> SGD: \(\theta = \theta - \alpha \nabla_\theta J(\theta; x^{(i)}, y^{(i)})\)
> 
> **Prediction**
> 
> \(P(y^{(i)} = k | x^{(i)}; \theta) = \frac{\exp(\theta^{(k) \top} x^{(i)})}{\sum_{j=1}^K \exp(\theta^{(j) \top} x^{(i)})}\)

# Getting ready

In this recipe, we will classify images using scikit-learn's implementation of the multiclass logistic regression classifier with the HOG features extracted from the images. Let's start by importing all the required libraries using the following code snippet:

```
%matplotlib inline
import numpy as np
from skimage.io import imread
from skimage.color import gray2rgb
from skimage.transform import resize
from skimage.feature import hog
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
```