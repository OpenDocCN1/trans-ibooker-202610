```
>>> device = "cuda" if torch.cuda.is_available() else "cpu"
>>> print(f"Using {device} device")
>>> model = CNN().to(device)
>>> print(model)
Using cuda device
CNN(
(conv1): Sequential(
    (0): Conv2d(1, 16, kernel_size=(5, 5), stride=(1, 1), padding=(2, 2))
    (1): ReLU()
    (2): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
)
(conv2): Sequential(
    (0): Conv2d(16, 32, kernel_size=(5, 5), stride=(1, 1), padding=(2, 2))
    (1): ReLU()
    (2): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
)
(out): Linear(in_features=1568, out_features=10, bias=True)
)
```

When the neural network architecture starts to scale up and become complex, it is often helpful to print out the architecture for better clarification of its composition. In the code snippet below, we resort to the `torchsummary` package to ease the visualization task by passing in the size of an input entry. The output shows the model architecture from top to bottom, with each layer sequentially suffixed by an integer. The output shape and number of parameters in each layer are also provided. There are a total of (trainable) 28,938 parameters used in the model. Note that we do not have any non-trainable parameters; this relates to the level of model fine-tuning in transfer learning.

```
>>> from torchsummary import summary
>>> summary(model, input_size=(1, 28, 28))
-----------------------------------------------------------------
        Layer (type)               Output Shape         Param #
=================================================================
        Conv2d-1           [-1, 16, 28, 28]             416
        ReLU-2           [-1, 16, 28, 28]               0
  MaxPool2d-3           [-1, 16, 14, 14]               0
        Conv2d-4           [-1, 32, 14, 14]          12,832
        ReLU-5           [-1, 32, 14, 14]               0
  MaxPool2d-6           [-1, 32, 7, 7]                0
    Linear-7                 [-1, 10]             15,690
=================================================================
Total params: 28,938
Trainable params: 28,938
Non-trainable params: 0
-----------------------------------------------------------------
Input size (MB): 0.00
Forward/backward pass size (MB): 0.32
Params size (MB): 0.11
Estimated Total Size (MB): 0.44
-----------------------------------------------------------------
```

Note that the final output \(\hat{\mathbf{y}} = [\hat{y}_1, \hat{y}_2, ..., \hat{y}_{10}]\) for each input image is an array of ten numeric values that correspond to the predicted output for each class. The class with the largest value becomes the final prediction for that image entry. Since this is a classification task, these numbers are further transformed into an array of probabilities. Such transformation is