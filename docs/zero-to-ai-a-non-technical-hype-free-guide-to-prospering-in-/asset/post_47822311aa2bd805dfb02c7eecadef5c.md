
You can imagine an algorithm with a high precision and low recall as a sniper: it wants to be sure before shooting, so it stays conservative and doesn’t shoot unless it’s 100% sure. This means that it misses some targets, but every time it fires, you’re confident that it’s going to hit the right target. On the other hand, an algorithm with a high recall and low precision is like a machine gun: it shoots a lot and hits many of the targets it should hit, but also hits some wrong ones along the way. Table 3.5 summarizes the difference between precision, recall, and accuracy, and what these metrics tell you.

**Table 3.5 Accuracy, precision, and recall**

| Metric | Definition | What this metric is telling you |
| --- | --- | --- |
| Accuracy | (True Positives + True Negatives) / Total examples | How many mistakes is the algorithm making? No difference between misclassifying a positive and a negative. |
| Precision | True Positives / (True positives + False positives) | How much should you trust the algorithm when it spots a positive example? |
| Recall | True Positives / (True positives + False negatives) | Out of all the positive examples, how many did the algorithm spot? |

### 3.5.2 Clustering algorithms

When it comes to unsupervised learning, evaluating performance is tricky because there’s no objective “greatness” metric: because we don’t have labels to compare, we can’t define whether the algorithm’s output is “right” or “wrong.” Remember also that most clustering algorithms require you to define the number of clusters you want it to identify, so another question you’ll have to answer is whether your choice for the number of clusters was good. How do we get out of this apparently foggy situation?

First of all, some mathematical tools can tell a data scientist whether a clustering has been done well. Unfortunately, having an algorithm that performs great from a mathematical point of view isn’t necessarily a sign of it being useful for business purposes. If you bought this book, we assume that you’re not using ML to publish a scientific paper but rather to help your organization. If this is true, mathematical stunts won’t be of any interest to you. What you should do instead is look at your results and ask yourself these questions:

1. Are the results interpretable? In other words, are the cluster centers interpretable as buyer personas that make logical sense? If the answer is yes, move to question 2.

2. Are the results actionable? In other words, are my clusters different enough that I can come up with different strategies to target customers belonging to the different centers?

If the answer to these questions is yes, congrats: you can start testing the results in the real world, collect data, and move forward either by iterating and tweaking your algorithm when you have more data, or by using the new knowledge to redesign your approach. Luckily, underperforming unsupervised algorithms are usually less risky than underperforming supervised learning ones, as the concept of “right” or “wrong” prediction is foggier. For this reason, you don’t have to worry much about metrics, but rather about the testing methodology you should have in place to evaluate the business impact of your project. We’ll talk about the iteration and design process extensively in part 2 of this book.

## 3.6 Tying ML metrics to business outcomes and risks

Now that you have gained some familiarity with common ML metrics, let’s see what they imply in a business scenario. Let’s assume you have deployed a great churn predictor that has identified a group of customers likely to leave your service, and you want to reach out with a personalized phone call to each of them.

If your data science team has built a model with high *precision* but low *recall*, you won’t waste many phone calls: every time you call, you’ll talk with a user who is really considering leaving you. On the other hand, some users are leaving the service, but your algorithm has failed to spot them. A high-recall, low-precision algorithm would instead have you make a lot of phone calls—so you’ll reach out to a large chunk (even all) of the customers planning to abandon your service, but you’ll waste phone calls to other users who weren’t planning to unsubscribe at all.

The trade-off between precision and recall is clearly a function of the business. If each customer has a high value and reaching out is cheap, go with a high recall. If your customers are not making expensive purchases, don’t want to be disturbed unnecessarily, and calling them is expensive, go with a high-precision one. An even more sophisticated strategy could be to reserve more-expensive actions like phone calls for customers with higher value or a higher probability of churn, and use cheaper actions like email for others. As you can see in figure 3.9, you can decide whether to focus on recall or on precision based on two parameters: the cost of losing a customer, and the cost of the actions you pursue to try to retain them.

![](img/51e9508eebbc565953ed15bd4b5bec8f_77_0.png)

*Figure 3.9 A cheat sheet to decide whether to focus on high-precision, high-recall or just general accuracy based on key business metrics*

The problem becomes even more serious in safety-critical applications. If we have a classifier used for medical diagnoses, the cost of a false positive and false negative are very different. Suppose our algorithm is detecting lung cancer. In table 3.6, you can see what each error means and its implications.

**Table 3.6 Implications of true positives, true negatives, false positives, and false negatives**

| Metric | What does it mean | Implication |
| --- | --- | --- |
| True positive | The algorithm predicts that the patient has cancer, and it’s right: the patient really is sick. | The patient gets the treatment needed right away. |
| True negative | The algorithm predicts the patient is healthy, and indeed the patient is. | The patient goes home relieved. |
| False positive | The algorithm predicts that the patient has cancer (hence the positive), but the patient actually is healthy (hence the false). | The patient gets scared and maybe starts a treatment, but is actually OK. |
| False negative | The algorithm predicts the patient is healthy (negative), but the patient is sick instead (false). | The patient goes home feeling relieved but actually is sick and doesn’t receive the treatment needed. |

As you can see, the cost associated with a mistake is very different, depending on whether we’re misclassifying healthy or sick patients. A false positive can imply additional exams, or maybe unnecessary treatment, but a false negative will deprive a patient of the therapy desperately needed to survive.

In such a scenario, you clearly see how dangerous it is to optimize for the wrong metric. Having high accuracy here wouldn’t be indicative of a good algorithm, as it would weight false negatives and false positives in the same way. What we want in this case is a high recall: we want the highest number of patients who are sick to be identified, even if that means having some false positives. False positives may lead to additional unnecessary exams and frightening some families, but it will ensure that the highest number of sick people are spotted and taken care of.

As you can see, every algorithm has an indicator of its performance, and you need to be able to assess which metric is the most important for you, so that your data science team can work on maximizing it. We’ve seen that the best choice is not always obvious. From a business point of view, the most powerful idea is to tie each misclassification to a dollar amount. If your assumptions are correct, this will automatically lead to the best business outcome. Our imaginary telecom company could develop a retention plan for disgruntled customers that gives them a $100 bonus if they stay with the company for the next 12 months, thus retaining a customer that would have otherwise defected to a competitor in the near future.

On the other hand, let’s assume that each lost customer loses the company $500 in profit. Now that we have all these numbers, we can easily compute just how much money false negatives and false positives cost us. Each false negative (a customer who
