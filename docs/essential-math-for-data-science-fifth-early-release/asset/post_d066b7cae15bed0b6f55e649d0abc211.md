
```
print(confidence_interval(p=.95, sample_mean=64.408, sample_std=2.05,
n=31))
# (63.68635915701992, 65.12964084298008)
```

So the way to interpret this is “based on my sample of 31 golden retriever weights with sample mean 64.408 and sample standard deviation of 2.05, I am 95% confident the population mean lies between 63.686 and 65.1296”. That is how we describe our confidence interval.

One interesting thing to note here too is that in our margin of error formula, the larger n becomes the narrower our confidence interval becomes! This makes sense because if we have a larger sample, we are more confident in the population mean falling in a smaller range, hence why it’s called a confidence interval.

One caveat to put here is that for this to work, our sample size must be at least 31 items. This goes back to the central limit theorem and if we want to apply a confidence interval to a smaller sample, we need to use a distribution with higher variance (fatter tails reflecting more uncertainty). This is what the T-distribution is for and we will visit that at the end of this chapter.

In Chapter 5 we will continue to use confidence intervals for linear regressions.

## Understanding P-Values

When we say something is statistically significant, what do we mean by that? We hear it used loosely and frequently but what does it mathematically mean? Technically, it has to do with something called the p-value which is a hard concept for many folks to grasp. But I think the concept of p-values makes more sense when you trace it back to its invention. While this is an imperfect example, it gets across some big ideas.

In 1925, a mathematician named Ronald Fisher was at a party. One of his colleagues Muriel Bristol claimed she could detect when tea was poured before milk simply by tasting it. Intrigued by the claim, Ronald set up an experiment on the spot.

He prepared 8 cups of tea. 4 had milk poured first, the other 4 had tea poured first. He then presented them to his connoisseur colleague and asked her to identify the pour order for each. Remarkably she identified them all correctly, and the probability of this happening by chance is 1 in 70, or 0.01428571.

This 1.4% probability is what we call the **p-value**, the probability something occurring by chance rather than because of a hypothesized explanation. Without going down a rabbit hole of combinatorial math, the probability that Muriel completely guessed the cups correctly is 1.4%. What exactly does that tell you?

When we frame an experiment, whether it is determining if organic donuts cause weight gain or living near power lines causes cancer, we always have to entertain the possibility that random luck played a role. Just like there is a 1.4% chance Muriel identified the cups of tea correctly simply by guessing, there’s always a chance randomness just gave us a good hand like a slot machine. This helps us frame our **null hypothesis (H0)**, saying that the variable in question had no impact on the experiment and any positive results is just random luck. The **alternative hypothesis (H1)** poses that a variable in question (called the **controlled variable**) is causing a positive result.

Traditionally the threshold for statistical significance is a p-value of 5% or less, or .05. Since .014 is less than .05, this would mean we can reject our null hypothesis that Muriel was randomly guessing. We can then promote the alternative hypothesis that Muriel has a special ability to detect whether tea or milk was poured first.

Now one thing this tea party example did not capture is that when we calculate a p-value, we capture all probability of that event or rarer. We will address this as we dive into the next example using the normal distribution.

## Hypothesis Testing

Past studies has shown that the mean recovery time for a cold is 18 days, with a standard deviation of 1.5 days, and follows a normal distribution.

This means there is approximately 95% chance of recovery taking between 15 and 21 days as shown in Figure 3-16 and Example 3-18.

![](img/8e0dcc1636d032e7728a07e97751e446_142_0.png)

Figure 3-16. There is 95% chance of recovery between 15 and 21 days

### Example 3-18. Calculating the probability of recovery between 15 and 21 days

```
from scipy.stats import norm

# Cold has 18 day mean recovery, 1.5 std dev
mean = 18
std_dev = 1.5

# 95% probability recovery time takes between 15 and 21 days.
x = norm.cdf(21, mean, std_dev) - norm.cdf(15, mean, std_dev)

print(x) # 0.9544997361036416
```

We can infer then from the remaining 5% probability that there's a 2.5% chance of recovery taking longer than 21 days and a 2.5% chance of less than 15 days. Hold onto that bit of information because it will be critical later! That drives our p-value.

Now let’s say an experimental new drug was given to a test group of 40 people and it took an average of 16 days for them to recover from the cold as shown in Figure 3-17.

![](img/8e0dcc1636d032e7728a07e97751e446_143_0.png)

Figure 3-17. A group taking a drug took 16 days to recover

Did the drug have an impact? If you reason long enough, you may realize what we are asking is this: does the drug show a statistically significant result? Or did the drug not work and the 16-day recovery was a coincidence with the test group? That first question frames our alternative hypothesis, while the second question frames our null hypothesis.

There are two ways we can calculate this: the One-tailed and two-tailed test. We will start with the one-tailed.

### One-Tailed Test

When we approach the one-tailed test, we typically frame our null and alternative hypotheses using inequalities. We hypothesize around the population mean and say that it either is greater than/equal to 18 (the null hypothesis H0) or less than 18 (the alternative hypothesis H1).

```
H0 : population mean ≥ 18
H1 : population mean < 18
```

To reject our null hypothesis, we need to show that our sample mean of the patients who took the drug is not likely to have been coincidental. Since a P-value of .05 or less is traditionally considered statistically significant, we will use that as our threshold (Figure 3-17). When we calculate this in Python using the inverse CDF as shown in Figure 3-18 and Example 3-19, we find that approximately 15.51 is the number of recovery days that gives us .05 area on the left tail.

![](img/8e0dcc1636d032e7728a07e97751e446_145_0.png)

Figure 3-18. Getting the x-value with 5% of area behind it

### Example 3-19. Python code for getting x-value with 5% of area behind it

```
from scipy.stats import norm

# Cold has 18 day mean recovery, 1.5 std dev
mean = 18
std_dev = 1.5

# What x-value has 5% of area behind it?
x = norm.ppf(.05, mean, std_dev)

print(x) # 15.53271955957279
```

Therefore if we achieve an average 15.53 or less days of recovery time in our sample group, our drug is considered statistically significant enough to have shown an impact. However, our sample mean of recovery time is actually 16 days and does not fall into this null hypothesis rejection zone. Therefore the statistical significance test has failed as shown in Figure 3-19.

![](img/8e0dcc1636d032e7728a07e97751e446_146_0.png)

Figure 3-19. We have failed to prove our drug test result is statistically significant

The area up to that 16-day mark is our p-value, which is .0912 and we calculate it in Python as shown in Example 3-20.

### Example 3-20. Calculating the 1-tailed p-value

```
from scipy.stats import norm

# Cold has 18 day mean recovery, 1.5 std dev
mean = 18
std_dev = 1.5

# Probability of 16 or less days
p_value = norm.cdf(16, mean, std_dev)

print(p_value) # 0.09121121972586788
```

Since the p-value of .0912 is greater than our statistical significance threshold of .05, we do not consider the drug trial a success and fail to reject our null hypothesis.

### Two-Tailed Test

The previous test we performed is called the one-tailed test because it only looks for statistical significance on one tail. However it is often safer and better
