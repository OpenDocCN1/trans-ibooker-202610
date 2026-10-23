defected to a competitor before we could entice them with a discount) costs us $500 in lost revenue. Each false positive (a loyal customer who would not have churned but received a gift anyway) costs us $100 in lost revenue. We can now use some basic accounting to tie the performance of the model to a monetary value:

Total cost = $500 * FN + $0 * TN + $100 * FP + $100 * TP

Here, FN is the number of false negatives, TN is true negatives, FN is false negatives, and TP is true positives.

You can use these same ideas for other situations where you’re using a binary classification model. For example, say that you’re developing an automated quality control system for a manufacturing line. You can tweak the model to either let more items through (increase the false-negative rate) or reject more items right away (increase the false-positive rate). The former might let more faulty items through, thus leading to more issues downstream in the manufacturing process. The latter might result in unneeded waste of material, as perfectly fine products are rejected. In any case, you can adapt the preceding formula to your specific business situation.

Whatever your strategy, you still need to be comfortable with the fact that no machine learning algorithm will be perfect (if it is, something is wrong). You’ll make mistakes: make sure that these mistakes are acceptable and that both you and your algorithm learn from them.

# 3.7 Case studies

This section presents two case studies from real companies that used AI for their marketing efforts. The first one is Opower, an energy company that used clustering to segment its users based on their energy consumption habits. The second one is a Target initiative: the retail store used the data of its customers to identify signs of a pregnancy early on and start advertising diapers.

# 3.7.1 AI to refine targeting and positioning: Opower

The relationship that energy companies have with customers is peculiar: although consumer companies normally get paid when the customer makes a purchase, with energy, there’s a lag: people first consume energy for a period of time and get billed later. Also, people don’t buy energy for the sake of it: it’s a necessary expense to enjoy other goods (such as turning on a TV).

These two factors make it hard for every utility to know its customers and interact with them. This isn’t a problem just for the marketing department of energy companies, but also for their overall costs and profitability. In fact, producing energy doesn’t always cost utilities the same price: it depends on the kind of power plant they use to produce it, a choice that depends on how much energy they need to produce and how well in advance they planned for it.