A real-world example would be a Twitter analytics tool that is used to monitor the overall sentiment and/or emotion of a given hashtag or account. To ensure the model is performing as expected, samples of predictions can be run through XAI to highlight the words that are contributing the most to a given score. Let’s now assume that someone in QA discovers a word that is highly negative in sentiment but is not being highlighted. This word can be flagged as needing to be added to the corpus along with the negative sentiment. Because of the fast-paced nature of the Twitterverse and the constant evolution of the language, with new acronyms and slang terms being introduced all the time, this kind of adaptation is often required.

# Image/Video Models (Heatmaps)

Image models typically fall into one of two major categories: image classification, where one or more labels might be the output of the model, and object detection, where a box is usually drawn around an object in the image along with some labels or tags. In either case, the underlying algorithms are usually of the same type—a form of deep learning algorithm called a neural network—and can be explained in similar ways.

Convolutional neural networks (CNNs) are a common type of neural network used in image models. Without going into the nitty-gritty details, these models break down the image into smaller chunks and then decide based on those chunks which pixels are important or not important.

Similar to how in text models the words that contribute the most to the model’s prediction are highlighted, in image models, pixels are highlighted to show which areas in the image the model decided to focus on versus ignore. The actual layers of the model might not be directly interpretable, however, as these models are usually complicated neural networks.

Figure 2-1 illustrates what this overlay might look like for a model that is identifying a cat.