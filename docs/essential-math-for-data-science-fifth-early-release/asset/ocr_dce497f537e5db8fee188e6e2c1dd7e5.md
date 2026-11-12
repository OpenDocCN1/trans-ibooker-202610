Example 3-5 we calculate the mode for our pet dataset, and sure enough we see this is bimodal as both 2 and 3 occur the most (and equally) as often.

# Example 3-5. Calculating the mode in Python

```
# Number of pets each person owns
from collections import defaultdict

sample = [1, 3, 2, 5, 7, 0, 2, 3]

def mode(values):
    counts = defaultdict(lambda: 0)

    for s in values:
        counts[s] += 1

    max_count = max(counts.values())
    modes = [v for v in set(values) if counts[v] == max_count]
    return modes

print(mode(sample)) # [2, 3]
```

In practicality, the mode is often not used a lot unless your data is repetitive.
This is commonly encountered with integers, categories, and other discrete
variables.

# Variance and Standard Deviation

When we start talking about variance and standard deviation, this is where it gets
interesting. One thing that confuses people with variance and standard deviation
is there are some calculation differences for the sample versus the population.
We will do our best to cover these differences clearly.

# Population Variance and Standard Deviation

In describing data, we are often interested in measuring the differences between
the mean and every data point. This gives us a sense of how “spread out” the
data is.

Let’s say I’m interested in studying the number of pets owned by members of
my work staff (note that I’m defining this as my population, not a sample). I
have 7 people on my staff.

I take the mean of all the numbers of pets they own, and I get 6.571. Let’s
subtract this mean from each value. This will show us how far each value is