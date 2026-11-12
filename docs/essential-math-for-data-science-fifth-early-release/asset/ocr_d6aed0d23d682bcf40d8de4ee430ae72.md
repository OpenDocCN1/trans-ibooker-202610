surprising problems including scheduling constrained resources (like workers, server jobs, or rooms), solving Sudokus, and optimizing financial portfolios.

If you want to learn more, there are some good YouTube videos out there including PatrickJMT and Josh Emmanuel. If you want to deep dive into discrete optimization, Professor Pascal Van Hentenryck has done a tremendous service putting a course together on Coursera.

# # MNIST Classifier Using Scikit-learn

Example A-13 shows how to use Scikit-learn's neural network for classification.

```
Example A-13.
import numpy as np
import pandas as pd
# load data
from sklearn.model_selection import train_test_split
from sklearn.neural_network import MLPClassifier
df = pd.read_csv('https://bit.ly/3ilJc2C', compression='zip', delimiter=",")
# Extract input variables (all rows, all columns but last column)
# Note we should do some linear scaling here
X = (df.values[:, :-1] / 255.0)
# Extract output column (all rows, last column)
Y = df.values[:, -1]
# Get a count of each group to ensure samples are equitably balanced
print(df.groupby(["class"]).agg({"class": [np.size]}))
# Separate training and testing data
# Note that I use the 'stratify' parameter to ensure
# each class is proportionally represented in both sets
X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=.33,
random_state=10, stratify=Y)
nn = MLPClassifier(solver='sgd',
hidden_layer_sizes=(100, ),
activation='logistic',
max_iter=480,
learning_rate_init=.1)
nn.fit(X_train, Y_train)
print("Training set score: %f" % nn.score(X_train, Y_train))
print("Test set score: %f" % nn.score(X_test, Y_test))
# Display heat map
import matplotlib.pyplot as plt
fig, axes = plt.subplots(4, 4)
```