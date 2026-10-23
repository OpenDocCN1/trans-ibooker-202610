
different messages, which Opower did by using behavioral science and A/B testing (sending different messages to different people and measuring the results).

A final implementation note on the number of clusters that the algorithm identified here. Opower had five, but where did this number come from? With some of the widely used algorithms, you need to come up with the number of clusters the algorithm has to look for, and the algorithm will optimize for it.

Mathematical tools provide data scientists clues about what number would yield the best results from a mathematical standpoint, but it wouldn’t necessarily be the best solution for your business needs, so there’s no “right” or “wrong” solution. Because there’s no script for deciding the number of clusters, coming up with this number can sometimes feel like doing grocery shopping: “Do we want five clusters? Let’s do six; it’s better to have a little more, just in case we run out of them.” We can argue that making a decision is actually much simpler.

Think about this: every time you ask the algorithm to find one more cluster, it’s going to see new, subtler patterns in the data. Assuming we have 1,000 users, the upper limit would be to find 1,000 clusters, where each user has a cluster of their own. Before you reach that extreme situation, you’ll sometimes find that you can’t really gain any new knowledge from adding a new cluster. This is a good moment to stop. Adding clusters will make the results of the algorithm more specific, but also less interpretable. Figure 3.14 illustrates this trade-off.

![](img/51e9508eebbc565953ed15bd4b5bec8f_84_0.png)

The ideal process would be to start with a number of clusters that is rather small; ideally, this number can be provided by your data scientists, who will use mathematical tools to find out what makes sense from a technical standpoint. From there, you can move either down, condensing different clusters, or up, increasing the granularity of the clustering. Every time you increase or decrease the number of clusters, try interpreting the results and ask yourself whether a higher number of clusters helps you better understand the phenomenon. If the answer is no, we can argue that you found the

Target was reportedly able to increase sales thanks to its new strategy. However, increased revenue came at a cost: people started sensing they were being spied on and weren’t happy with that. The first warning sign that the general public might not like such intensive targeting techniques came when a disgruntled father complained about the coupons for baby products that his high-school daughter had been getting from Target. As reported in the same Forbes article, he complained to the store manager, shouting:

> My daughter got this in the mail! She’s still in high school, and you’re sending her coupons for baby clothes and cribs? Are you trying to encourage her to get pregnant?

It turns out, the girl was actually pregnant, and Target had figured it out before her father did. The Target’s manager reportedly called the angry father a while later, and got his response as reported by Forbes:

> I had a talk with my daughter. It turns out there’s been some activities in my house I haven’t been completely aware of. She’s due in August. I owe you an apology.

As reported in the Forbes article, Target ended up mixing personalized recommendations obtained through ML with seemingly random items from the catalog (for instance, adding a coupon for a lawn mower right next to one for diapers), to avoid making customers feel like they were being stalked.

## CASE QUESTION

Are special discounts an effective way to win customers over competitors?

## CASE DISCUSSION

Special offers and coupons are an extremely common strategy that retailers use to attract customers. However, when everybody knows a person is looking for a specific product that everybody can sell, the effects are usually a promotions war, eating retailers’ margins. This may be good for customers but definitely is not for retailers. We argue that offering discounts to customers can be a valid marketing idea if the following are true:

- 1. The retailer knows who to send the coupons to (it’s effective at targeting its users).
- 2. Other retailers are not targeting that person at the same time, to avoid the price wars mentioned previously.

The initial strategy that retailers adopted was clever: using openly available registry office data for newborns to target families immediately after a baby is born. This strategy is effective at point 1 (targeting the right customer), but fails at point 2: as the name says, open data is open, and every retailer can use it.

Target’s case is a perfect example of how you can use data and machine learning to win over competitors, targeting the perfect customer before anyone else. Target started from its core business sales data: the shopping habits of customers, collected through fidelity cards. It was also able to build a label “is waiting for a child”/“is not

waiting for a child” for past customers, by looking at whether this customer actually started buying diapers in the future. We hope you already figured out that this looks like a classic supervised learning classification problem:

- - The features are the shopping habits of the customer (let data scientists figure out how to build them exactly).
- - The label is whether that customer is waiting for a baby, according to feature purchases of baby-related goods.
- - After the model is built for past data, it can be applied to current customers, identifying the ones waiting for a baby.

Notice that the label wasn’t immediately available: Target’s customers don’t call the retailer, communicating that a baby is born. However, this label can be assigned to past customers by looking at their past purchasing behavior: if a customer started buying diapers or a baby stroller six months ago, she must have been pregnant within the nine months before and can be labeled as a positive example.

Think about how powerful this approach is. Patterns exist in the purchasing habits of pregnant women: they may stop buying certain foods like sausages or sushi, eat healthier food, or quit cigarettes. The problem is that when there’s a lack of clear indicators (for example, the purchase of prenatal vitamins or diapers) it’s hard for a human to spot patterns by looking at customer purchases. But it’s not hard for an ML algorithm, which can figure out which purchasing habits suggest a pregnancy.

Something else you should keep in mind is that what may sound like a nice story, and a great move from Target to boost sales, can also be a ticking PR time bomb due to aggressive targeting in such sensitive domains.

To summarize, you should keep the following in mind from this case:

- - ML can help you predict future purchases, anticipating competitors.
- - Core business data has a much higher value than openly available data (more about this in chapter 8).
- - Be on the lookout for the creepiness factor: ML can be so powerful that people might feel like you’re spying on them.

## Summary

- - The core reasons behind the effectiveness of AI in marketing and sales are the ability to target specific customers at scale and make data-driven decisions.
- - A churn prediction model is a typical application of AI in marketing, and can spot which customers are most likely to quit your service.
- - Likewise, machine learning can help marketers find upselling opportunities by identifying which customers are more inclined to buy more.
- - Unsupervised learning is a subset of machine learning that enables computers to find structure in data, finding clusters of similar data points. Clustering also has uses in marketing.
- - There are different ways and metrics to evaluate the performance of classification algorithms (we covered accuracy, recall, and precision). Each one tells us something different about the algorithm, and you can look at your business objectives to choose the most appropriate one.
- - Opower used unsupervised learning to divide energy consumers into clusters and then targeted them individually to optimize their energy consumption patterns.
- - Target used supervised learning to identify which of their customers was about to have a baby, and then offered them coupons for diapers and baby products.
