\frac{\partial f_L}{\partial w} = \left( \left( \cdots \left( \left( \frac{\partial f_L}{\partial f_{L-1}} \frac{\partial f_{L-2}}{\partial f_{L-2}} \right) \frac{\partial f_{L-2}}{\partial f_{L-3}} \right) \cdots \frac{\partial f_3}{\partial f_2} \right) \frac{\partial f_2}{\partial f_1} \right) \frac{\partial f_1}{\partial w}.

Following a similar line of reasoning, the total number of operations required by the backward-mode multiplication would be:

p_L p_{L-1} p_{L-2} + p_L p_{L-2} p_{L-3} + \dots + p_L p_1 p_0 = p_L \sum_{l=1}^{L-1} p_l p_{l-1}.

Figure 4.15 illustrates the backward-mode multiplication.

![](bbox=[0.198, 0.306, 0.883, 0.604])

Figure 4.15 Illustrating the backward-mode multiplication process and its computational cost. The multiplication starts at the innermost level at the end of the neural network and gradually expands to outer layers towards the beginning.

Recall that our goal is to calculate the gradient vector that contains the partial derivatives of the model's cost with respect to each weight. Due to the large size of modern neural networks and the many rounds of optimization involved, we would hope to calculate the gradient efficiently. Therefore, we need to analyze and compare the computational cost of forward mode and backward mode multiplication and go with the faster option.

Apparently, the computational cost of these two modes varies depending on the size of layers in the composite function. Also, the formula for the computational cost derived earlier suggest that the forward mode relies on the constant multiplier p_0 and the backward mode relies on p_L. In a typical model training process, a scalar cost is often selected as the performance metric based on the current model's prediction. The single-valued cost indicates that p_L = 1. On the other hand, p_0 directly depends on the dimension of the input data, which can grow to hundreds of thousands of features per entry in the case of image data.