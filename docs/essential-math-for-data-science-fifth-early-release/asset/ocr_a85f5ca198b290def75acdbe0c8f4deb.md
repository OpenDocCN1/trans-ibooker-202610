so it converges on the right answer approximately, but not so much that it wastes computation time when it already found an acceptable solution.

One other question you may have is why I started the `best_loss` at an extremely large number. I did this to initialize the best loss with a value I know will be overwritten once the search starts, and it will then be compared to the new loss of each iteration to see if it results in an improvement. I could have also used positive infinity `float( 'inf' )` instead of a very large number.

# # Hill Climbing and Logistic Regression

Just like in the previous example with linear regression, we can also apply hill climbing to logistic regression. Again use this technique if you find the calculus and partial derivatives to be too much at once.

The hill climbing methodology is identical: adjust m and b with random values from a normal distribution. However we do have a different objective function, the maximum likelihood estimation, as discussed in Chapter 6. Therefore we only take random adjustments that increase the likelihood estimation, and after enough iterations we should converge on a fitted logistic regression.

This is all demonstrated in Example A-10.

_Example A-10. Using hill climbing for a simple logistic regression._

```
import math
import random
import numpy as np
import pandas as pd

# Desmos graph: https://www.desmos.com/calculator/6cb10atg3l
points = [p for p in pd.read_csv("https://tinyurl.com/y2coco07").itertuples()]
best_likelihood = -10_000_000
b0 = .01
b1 = .01

# calculate maximum likelihood
def predict_probability(x):
    p = 1.0 / (1.0001 + math.exp(-(b0 + b1 * x)))
    return p

for i in range(1_000_000):
```