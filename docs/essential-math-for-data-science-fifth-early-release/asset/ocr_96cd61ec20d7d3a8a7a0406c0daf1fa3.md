# Example 6-18. Using the AUC as the Scikit-learn parameter:

```
# put Scikit_learn model here
results = cross_val_score(model, X, Y, cv=kfold, scoring='roc_auc')
print("AUC: %.3f (%.3f)" % (results.mean(), results.std()))
# AUC: 0.791 (0.051)
```

# Class Imbalance

There is one last thing to cover before we close this chapter. As we saw earlier when discussing confusion matrices, **class imbalance**, which happens when data is not equally represented across every outcome class, is a problem in machine learning. Unfortunately many problems of interest are imbalanced such as disease prediction, security breaches, fraud detection, and so on. Class imbalance is still an open problem with no great solution. However, there are a few techniques you can try.

First you can do obvious things like collect more data or try different models, as well as utilize confusion matrices and ROC/AUC curves. All of this will help track poor predictions and pro-actively catch errors.

Another common technique is to duplicate samples in the minority class until it is equally represented in the dataset. You can do this in Scikit-learn as shown in Example 6-19 when doing your train-test splits. Pass the `stratify` option with the column containing the class values, and it will attempt to equally represent the data of each class.

# Example 6-19. Using the “stratify” option in Scikit-learn to balance classes in the data

```
X, Y = ...
X_train, X_test, Y_train, Y_test = \\
train_test_split(X, Y, test_size=.33, stratify=Y)
```

There are also a family of algorithms called SMOTE, which generate synthetic samples of the minority class. What would be most ideal though is to tackle the problem in a way that uses anomaly detection models, which are deliberately designed for seeking out a rare event. These seek outliers however, and are not necessarily a classification since they are unsupervised algorithms. All these techniques are beyond the scope of this book but are worth mentioning as they *might* provide better solutions to a given problem.

# Conclusions

Logistic regression is the workhorse model for predicting probabilities and classifications on data. Logistic regressions can predict more than one category rather than just a true/false. You just build separate logistic regressions modeling whether or not it belongs to that category, and the model that produces the highest probability is the one that wins. You may discover that Scikit-learn, for the most part, will do this for you and detect when your data has more than two classes.