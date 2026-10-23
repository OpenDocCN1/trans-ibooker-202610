you, your partner, or your daughter are knocking at the door, a camera will take a picture of the face, and if it recognizes the person as one of these three, it’s going to open the door. If it doesn’t recognize anyone, it’s going to stay locked. How would we build something like this? Let’s start by breaking down the problem.

Our input data is an image of a face. The task is to understand whom it belongs to: you, your partner, your daughter, or somebody else. Just like the kitchen/bathroom example at the beginning of the chapter, it’s a supervised classification task. As we’ve seen, deep learning is the best tool to solve most image classification problems. We might even take advantage of transfer learning by retraining the last layers of a network that was trained on ImageNet.

As always, the next step is to find the training data: you open your family album and take a few hundred pictures of your family. It’s important that you get as many as you can, so the algorithm will have examples of your faces in different light conditions, different haircuts, and so on. To complete the dataset, you download a few hundred pictures of random people from the internet and label those as “other.” This is so the model will learn to spot the most distinctive features in your family’s faces, and ignore any attributes that are shared by all human faces.

![](bbox=[0.15, 0.42, 0.62, 0.62])

Figure 4.12 Face recognition is a specific application of image classification in which the input images are human faces.

This approach sounds like it should work, and it does. Here’s the challenge, though: your daughter is in high school, and her friend Catherine often visits your home to do homework with your daughter. It would be nice to add Catherine to the list of trusted people so the door will unlock for her too. You now have five classes instead of four:

+   - You
+   - Partner
+   - Daughter
+   - Catherine the study buddy
+   - Other

You’d have to ask Catherine for a few hundred pictures of her face and train a whole new model. Neither you, Catherine, or her parents would be happy to do that just to speed up the door-opening process.