# HOW MUCH SAMPLE IS ENOUGH?

While 31 is the textbook number of items you need in a sample to satisfy the central limit theorem and see a normal distribution, this sometimes is not the case. There are cases you will need an even larger sample, such as when the underlying distribution is asymmetrical or multimodal (meaning it has several peaks rather than one at the mean).

In summary, having larger samples are better when you are uncertain of the underlying probability distribution. You can read more in this article.

# Confidence Intervals

You may have heard the term confidence interval, which often confuses statistics newcomers and students. A confidence interval is a range calculation showing how confidently we believe a sample mean (or other parameter) falls in a range for the population mean.

**Base on a sample of 31 golden retrievers with a sample mean of 64.408 and a sample standard deviation of 2.05, I am 95% confident that the population mean lies between 63.686 and 65.1296. How do I know this? Let me show you and if you get confused, circle back to this paragraph and remember what we are trying to achieve. I highlighted that in bold for a reason!**

I first start out by choosing a level of confidence (LOC), which will contain the desired probability for the population mean range. I want to be 95% confident that my sample mean falls in the population mean range I will calculate. That’s my LOC. We can leverage the central limit theorem and infer what this range for the population mean is. First, I need the **critical Z value** which is the symmetrical range in a standard normal distribution that gives me 95% probability in the center as highlighted in Figure 3-14.