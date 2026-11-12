We will use these chained gradients to calculate the slope for the cost function C with respect to W1, B1, W2, and B2.

# # AUTOMATIC DIFFERENTIATION

As you can see, untangling derivatives even with the chain rule and symbolic libraries like SymPy is still tedious. This is why differentiable programming libraries are rising, such as the JAX library made by Google. It is nearly identical to NumPy except it allows calculating derivatives on parameters packaged as matrices.

If you want to learn more about automatic differentiation, this YouTube video does a great job explaining it.

https://youtu.be/wG_nFlawSSY

# # Stochastic Gradient Descent

We are ow ready to integrate the chain rule to perform stochastic gradient descent. To keep things simple, we are going to only sample one training record on every iteration. Batch and mini-batch gradient descent are commonly used in neural networks and deep learning, but there’s enough linear algebra and calculus to juggle just one sample per iteration.

Let’s take a look at our full implementation of our neural network, with backpropogated stochastic gradient descent, in Example 7-11.

# # Example 7-11. Implementing a Neural Network using Stochastic Gradient Descent

```
```python
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split

all_data = pd.read_csv("https://tinyurl.com/y2qmhfsr")

# Learning rate controls how slowly we approach a solution
# Make it too small, it will take too long to run.
# Make it too big, it will likely overshoot and miss the solution.
L = 0.05

# Extract the input columns, scale down by 255
all_inputs = (all_data.iloc[:, 0:3].values / 255.0)
all_outputs = all_data.iloc[:, -1].values

# Split train and test data sets
X_train, X_test, Y_train, Y_test = train_test_split(all_inputs, all_outputs,
test_size=1 / 3)
n = X_train.shape[0]

# Build neural network with weights and biases
```
```