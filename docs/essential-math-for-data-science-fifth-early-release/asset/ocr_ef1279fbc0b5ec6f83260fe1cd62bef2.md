```
print(confidence_interval(p=.95, sample_mean=64.408, sample_std=2.05,
n=31))
# (63.68635915701992, 65.12964084298008)
```

So the way to interpret this is “based on my sample of 31 golden retriever weights with sample mean 64.408 and sample standard deviation of 2.05, I am 95% confident the population mean lies between 63.686 and 65.1296”. That is how we describe our confidence interval.

One interesting thing to note here too is that in our margin of error formula, the larger n becomes the narrower our confidence interval becomes! This makes sense because if we have a larger sample, we are more confident in the population mean falling in a smaller range, hence why it’s called a confidence interval.

One caveat to put here is that for this to work, our sample size must be at least 31 items. This goes back to the central limit theorem and if we want to apply a confidence interval to a smaller sample, we need to use a distribution with higher variance (fatter tails reflecting more uncertainty). This is what the T-distribution is for and we will visit that at the end of this chapter.

In Chapter 5 we will continue to use confidence intervals for linear regressions.

# Understanding P-Values

When we say something is statistically significant, what do we mean by that? We hear it used loosely and frequently but what does it mathematically mean? Technically, it has to do with something called the p-value which is a hard concept for many folks to grasp. But I think the concept of p-values makes more sense when you trace it back to its invention. While this is an imperfect example, it gets across some big ideas.

In 1925, a mathematician named Ronald Fisher was at a party. One of his colleagues Muriel Bristol claimed she could detect when tea was poured before milk simply by tasting it. Intrigued by the claim, Ronald set up an experiment on the spot.

He prepared 8 cups of tea. 4 had milk poured first, the other 4 had tea poured first. He then presented them to his connoisseur colleague and asked her to