There are two versions of the mean you will see: the sample mean \(\overline{x}\) and the population mean \(\mu\) as expressed below:

\[\overline{x} = \frac{x_1 + x_2 + x_3 + ... + x_n}{n} = \sum \frac{x_i}{n}\)

\[\mu = \frac{x_1 + x_2 + x_3 + ... + x_n}{N} = \sum \frac{x_i}{N}\]

Recall the summation symbol \(\sum\) means add all the items together. The \(n\) and the \(N\) represent the sample and population size respectively, but mathematically they represent the same thing: the number of items. The same goes for calling the sample mean \(\overline{x}\) ("x-bar") and the population mean \(\mu\) ("mu"). Both \(\overline{x}\) and \(\mu\) are the same calculation, just different names depending on whether it’s a sample or population we are working with.

The mean is likely familiar to you, but here’s something less known about the mean: the mean is actually a weighted average called the **weighted mean**. The mean we commonly use gives equal importance to each value. But we can manipulate the mean and give each item a different weight.

weighted mean = 
\(\frac{(x_1 \cdot w_1) + (x_2 \cdot w_2) + (x_3 \cdot w_3) + ... (x_n \cdot w_n)}{w_1 + w_2 + w_3 + ... + w_n}\)

This can be helpful when we want some values to contribute to the mean more than others. A common example of this is weighting academic exams to give a final grade. If you have 3 exams and a final exam, and we give each of the 3 exams 20% weight and the final exam 40% weight of the final grade, here is how we express it in Example 3-2.

# *Example 3-2. Calculating a weighted mean in Python*

```
# Three exams of .20 weight each and final exam of .40 weight
sample = [90, 80, 63, 87]
weights = [.20, .20, .20, .40]

weighted_mean = sum(s * w for s,w in zip(sample, weights)) /
sum(weights)

print(weighted_mean) # prints 81.4
```