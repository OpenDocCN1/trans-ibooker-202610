# Listing 7.1 Load the necessary packages

```
import math, argparse
import numpy as np
from numpy import random
import torch
import torch.nn as nn
from sklearn import datasets
from sklearn.model_selection import train_test_split
import matplotlib.pyplot as plt

SEED = 8 #A
random.seed(SEED)
torch.manual_seed(SEED)
np.random.seed(SEED)
```

#A set seed to ensure reproducibility

Next, we generate a dummy dataset consisting of 100 observations and 20 features, with a noise of 80 as the standard deviation of the Gaussian noise applied to the output using the `make_regression()` function. Note that this function has a parameter called `n_informative`, representing the number of valuable features used to generate the output. The default value for this parameter is 10. Since we are creating a dataset with 20 features, this effectively means that only half of these features are useful.

Then we apply the bias trick and add a column of ones to the design matrix `X` using the `np.c_()` function. The dataset is then split into training and test sets using the `train_test_split()` function in `sklearn`. The observation and feature dimensions are stored in `n_samples` and `n_features` respectively.

# Listing 7.2 Generate dummy dataset

```
X, y = datasets.make_regression(n_samples=100, n_features=20, noise=80, random_state=SEED) #A
X = np.c_[np.ones(X.shape[0]), X] #B
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=SEED) #C
n_samples, n_features = X_train.shape
```

#A prepare dummy data
#B apply the bias trick
#C train test split

For the first part of the implementation, we will focus on the coordinate-wise gradient descent approach mentioned earlier. We will look at the results of vanilla least squares, and then compare them with the regularized version of the same model using ridge regression.

# LINEAR REGRESSION USING COORDINATE-WISE GRADIENT DESCENT

To start, let us create a class called `GD_regression` to host a list of attributes and methods used for model estimation. In the following cost listing, we include input arguments such as the learning rate `lr`, number of training epochs `num_epochs`, and the scaling factor for the `l2` penalty `l2_penalty` in the initialization method. We also include two arrays to store the training and test set losses across different epochs respectively.