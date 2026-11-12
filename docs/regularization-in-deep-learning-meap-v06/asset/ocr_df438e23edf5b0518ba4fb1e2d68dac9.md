![](bbox=[0.203, 0.139, 0.881, 0.548])

Figure 6.16 Illustrating the soft parameter sharing scheme for three tasks. The architecture consists of three dedicated sequences of network for each task respectively. The same input data is fed into the network three times as in the case of single task learning. However, the network applies a soft constraint (i.e., lasso or ridge penalty as indicated by the double arrows in between sequences) on the parameters of certain hidden layers so as to encourage a similar representation when jointly learning all three tasks.

The hard parameter sharing scheme is intuitive to implement as we only need to modify the model's architecture as a tree structure, consisting of a trunk and a few branches. The soft parameter sharing, however, requires modifying the loss function to overlay the soft constraints. Let us look at how this could be achieved.

# 6.3.2 Modifying the loss function to achieve soft parameter sharing

Assume that we have three learnings tasks: A, B, and C. Denote the $i^{th}$ layer's weights of task $j$ by $W_i^{(j)}$, where $j \in \{A,B,C\}$. One way to impose the soft constraint on the similarities among corresponding weights is to augment the cost function with an additional term: the sum of squared Frobenius norm (also called the Euclidean norm) across all shared layers. As shown in figure 6.17, assuming a regression task, we would use the mean squared error as the base loss and add the layer-wise sum of squared Frobenius norm across all shared