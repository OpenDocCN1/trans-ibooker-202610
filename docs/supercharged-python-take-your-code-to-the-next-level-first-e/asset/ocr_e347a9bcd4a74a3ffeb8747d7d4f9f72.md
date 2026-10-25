| deviations | |
|---|---|
| One | 68 percent, on average, should fall within one standard deviation of the mean. |
| Two | 95 percent, on average, should fall within two standard deviations of the mean. |
| Three | 99.7 percent, on average, should fall within three standard deviations of the mean. |

Here’s how to read Table 11.2. As an example, suppose you have a normal distribution with a mean of 100 and a standard deviation of 20. You should expect, in the long run, about 68 percent of the numbers produced by the `normalvariate` function to fall within 80 and 120. You should expect 95 percent of the numbers produced to fall within 40 and 160.

Yet with all the probability distributions in the `random` package, they are just that: _probability_ distributions. In the short run especially, nothing is certain. For a given trial, the probability a number will fall into the range 40 to 160, given the conditions outlined here, is 95 percent; there’s a 5 percent change of falling outside the range.

But that’s not saying that such occurrences _cannot happen_. Events with only a 5 percent probability can and do happen all the time. And events with probabilities of 1 in a million or less happen every day—every time someone wins the lottery!

Therefore, if you take only a few sample results, you may not