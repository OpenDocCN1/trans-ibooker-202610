Sampling was used to resolve class imbalance and also to prevent overfitting.

The `WeightedRandomSampler()` was used to sample the images corresponding to all of the 43 classes with equal frequency (2,000 images per class, which increases the total number of input images too) and then they were passed to the PyTorch data loader.

The **contrast-limited adaptive histogram equalization** (CLAHE) algorithm was used to partition the images into contextual regions followed by an application of the histogram equalization to every region. The hidden features of the image appear to be more visible with CLAHE than with vanilla histogram equalization since the distribution of used gray values is evened out.

The input color images were converted to grayscale images by first transforming them from RGB to the YCbCr colorspace and then extracting the intensity channel Y from the YCbCr color space.

The abstract class `torch.utils.data.Dataset` represents a dataset that can be iterated over with `torch.utils.data.DataLoader`.

In order to prevent overfitting, dropout and batch normalization regularization techniques were used.

The `livelossplot` library was used to plot the live training loss.

The `torch.no_grad()` function stops the calculation of the gradients in the validation phase (since we just want to evaluate the model that we learned so far on the held-out validation dataset and don't want to update the weights in this phase).

# # There's more...

Compute the accuracy of the model trained on the test dataset and plot the confusion matrix. What is the test accuracy achieved? How can you improve it?

# # See also

For more information, read through the following sources:

+   - https://pytorch.org/tutorials/beginner/blitz/cifar10_tutorial.html
+   - https://medium.com/@wolfapple/traffic-sign-recognition-2b0c3835e104
+   - https://github.com/wolfapple/traffic-sign-recognition/blob/master/notebook.ipynb