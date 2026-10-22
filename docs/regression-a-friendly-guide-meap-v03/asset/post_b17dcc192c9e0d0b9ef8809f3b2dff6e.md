
808                Zapteryx xyster    78.0    4264.716085          1

809 rows × 4 columns

Response values of 1 and 0 indicate high and low risks of extinction, respectively; this is the same binary grouping used by Ripple et al. [14] in their extensive analysis of extinction risks for all vertebrates, not just cartilaginous fish. Having prepared the binary response variable, fitting the logistic model is straightforward:

```
sharks["LogWeight"] = np.log(sharks["Weight"])
sharks_model = smf.glm("Threatened ~ LogWeight", data=sharks, family=sm.families.Binomial())
sharks_fit = sharks_model.fit()
sharks_fit.params
```

Intercept    -3.173571
LogWeight     0.293120
dtype: float64

We now have the usual array of options for expressing the fitted model. In terms of probabilities and the logistic function σ(·), the model is

Probability of being threatened = σ(−3.173571 + 0.293120 * log(Weight)),

which is easy to visualize with regplot:

```
sns.regplot(data=sharks, x="LogWeight", y="Threatened", logistic=True)
```

Figure 4.12. Visualizing the sharks data with the fitted logistic model

![](img/f25e2c651d2b8852d1c723f946646ff0_153_0.png)

The blue logistic curve shows how the fitted probabilities of endangerment vary with the logarithm of the species weights. The log-odds formulation of the logistic model is

log(Odds of being threatened) = -3.173571 + 0.29312 * log(Weight),

which we can render more palatable by exponentiating and eliminating the logarithms:

Odds of being threatened = e^{-3.17357} * e^{0.29312 * log(Weight)} = 0.0419 * Weight^{0.29312}.

This last form of the model perhaps shows most clearly that larger

cartilaginous fish are more likely to be threatened and have higher extinction risks. According to the likelihood ratio test, this model is definitely better than the null model:

```
1 - chi2.cdf(sharks_fit.null_deviance - sharks_fit.deviance, df=1)
```

```
8.770761894538737e-15
```

This astronomically small p-value reflects the increased predictive power gained by incorporating LogWeight into the model. But how good is this model, really? An easy way to check, using only what we’ve discussed so far, is to convert the fitted logistic model into a binary classifier. Here’s what we get if we use even odds (probability 50%) as the classification threshold:

```
sharks["Class"] = (sharks_fit.fittedvalues > 0.50).astype(int)
sharks
```

```
pd.crosstab(sharks["Threatened"], sharks["Class"])
```

Class 0 1
Threatened
0 492 37
1 211 69

```
np.mean( sharks["Threatened"] == sharks["Class"] )
```

```
0.6934487021013597
```

With a success rate just under 70%, this classifier certainly does better than random guessing, but it seems likely that a more effective model could be built using more information (e.g., additional predictor variables). Of particular concern is the high false negative rate: this classifier puts 211 of the threatened fish into the nonthreatened category, a mistake that could have disastrous policy implications if it went unexamined. Of course, the problem might be the threshold used to construct this classifier— let’s check out the ROC curve to investigate this:

```
fpr, tpr, thresholds = metrics.roc_curve(sharks["Threatened"], sharks_fit.fittedvalues)
chronic_auc = metrics.auc(fpr, tpr)
chronic_auc
```

```
0.6589994598973805
```

```
plt.figure()
plt.plot(fpr, tpr, label='ROC curve    AUC: %0.2f' % chronic_auc)
plt.plot([0,1], [0,1], 'r--', label='Random classification')
# plt.xlim([0, 1])
# plt.ylim([0, 1.05])
plt.xlabel('False Positive Rate (1-Specificity)')
plt.ylabel('True Positive Rate (Sensitivity)')
plt.title('ROC curve for shark extinction risk classifier')
plt.legend(loc="lower right")
```

Figure 4.13. Extinction risk classifiers based on the simple logistic model

![](img/f25e2c651d2b8852d1c723f946646ff0_155_0.png)

Again, this is better than random guessing, but it would be nice to have a

better classifier for such an important problem. More on this in later chapters!

## 4.5 Exercises

+   1. For the NAMCS logistic model with ungrouped responses:
+      1. Compute the likelihood ratio and conduct the likelihood ratio test.
+      2. Compute and compare $R_M^2$, $R_S^2$, and $R_D^2$.
+      3. Look at the histograms related to the coefficient of discrimination.
+   2. For the simple logistic model for chasmogamous bud development:
+      construct a binary classifier, compute its error and success rates, plot the associated ROC curve, and determine its AUC.
+   3. Use the violet data to build and assess a logistic model that relates the probability of *cleistogamous* bud formation to photoperiod length.
+   4. For Collett’s throat data:
+      1. Construct and analyze a binary classifier.
+      2. Construct a model that excludes the surgery that lasted 135 minutes, and compare it to the model that includes this outlier.
+   5. Sources for some interesting exercises:
+      1. NHANES
+      2. heart failure example from UCI-ML repository
+      3. geohazard risk example (e.g., sinkholes in Florida, landslides in Mexico)

## 4.6 Summary

+   - Simple logistic regression uses the logistic curve to relate the probability of success (outcome=1) to the value of the predictor variable.
+   - Logistic models can be expressed in terms of probabilities, odds, or the logarithm of the odds; all three formulations are useful.
+   - To see if a logistic model is appropriate for a small dataset, plot the raw binary responses and look for a natural separation between successes (1s) and failures (0s).
+   - Use the likelihood ratio statistic $G^2$ to compare the null model and the logistic model; for simple logistic regression, $G^2$ has a $\chi^2$ distribution with 1 degree of freedom.
+   - Use the logistic analogues of the coefficient of determination ($R_M^2$, $R_S^2$, and $R_D^2$) to compare the predictive power of the null and logistic models.

[12] Sternberger et al., "Environmental impact on the temporal production of chasmogamous and cleistogamous flowers in the mixed breeding system of *Viola pubescens*", PLOS One 15 (3): e0229726

[13] McCullagh and Nelder, *Generalized Linear Models*; Agresti, *Categorical Data Analysis*

[14] Ripple et al., "Extinction risk is most acute for the world’s largest and smallest invertebrates", PNAS October 3, 2017 114 (40) 10678-10683

## 5 The simple logistic regression model

### This chapter covers

+   - Understanding the theory behind binary and binomial logistic regression
+   - Computing and interpreting interval estimates for parameters and predictions
+   - Assessing the fit of simple logistic regression models with deviances and residuals

When working with a binary response variable, we know a priori—assuming the standard numerical encoding—that the only possible values of the response are 0 and 1, but there’s no way to know which outcome will occur before making an observation. While this inherent randomness is built into the models that we considered in the last chapter, understanding the practical implications of all of this uncertainty requires a thorough theoretical foundation. That’s what we’ll establish in this chapter! After explaining the ins and outs of Bernoulli random variables, we’ll explain how to draw inferences about model parameters and predictions, and we’ll gain more insight into the role of the deviance in assessing the fit of a model. These tasks are similar to what we did in Chapter 3 with simple linear regression models, but the nonlinearity of the logistic function levels up the technical complexity, giving you plenty of new things to think about.

As we saw at the beginning of the last chapter with the NAMCS data, grouping responses into multiple categories can quickly reveal important relationships among variables. When there’s enough data to do this in a reasonable way, there are better tools for interrogating and assessing the model than when working with binary responses. Specifically, it helps to view the data as a collection of binomial experiments, so we need to understand binomial distributions, binomial random variables, and binomial logistic regression models. Once we’ve developed that probabilistic machinery, we’ll explain the deviance properly and learn to analyze residuals
