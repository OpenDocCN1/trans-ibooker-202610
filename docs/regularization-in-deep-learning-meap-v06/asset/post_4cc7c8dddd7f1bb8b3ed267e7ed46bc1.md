
generalizing to the test set (recall our discussion on preferring a wide basin over a sharp valley in chapter 4). All local minima are located differently, and these minima are accompanied by an abrupt change of loss (i.e., large gradients) in the neighborhood.

Figure 8.4 Visualizing the loss curves using full-batch GD and mini-batch SGD, where the latter curves are disparate in shape and distant from the former.

A disparate group of sharp mini-batch loss curves makes it difficult for SGD to converge. For example, the loss curve based on one mini-batch may send the optimizer in a totally different direction than another loss curve that sits on a different mini-batch. SGD has a hard time converging in the presence of sharp loss curves with high variance since the large gradients will likely cause the optimizer to diverge and jump out of the region that contains w*.

Therefore, we would expect SGD to prefer the first scenario with a homogeneous group of loss curves and little variance, as opposed to the second scenario with a disparate group of loss curves and high variance. SGD convergence would be relatively easy to achieve in the first scenario. The point is, if the SGD update is very sensitive to which mini-batch from the training set is currently used, then it will probably be very sensitive to the future test set and thus struggles to generalize well. When there are large differences among the mini-batches in the training set, as seen in figure 8.4, the difference between the training set and the test set will likely be high, thus ending up with poor generalization performance. SGD therefore prefers the first scenario and uses its stochasticity to induce a better generalization performance.

## 8.1.4 Analyzing the mean iterate

A recent paper that captures this intuition helps shed some light on the role of SGD in preferring the area of the loss curve with less variance between the mini-batches. As introduced in the paper titled "On the Origin of Implicit Regularization in Stochastic Gradient Descent" by Samuel Smith et al., the authors used a modified loss to approximate the mean iterates of the path of gradient flow using SGD. The adjusted loss, which includes an explicit term on the variance of losses between full-batch GD and mini-batch SGD, can explain the SGD's preference over low-variance regions.

Let us look at this perspective in more detail. As shown in figure 8.5 (recreated from the original paper), we plot a sample path for the weight updates for a two-dimensional loss function using both full-batch GD and mini-batch SGD. The left panel shows the deterministic gradient descents using the full-batch GD algorithm and converging to a stationary point. The middle panel shows the collection of random gradients at each step, where each circle represents all possible weight values until the current step due to different mini-batches used in SGD. The random mini-batch used in the current iterate will determine the exact location. As the random noises accumulate due to the stochastic nature of SGD, the circles (distributions of possible places) will become bigger and bigger due to a larger space of possible results in future steps. The right panel represents the mean iterate of these circles using SGD.

Figure 8.5 Comparing the paths of weight updates for different gradient descent algorithms. The left plot shows the path using the full-batch gradient descent algorithm, the middle plot shows the distribution of paths using different mini-batches in each update using SGD, and the right plot shows the mean iterate using SGD.

The distributions of possible paths make it difficult to analyze the behavior of SGD, and one of the main highlights of this paper is to focus on the mean iterate at each timestep to characterize its expected behavior. Plotting out the mean iterate, as shown in the right panel of figure 8.5, makes the path non-stochastic and deterministic, thus ignoring the randomness along each timestep and making it easier to analyze the mean value only.

Mathematically, recall that the SGD update algorithm takes the following form:

```
w_{t+1} = w_t - \eta \nabla_w Q(w_t, D_t)
```

By treating w_t as a random variable, we can analyze its mean/expected value by taking the expectation with respect to all possible random mini-batches of the training set, giving:

```
\mu_t = \mathbb{E}_{D_t}[w_t]
```

Here, we integrate out the randomness in the subset \(D_t\) to present the expected weight value at each timestep. This operation thus gives the mean iterate at timestep \(t\), and accumulating the mean iterates across different timesteps results in the right panel plot in figure 8.5.

Next, we treat the mean iterates as following a continuous gradient flow \(\mathcal{Q}\) i.e., \(\mu_t = \mathcal{Q}(t)\), where the timestep \(t\) is now continuous instead of hopping around in the parameter space in a discrete fashion. The continuous approximation function takes the time variable, which is also continuous, as input and outputs the corresponding mean iterate at the specific timestep. The SGD update algorithm for the mean iterate thus becomes:

```
\(\mathcal{Q}_1 = \mathcal{Q} - \eta \nabla_{\mathbf{w}} \hat{Q}_{SGD}(\mathcal{Q}, D)\)
```

Where \(\hat{Q}_{SGD}(\mathcal{Q}, D)\) is a modified loss function (different from the empirical loss function \(Q(\mathbf{w}_t, D_t)\)) and \(\nabla_{\mathbf{w}} \hat{Q}_{SGD} (\mathcal{Q}, D)\) denotes the gradient of the new loss function with respect to the weights, taking as input the current mean iterate \(\mu_t\) and the training set \(D\). The approximate updates of the mean iterate are now based on the modified loss function \(\hat{Q}_{SGD}(\mathcal{Q}, D)\) in a full-batch manner, and information about the mini-batch subset \(D_t\) is now encoded in \(\mu_t\)

The modified loss function, as shown below, can lead us one step closer to the low-variance property of SGD. The modified loss function \(\hat{Q}_{SGD}(\mathbf{w}, D)\) now consists of two parts: the full-batch loss function \(Q(\mathbf{w}, D)\) that does not have any gradient noise, and the variance \(\mathbb{E} \| \nabla_{\mathbf{w}} Q(\mathbf{w}, D_t) - \nabla_{\mathbf{w}} Q(\mathbf{w}, D) \|^2\) in gradients between mini-batch SGD and full-batch GD, multiplied by a constant \((\eta/4)\). The second term is essentially the (\(L2\)) norm of the difference between the \(\nabla_{\mathbf{w}} Q(\mathbf{w}, D_t)\), the gradient of SGD, and its mean value \(\nabla_{\mathbf{w}} Q(\mathbf{w}, D)\), based on the unbiasedness property. This term makes the implicit regularization effect of SGD explicit: running the mini-batch SGD algorithm is the same as running the full-batch GD with an additional regularization term on the variance of its gradients.

```
\(\hat{Q}_{SGD}(\mathbf{w}, D) = Q(\mathbf{w}, D) + \frac{\eta}{4} \mathbb{E} \| \nabla_{\mathbf{w}} Q(\mathbf{w}, D_t) - \nabla_{\mathbf{w}} Q(\mathbf{w}, D) \|^2\)
```

This regularization term is also data dependent. Depending on the specific location in the parameter space and the dataset used, the SGD algorithm will avoid regions where the variance of the gradients will be high when minimizing the modified loss function. A region with high variance corresponds to the previous figure 8.4, where the SGD algorithm has trouble converging to a stationary solution. When the variance is small, as shown in figure 8.3, SGD will converge much easier. Figure 8.6 summarizes the intuition on the implicit regularization effect in SGD, which prefers regions with a relatively wide minimum (as opposed to a narrow minimum) within the parameter space that are insensitive to the variance of the gradients. Therefore, SGD tends to end up with more generalizable model weights.

Figure 8.6 Illustrating the implicit regularization effect of SGD via a modified loss function.

However, such a type of analysis based on the mean iterate has its limitations. For example, when the loss function is bi-modal, as shown in figure 8.7, the trajectories may not necessarily display a bigger variance in the later updates. Instead, the updates would gradually converge to one of these two minima.

Figure 8.7 Trajectories of SGD updates with a bi-modal loss function. The variance decreases as the SGD algorithm converges to one of the local minima.

The mean iterate analysis is thus a good indicative explanation of the implicit regularization effect but does not apply to all the cases.
