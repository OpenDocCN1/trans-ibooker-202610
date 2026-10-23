![](bbox=[0.19, 0.06, 0.65, 0.39])

# 5.3.1  Sentiment analysis

A surprising amount of textual content produced by users on the internet deals with opinions and feelings. For example, think about user reviews on Amazon, Twitter replies, or even blog articles. *Sentiment analysis algorithms* digest all this free-form text, and figure out whether the author had a positive, negative, or neutral opinion about the subject.

Sentiment analysis is an important task, and it can be used for much more than sorting reviews of houses. You could monitor the results of your marketing campaign in real time by following feeds from social media, or even gauge the reaction to individual presentations during a conference. Wall Street trading firms routinely use sentiment analysis to measure what the public thinks about a company and to inform their trading decisions.

But let's go back to our real estate listings website. These are excerpts from some of the reviews posted by users:

+   - The kitchen appliances are old and smelly.
+   - Views from the living room are amazing at sunset.

What we want to do is build an algorithm that can label each sentence as positive or negative, as in figure 5.6.