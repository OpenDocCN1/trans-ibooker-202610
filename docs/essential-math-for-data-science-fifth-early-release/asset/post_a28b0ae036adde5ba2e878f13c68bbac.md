
Just like linear regression, there is an $R^2$ for a given logistic regression. If you recall from Chapter 5, the $R^2$ indicates how well a given independent variable explains a dependent variable. Applying this to our chemical exposure problem, it makes sense we want to measure how much chemical exposure hours explains showing symptoms.

There is not really a consensus on the best way to calculate the $R^2$ on a logistic regression, but a popular technique known as McFadden's Pseudo $R^2$ closely mimics the $R^2$ used in linear regression. We will use this technique in the following examples and here is the formula:

$$ R^2 = \frac{(\text{log likelihood}) - (\text{log likelihood fit})}{(\text{log likelihood})} $$

We will learn how to calculate the "log likelihood fit" and "log likelihood" so we can calculate the $R^2$.

We cannot use residuals here like in linear regression, but we can project the outcomes back onto the logistic curve as shown in Figure 6-12, and look up their corresponding likelihoods between 0.0 and 1.0.

![](img/8e0dcc1636d032e7728a07e97751e446_310_0.png)

Figure 6-12. Projecting the output values back onto the logistic curve

We can then take the log() of each of those likelihoods and sum them together. This will be the log likelihood of the fit. Just like we did calculating maximum likelihood, we will convert the "false" likelihoods by subtracting from 1.0.

### Example 6-10. Calculating the log-likelihood of the fit

```python
from math import log, exp
import pandas as pd

patient_data = pd.read_csv('https://bit.ly/33ebs2R', delimiter=",").itertuples()

b0 = -3.17576395
b1 = 0.69267212

def logistic_function(x):
    p = 1.0 / (1.0 + exp(-(b0 + b1 * x)))
    return p

# Sum the log-likelihoods
log_likelihood_fit = 0.0

for p in patient_data:
    if p.y == 1.0:
        log_likelihood_fit += log(logistic_function(p.x))
    elif p.y == 0.0:
        log_likelihood_fit += log(1.0 - logistic_function(p.x))

print(log_likelihood_fit) # -9.946161673231583
```

Using some clever binary multiplication and Python comprehensions, we can consolidate that `for` loop and `if` expression into one line that returns the `log_likelihood_fit`. Similar to what we did in the maximum likelihood formula, we can use some binary subtraction between the true and false cases to mathematically eliminate one or the other. In this case we multiply by 0 and therefore apply either the true or false case, but not both, to the sum accordingly.

### Example 6-11. Consolidating our log likelihood logic into a single line

```python
log_likelihood_fit = sum(log(logistic_function(p.x)) * p.y +
    log(1.0 - logistic_function(p.x)) * (1.0 - p.y)
    for p in patient_data)
```

If we were to express the likelihood of the fit in mathematic notation, this is what it would look like. Note that f(x_i) is the logistic function for a given input variable x_i.

$$
log\ likelihood\ fit = \sum_{i=1}^{n} (log(f(x_i)) \times y_i) + (log(1.0 - f(x_i)) \times (1 - y_i))
$$

As calculated above in Example 6-10 and Example 6-11, we have -9.9461 as our log-likelihood of the fit. We need one more datapoint to calculate the $R^2$: the log-likelihood that estimates without using any input variables, and simply uses the number of true cases divided by all cases (effectively leaving only the intercept). Note we can count the number of symptomatic cases by summing all the y-values together $\sum y_i$, because only the 1's and not the 0's will count into the sum. Here is the formula:

$$
log\ likelihood = \frac{\sum y_i}{n} \times y_i + \left(1 - \frac{\sum y_i}{n}\right) \times (1 - y_i)
$$

Here is the expanded Python equivalent of this formula applied in Example 6-12.

### Example 6-12. Log of likelihood of patients

```python
import pandas as pd
from math import log, exp

patient_data = list(pd.read_csv('https://bit.ly/33ebs2R',
delimiter=",").itertuples())

likelihood = sum(p.y for p in patient_data) / len(patient_data)

log_likelihood = 0.0

for p in patient_data:
    if p.y == 1.0:
        log_likelihood += log(likelihood)
    elif p.y == 0.0:
        log_likelihood += log(1.0 - likelihood)

print(log_likelihood) # -14.341070198709906
```

To consolidate this logic and reflect the formula, we can compress that `for` loop and `if` expression into a single line, using some binary multiplication logic to handle both true and false cases:

### Example 6-13. Consolidating the log likelihood into a single line

```python
log_likelihood = sum(log(likelihood) * p.y + log(1.0 - likelihood) * (1.0 - p.y) \
    for p in patient_data)
```

Finally, just plug these values in and get your $R^2$:

![](img/8e0dcc1636d032e7728a07e97751e446_312_0.png)

And here is the Python code shown in Example 6-14, calculating the $R^2$ in its entirety.

### Example 6-14. Calculating the $R^2$ for a logistic regression

```python
import pandas as pd
from math import log, exp

patient_data = list(pd.read_csv('https://bit.ly/33ebs2R',
delimiter=",").itertuples())

# Declare fitted logistic regression
b0 = -3.17576395
b1 = 0.69267212

def logistic_function(x):
    p = 1.0 / (1.0 + exp(-(b0 + b1 * x)))
    return p

# calculate the log likelihood of the fit
log_likelihood_fit = sum(log(logistic_function(p.x)) * p.y +
    log(1.0 - logistic_function(p.x)) * (1.0 - p.y)
    for p in patient_data)
# calculate the log likelihood without fit
likelihood = sum(p.y for p in patient_data) / len(patient_data)
log_likelihood = sum(log(likelihood) * p.y + log(1.0 - likelihood) * (1.0 - p.y) \
    for p in patient_data)
# calculate R-Square
r2 = (log_likelihood - log_likelihood_fit) / log_likelihood
print(r2)  # 0.306456105756576
```

Okay so we got an $R^2 = .306456$, so does hours of chemical exposure explain whether someone shows symptoms? As we learned in Chapter 5 on linear regression, a poor fit will be closer to an $R^2$ of 0.0 and a greater fit will be closer to 1.0. Therefore we can conclude that hours of exposure is mediocre for predicting symptoms, as the $R^2$ is 0.30645. There must be other variables that help better predict if someone will show symptoms besides time of exposure. This makes sense because we have a large mix of patients showing symptoms versus not showing symptoms for most of our observed data, as shown in Figure 6-13.

![](img/8e0dcc1636d032e7728a07e97751e446_313_0.png)

Figure 6-13. Our data has a mediocre $R^2$ of 0.30645, because there is a lot of variance in the middle of our curve

But if we did have a clean divide in our data, where 1 and 0 outcomes are cleanly separated as shown below in Figure 6-14, we would have a perfect $R^2$ of 1.0.

![](img/8e0dcc1636d032e7728a07e97751e446_314_0.png)

Figure 6-14. This logistic regression has a perfect R^2 of 1.0 because there is a clean divide in outcomes predicted by hours of exposure.

## P-Values

Just like linear regression, we are not done just because we have an R^2. We need to investigate how likely we would have seen this data by chance rather than because of an actual relationship. This means we need a P-Value.

To do this, we will need to learn a new probability distribution called the Chi-Square Distribution, annotated as chi^2 Distribution. It is continuous and used in several areas of statistics, including this one!

If we take each value in a standard normal distribution (mean of 0 and standard deviation of 1) and square it, that will give us the chi^2 distribution with 1 degree of freedom. For our purposes, the degrees of freedom will depend on how many parameters n are in our logistic regression, which will be n - 1. You can see examples of different degrees of freedom in Figure 6-15. Since we have two parameters (hours of exposure and whether symptoms were shown), our degrees of freedom will be 1 because 2 - 1 = 1.

![](img/8e0dcc1636d032e7728a07e97751e446_315_0.png)

Figure 6-15. A $\chi^2$ Distribution with differing degrees of freedom.

We will need the log likelihood fit and log likelihood as calculated in the previous subsection on $R^2$. Here is the formula that will produce the $\chi^2$ value we need to look up.

$$
\chi^2 = 2 (\text{log likelihood fit}) - (\text{log likelihood})
$$
