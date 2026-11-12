original gradient based on the full dataset. These frequent parameter updates would result in a noisy but more robust traveling path of the weights, thus resulting in faster convergence, especially in the case of a large dataset. Besides, the noisy updates are more likely to jump out of a potentially local minimum.

The other variant is called mini-batch gradient descent, which is in between the batch and the stochastic version in terms of the number of examples used in each parameter update. By splitting the dataset into small batches, the mini-batch gradient descent algorithm inherits the efficiency and stability in batch gradient descent, as well as the robustness and fast convergence of stochastic gradient descent. This is one of the most popular algorithms in training deep neural networks.

# 2.4.5 The stochastic gradient descent algorithm

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