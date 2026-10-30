
## Gathering and preparing the data

Let's start gathering data by buying multiple apples and lemons. We will start with the creation of tables with two features – **color** and **sugar content**.

> **Important Note**
> Color is described in terms of wavelength interval in **nanometers (nm)**.

This is an example involving a couple of fruits, but we need far more samples in order to obtain accurate predictions:

| Color [nm] | Sugar content [g] | Fruit |
|---|---|---|
| 590 | 10 | Apple |
| 570 | 2 | Lemon |
| 610 | 15 | Apple |
| 500 | 3 | Lemon |

At this stage, we should try to visualize the data to make sure we haven't collected too many fruits of one kind; otherwise, the model will be biased toward that fruit.

Now, we need to split the data into **training** and **evaluation data**. The data that's used for training should not be used for evaluation if our model is correct as we will always get good results. A good rule of thumb is that we should use 80% of the data for training and the remaining 20% as evaluation data. Once we have this division, additional steps may be required to prepare the data, such as normalization and deduplication. In our case, these are not required.

## Choosing a model

Now, we need to establish which model we want to use. There are thousands of models that have been developed that we can choose from. They can be suitable for images, text, or numerical-based data. The model should be chosen according to the use case. Since we have only two features in our case, the model can be a simple linear model.

## Training

Now, we'll get to the heart of ML: the actual training. Let's look at the model we have chosen for training. Here, we have X = W x Y + b, where W is weight and b is bias, while X and Y are the features. During training, we will manipulate the W and b values and use the training data to verify whether we are getting correct predictions. Each of these manipulations is referred to as a training step:

![](img/de2c97ab017f36b3750a5bade111edd7_441_0.png)

Figure 14.3 – ML training

While at the outset we may obtain poor results, gradually, we will get accurate predictions for the W and b values. If we are not getting the results we anticipated, we may need to choose a more complex model:

![](img/de2c97ab017f36b3750a5bade111edd7_441_1.png)

Figure 14.4 – Adjusting weight and bias

In the preceding plots, we can see how the line changes when we manipulate W and b. In the beginning, we may get results that do not appear to be fit for purpose, but eventually, we achieve values with satisfying results.

## Evaluation

Once the training is complete, it is time to evaluate the model. This is where our validation data will be used. You will test this data against your trained model and see how correct it is:

![](img/de2c97ab017f36b3750a5bade111edd7_442_0.png)

Figure 14.5 – ML evaluation

This is supposed to simulate how the model would work with real-world data.

## Hyperparameter tuning

When we were training our models, we made some assumptions regarding parameters. Those parameters are so-called hyperparameters. By tuning them, we can get higher accuracy rates. For example, we can use **learning rate parameters**, which indicate the extent of the changes made to W and b. If these parameters are too small, it can take a long time for our model to converge. If these parameters are too big, our model may never converge, thereby missing the ideal state. The other parameter we might want to tune is how many times we run through the training process. By changing it, we could potentially obtain higher accuracy.

Keep in mind that this process is rather experimental and requires some experience to get it set right. Have a look at the *Further reading* section for more information on hyperparameter tuning.

## Prediction

Finally, it is time to use our model to evaluate whether a given fruit is an apple or a lemon based on sugar content and color. There are a couple of ways in which we can do this. As you will see in the *Vertex AI* section of this chapter, we can host our model on GCP and access it via an API whenever we need to make a prediction.

Of course, this is a very simple example, but it should be good enough for you to grasp the idea of how ML works.

## Learning models

Now that we have a basic understanding of ML and how to train a model, let's have a look at three types of ML learning. These are as follows:

- Supervised learning
- Unsupervised learning
- Semi-supervised ML

The preceding three types of ML are defined as follows:

- **Supervised learning**: Supervised learning is the most common model. It is used when the training data and validation data are labeled. What the model does is learn how to set a label for input data. It does this based on what it has learned from some labeled training data. We can further classify supervised learning into the following categories:
  - **Classification**: This occurs when the output data is a category, such as *apple*, *pear*, or *orange*.
  - **Regression**: This occurs when the output data is a value, such as *cost* or *temperature*.
- **Unsupervised learning**: Unsupervised learning is used when the training data is not labeled. The model attempts to learn the structure of the data and export information or features that might be useful for classification. Since the data isn't labeled, the accuracy cannot be measured. As an example, the model can be used on data that consists of attributes such as weight and height for people of different genders. The height and weight information can be extracted from the data to perform classification into groups. If you draw a plot from the data, you may observe that the data can be used to create patterns and groups.

Again, as the labels are not known, we cannot explicitly say which group represents which gender, but we can presume that one of the groups can be represented by males and the other by females. We can further classify unsupervised learning into the following categories:

- **Clustering**: This occurs when you want to group the data, for example, grouping consumers according to their preference for coffee or tea.
- **Association**: This occurs when you want to link two different actions or behaviors, for example, a customer buying product A also buying product B.
- **Semi-supervised ML:** Semi-supervised training occurs when part of the training data is labeled and part of it isn’t. You can use a mix of both of the preceding methods. Unsupervised learning can be used to structure the data. Supervised learning can be used to label unlabeled data.

## GCP ML options

With GCP, you have multiple options when it comes to leveraging ML. Which one you choose largely depends on your use case and how knowledgeable you are on the topic. The following options are available:

- **TensorFlow (for a data scientist):** This is an option for those who want to work with ML from scratch. It is a software library that’s developed and open-sourced by Google. There are more libraries on the market, but this one is the most popular and is used by other cloud providers for their managed ML services.
- **Vertex AI (for a data scientist):** This is an option for those who want to train their own models but who use Google for training and predictions. It is a managed TensorFlow/Kubeflow service that offloads all infrastructure and software bits from users.
- **Pretrained ML models (for a developer):** This is an option for those who want to leverage ML without having any knowledge of it. It allows Google-developed models to be used to perform predictions.
- **AutoML (for a developer):** This is an option for those who want to leverage ML without having any knowledge of it and where the pretrained models are not fit for purpose. It allows models to be trained by supporting labeled data.

## TensorFlow

TensorFlow is the most popular open source ML library in the world for developing and training ML models. As a library, it is now part of the GCP offering and can be used on different platforms. For the development phase, you can use your laptop. For the run phase, you can still use your laptop, a public cloud, or even a mobile device.
