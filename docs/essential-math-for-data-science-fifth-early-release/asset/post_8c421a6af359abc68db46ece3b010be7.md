

indistinguishable from the normal distribution, which neatly reflects the ideas behind the central limit theorem.

Example 3-23 below shows how to find the critical t-value for 95% confidence. You can use this for confidence intervals and hypothesis testing when you have a sample size of 30 or less. It's conceptually the same as the critical z-value, but we are using a T-distribution instead of a normal distribution to reflect greater uncertainty. The smaller the sample size, the larger the range reflecting greater uncertainty.

## Example 3-23

```
from scipy.stats import t

# get critical value range for 95% confidence
# with a sample size of 25

n = 25
lower = t.ppf(.025, df=n-1)
upper = t.ppf(.975, df=n-1)

print(lower, upper)
-2.063898561628021 2.0638985616280205
```

Note that df is the “degrees of freedom” parameter, and as outlined earlier it should be one less than the sample size.

## BEYOND THE MEAN

We can use confidence intervals and hypothesis testing to measure other parameters besides the mean, including variance/standard deviation as well as proportions (e.g. 60% of people report improved happiness after walking 1 hour a day). These require other distributions such as the Chi-Square distribution instead of the normal distribution. As stated, these are all beyond the scope of this book but hopefully you have a strong conceptual foundation to extend into these areas if needed.

We will however use confidence intervals and hypothesis testing throughout Chapter 5 and Chapter 6.

## Big Data Considerations and Texas Sharpshooter Fallacy

One final thought before we close this chapter. As we have discussed, randomness plays such a role in validating our findings and we always have to account for its possibility. Unfortunately with “big data”, machine learning, and other data-mining tools the scientific method has suddenly become a practice done backwards. This can be precarious and allow me to demonstrate why, adapting an example from Gary Smith in his book Standard Deviations.

Let’s pretend I draw 4 playing cards from a fair deck. There’s no game or objective here other than to draw 4 cards and observe them. I get 2 tens, a three, and a two. “This is interesting” I say. “I got 2 tens, a three, and a two. Is this meaningful? Are the next four cards I draw also going to be two consecutive numbers and a pair? What’s the underlying model here?”

See what I did there? I took something that was completely random and I not only looked for patterns, but I tried to make a predictive model out of them. What has subtly happened here is I never made it my objective to get these four cards with these particular patterns. I observed them after they occurred.

This is exactly what data mining falls victim to every day: finding coincidental patterns in random events. With huge amounts of data and fast algorithms looking for patterns, it’s not hard to find things that look meaningful but actually are just random coincidences.

This is also analogous to me firing a gun at a wall. I then draw a target around the hole and I then bring my friends over to show off my amazing marksmanship. Silly, right? Well many people in data science figuratively do this everyday and it is known as the Texas Sharpshooter Fallacy. They set out to act without an objective, stumble on something rare, and then point out what they found somehow creates predictive value.

The problem is the law of truly large numbers says rare events are likely to be found, we just do not know which ones. When we encounter rare events, we highlight and even speculate what might have caused them. The issue is this. The probability of a specific person winning the lottery is highly unlikely, but yet someone is going to win the lottery. Why should we be surprised when there is a winner? Unless somebody predicted the winner, nothing meaningful happened other than a random person got lucky.

This also applies to correlations which we will study in Chapter 5. With an enormous dataset with thousands of variables, is it hard to find statistically significant findings with a .05 p-value? You betcha! I’ll find thousands of those. I’ll even show evidence the number of Nicholas Cage movies correlates with the number of pool drownings in a year.

So to prevent the Texas Sharpshooter Fallacy and falling victim to big data fallacies, try to use structured hypothesis testing and gather data for that objective. If you utilize data mining, try to obtain fresh data to see if your findings still hold up. Finally, always consider the possibility that things can be coincidental and if there is not a common sense explanation then it probably was coincidental.

We learned how to hypothesize before gathering data, but data mining gathers data then hypothesizes. Ironically, we are often more objective starting with a hypothesis, because we then seek out data to deliberately prove and disprove our hypothesis.

## Conclusions

We learned a lot in this chapter, and you should feel good about getting this far. This was probably one of the harder topics in this book! We learned not only descriptive statistics from the mean to the normal distribution, but we also tackled confidence intervals and hypothesis testing.

Hopefully you see data a little differently. It is snapshots of something rather than a complete capturing of reality. Data on its own is not very useful, and we need context, curiosity, and analysis of where it came from before we can make meaningful insights with it. We covered how to describe data as well as infer attributes about a larger population based on a sample. Finally we addressed some of the data mining fallacies we can stumble into if we are not careful, and how to remedy that with fresh data and common sense.

Do not feel bad if you need to go back and review some of the content in this chapter, because there’s a lot to digest. It is also important to get in the hypothesis testing mindset if you want to be successful at a data science and machine learning career. Few practitioners take time to link statistics and hypothesis testing concepts to machine learning, and it is unfortunate. Understandability and explainability is the next frontier of machine learning, so continue to learn and integrate these ideas as you progress throughout the rest of this book and the rest of your career.

## Exercises

- 1. You bought a spool of 1.75mm filament for your 3D printer. You want to measure how close the filament diameter really is to 1.75 mm. You use a caliper tool and sample the diameter five times on the spool: 1.78, 1.75, 1.72, 1.74, 1.77 Calculate the mean and standard deviation for this set of values.

- 2. A manufacturer says the Z-Phone smart phone has a mean consumer life of 42 months with a standard deviation of 8 months. Assuming a normal distribution, what is the probability a given random Z-Phone will last between 20 and 30 months?

- 3. I am sceptical that my 3D printer filament is not 1.75mm in average diameter as advertised. I sampled 34 measurements with my tool. The sample mean is 1.715588 and the sample standard deviation is 0.029252. What is the 99% confidence interval for the mean of my entire spool of filament?

- 4. Your marketing department has started a new advertising campaign and wants to know if it affected sales, which in the past averaged $10,345 a day with a standard deviation of $552. The new advertising campaign ran for 45 days and averaged $11,641 in sales. Did the campaign affect sales? Why or why not? (Use a 2-tailed test for more reliable significance)

Answers are in Appendix B.

## Chapter 4. Linear Algebra

Changing gears a little bit, let's venture away from probability and statistics and into linear algebra. Sometimes people confuse linear algebra with basic algebra, thinking maybe it has to do with plotting lines using the algebraic function y = mx + b. This is why linear algebra should probably have been called “vector algebra” or “matrix algebra” because it is much more abstract. Linear systems play a role, but in a much more metaphysical way.

So, what exactly is linear algebra? Well, linear algebra concerns itself with linear systems, but represents them through vector spaces and matrices. If you do not know what a vector or a matrix is, do not worry! We will define and explore them in depth. Linear algebra is hugely fundamental to many applied areas of math, statistics, operations research, data science, and machine learning. When we work with data in any of these areas, we are using linear algebra and perhaps you may not even know it.

You can get away with not learning linear algebra for awhile, using machine learning and statistics libraries that do it all for you. But if we are going to get intuition behind these black boxes and be more effective at working with data, understanding the fundamentals of linear algebra is an inevitability. Linear algebra is an enormous topic that can fill thick textbooks, so of course we cannot gain total mastery in just one chapter of this book. However, we can learn enough to be more comfortable with it and navigate the data science domain effectively. There will also be opportunities to apply it in the remaining chapters in this book, including Chapter 5 and Chapter 7.

## What is a Vector?

Simply put a vector is an arrow in space with a specific direction and length, often representing a piece of data. It is the central building block to linear algebra, including matrices and linear transformations. In its

