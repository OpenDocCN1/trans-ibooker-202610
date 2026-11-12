$P(X) = .7$

Conversely, you can turn an odds into a probability simply by dividing the
probability of the event occurring by the probability it will not occur:

$O(X) = \frac{P(X)}{1-P(X)}$

$O(X) = \frac{.70}{1-.70}$

$O(X) = \frac{7}{3}$

# ODDS ARE USEFUL!

While many people feel more comfortable expressing probabilities as
percentages or proportions, odds can be a helpful tool. If I have an odds of 2.0,
that means I feel an event is 2 times more likely to happen than not to happen.
That can be more intuitive to describe a belief than a percentage of 66. 666%.
For this reason, odds are helpful quantifying subjective beliefs especially in a
gambling/betting context. It plays a role in Bayesian statistics (including the
Bayes Factor) as well as logistic regressions with the log-odds which we will
cover in Chapter 6.

# Probability versus Statistics

Sometimes people use the terms *probability* and *statistics* interchangeably, and while
this is understandable to conflate the two disciplines they do have distinctions.
**Probability** is purely theoretical of how likely an event is to happen, and does not
require data. **Statistics** on the other hand cannot exist without data, and uses it to
discover probability and provides tools to decribe data.

Think of predicting the outcome of rolling a “4” on a die (that’s the singular of
“dice”). Approaching the problem with a pure probability mindset, one simply says
there are 6 sides on a die. We assume each side is equally likely, so the probability of
getting a “4” is $\frac{1}{6}$, or 16.666%.

However, a zealous statistician might say “No! We need to roll the die to get data. If
we can get 30 rolls or more, and the more rolls we do the better, only then will we