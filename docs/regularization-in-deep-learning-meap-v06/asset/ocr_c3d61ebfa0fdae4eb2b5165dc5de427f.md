i.e., tweaking the number of nodes in the hidden layer. In the model initialization codes in the `__init__` function, we use `nn.Linear()` to define two fully connected layers, one as the hidden layer and the other as the output layer. Note how we specify the input size as 1*28*28 for the hidden layer and output size as a default value of 10 for the output layer; these are based on the property of MNIST data. We also place the hidden layer size `num_nodes` as an explicit input parameter in the initialization function to create models of different sizes later. The `forward()` function does a similar job as in our previous example. The only thing to note is that we directly flatten the input data at the beginning to cater to the fully connected layer since the convolutional layer is not used here.

```
Listing 4.11 Define the single layer neural network model class

import torch.nn as nn
import torch.nn.functional as F
class SimpleNN(nn.Module):
    def __init__(self, num_nodes, num_classes=10): #A
        super(SimpleNN, self).__init__()
        self.fc = nn.Linear(1 * 28 * 28, num_nodes) #B
        self.out = nn.Linear(num_nodes, num_classes) #C

    def forward(self, x): #D
        x = x.view(x.shape[0], -1) #F
        x = F.relu(self.fc(x))
        output = self.out(x)
        return output
```

+   * #A Model initialization codes
+   * #B The single fully connected layer
+   * #C The final fully connected layer which outputs 10 classes
+   * #D Model architecture codes
+   * #F Flatten all dimensions except for the batch size

Next, we need a function to tell us how well the model performs on the training and test sets. These would indicate the model’s generalization performance when we vary the hidden layer’s size. In the code listing below defines a function called `check_accuracy` that calculates the prediction accuracy for the entire training or test set depending on the `type` parameter. The codes largely follow the same structure as the `test()` function in the previous example, except that we generalize the input image loader to be based on a user-specified parameter; this saves us from writing two similar functions for both training and test sets.