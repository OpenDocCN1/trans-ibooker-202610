
Figure 2.15 shows the output of the optimization schedule.

![](img/ee98e91e6845f41077b163bec833ee40_53_0.png)

Figure 2.15 Iteratively updating the weights using the gradient descent algorithm. As the optimization proceeds, the star representing the location of the parameters gradually approaches the middle point where the local minimum exists.

Note that a very small number is often introduced as an additional tolerance parameter to determine if the optimization process should be stopped. For example, when each update only results in a slight change in the parameter value, the optimization loop could be terminated if the absolute or relative change in the parameter value is smaller than the preset tolerance threshold, even before reaching the maximum number of iterations.

## 2.4.4 Different types of gradient descent

In each iteration, the gradient descent algorithm requires using the full dataset **_X_** and **_y_** to calculate the gradient. When the size of the dataset _n_ is small, running the gradient descent is fine. However, _n_ could be very large in some cases. For example, the financial transactional data could easily be millions or billions of records. Even if storing the gradient is possible, performing each iteration update for the full dataset would be extremely slow. A more efficient approach to generate the gradients is thus preferred.

The use of the full dataset occurs at the differentiation step. When calculating the gradient vector \(\nabla_{w1}Q = \sum_{i=1}^{n}(-2x_i)(v_i - w_0 - w_1x_i)\), all data points from _i_ = 2 to _i_ = _n_ are used. The level of summation is where further improvements over gradient descent are made. Instead of passing the entire dataset for parameter update all at once, what if we divide the full dataset into different slices or batches, and use a batched version of parameter update? In this way, the issue on the execution speed and storage could be alleviated due to a smaller size of the dataset used in each update.

Recall that the gradient of a univariate linear regression model with respect to the single feature is a summation over $n$ terms. Equivalently, this gradient could be decomposed into a series of individual and sequential updates. The joint outcome of these individual and sequential updates eventually leads to the same final update. Figure 2.16 shows the decomposition process.

![](img/ee98e91e6845f41077b163bec833ee40_54_0.png)

**Figure 2.16** Rewriting the calculation of gradient descent update as a series of individual point-wise updates. Note that the weights in the square bracket remains the same across all individual updates.

It is important to note that the residual in the parenthesis of each individual update remains the same using the gradient descent algorithm. This means that all intermediate parameter updates are based on the same original parameter values.

The gradient descent algorithm is called the batch gradient descent algorithm, where the batch refers to the use of the entire dataset. Under the batch gradient descent, all residuals are calculated and aggregated before updating the parameters, thus making the parameter update more stable. Based on these intermediate updates, or equivalently the amount of data used for each parameter update, the gradient descent algorithm has two popular variants.

The first variation is the *stochastic gradient descent* (SGD) algorithm, which calculates the gradient based on one data point at a time instead of summing across all data points. It is thus easy to fit a single training example into memory and computationally fast to process one instance at a time. Due to the stochasticity in choosing the current data point and calculating the point-wise gradient, the resulting gradient is thus a stochastic estimate of the original gradient based on the full dataset. These frequent parameter updates would result in a noisy but more robust traveling path of the weights, thus resulting in faster convergence, especially in the case of a large dataset. Besides, the noisy updates are more likely to jump out of a potentially local minimum.

The other variant is called mini-batch gradient descent, which is in between the batch and the stochastic version in terms of the number of examples used in each parameter update. By splitting the dataset into small batches, the mini-batch gradient descent algorithm inherits the efficiency and stability in batch gradient descent, as well as the robustness and fast convergence of stochastic gradient descent. This is one of the most popular algorithms in training deep neural networks.

## 2.4.5 The stochastic gradient descent algorithm

Unlike the gradient descent algorithm, the SGD changes the residuals using the latest parameter values after each update. Such operation makes it easy for sequential updates in situations such as online learning, where access to the entire dataset is not required and the new data points arrive sequentially as a stream.

Recall the decomposed intermediate steps in figure 2.16, where w0 and w1 remain unchanged throughout all the steps in one round of gradient descent update. In SGD, however, they are changed in each step. With each example randomly selected in the parameter update, the stochastic sequence of updates thus follow:

```
$w_i \leftarrow w_1 + 2\eta[x_i(y_i - w_0 - w_1x_i)], i = 1..., n$
```

Where $i$ is a randomly selected index from a total of $n$ examples. The code listing below demonstrates the change in parameter update using one round of SGD.

```
Listing 2.9 Performing one round of SGD update
index = np.random.randint(X.shape[0]-1) #A
w0_grad = -2*(y[index] - w[1]*X[index] - w[0]) #B
w1_grad = -2*(X[index]*(y[index] - w[1]*X[index] - w[0])) #B
w[0] = w[0] - learning_rate*w0_grad #C
w[1] = w[1] - learning_rate*w1_grad #C
```

#A Choose a random example
#B Compute the gradients
#C Update the parameters

Under SGD, each complete pass across the whole dataset requires n rounds of update. One full pass is also called an epoch, and model training often requires multiple epochs to reach convergence. In the codes of full batch gradient descent, the epoch corresponds to the max_iters variable. For completeness, we show the training procedure using SGD, where most of the codes are similar to the full batch gradient descent.

Listing 2.10 Optimization using SGD

```
w = w_original #A
max_iters = 6000 #B
learning_rate = 0.001 #C
f, ax = plt.subplots() #D
cp = ax.contour(w0_grid, w1_grid, Q_grid, colors='black', linestyles='dashed',
                levels=[0, 0.5, 1, 2, 4, 8, 16, 32, 64, 128, 256, 512])
ax.plot(w[0], w[1], 'g*', markersize=5)
plt.clabel(cp, inline=True,
           fontsize=10)
for i in range(max_iters):
    index = np.random.randint(X.shape[0]-1)
    w0_grad = -2*(y[index]- w[1]*X[index] - w[0])
           w1_grad = -2*(X[index]*(y[index] - w[1]*X[index] - w[0]))
           w[0] = w[0] - learning_rate*w0_grad
           w[1] = w[1] - learning_rate*w1_grad
           if i % 100 == 0: #E
               ax.plot(w[0], w[1], 'g*', markersize=5)
plt.xlabel('w0')
plt.ylabel('w1')
plt.show()
```

#A Reset to original weights
#B maximum iterations for gradient descent update
#C learning rate or step size per update
#D plot the cost function as a contour plot
#E plot the current parameters at every 100th iteration

Figure 2.17 shows the travelling path of the weights using SGD, which looks wigglier compared with the path using the full batch gradient descent. Smoothing the volatile ups and downs and therefore accelerating the updates using optimizer variants such as momentum and adam, however, may not necessarily contribute to its generalization property. It turns out that such volatility is an important reason behind the good generalization performance of models trained using SGD. We will cover more on this point in a later section.

![](img/ee98e91e6845f41077b163bec833ee40_57_0.png)

Figure 2.17 Parameter travelling path using SGD. Due to the stochasticity in choosing the example for one step of SGD update, its travelling path is clearly noisier than the path using full batch gradient descent.

## 2.4.6 The impact of the learning rate

The learning rate used in the gradient descent family of algorithms is a crucial hyper-parameter. The hyper-parameter is usually determined manually or through some heuristics before training starts and stays fixed during model training. For each round of parameter updates using a batch of training data, the learning rate remains fixed, changing only the parameters. However, the learning rate between different rounds of parameter updates may differ and could be designed to change following a pre-allocated schedule.

The learning rate \(\eta\) determines the step size of parameter update. If \(\eta\) is too small, each update will only slightly nudge the parameters towards the optimum, leading to a slow convergence. However, when \(\eta\) is too large, the optimization process may diverge, leading to parameter updates that diverge away from the optimum. Figure 2.18 demonstrates these two scenarios.
