+   • It resembles a lot of phenomena in nature and daily life, and even generalizes non-normal problems because of the central limit theorem which we will talk about shortly.

# The Probability Density Function (PDF)

The standard deviation plays an important role in the normal distribution, because it defines how “spread out” it is. It is actually one of the parameters alongside the mean. The probability density function (PDF) that creates the normal distribution is as follows:

f(x) = \frac{1}{\sigma} * \sqrt{2\pi} * e^{-\frac{1}{2} \left(\frac{x-\mu}{\sigma}\right)^2}

Wow that’s a mouthful, isn’t it? We even see our friend Euler’s Number e from Chapter 1 and some crazy exponents. Here is how we can express it in Python in Example 3-9.

# Example 3-9. The normal distribution function in Python

```
# normal distribution, returns likelihood
def normal_pdf(x: float, mean: float, std_dev: float) -> float:
    return (1.0 / (2.0 * math.pi * std_dev ** 2) ** 0.5) *
    math.exp(-1.0 * ((x - mean) ** 2 / (2.0 * std_dev ** 2)))
```

There’s a lot to take apart here in this formula, but what’s important is that it accepts a mean and standard deviation as parameters, as well as an x value so you can look up the likelihood at that given value.

Just like the beta distribution in Chapter 2, the normal distribution is continuous. This means to retrieve a probability we need to integrate a range of x values to find an area.

In practice though, we will use SciPy to do these calculations for us.

# Integrating the Normal Distribution

With the normal distribution, the vertical axis is not the probability but rather the likelihood for the data. To find the probability we need to look at a given range, and then find the area under the curve for that range. Let’s say I want to find the probability of a golden retriever weighing between 62 and 66 pounds. Here is a visual in Figure 3-7 of the range we want to find the area for.