as evident on the plot. Exercise 7.1.3 asks you to use this result to construct a simple binary classifier: if the photoperiod is less than 13.5, predict that a bud will be observed; if the photoperiod is greater than 13.5, predict that there won’t be any buds. Following what we did in Section 2 with the NAMCS data, you should evaluate how well this binary classifier performs on the actual observations by computing the success rate, the error rate, and the area under the ROC curve.

# 4.3.3 Comparing the null and logistic models

For both the violet data and the NAMCS data, it’s clear that the logistic models are superior to the null models that ignore the relevant predictors. What we need now is an objective way to quantify the difference in predictive power between these two competing models. While constructing a binary classifier can provide a rough evaluation of the effectiveness of the logistic model, that won’t tell us anything about its performance relative to the null model. In this section, we’ll discuss two approaches to comparing these models: the likelihood ratio test and logistic regression versions of the coefficient of determination.

# Deviances and the likelihood ratio test

Since we fit a logistic model to data by maximizing the likelihood, a natural way to compare models is simply to compare their likelihoods. For the violet data, the likelihood for the fitted model is

```
chasm["Likelihood"] = (chasm.Fit_prob**chasm.Bud)*((1-chasm.Fit_p
chasm
```

Photoperiod Bud Null_prob Fit_prob Likelihood
1 12.833333 1 0.41875 0.975390 0.975390
3 13.166667 1 0.41875 0.862713 0.862713
4 13.366667 0 0.58125 0.675466 0.324534
7 13.716667 0 0.58125 0.231345 0.768655
9 13.966667 0 0.58125 0.070307 0.929693
311 13.116667 1 0.41875 0.892282 0.892282
312 13.350000 0 0.58125 0.695317 0.304683
314 13.666667 0 0.58125 0.284045 0.715955
316 14.100000 0 0.58125 0.034937 0.965063