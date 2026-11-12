An instance of a deep neural network was created by loading the model weights from a pretrained model (on ImageNet). For example, with `models.vgg19(pretrained=True)`, the weights of the model are downloaded for the first time and then cached (a PyTorch model commonly has extension the `.pt/.pth`).

The model was set to its eval mode using the `resnet101.eval()` method (for the model ResNet101, for example).

To run inference (that is, to run a forward pass through the pretrained model with the input image), the `resnet101()` method was called (for the ResNet101 model, for example) it outputted a 1,000-element (class) vector, where each element value represents the probability that the input image belongs to a particular class (according to the pretrained model).

Finally, we needed to find out the index of the class corresponding to the maximum score (using the `np.argmax()` function) from the output vector. This class index is used to extract the name of the predicted class from the list of classes. We can get the probability corresponding to the predicted class from the output probability vector (for example, the ResNet101 predicted the cheetah image correctly with 99.57% confidence).

The performance of the pretrained models can be compared using the following evaluation criteria: top-1/top-5 error, inference time on CPU/GPU, model size. A good model needs to have small values of top-1/top-5 error, inference time, and model size.

# There's more...

Use the pretrained models from Keras to perform the image classification task. Compare the top-1 versus top-5 accuracy for different models. Also, use a pretrained model from Caffe to classify an image. Finally, train your own image dataset and learn the weights of a model (for example, VGG19) and then use it to predict unseen images.

# See also

For more information, read through the following sources:

+   - http://cs231n.github.io/classification/
+   - https://pytorch.org/docs/stable/torchvision/models.html
+   - https://arxiv.org/pdf/1512.00567.pdf
+   - https://arxiv.org/pdf/1801.04381.pdf
+   - https://arxiv.org/pdf/1512.03385.pdf