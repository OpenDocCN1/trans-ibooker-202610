```
cohorts_null_model = sm.GLM( cohorts[["Successes","Failures"]],
    cohorts["Bias"], i
    family=sm.families.Binomial())
cohorts_null_fit = cohorts_null_model.fit()
print(cohorts_null_fit.summary())
```

# Generalized Linear Model Regression Results

Dep. Variable:       ['Successes', 'Failures']   No. Observations:
Model:                            GLM               Df Residuals:
Model Family:                Binomial             Df Model:
Link Function:                 logit               Scale:
Method:                          IRLS               Log-Likelihood:
Date:                Sun, 24 Oct 2021             Deviance:
Time:                        12:51:42             Pearson chi2:
No. Iterations:                    4
Covariance Type:        nonrobust

|          | coef   | std err | z      | P>|z| | [0.0 |
|----------|--------|---------|--------|-------|------|
| Bias     | 0.3096 | 0.011   | 27.118 | 0.000 | 0.2  |

This is actually the same as the null model we found in the last chapter—to confirm this, we just have to convert this result to the sample proportion we found then:

log(Odds of condition) ≈ 0.3096 ⇒ Odds of condition ≈ 1.363 ⇒ Probability of condition ≈ 0.577.

After all this work to establish an alternative theoretical framework, it may be underwhelming to see that this binomial null model is the same as before, but it shouldn’t be. After all, we’re using the same data and proposing the same functional relationship between the odds of having a condition and age; the differences are in how we package the data and how we analyze the results. It should now come as no surprise to see that the best fitting binomial logistic model incorporating Age is the same, too:

```
cohorts_model = sm.GLM( cohorts[["Successes","Failures"]],
    cohorts[["Bias","Age"]],
    family=sm.families.Binomial())
cohorts_fit = cohorts_model.fit()
print(cohorts_fit.summary())
```