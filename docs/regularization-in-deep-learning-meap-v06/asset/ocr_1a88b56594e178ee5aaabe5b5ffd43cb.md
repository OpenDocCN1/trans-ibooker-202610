Figure 8.6 Illustrating the implicit regularization effect of SGD via a modified loss function.

However, such a type of analysis based on the mean iterate has its limitations. For example, when the loss function is bi-modal, as shown in figure 8.7, the trajectories may not necessarily display a bigger variance in the later updates. Instead, the updates would gradually converge to one of these two minima.

Figure 8.7 Trajectories of SGD updates with a bi-modal loss function. The variance decreases as the SGD algorithm converges to one of the local minima.

The mean iterate analysis is thus a good indicative explanation of the implicit regularization effect but does not apply to all the cases.