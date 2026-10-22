
```
hat{π}_i = 1 / (1 + e^{(-a - b x_i)})
```

so the likelihood L(a, b) really is just a function of a and b— the x_i's and y_i's are all known. All of those products and exponents in L(a, b) are kind of a pain, though, so we do the natural thing (ha!) and take the logarithm of the likelihood:

```
log L(a, b) = log(∏_i hat{π}_i^{y_i} (1 - hat{π}_i)^{1 - y_i}) = ∑_i log(hat{π}_i^{y_i} (1 - hat{π}_i)^{1 - y_i}) = ∑_i [y_i log hat{π}_i + (1 - y_i) log(1 - hat{π}_i)]
```

We’ve repeatedly used the properties of logarithms here, and the end result is crucial— the log-likelihood is

```
log L(a, b) = ∑_i [y_i log hat{π}_i + (1 - y_i) log(1 - hat{π}_i)]
```

This is the function we’re going to maximize to obtain the coefficients. Doing so is equivalent to maximizing the likelihood, since the logarithm is a strictly increasing function, but working with the log-likelihood simplifies the bookkeeping— sums are more convenient than products— and the optimization. This does not mean that maximizing the log-likelihood is easy, because it’s not!

## Coefficients for simple logistic regression

The optimal coefficients a and b maximize the log-likelihood function that we just derived. This raises at least two questions:

- Why does log L(a, b) even have a maximum?
- For what values of a and b does log L(a, b) achieve its maximum?

Unfortunately, neither of these questions has a nice simple answer. To confirm that the log-likelihood has a maximum, we have to apply the appropriate version of the Second Derivative Test— in this case, this means checking that the 2 × 2 matrix of second derivatives, the so-called Hessian matrix, is negative-definite, the analogue of being concave down for a function of 2 unknowns. If you know enough calculus for this comment to

make sense, you can compute the Hessian and check things for yourself; otherwise, try graphing the log-likelihood to see what happens, or just trust that the log-likelihood does indeed have a maximum.

As for the second question, the log-likelihood and its derivatives are *nonlinear* functions of $a$ and $b$, so there are no formulas for the optimal coefficients; iterative numerical methods have to be used to find them. Fortunately, languages like R and Python provide reliable implementations of algorithms for computing logistic regression coefficients, so we don’t have to write our own— which is fortunate, since most of us don’t stand a chance of writing better versions than our favorite languages provide. While we won’t go into the details of how the coefficients are computed, learning more about the methods would be time well spent!

As just mentioned, statsmodels provides the functionality we need to maximize the log-likelihood and thereby fit the logistic curve to our data: we just need to use its glm function again. Here goes:

```
chasm_model = smf.glm("Bud ~ Photoperiod", data=chasm, family=sm.
chasm_fit = chasm_model.fit()
chasm_fit.params
```

Intercept      74.583697
Photoperiod    -5.524989
dtype: float64

We now have our logistic model, which we can state in various ways—abstractly in terms of the variables $x_i$ and $\pi_i$,

$\log\left(\frac{\hat{\Pi}_i}{1-\hat{\Pi}_i}\right)=74.584-5.525x_i,$

or in plain language to emphasize the context of the data:

$\log(\text{Odds of bud formation}) = 74.584 - 5.525 * \text{Photoperiod}.$

As we saw with the NAMCS model, other versions of the model give explicit formulas for the probability or for the odds of bud formation. The former has

the advantage of yielding the probability directly, which is easier to interpret than the odds but has the disadvantage of being unwieldy; the latter expresses the odds as a product and emphasizes the odds ratio, which turns out not to be very useful in this example. (The odds ratio is e^-5.525 = .004, so the odds of bud formation decrease dramatically with a one hour increase in the photoperiod, which we already know from plotting the data.) Of much more importance here is the photoperiod length that distinguishes bud development from being likely to being unlikely, a value best determined from the formula above. It’s good to be comfortable with all of these versions, depending on your audience and what you’re doing. It’s even better to augment these formulas with a visualization of the results, so let’s plot the original data (in orange) with the fitted logistic curve (in blue):

```
chasm["Fit_prob"] = chasm_fit.fittedvalues
chasm
```

```
Photoperiod  Bud  Null_prob  Fit_prob
1  12.833333  1  0.41875  0.975390
3  13.166667  1  0.41875  0.862713
4  13.366667  0  0.58125  0.675466
7  13.716667  0  0.58125  0.231345
9  13.966667  0  0.58125  0.070307
...  ...  ...  ...  ...
311  13.116667  1  0.41875  0.892282
312  13.350000  0  0.58125  0.695317
314  13.666667  0  0.58125  0.284045
316  14.100000  0  0.58125  0.034937
318  14.550000  0  0.58125  0.003004

160 rows × 4 columns
```

```
sns.scatterplot( data=chasm, x="Photoperiod", y="Fit_prob")
sns.lineplot( data=chasm, x="Photoperiod", y="Fit_prob")
sns.scatterplot( data=chasm, x="Photoperiod", y="Bud" )
plt.ylabel("Bud")
plt.title("Bud observations and fitted probabilities")
```

Figure 4.8. Fitting the logistic curve to the violet data summarizes the relationship between Bud and Photoperiod

![](img/f25e2c651d2b8852d1c723f946646ff0_137_0.png)

The logistic curve fits the violet data well, showing clearly that the probability of observing a chasmogamous bud in a plot decreases as the photoperiod gets longer; this corresponds to the fact that the coefficient of Photoperiod, -5.525, is negative. In particular, we see that the odds of bud formation shift from being greater than 1 (probability greater than 50%) to being less than 1 (probability less than 50%) when the photoperiod is about 13.5 hours. We can use the model to confirm this: when the odds of a bud developing equal 1 (i.e., a 50-50 chance), the log of the odds equals zero and 0 = 74.584 −5.25 * Photoperiod ⟹ Photoperiod = 74.584 / 5.525 = 13.5,

as evident on the plot. Exercise 7.1.3 asks you to use this result to construct a simple binary classifier: if the photoperiod is less than 13.5, predict that a bud will be observed; if the photoperiod is greater than 13.5, predict that there won’t be any buds. Following what we did in Section 2 with the NAMCS data, you should evaluate how well this binary classifier performs on the actual observations by computing the success rate, the error rate, and the area under the ROC curve.

## 4.3.3 Comparing the null and logistic models

For both the violet data and the NAMCS data, it’s clear that the logistic models are superior to the null models that ignore the relevant predictors. What we need now is an objective way to quantify the difference in predictive power between these two competing models. While constructing a binary classifier can provide a rough evaluation of the effectiveness of the logistic model, that won’t tell us anything about its performance relative to the null model. In this section, we’ll discuss two approaches to comparing these models: the likelihood ratio test and logistic regression versions of the coefficient of determination.

### Deviances and the likelihood ratio test

Since we fit a logistic model to data by maximizing the likelihood, a natural way to compare models is simply to compare their likelihoods. For the violet data, the likelihood for the fitted model is

```
chasm["Likelihood"] = (chasm.Fit_prob**chasm.Bud)*((1-chasm.Fit_p
chasm
```

| Photoperiod | Bud | Null_prob | Fit_prob | Likelihood |
|---|---|---|---|---|
| 1 | 12.833333 | 1 | 0.41875 | 0.975390 | 0.975390 |
| 3 | 13.166667 | 1 | 0.41875 | 0.862713 | 0.862713 |
| 4 | 13.366667 | 0 | 0.58125 | 0.675466 | 0.324534 |
| 7 | 13.716667 | 0 | 0.58125 | 0.231345 | 0.768655 |
| 9 | 13.966667 | 0 | 0.58125 | 0.070307 | 0.929693 |
| 311 | 13.116667 | 1 | 0.41875 | 0.892282 | 0.892282 |
| 312 | 13.350000 | 0 | 0.58125 | 0.695317 | 0.304683 |
| 314 | 13.666667 | 0 | 0.58125 | 0.284045 | 0.715955 |
| 316 | 14.100000 | 0 | 0.58125 | 0.034937 | 0.965063 |
| 318 | 14.550000 | 0 | 0.58125 | 0.003004 | 0.996996 |

160 rows × 5 columns

```
max_likelihood = chasm["Likelihood"].product()
max_likelihood
```

3.532480672759031e-18

which is larger than the null model’s likelihood by a factor of

```
max_likelihood / null_likelihood
```

6.185161199884674e+29

The observations are therefore much more likely under the logistic model than the null model, but this number is too large to work with, just as likelihoods are too small to interpret sensibly. How do we deal with such astronomically small and large numbers? Take their logarithms! The log of max_likelihood is precisely the log-likelihood for the fitted model:

```
np.log(max_likelihood)
```

-40.1845313095955

One annoying thing about this number is that it’s negative— for various reasons, we usually like to quantify things with positive numbers. When dealing with log-likelihoods, we fix this by multiplying by -2, just as we did earlier to compute the null deviance; when applied to the log-likelihood of the logistic model, we get the residual deviance (also known as the model deviance):

```
residual_deviance = -2*np.log(max_likelihood)
residual_deviance
```

80.369062619191

Not surprisingly, chasm_fit includes this information— we just need to print its summary:

```
print( chasm_fit.summary() )
```

Generalized Linear Model Regression Results
