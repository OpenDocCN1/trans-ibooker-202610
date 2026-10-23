
defected to a competitor before we could entice them with a discount) costs us $500 in lost revenue. Each false positive (a loyal customer who would not have churned but received a gift anyway) costs us $100 in lost revenue. We can now use some basic accounting to tie the performance of the model to a monetary value:

Total cost = $500 * FN + $0 * TN + $100 * FP + $100 * TP

Here, FN is the number of false negatives, TN is true negatives, FP is false positives, and TP is true positives.

You can use these same ideas for other situations where you’re using a binary classification model. For example, say that you’re developing an automated quality control system for a manufacturing line. You can tweak the model to either let more items through (increase the false-negative rate) or reject more items right away (increase the false-positive rate). The former might let more faulty items through, thus leading to more issues downstream in the manufacturing process. The latter might result in unneeded waste of material, as perfectly fine products are rejected. In any case, you can adapt the preceding formula to your specific business situation.

Whatever your strategy, you still need to be comfortable with the fact that no machine learning algorithm will be perfect (if it is, something is wrong). You’ll make mistakes: make sure that these mistakes are acceptable and that both you and your algorithm learn from them.

## 3.7 Case studies

This section presents two case studies from real companies that used AI for their marketing efforts. The first one is Opower, an energy company that used clustering to segment its users based on their energy consumption habits. The second one is a Target initiative: the retail store used the data of its customers to identify signs of a pregnancy early on and start advertising diapers.

### 3.7.1 AI to refine targeting and positioning: Opower

The relationship that energy companies have with customers is peculiar: although consumer companies normally get paid when the customer makes a purchase, with energy, there’s a lag: people first consume energy for a period of time and get billed later. Also, people don’t buy energy for the sake of it: it’s a necessary expense to enjoy other goods (such as turning on a TV).

These two factors make it hard for every utility to know its customers and interact with them. This isn’t a problem just for the marketing department of energy companies, but also for their overall costs and profitability. In fact, producing energy doesn’t always cost utilities the same price: it depends on the kind of power plant they use to produce it, a choice that depends on how much energy they need to produce and how well in advance they planned for it.

The best-case scenario is a perfectly flat demand: utilities can start producing X amount of energy using their best power plants and never change. This is, of course, never the case, as daily demand has a duck shape instead, with two peaks usually happening when people wake up and when they come back home from work. To fulfill demand during these peaks, utilities need to use power plants that are easy to turn on and off and modulate, but flexibility comes at a price: these plants are more expensive to run, consume more energy, and burn more-expensive fuels. As represented in figure 3.10, this problem is getting worse: today’s habits are making the energy demand curve less flat, with a bigger valley during the day and sharper peaks in the morning and in the evening. The dream of every utility company is to be able to shape their customers’ habits by moving their peaks to different times of the day, achieving an overall almost flat demand.

![](img/51e9508eebbc565953ed15bd4b5bec8f_80_0.png)

Figure 3.10 Typical load curves for the energy consumption of California homes

## OPOWER’S STRATEGY

Opower was founded in 2007 with the mission of providing energy companies the tools to target their users and engage them in changing their consumption habits, both to save energy and to achieve an overall flatter demand. The company was getting smart meter data from utilities’ customers, and could send them reports with details on their energy consumption and tips to improve their habits, using mail, email, internet portals, text messages, and sometimes in-home energy displays.

![](img/51e9508eebbc565953ed15bd4b5bec8f_81_0.png)

Figure 3.11 Energy consumption values of a household at different times of the day

The company’s secret sauce is a mixture of behavioral science and data science. Opower turned to one of the core applications of machine learning—unsupervised learning—which it used to identify particular user groups that could be targeted similarly.

The most characteristic element of an energy user is their consumption pattern: how much energy they use during different hours of the day. We can therefore see each user as in figure 3.11: a combination of 24 numbers that represent the amount of energy consumed for each hour of the day. These numbers draw an accurate picture of the habits of the users, just as the number of purchases, average expense, and items bought would be in an e-commerce scenario.

Spotting different groups of users based on their consumption patterns isn't an easy task. If we represent 1,000 users in this way and plot all of them on a graph, we get what data scientists like to call a hairball: an intricate mix of lines, one on top of the other, impossible for a human eye to distinguish. On the other hand, identifying patterns is where unsupervised algorithms are the most useful; using these, Opower was able to spot five user clusters. We gathered data from the internet and ran an unsupervised algorithm to mimic the results that Opower achieved; you can see the hairball and the final clusters in figure 3.12.

![](img/51e9508eebbc565953ed15bd4b5bec8f_82_0.png)

![](img/51e9508eebbc565953ed15bd4b5bec8f_82_1.png)

Figure 3.12 Turning a “hairball” of 1,000 users into five clusters

Looking at these clusters, we can try interpreting each of them:

- *Twin Peaks*—They consume a lot of energy in the early morning, way more than users from other clusters. They also have a peak during the evening at about dinner time, but a bit lower than their morning peak.
- *Evening Peakars*—They have a high peak during the evening that they reach smoothly, starting from the early morning. The first morning peak is absent.
- *Day Timers*—They don’t have the classic peaks that all other clusters have. Instead, they have a high consumption during the day that ramps up and slows down smoothly.
- *Steady Eddies*—They’re the steadiest ones; the two classic peaks are very smooth.
- *Night Owls*—They have classic peaks during the early morning and late evening but tend to consume energy during the night.

Opower used the results of the clustering to target different users in different ways. Its infrastructure allowed splitting each cluster into groups and testing different messages. From the analysis of the results and using behavioral science, Opower was able

![](img/51e9508eebbc565953ed15bd4b5bec8f_83_0.png)

# Figure 3.13 Decision tree that Opower used to target its users

to constantly improve its targeting. An example of this segmentation is represented in figure 3.13: Opower could pick a specific cluster (the Night Owls, in this case), and split them into two groups that it targeted with different emails. This approach allowed Opower to optimize its messaging and target people in the best way possible.

Thanks to its approach, Opower was able to help consumers save 10% on their energy bills, while making utilities more profitable, thanks to the better distribution of the total demand. As a result of its performance, Opower was acquired by Oracle in 2016 for $532 million.

## CASE QUESTION

How can you persuade a group of people to change their behavior?

## CASE DISCUSSION

Think for a second about the complexity of Opower’s mission: it has to enable millions of people to change their way of consuming energy by sending them some kind of report. You’d be naïve to think the same message could work for all of them.

Every person responds to different incentives and different messaging, so ideally you’d have to target everyone with a different message. This is, of course, not practical or scalable, but you can still go a long way by splitting your audience into groups that have something in common. For Opower, this first step was done using unsupervised learning. Remember that unsupervised learning is a class of ML algorithms that looks for patterns in data, finding groups of similar data points based on their features. In Opower’s case, the most important characteristic was the user’s energy consumption habits, so Opower needed a way to account for this. Opower’s idea to express energy consumption habits into features (a bunch of numbers) was to consider in each hour of the day the amount of energy a certain user was consuming. The result was a representation of each person made up of 24 numbers, which is easy enough for a human to grasp, but very easy for a computer: once an unsupervised learning algorithm is fed with this data, it can easily compare two users and spot similarities.

Unsupervised learning provided Opower with the first, most important distinction across users: their energy consumption. The second step was measuring the effects of
