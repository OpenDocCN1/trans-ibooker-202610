
## Binomial Distribution

For the remainder of this chapter, we are going to learn two probability distributions: the binomial and beta distributions. While we will not be using these for the rest of the book, they are useful tools in themselves and fundamental in understanding how events occur given a number of trials. They will also be a good segue on understanding probability distributions which we will use heavily in Chapter 3. Let’s explore a use case that could occur in a real-world scenario.

Let’s say you are working on a new turbine jet engine and ran 10 tests. The outcomes yielded 8 successes and 2 failures:

- ✓ ✓ ✓ ✓ ✓ X X ✓ ✓

You were hoping to get a 90% success rate, but based on the data above you conclude that your tests have failed with only 80% success. Each test is time-consuming and expensive, so you decide it is time to go back to the drawing board to re-engineer the design.

However one of your engineers insists there should be more tests. “The only way we will know for sure is to run more tests,” she argues. “What if more tests yield 90% or greater success? After all, if you flip a coin 10 times and get 8 heads, it does not mean the coin is fixed at 80%.”

You briefly consider the engineer’s argument and realize she has a point. Even a fair coin flip will not always have an equally split outcome, especially with only 10 flips. You are most likely to get 5 heads but you can also get 3, 4, 6, or 7 heads. You could even get 10 heads, although this is extremely unlikely. So how do you determine the likelihood of 80% success assuming the underlying probability is 90%?

One tool that might be relevant here is the **binomial distribution**, which measures how likely *k* successes can happen out of *n* trials given *p* probability.

Visually, a binomial distribution looks like Figure 2-1.

![](img/8e0dcc1636d032e7728a07e97751e446_79_0.png)

Figure 2-1. A binomial distribution

As you see above, we see the probability of k successes for each bar out of 10 trials.
This binomial distribution assumes a probability p of 90%, meaning there is a .90 (or 90%) chance for a success to occur. If this is true, that means there is a .1937 probability we would get 8 successes out of 10 trials. The probability of getting 1 success out of 10 trials is extremely unlikely, .00000008999, hence why the bar is not even visible.

We can also calculate the probability of 8 or less successes by adding up bars for 8 or less successes. This would give us .2639 probability of 8 or less successes.

So how do we implement the binomial distribution? We can do it from scratch relatively easily (as shared in the Appendix A), or we can use libraries like SciPy. Example 2-2 shows how we use SciPy's binom.pmf() function (PMF stands for “probability mass function”) to print all 11 probabilities for our binomial distribution from 0 to 10 successes.

### Example 2-2. Using SciPy for the binomial distribution

```
from scipy.stats import binom

n = 10
p = 0.9

for k in range(n + 1):
    probability = binom.pmf(k, n, p)
    print("{0} - {1}".format(k, probability))
```

```
# OUTPUT:
# 0 - 9.99999999999996e-11
# 1 - 8.99999999999996e-09
# 2 - 3.644999999999996e-07
# 3 - 8.74800000000003e-06
# 4 - 0.00013778099999999
# 5 - 0.00148803479999998
# 6 - 0.0116026099999996
# 7 - 0.0573956280000001
# 8 - 0.193710244999993
# 9 - 0.3874204890000037
# 10 - 0.3486784401000004
```

As you can see, we provide n as the number of trials, p as the probability of success for each trial, and k is the number of successes we want to look up the probability for. We iterate each number of successes x with the corresponding probability we would see that many successes. As we can see above in the output, the most likely number of successes is 9.

But if you add up the probability of 8 or less successes, we would get .2639. This means there is a 26.39% chance we would see 8 or less successes even if the underlying success rate is 90%. So maybe the engineer is right. 26.39% chance is not nothing and certainly possible.

However we did make an assumption here in our model which we will discuss next with the Beta Distribution.

### BINOMIAL DISTRIBUTION FROM SCRATCH

Turn to the Appendix A to learn how to build the Binomial Distribution from scratch without Scikit-learn.

## Beta Distribution

What did I assume with my engine test model using the binomial distribution? Is there a parameter I assumed to be true and then built my entire model around it? Think carefully and read on.

What might be problematic about my binomial distribution above is I ASSUMED the underlying success rate is 90%. That’s not to say my model is worthless. I just showed if the underlying success rate is 90%, there is a 26.39% chance I would see 8 or less successes with 10 trials. So the engineer is certainly not wrong that there could be an underlying success rate of 90%.

But let’s flip the question and consider this: what if there are other underlying rates of success that would yield 8/10 successes besides 90%? Could we see 8/10 successes with an underlying 80% success rate? 70%? 30%? When we fix the 8/10 successes, can we explore the probabilities of probabilities?

Rather than create countless binomial distributions to answer this question, there is one tool that we can use. The **beta distribution** allows us to see the likelihood of different underlying probabilities for an event to occur given *alpha* successes and *beta* failures.

Here is a chart of the beta distribution given 8 successes and 2 failures in Figure 2-2.

![](img/8e0dcc1636d032e7728a07e97751e446_82_0.png)

Figure 2-2. Beta distribution

### BETA DISTRIBUTION ON DESMOS

If you want to interact with the Beta Distribution, a Desmos graph is provided here:

https://www.desmos.com/calculator/xylhmcwo71

Notice that the x-axis represents all underlying rates of success from 0.0 to 1.0 (0% to 100%), and the y-axis represents the likelihood of that probability given 8 successes and 2 failures. In other words, the beta distribution allows us to see the probabilities of probabilities given 8/10 successes. Think of it as a meta-probability so take your time grasping this idea!

Notice also that the beta distribution is a continuous function, meaning it forms a continuous curve of decimal values (as opposed to the tidy and discrete integers in the binomial distribution). This is going to make the math with the beta distribution a bit harder, as a given density value on the y-axis is not a probability. We instead find probabilities using areas under the curve.

The beta distribution is a type of **probability distribution**, which means the area under the entire curve is 1.0, or 100%. To find a probability, we need to find the area within a range. For example, if we want to evaluate the probability 8/10 successes would yield 90% or higher success rate, we need to find the area between 0.9 and 1.0, which is 22.5%, as shaded in Figure 2-3.

![](img/8e0dcc1636d032e7728a07e97751e446_83_0.png)

Figure 2-3. The area between 90% and 100%, which is 22.5%

As we did with the binomial distribution, we can use SciPy to implement the beta distribution. Every continuous probability distribution has a **cumulative density function (CDF)** which calculates the area up to a given x value. Let’s say I wanted to calculate the area up to 90% (0.0 to .90) as shaded in Figure 2-4.

![](img/8e0dcc1636d032e7728a07e97751e446_84_0.png)

Figure 2-4. Calculating the area up to 90% (0.0 to .90)

It is easy enough to use SciPy with its `beta.cdf()` function, and the only parameters I need to provide are the x value, the number of successes a, and the number of failures b as shown in Example 2-3.

### Example 2-3. Beta distribution using SciPy

```
from scipy.stats import beta

a = 8
b = 2

p = beta.cdf(.90, a, b)

# 0.7748409780000001
print(p)
```

So according to our calculation above, there is a 77.48% chance the underlying probability of success is 90% or less.

How do we calculate the probability of success being 90% or more as shaded below in Figure 2-5?
