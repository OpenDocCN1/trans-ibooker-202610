```
#A double the depth of feature maps for every conv layer
#B input image is 32*32, after 3 MaxPooling layers it reduces to 4*4 64 feature maps
#C 10 output nodes that map to 10 classes
#D apply relu to each conv layer output
#E max pooling layer with 2x2 kernel to shrink the size by half
#F flatten the images as 1D input to the fully connected layer
```

We can then instantiate a model instance and observe its architecture as follows, besides sending it over to GPU instance if available.

```
>>> device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
>>> model = LeNet().to(device)
LeNet(
(conv1): Conv2d(3, 16, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
(conv2): Conv2d(16, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
(conv3): Conv2d(32, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
(fc1): Linear(in_features=1024, out_features=500, bias=True)
(dropout1): Dropout(p=0.5, inplace=False)
(fc2): Linear(in_features=500, out_features=10, bias=True)
)
```

This architecture corresponds to a decent sized neural network with around half a million weights in total:

```
>>> print("Number of parameters:", sum(p.numel() for p in model.parameters() if p.requires_grad))
Number of parameters: 541094
```

Next, we follow a similar optimization procedure in the previous chapter such as using the SGD optimization algorithm with a constant 0.01 learning rate. In each epoch, we iteratively train the model using the data loader for the training set in a mini-batch mode. After that, we evaluate the model performance on the whole validation set in a gradient-free context since backpropagation is not required. See the code listing below for the full training procedure without using data augmentation.