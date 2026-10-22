Skew: 0.119 Prob(JB):
Kurtosis: 1.978 Cond. No.
==============================================

Notes:
[1] Standard Errors assume that the covariance matrix of the er

The std err column in the middle provides the standard errors for the regression coefficients, and the numbers shown are exactly what the formulas give, as you can check. To use these standard errors, we have to deal with one more thing: the price we pay for using the RSE as our estimate of σ is that the distribution we need to work with is no longer normal— we have to use the t-distribution with (n − 2) degrees of freedom instead. If we knew the value of β, for example, and had all possible sets of responses to our fixed values of the predictor, we could compute the quantity

t = (b − β) / SE(b)

for each set of responses; the resulting t-scores would follow the distribution t(n − 2).

We can finally proceed to estimating β and α, by manipulating and modifying the inequalities above. The multiplier 2 that appears in them comes from the standard normal distribution: 95% of z-scores are within 2 standard deviations of the mean. In fact, the multiplier is actually closer to 1.96, as we can check using the norm.ppf function from the scipy.stats library:

```
>>> import scipy.stats as st
>>> st.norm.ppf(.975 )

    1.959963984540054
```

norm.ppf takes a percentile, 97.5% here, and returns the z-score with that percentile rank; since we want the middle 95% of z-scores, we add on the 2.5% in the left tail to get the correct percentile. We now have to replace this multiplier with the analogous quantity for the t-distribution t(n − 2)— we have to determine the middle 95% of t-scores when there are (n − 2) degrees of freedom, which we can do with the t.ppf function. For the UNICEF data,