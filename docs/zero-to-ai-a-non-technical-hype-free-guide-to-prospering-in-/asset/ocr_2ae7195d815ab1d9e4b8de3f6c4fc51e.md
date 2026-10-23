![](bbox=[0.15, 0.08, 0.85, 0.28])

Figure 5.8  In these word embeddings represented on 3-D planes, positive and negative words are on the left, while words related to neighborhoods and living rooms are on the right.

learning is a way of “warming up” a model with large amounts of easily available generic data before training it by using domain-specific training data, which is often harder to get. Just like computer vision applications, in which models can learn simple shapes and patterns, NLP models will pick up basic vocabulary and grammar, which are useful for pretty much everything (“beautiful” has the same meaning whether you’re talking about houses or anything else).

The NLP research community has released several DL models that were trained on huge text databases and can thus understand and create embeddings of almost a complete vocabulary. Figure 5.9 sketches out the basic idea behind adopting freely available embeddings for your specific classification task.

![](bbox=[0.17, 0.56, 0.77, 0.86])

Figure 5.9  A typical NLP pipeline: For the first three steps (in the gray box), you can take advantage of open source models and embeddings. The last three steps involve domain-specific datasets.