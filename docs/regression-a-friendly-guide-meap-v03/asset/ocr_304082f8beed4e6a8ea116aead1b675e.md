An obvious novelty of this dataset, relative to the other two we’ve looked at so far in this chapter, is its small size; we’ll see how this affects model assessment later. Even though we have a small dataset, the first stages of model building are the same as always. To investigate how SoreThroat depends on Duration, for instance, we’ll start by computing the null model, fitting the logistic model with glm, and then applying the likelihood ratio test for an initial assessment:

```
pi_null = throat["SoreThroat"].mean()
pi_null

0.6285714285714286

throat_null = np.log( pi_null / (1 - pi_null) )
throat_null

0.5260930958967791

throat_model = smf.glm("SoreThroat ~ Duration", data=throat, fami
throat_fit = throat_model.fit()
throat_fit.params

Intercept  -2.213582
Duration    0.070383
dtype: float64
```

According to the null model based on this data, a patient has a 62.86% chance of having a sore throat after surgery; this is equivalent to a log-odds of 0.526, as shown. If we include Duration as a predictor, the simple logistic model is

log(Odds of sore throat) = −2.213582 + 0.070383 * Duration ,

which says that sore throats are more likely with longer surgeries, since the coefficient of Duration is positive. The likelihood ratio test has a small p-value of around .04% :

```
1 - chi2.cdf(throat_fit.null_deviance - throat_fit.deviance, df=1

0.0004007973384960728
```

This means that there’s strong evidence that this model is better than the null model. We’ll learn other tools for assessing this model in the next chapter,