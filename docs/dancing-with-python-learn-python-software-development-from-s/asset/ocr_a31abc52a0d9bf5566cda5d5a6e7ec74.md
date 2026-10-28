This is only a sketch of what machine learning software and practitioners do with simple neural networks, but I hope it is enough to give you an idea.

# 15.8.3 Deep learning

When we have more than one hidden layer, we are performing *deep learning*. People vary in their views of the minimum layers you need for deep learning, but, for simplicity, I say it is two. Figure 15.19 shows a network with three hidden layers.

![](bbox=[0.184, 0.282, 0.799, 0.604])

Figure 15.19: Deep neural network with 4 inputs, 3 hidden layers, and 3 outputs

Note that it is not always the case that every hidden layer has the same number of nodes as every other. We may not have every node in one layer connected to every node in the next. When we do, the network is *fully connected*.

What happens within each level? A fully connected layer may multiply its inputs by weights and then add a bias value. Another layer may have the sole function to apply one of the activation functions we saw previously.

A *normalization layer* applies a normalization or standardization transformation we discussed in *section 15.3* or another transformation appropriate to the data type. This activity might be part of other layers, such as the input layer.