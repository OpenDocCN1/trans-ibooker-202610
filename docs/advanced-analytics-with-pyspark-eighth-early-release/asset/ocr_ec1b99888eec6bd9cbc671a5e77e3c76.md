evaluate a metric for each of them, and choose the combination that produces the best value of the metric.

In the following example, eight possible combinations are tried: rank = 5 or 30, regParam = 4.0 or 0.0001, and alpha = 1.0 or 40.0. These values are still something of a guess, but are chosen to cover a broad range of parameter values. The results are printed in order by top AUC score:

```
from pprint import pprint
from itertools import product

ranks = [5, 30]
reg_params = [4.0, 0.0001]
alphas = [1.0, 40.0]
hyperparam_combinations = list(product(*[ranks, reg_params,
alphas]))

evaluations = []

for c in hyperparam_combinations:
    rank = c[0]
    reg_param = c[1]
    alpha = c[2]
    model =
ALS().setSeed(0).setImplicitPrefs(true).setRank(rank).setRegParam
(reg_param).setAlpha(alpha).setMaxIter(20).setUserCol("user").set
ItemCol("artist").setRatingCol("count").setPredictionCol("predict
e").\
    fit(trainData)

    auc = area_under_curve(cv_aata, b_all_artist_ids,
model.transform)

    model.userFactors.unpersist() ①
    model.itemFactors.unpersist()

    evaluations.append((auc, (rank, regParam, alpha)))

evaluations.sort(key=lambda x:x[0], reverse=True) ②
pprint(evaluations)
```

...
(0.8928367485129145, (30, 4.0, 40.0))
(0.891835487024326, (30, 1.0E-4, 40.0))
(0.8912376926662007, (30, 4.0, 1.0))