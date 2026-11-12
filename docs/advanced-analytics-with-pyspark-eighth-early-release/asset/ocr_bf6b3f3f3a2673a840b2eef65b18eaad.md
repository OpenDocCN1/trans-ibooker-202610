The above output contains a lot of information about the fitted model, but it also tells us that “entropy” apparently worked best as the impurity measure and that a max depth of 20 was not surprisingly better than 1. It might be surprising that the best model was fit with just 40 bins, but this is probably a sign that 40 was “plenty” rather than “better” than 300. Lastly, no minimum information gain was better than a small minimum, which could imply that the model is more prone to underfit than overfit.

You may wonder if it is possible to see the accuracy that each of the models achieved for each combination of hyperparameters. The hyperparameters as well as the evaluations are exposed by getEstimatorParamMaps and validationMetrics, respectively. They can be combined to display all of the parameter combinations sorted by metric value:

```
validator_model = validator.fit(train_data)

metrics = validator_model.validationMetrics
params = validator_model.getEstimatorParamMaps()
metrics_and_params = list(zip(metrics, params))

metrics_and_params.sort(key=lambda x: x[0], reverse=True)
metrics_and_params
```

```
...
[(0.9130409881445563,
 {Param(...name='minInfoGain' ...)): 0.0,
 Param(...name='maxDepth' ...)): 20,
 Param(...name='maxBins' ...)): 40,
 Param(...name='impurity' ...)): 'entropy'}),
(0.9112655352131498,
 {Param(...name='minInfoGain',...)): 0.0,
 Param(...name='maxDepth' ...)): 20,
 Param(...name='maxBins' ...)): 300,
 Param(...name='impurity' ...)): 'entropy'}),
...
```

What was the accuracy that this model achieved on the CV set? And finally, what accuracy does the model achieve on the test set?

```
metrics.sort(reverse=True)
print(metrics[0])
...
```