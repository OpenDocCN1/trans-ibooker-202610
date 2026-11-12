
## Regularization in Deep Learning

Peng Liu

![](img/ee98e91e6845f41077b163bec833ee40_0_0.png)

MEAP
MANNING

![](img/ee98e91e6845f41077b163bec833ee40_1_0.png)

## MEAP Edition
Manning Early Access Program
Regularization in Deep Learning
Version 6

Copyright 2023 Manning Publications

For more information on this and other Manning titles go to
manning.com

## welcome

Thank you for purchasing the MEAP edition of Regularization in Deep Learning.

One of the most important goals in building machine learning and especially deep learning models is to achieve good generalization performance in the test dataset. The training task is considered to be completed when we have obtained a generalizable model, often with the help of proper regularization in the training process. While the theory of generalization still remains a mystery, it is an active research area with new insights being proposed.

Currently, there are quite a number of regularization techniques that have proved to be empirically effective in a specific training context. However, these resources are often scrambled and disconnected. This book intends to bridge the gap by offering a systematic and well-illustrated perspective on different regularization techniques, covering data, model, cost function, and optimization procedure. It even goes one step further by mixing the most recent research breakthroughs with practical coding examples on regularization in deep learning models.

This book entails this complex and ever-growing topic in a unique way. It introduces minimal mathematics and technical concepts in a well-illustrated manner and provides practical examples and code walkthroughs offered via a step-by-step fashion. The teaching is designed to be intuitive, natural, and progressive, instead of forcing in a particular concept.

We hope that you will enjoy reading the book and acquire a few useful tools under your belt to build better-performing models. We also encourage you to post any questions or comments you have about the content in the liveBook Discussion forum. We appreciate knowing where we can make improvements and increase your understanding of the subject.

Dr Peng Liu

## brief contents

- 1 Introducing Regularization
- 2 Generalization: A Classical View
- 3 Generalization: A Modern View
- 4 Fundamentals of Training Deep Neural Networks
- 5 Regularization via Data
- 6 Regularization via Model
- 7 Regularization via Objective Function
- 8 Regularization via Optimization
- 9 Regularization via Smart Predict-then-Optimize
- 10 Frontiers in Generalization

## Introducing Regularization

### This chapter covers

- * The what and why of regularization in deep learning
- * Underfitting versus overfitting
- * Bias and variance trade-off
- * A typical model training process
- * Different types of regularization

We all want to do well in exams. Since young, we have most likely been taught to study hard to achieve high marks, not only in the usual practice exam but also in the final exam. In the world of modeling, the practice exam is analogous to the training data given to us, where both questions and answers are available. We want to train a model based on the training data and use the trained model to make predictions for the test data, which is the final exam.

Our goal is to train an excellent mental model that works well for the practice exam and final exam. We know that the common problem-solving patterns will largely remain the same between the two exams, so it is essential to understand and learn these patterns during the practice exam in order to be able to do well in the final exam. Depending on our preparation strategy, our trained mental model could perform good or bad in either exam, ending up with one of the four possible outcomes: good in both exams, bad in both exams, good in the practice exam but unfortunately bad in the final exam, and bad in the practice exam but surprisingly good in the final exam. These four different exam outcomes are illustrated in figure 1.1.

![](img/ee98e91e6845f41077b163bec833ee40_5_0.png)

Figure 1.1 There are four different outcomes for the practice and final exams, where the practice exam contains question-answer pairs, and the final exam only contains the questions. This also corresponds to the four possible scenarios in model development, where the available training data contain the input-output pairs, and the test data only contain the input data. The trained model is assessed on both training and test data, although a more significant focus is given on the latter.

In figure 1.1, the lower left quadrant represents the worst case of all - bad results for both exams. Although unfortunate, this scenario is somewhat expected - insufficient preparation during the practice period will likely lead to poor performance in the final exam. We need to fully utilize the practice exam, whose solutions are available, and train our problem-solving skills to form a good mental model that will be applied to the test exam. If the model is not good enough in the first place, we could barely expect to do well in the final exam.

Note that it is still possible to get high marks in the final exam when we are not well prepared based on the practice exam, purely by chance. However, this scenario, as represented by the upper left quadrant, is improbable. We may happen to get lucky in our random guessing, which does not guarantee a stable final exam performance on average.

The upper right quadrant is the best of all - the mental model is well trained based on the practice exam and is suitably applied to the final exam. We manage to grasp the gist by continuously fine-tuning our mental model, which generalizes to future, unforeseen problems in the final exam. Life is good in this region.

Some students may care too much about their performance in the training exams. The extensive pressure from their parents, teachers, or peers may push them to over-learn from the training materials. As represented by the lower right quadrant, this excessive stress on the training performance has led them to eventually memorize the answers to all of the training questions, all the way to obtain full marks in the practice exam, thanks to perfect question-answer memorization.

However, we all know that the final test exam will most likely be different from the training exam one way or the other. By merely remembering the answers instead of learning the patterns, they suffer from _overfitting_, a phenomenon where the trained mental model fits the practice exam too well, sometimes perfectly, but fails to generalize to new questions in the final exam. On the contrary, underfitting means we have not tried hard enough, resulting in a too simple model, and do not perform well in practice or final exam. However, the overfitting problem occurs frequently in many aspects of life; the desire to do exceptionally well on things at hand blinds us from seeing the big picture. The trick is, do not memorize the answer; learn from it.

In the example above, the overfitting region in the lower right quadrant means that the model has been trained to perform well on the training set but does not generalize well to the test set. Such an overfitting problem can be lessened by proper _regularization_. Regularization offers a suite of techniques designed as slight modifications to the model development process. With proper regularization, the trained mental model can better learn the underlying patterns between the input data (question) and the output label (answer). Such an adequately regularized mental model will better generalize to the final exam and output more accurate answers to new questions.

Regularization contains a suite of strategies used to reduce the generalization error of machine learning models, in particular deep learning models. An adequate regularization could help control the complexity of a neural network, reduce the generalization gap and address the issue of overfitting. Given finite training data or limited optimization procedure, proper regularization could help generalize a trained model to a future unseen test dataset, usually sampled from the same hidden distribution as the training set. It could reduce overfitting while at the same time keeping the training error as low as possible. It typically works by tweaking either data, model architecture, cost function, or optimization procedure to enhance the overall learning algorithm and control the degree of overfitting.

In this book, our primary focus is to bring us from the lower right quadrant (the overfitting region) to the upper right quadrant (the goal region). We assume a good amount of effort has been made to do well in the training exam, which is relatively easy to achieve given our currently available learning resources. The challenge that occurs more frequently in practice is overfitting. How can we detect it? When do we need to worry about it? What techniques are available to remediate this problem? Furthermore, why do some techniques work better than others? These are some of the questions the book aims to address.

### 1.1 Why do we need regularization?

The model complexity can be represented by the number of model parameters used and the model architecture that regulates the interaction between the input features and the model parameters, jointly producing the output label. Deep neural networks are usually complex models due to their large number of parameters and sophisticated architecture, thus prone to being over-parameterized and overfitting. An overly complex model may learn and understand the underlying patterns of the training data very well, but at the same time
