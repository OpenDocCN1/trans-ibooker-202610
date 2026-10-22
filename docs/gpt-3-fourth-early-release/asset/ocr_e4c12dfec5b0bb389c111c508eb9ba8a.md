Sentiment analysis(positive, neutral, negative): positive
Tweet: "I think it is neither super likely nor super unlikely that the super intelligent AI will emerge one day."
Sentiment analysis(positive, neutral, negative): neutral
Tweet: "Super intelligent AI is going to be the most exciting discovery in human history."
Sentiment analysis(positive, neutral, negative):

# And the output:

positive

![](bbox=[0.15, 0.295, 0.85, 0.62])

# Batch Classification

After understanding the few-shot classification with GPT-3, let’s dive deeper into the batch classification which allows you to classify input samples in batches in a single API call instead of just classifying one example per API call. It is suitable for applications where you want to classify multiple examples in a single go, just like the tweet sentiment analysis task we examined, but analysing a few tweets in a row.

As with few shots classification, you want to provide enough context for the model to achieve the desired result but in a batch configuration format. Here, we define the different categories of tweet sentiment classification