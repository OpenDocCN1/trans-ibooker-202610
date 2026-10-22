318    14.550000    0    0.58125  0.003004    0.996996

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