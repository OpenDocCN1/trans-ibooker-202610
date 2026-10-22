
for logistic models.

Since this chapter builds on the previous one, you probably don’t need to create any new notebooks. Launch the ones you used to analyze the violet data, the NAMCS data, and Collett’s throat data, grab a cup of coffee, and start reading!

## 5.1 Bernoulli models for binary data

Since we don’t know what will happen with a binary response variable until we observe a specific outcome, the best we can hope for is to know the exact chance of each possibility. Mathematically, this defines a *Bernoulli random variable*, a random variable $Y$ satisfying

$P(Y = 1) = \Pi$ and $P(Y = 0) = 1 - \Pi$

for some known probability $\Pi$, where $P(\cdot)$ stands for the probability of the event in parentheses. If $Y$ corresponds to the result of flipping a fair coin, for example, with $Y = 1$ (a success) when it lands on heads and $Y = 0$ (a failure) when it lands on tails, then $\Pi = \frac{1}{2}$. When rolling a fair 6-sided die, we might define $Y = 1$ when rolling a 1 and $Y = 0$ otherwise, making $\Pi = \frac{1}{6}$. In both cases, we’re relying on the definition of *fair* to determine the *theoretical probabilities*; we don’t have to collect any data at all. In most practical situations, by contrast, theoretical probabilities aren’t available, so we have to collect data and work with *empirical probabilities.*

Since events of interest are rarely as simple as tossing a coin or rolling a die, a single static probability is often a poor model for what will happen. This is precisely what we saw in the null models in the previous chapter, which we improved significantly by incorporating predictor variables. Even though the logistic models we constructed were far better than the null models, they were by no means perfect—no model ever is. It is therefore essential to know how to assess such models, to measure their fits and to quantify their uncertainties. This section pursues these goals, starting with the underlying theoretical model and then explaining how to draw inferences about its parameters from empirical estimates.

### 5.1.1 The theoretical simple logistic model

Instead of assuming that the probability of success has to be constant, let’s be (somewhat) more realistic and allow this probability to depend on a quantitative predictor variable whose values are known precisely. Specifically, we suppose that

$\Pi_X = \sigma(\alpha + \beta X),$

where X is the value of the predictor, $\Pi_X = P(Y = 1|X)$ is the probability that Y = 1 for this given value X, and $\sigma(\cdot)$ is the logistic function. This theoretical model thus has two parameters, α and β, and their values define this particular Bernoulli variable—in fact, we now have a family of Bernoulli variables, one for each value of the predictor. As long as the probability $\Pi_X$ is neither 0 nor 1, we can rephrase this theoretical model in terms of the odds that $Y = 1$:

$\log\left(\frac{\Pi_X}{1 - \Pi_X}\right) = \alpha + \beta x.$

We’ll use these forms interchangeably, but we have to remember the two exceptional cases when the log-odds is undefined (when $\Pi_X = 0$ and when $\Pi_X = 1$).

Whichever formulation we use, the randomness of the model is baked into the probability $\Pi_X$—even if the exact values of the coefficients $\alpha$ and $\beta$ were known, which is impossible, this model would not be deterministic. This differs markedly from the theoretical linear regression model, in which the random error $\mathcal{E}$ is necessarily an explicit term; both of $\mathcal{E}$'s distinct descriptors, its average and its variance, have to be related to those of the quantitative response. For our Bernoulli random variable $Y$, on the other hand, the probability of success $\Pi_X$ determines both its average—that is, its expected value, $E(Y|X)$—and its variance:

$E(Y|X) = \Pi_X \quad \text{and} \quad \text{Var}(Y|X) = \Pi_X(1 - \Pi_X).$

To verify these results, we just have to remember that a probability is a proportion. To compute the average $E(Y|X)$, for example, we add up all of the possible values of $Y$ for this value $X$ and divide by how many there are; since $\Pi_X$ is the fraction of 1’s and $1 - \Pi_X$ is the fraction of 0’s, the average is $E(Y|X) = 1*\Pi_X + 0*(1-\Pi_X) = \Pi_X.$

This calculation shows that the probability $\Pi_X$ is the mean response to this given value of the predictor; from this perspective, logistic regression and linear regression are the same, as both models relate the average response to a linear function of the predictor. To compute the variance, average the squared deviations of $Y$ from its average, $\Pi_X$—we leave this calculation as an exercise at the end of the chapter. As you work it out, note that the variance will change for different values of the predictor unless $\beta = 0$, so we’re not assuming constant variance as we do with linear regression.

What we’ve done so far concerns the first assumption of the theoretical logistic model: for each given value $X$ of the predictor, the response $Y$ is a Bernoulli random variable whose probability of success, $\Pi_X$, depends on $X$ as given above. This model has just one other assumption: different observations of the response variable are *independent*. This is an essential part of the model, as it justifies defining and working with the likelihood function, $L(\alpha, \beta)$; recall from the violet example in the last chapter that $L(\alpha, \beta)$ is the collective probability, according to the model, of the observed responses. In general, if there are $n$ observed responses $Y_1, \dots, Y_n$ to the respective predictor values $X_1, ..., X_n$, then the independence of different observations means that the likelihood function is

$L(α, β) ∝ ∏_{i=1}^{n} π_i^{y_i} (1 - π_i)^{(1 - y_i)}$,

where $π_i = P(Y = 1 | X_i)$. To wrangle this product and all of these exponents into a more manageable form, we take the natural logarithm of $L(α, β)$ to get the log-likelihood function,

$\log L(α, β) ∝ \sum_{i=1}^{n} [y_i \log π_i + (1 - y_i) \log (1 - π_i)]$.

There is a key, albeit subtle, difference between these likelihood formulas and the ones we worked with in the violet example: these formulas depend on the model parameters $α$ and $β$, which we didn’t mention when analyzing the violet data. Since the model parameters can never be known, neither can the theoretical model’s log-likelihood. The process of fitting a simple logistic model to the observations $\{(x_1, y_1), ..., (x_n, y_n)\}$ results in the best possible approximation of $\log L(α, β)$, however—by construction, the fitted model

$\log(\hat{\Pi}_i / (1 - \hat{\Pi}_i)) = a + b x$

has coefficients a and b that maximize the corresponding log-likelihood log L(a, b). These coefficients are the best available estimates of the model parameters α and β, just as the the fitted probabilities $\{\hat{\Pi}_1, ..., \hat{\Pi}_n\}$ are our best estimates of the theoretical probabilities $\{\Pi_1, ..., \Pi_n\}$. As we’ll explain in the next section, the fitting process also provides the standard errors we need to compute interval estimates for the parameters and the theoretical probabilities.

### 5.1.2 Inferences for logistic models

Each set $\{(x1, y1), ..., (xn, yn)\}$ of observations will yield its own maximum likelihood estimates a and b of the theoretical model parameters α and β. In real life, we usually have only one set of observations, and we need to use the information in this one sample to estimate the model parameters and to predict future responses. To do this, we have to know something about how the computed coefficients can vary from sample to sample—in other words, we need to understand the sampling distributions of the random variables whose realizations are the statistics $a$ and $b$. In the case of linear regression, assuming that the error $\mathcal{E}$ is normally distributed enables the calculation of explicit formulas for the sampling distributions of the regression coefficients; while neither the calculations nor the resulting formulas are trivial, they’re not too bad once you get the hang of working with random variables. The situation is quite different for logistic regression, and we will not even attempt to derive or state all of the relevant formulas. Instead, we’ll rely on a fact whose proof can be found elsewhere [15]: when working with large samples, the maximum likelihood estimates of the logistic regression coefficients are approximately unbiased and normally distributed. As long as this large sample assumption holds, we can therefore apply standard methods based on normal distributions to compute interval estimates and test hypotheses. What this means in practice is that we need the standard errors for the coefficients, which are the best estimates based on our data of the standard deviations of the sampling distributions.

#### Confidence intervals for the parameters

As expected, smf.glm provides what we need. As an example, let’s look at the summary of the logistic model chasm_fit that we built in the last chapter:

```
print( chasm_fit.summary() )
```

```
Generalized Linear Model Regression Results
================================================================================
Dep. Variable:                     Bud   No. Observations:  
Model:                            GLM   Df Residuals:      
Model Family:                Binomial   Df Model:          
Link Function:                  logit   Scale:             
Method:                          IRLS   Log-Likelihood:    
Date:                Tue, 03 Aug 2021   Deviance:          
Time:                        13:26:08   Pearson chi2:      
No. Iterations:                    7   
Covariance Type:            nonrobust  
================================================================================
                       coef    std err          z      P>|z|      [0.
--------------------------------------------------------------------------------
Intercept          74.5837     12.132      6.148      0.000      50.
Photoperiod        -5.5250      0.900     -6.136      0.000      -7.
================================================================================
```
