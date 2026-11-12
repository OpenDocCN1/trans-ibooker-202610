three accuracies with an average and standard deviation.

# _Example 6-16. Performing a logistic regression with 3-fold cross validation_

```
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import KFold, cross_val_score

# Load the data
df = pd.read_csv("https://tinyurl.com/y6r7qjrp", delimiter=",,")

X = df.values[:, :-1]
Y = df.values[:, -1]

# "random_state" is the random seed, which we fix to 7
kfold = KFold(n_splits=3, random_state=7, shuffle=True)
model = LogisticRegression(penalty='none')
results = cross_val_score(model, X, Y, cv=kfold)

print("Accuracy Mean: %.3f (stdev=%.3f)" % (results.mean(), results.std()))
```

We can also use random-fold validation, leave-one-out cross validation, and all the other folding variants we performed in Chapter 5. That out of the way, let’s talk about why accuracy is a bad measure for classification.

# Confusion Matrices

Suppose a model observed people with the name “Michael” quit their job. The reason why first and last name are captured as input variables is indeed questionable, as it is doubtful someone’s name has any impact on whether they quit. However, to simplify the example, let’s go with it. The model then predicts that any person named “Michael” will quit their job.

Now this is where accuracy falls apart. I have 100 employees, including one named “Michael” and another named “Samantha”. Michael is wrongly predicted to quit, and it is Samantha that ends up quitting. What’s the accuracy of my model? It is 98% because there were only 2 wrong predictions out of 100 employees as visualized in Figure 6-17.