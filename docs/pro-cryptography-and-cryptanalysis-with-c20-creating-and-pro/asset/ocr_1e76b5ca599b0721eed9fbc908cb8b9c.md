# CHAPTER 22

# Text Characterization

In this chapter, we will analyze two important metrics for cipher and plaintext analysis: the chi-squared statistic and searching for patterns (monograms, bigrams, and trigrams). When working with classic and modern cryptography, text characterization as technique is a very important part of the cryptanalysis backpack.

# The Chi-Squared Statistic

The chi-squared statistic is an important metric that computes the similarity percent between two probability distributions. There are two situations when the result of the chi-squared statistic is equal to 0; it means that the two distributions are similar. If the distributions are very different, a higher number will be outputted.

The chi-squared statistic is defined by the following formula:

$$\chi^2(C,E)=\sum_{i=A}^{i=Z}\frac{(C_i - E_i)^2}{E_i}$$

In the following example (see Listing 22-1), we will compute an example of a chi-squared distribution.

# Listing 22-1. Chi-Squared Distribution Source Code

```
#include <iostream>
#include <random>

int main()
{
    const int number_of_experiments=10000;
    const int number_of_stars_distribution=100;    // maximum number of stars
                                                    to distribute
```