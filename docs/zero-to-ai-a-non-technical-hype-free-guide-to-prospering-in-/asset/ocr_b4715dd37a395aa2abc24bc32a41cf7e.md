You can imagine an algorithm with a high precision and low recall as a sniper: it wants to be sure before shooting, so it stays conservative and doesn’t shoot unless it’s 100% sure. This means that it misses some targets, but every time it fires, you’re confident that it’s going to hit the right target. On the other hand, an algorithm with a high recall and low precision is like a machine gun: it shoots a lot and hits many of the targets it should hit, but also hits some wrong ones along the way. Table 3.5 summarizes the difference between precision, recall, and accuracy, and what these metrics tell you.

Table 3.5 Accuracy, precision, and recall
| Metric | Definition | What this metric is telling you |
| --- | --- | --- |
| Accuracy | True Positives + True Negatives / Total examples | How many mistakes is the algorithm making? No difference in misclassifying a positive from a negative. |
| Precision | True Positives / True positives + False positives | How much should you trust the algorithm when it spots a positive example? |
| Recall | True Positives / True positives + False negatives | Out of all the positive examples, how many did the algorithm spot? |

# 3.5.2 Clustering algorithms

When it comes to unsupervised learning, evaluating performance is tricky because there’s no objective “greatness” metric: because we don’t have labels to compare, we can’t define whether the algorithm’s output is “right” or “wrong.” Remember also that most clustering algorithms require you to define the number of clusters you want it to identify, so another question you’ll have to answer is whether your choice for the number of clusters was good. How do we get out of this apparently foggy situation?

First of all, some mathematical tools can tell a data scientist whether a clustering has been done well. Unfortunately, having an algorithm that performs great from a mathematical point of view isn’t necessarily a sign of it being useful for business purposes. If you bought this book, we assume that you’re not using ML to publish a scientific paper but rather to help your organization. If this is true, mathematical stunts won’t be of any interest to you. What you should do instead is look at your results and ask yourself these questions:

+   1 Are the results interpretable? In other words, are the cluster centers interpretable as buyer personas that make logical sense? If the answer is yes, move to question 2.

+   2 Are the results actionable? In other words, are my clusters different enough that I can come up with different strategies to target customers belonging to the different centers?

If the answer to these questions is yes, congrats: you can start testing the results in the real world, collect data, and move forward either by iterating and tweaking your algorithm when you have more data, or by using the new knowledge to redesign your