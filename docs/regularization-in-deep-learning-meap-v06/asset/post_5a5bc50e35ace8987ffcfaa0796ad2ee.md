
## Listing 4.12 Calculate prediction accuracy for the entire training or test set

```
def check_accuracy(model, loaders, verbose=True, type="train"):
    model.eval() #A
    total_img = len(loaders[type].dataset) #B
    correct = 0
    with torch.no_grad(): #C
        for X, y in loaders[type]:
            X, y = X.to(device), y.to(device)
            pred = model(X)
            correct += (pred.argmax(1) == y).type(torch.float).sum().item() #D
    correct /= total_img
    if verbose:
        print(f"{type} accuracy: {correct:>0.3f}")
    return correct
```

- #A Set to evaluation mode
- #B Extract the total number of images
- #C Disable gradient calculation as backpropagation is not needed
- #D Add the correct prediction for each batch

We are now ready to test out the impact of model complexity on the predictive accuracy for both the training set and the test set. In the code listing below, we start with two nodes in the hidden layer and gradually double the size until 4096. The reason for such an extensive range is to see how the model performs as it increases complexity even after passing the interpolating threshold. We also set a total training budget of 500 epochs in `num_epochs` to ensure that the training procedure reaches convergence, i.e., the model parameters do not change much. Upon exhausting the training budget, we record the final prediction accuracy for the training set in `final_train_acc` and the test set in `final_test_acc`. Since running the full codes takes quite some time, we store these two variables in text files via the `pickle` package so that we do not need to start from scratch if the running gets disrupted.

## Listing 4.13 The full model training and testing

```
Listing 4.13 The full model training and testing
import pickle
from torch import optim
device = "cuda" if torch.cuda.is_available() else "cpu"
num_epochs = 500
final_train_acc = []
final_test_acc = []
num_nodes = [2, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 2048, 4096]
for n_node in num_nodes:
    print(f"------NODE: {n_node}------")
    model = SimpleNN(num_nodes=n_node)
    cost_func = nn.CrossEntropyLoss()
    optimizer = optim.SGD(model.parameters(), lr = 0.01)
    for t in range(num_epochs): #A
        if t % 10 == 0:
            print(f"Epoch {t}")
        train(model, loaders, verbose=False)
        final_train_acc.append(check_accuracy(model, loaders, type='train')) #B
        final_test_acc.append(check_accuracy(model, loaders, type='test')) #B
        with open(f"final_train_acc_{n_node}.txt", "wb") as fp:
            pickle.dump(final_train_acc, fp)
        with open(f"final_test_acc_{n_node}.txt", "wb") as fp:
            pickle.dump(final_test_acc, fp)
```

- #A start training
- #B save final accuracy

In order to align our focus on the loss instead of accuracy and observe where the interpolating threshold occurs, we will use one to minus off the accuracy to derive the misclassification rate and round it off to three decimals. This is achieved via list comprehension after loading the saved training and test accuracies for different sizes of the hidden layer (see the code listing below). We want to observe the changes in training and test misclassification rates in the y axis as the model complexity increases in the x-axis. In addition to plotting the two misclassification curves, we also draw the interpolating threshold by detecting the first occurrence of zero value in the `train` variable via the `index` method.

## Listing 4.14 Plotting the misclassification rate for training and test set

```
Listing 4.14 Plotting the misclassification rate for training and test set

import matplotlib.pyplot as plt
import numpy as np
with open("final_train_acc_4096.txt", "rb") as fp:
    train = pickle.load(fp)
with open("final_test_acc_4096.txt", "rb") as fp:
    test = pickle.load(fp)
train = [np.around(1-i,3) for i in train] #A
test = [np.around(1-i,3) for i in test] #A
x_axis = range(len(num_nodes))
plt.plot(x_axis, train, label = "train")
plt.plot(x_axis, test, label = "test", ls = '--.')
plt.xticks(x_axis, num_nodes)
plt.axvline(x=x_axis[train.index(0)], ls = '--') #B
plt.text(x_axis[train.index(0)] - 0.2, 0.1, 'Interpolation threshold',
   fontsize = 10, color = 'gray',
   rotation = 90, rotation_mode='anchor')
plt.xlabel("# hidden nodes")
plt.ylabel("Misclassification rate")
plt.legend()
plt.show()
```

- #A Convert accuracy to misclassification rate and perform rounding
- #B Detect the first occurrence of zero misclassification and add a vertical line

Running the codes above will produce figure 4.20. Recall that each point represents the final training or test misclassification rate upon completing the 500th epoch of the training procedure. The misclassification rate curves could thus be considered a reflection of the model's performance upon convergence. Both training and test curves exhibit a uniform decreasing trend, with the model reaching 100% training accuracy and perfectly interpolating the training data with 256 nodes in the hidden layer. Interestingly, the test misclassification rate decreases even beyond the interpolating threshold, which is counter-intuitive based on the classical generalization theory and an excellent example of the modern generalization theory.

Besides, the “double descent” curve is not present, and this could be due to the increasing gap between different model complexities so that the second descent is obscured. Another possibility is that the implicit regularization effect of the SGD optimization algorithm is strong enough to alleviate the “double descent” phenomenon. Proper regularization can avoid the “double descent” phenomenon and accelerate learning towards a generalizing model.

![](img/ee98e91e6845f41077b163bec833ee40_140_0.png)

Figure 4.20 The misclassification curves for both training and test sets as a function of the number of nodes in the hidden layer. Both curves exhibit a decreasing trend even if the model continues to become more complex, which corresponds to the modern generalization theory. In particular, when the model starts to perfectly interpolate the training data with 256 hidden nodes, the misclassification rate for the test set keeps decreasing with a larger hidden layer in size.

## 4.4.1 Multiple global minima

We have observed so far that the solutions (i.e., the model weights upon convergence) returned by training using SGD exhibit good generalization performance in the test set, and there are multiple solutions with such equally good performance. In other words, there are multiple equally good global minima in the loss surface, and using SGD tends to land in these solutions. However, when we are not using SGD, the model may converge to a bad solution for the same model configuration.

Let us analyze this specific attribute in detail. Figure 4.21 shows two different types of global minima based on the training loss curve: the sharp valley and the flat basin. The sharp valley is where the best solution as the global minimum is narrow and sensitive. When training neural networks to reach this region, the training procedure needs to be delicate enough to reach the bottom of the valley. In comparison, the multiple global minima living in the flat basin area on the right are much less sensitive to the location of the weights within that area where the error changes slowly.

This is a sharp valley where the error changes quickly and the area with the best solution is narrow

This is a flat basin where error changes slowly and the area with the best solutions is wide

Multiple global minima where we may reach any of them depending on the specific training procedure

Figure 4.21 Illustrating the two types of global minima. On the left shows a sharp valley where the error curve changes drastically upon reaching the bottom of the valley. On the right shows a set of equally good global minima along a flat basin where the error changes slowly.

These two distinct error profiles are commonly encountered in many deep learning problems. The overall recommendation is to train models that reach the flat area with many equally good global minima or basin of attraction. They tend to be more regularized and generalize better to the unseen test set (see figure 4.22). Despite similar error in the training set, as represented by zero misclassification rate due to multiple global minima, the corresponding test error may vary depending on the specific solution zone. Specifically, a wide and flat area has a lower test error than the one obtained at the sharp zone. One particular approach brought up recently is stochastic weight averaging, which averages a few “sharp” solution weights to arrive at a final “flat” solution within the basin zone. This technique belongs to ensemble learning, where multiple versions of models are used to generate the final prediction. We will cover a specific ensembling technique called bootstrap in a later section on regularization via data.
