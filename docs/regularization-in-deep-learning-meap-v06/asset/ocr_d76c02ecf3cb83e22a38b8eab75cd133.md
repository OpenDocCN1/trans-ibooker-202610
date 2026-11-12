First, we start with the weights in the initial hidden layer and perform forward propagation via a series of perceptron operations

Each hidden layer is a vector of perceptron functions that consists of one or more nodes

The final stop of a series of embedded prediction functions in a multilayer neural network, ending with $\mathbf{f}_L(\mathbf{f}_{L-1}(\dots \mathbf{f}_1(\mathbf{w})))$

![](bbox=[0.25, 0.225, 0.8, 0.355])

Last, we multiply these intermediate derivatives to get the final gradient, giving $\frac{\partial \mathbf{f}_L}{\partial \mathbf{w}} = \frac{\partial \mathbf{f}_L}{\partial \mathbf{f}_{L-1}} \frac{\partial \mathbf{f}_{L-1}}{\partial \mathbf{f}_{L-2}} \cdots \frac{\partial \mathbf{f}_2}{\partial \mathbf{f}_1} \frac{\partial \mathbf{f}_1}{\partial \mathbf{w}}$

Then, we perform backward propagation to calculate the intermediate derivatives

Figure 4.13 Calculating the gradient of a multilayer neural network with respect to the weights in the initial hidden layer. The process starts with the forward propagation to generate a series of predictions until reaching the last layer. Then the intermediate derivatives between two adjacent layers are calculated. The final gradient is then derived by multiplying these intermediate derivatives.

# 4.2.3 Different modes of multiplication

So far, we have been looking at how to calculate the gradient of a multilayer neural network with respect to the weights in the first hidden layer. It is also straightforward to generalize to the weights for other layers. The gradient of the final cost with respect to the weights at the first layer can be derived by multiplying these intermediate derivatives of the middle layers. However, each pair of layers results in a matrix of partial derivatives whose element corresponds to each unique pair of nodes in both layers, and multiplying multiple matrices, if not properly planned, could take a long time to compute.

Let us focus on the multiplication of the first two matrices: $\frac{\partial \mathbf{f}_1}{\partial \mathbf{w}}$ and $\frac{\partial \mathbf{f}_2}{\partial \mathbf{f}_1}$. Assume $\mathbf{w}$ is a $p_0$ -dimensional vector and $\mathbf{f}_1$ is a $p_1$ -dimensional vector. Taking the partial derivative of each function in $\mathbf{f}_1$ against each weight in $\mathbf{w}$ gives us a matrix $\frac{\partial \mathbf{f}_1}{\partial \mathbf{w}}$ of dimension $p_1 \times p_0$. Similarly, $\frac{\partial \mathbf{f}_2}{\partial \mathbf{f}_1}$ will be a $p_2 \times p_1$ matrix. Based on our previous introduction on matrix multiplication, when two matrices multiply with each other, the inner dimension should match each other and the outer dimension will form the resulting shape of the output. In this case, $\frac{\partial \mathbf{f}_2}{\partial \mathbf{f}_1} \frac{\partial \mathbf{f}_1}{\partial \mathbf{w}}$ will be a $p_2 \times p_0$ matrix. In addition, from a computational perspective, multiplying $\frac{\partial \mathbf{f}_1}{\partial \mathbf{w}}$ and $\frac{\partial \mathbf{f}_2}{\partial \mathbf{f}_1}$ will need a total of $p_2 p_1 p_0$ operations. The number of operations indicates how long the multiplication will take.