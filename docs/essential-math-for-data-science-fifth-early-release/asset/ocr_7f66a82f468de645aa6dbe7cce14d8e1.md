# Chapter 6

1. The accuracy is extremely high when you run this through Scikit-Learn. When I run it get at least 99.9% accuracy on average with the test folds.

```
Example B-21.
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import confusion_matrix
from sklearn.model_selection import KFold, cross_val_score
# Load the data
df = pd.read_csv("https://bit.ly/3imidqa", delimiter=",")
X = df.values[:, :-1]
Y = df.values[:, -1]
kfold = KFold(n_splits=3, shuffle=True)
model = LogisticRegression(penalty='none')
results = cross_val_score(model, X, Y, cv=kfold)
print("Accuracy Mean: %.3f (stdev=%.3f)" % (results.mean(), results.std()))
```

2. The confusion matrix will yield an extremely high number of true positives and true negatives, and very few false positives and false negatives. Run this code and you will see.

```
Example B-22.
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import confusion_matrix
from sklearn.model_selection import train_test_split
# Load the data
df = pd.read_csv("https://bit.ly/3imidqa", delimiter=",")
# Extract input variables (all rows, all columns but last column)
X = df.values[:, :-1]
# Extract output column (all rows, last column)\
Y = df.values[:, -1]
```