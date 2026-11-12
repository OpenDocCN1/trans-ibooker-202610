The following diagram shows the structure of the network and how the preceding denoising autoencoder works with the 1fw faces dataset:

![](bbox=[0.3, 0.165, 0.69, 0.585])

# How it works...

We used `nn.Sequential()` from PyTorch to build the autoencoder by sequentially specifying the building blocks (`nn.Module`). Each pixel from a noisy image forms an input node, hence the input has 50*37 nodes. The output also has the same number of nodes.

A fully connected layer of size 1,850 x 512 was instantiated with `nn.Linear(50*37, 512)`. `nn.ReLU(True)` was used to apply in-place ReLU non-linearity in between the input/hidden layers and `nn.Sigmoid()` before the final layer to apply the sigmoid function at the output layer. Binary cross-entropy loss was used with `nn.BCELoss()`. The `backward()` function was used to backpropagate and recompute the weights in the neural net.