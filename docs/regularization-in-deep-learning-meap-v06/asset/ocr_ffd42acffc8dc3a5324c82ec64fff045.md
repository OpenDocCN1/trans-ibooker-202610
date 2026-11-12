# 5.1.2 Label smoothing

Label smoothing is a regularization technique that alleviates overfitting by introducing a uniformly distributed noise in the one-hot encoded output label, assuming a classification task. The reason why we may want to perturb the target label is that the labels may be incorrectly tagged in the training dataset. By creating a “soft” target and reducing reliance on the original “hard” target output, we could prepare the model to get out of the potential mislabeling and prevent it from being overconfident about its predictions.

Mathematically, denote C as the total number of classes in the target output, \(y_{one\_hot}\) as the one-hot encoded output, and \(\alpha\) as a controlling hyperparameter that determines the intensity of the uniform noise. The transformed output \(y_{label\_smoothing}\) after label smoothing is expressed as the following:

\(y_{label\_smoothing} = (1 - \alpha)y_{one\_hot} + \alpha/C\)

When \(\alpha = 0\), we have the original one-hot encoding where the probability of the correct class label is one; we exploit this knowledge in calculating the training cost. When \(\alpha = 1\), the smoothed encoding vector contains a uniform distribution with equal probability for all classes, encouraging exploration across all classes. Thus, this is a version of balancing between exploitation and exploration, controlled by the hyperparameter \(\alpha\).

For example, assume we have a three-class classification problem where the label for one sample is the first class, i.e., \(y_{one\_hot} = [1,0,0]\). Upon setting \(\alpha = 0.3\), the smoothed encoding vector now becomes \((1 - 0.3)*[1, 0, 0] + \frac{0.3}{3} = [0.7, 0, 0] + 0.1 = [0.8, 0.1, 0.1]\), where we used the broadcasting mechanism in the second last step. We can see that the second and third classes are now also assigned a small probability, forming a soft target in the model.

In the next section, we will go through a concrete example on using data augmentation in training a deep neural network model.

# 5.2 Training deep neural networks using data augmentation

Let us start by visualizing the augmented images based on the city-view picture from chapter 1. We will use this image as an example and demonstrate how to apply various data augmentation techniques to generate new ones.

First, we need to download the image from the web. In the code listing below, we use the `requests` library to initiate an HTTP request via the `get()` method from a GitHub link, store the returned result in `response` and access its contents to retrieve the image data and convert it into a NumPy array. Since this is a color image in a PNG format that has four channels that correspond to red, green, blue, and transparency respectively, we ignore the fourth channel by selecting the channel index up to the third.