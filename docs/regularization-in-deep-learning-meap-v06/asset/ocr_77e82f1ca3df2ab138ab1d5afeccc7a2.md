# Listing 4.12 Calculate prediction accuracy for the entire training or test set

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

+   - #A Set to evaluation mode
+   - #B Extract the total number of images
+   - #C Disable gradient calculation as backpropagation is not needed
+   - #D Add the correct prediction for each batch

We are now ready to test out the impact of model complexity on the predictive accuracy for both the training set and the test set. In the code listing below, we start with two nodes in the hidden layer and gradually double the size until 4096. The reason for such an extensive range is to see how the model performs as it increases complexity even after passing the interpolating threshold. We also set a total training budget of 500 epochs in `num_epochs` to ensure that the training procedure reaches convergence, i.e., the model parameters do not change much. Upon exhausting the training budget, we record the final prediction accuracy for the training set in `final_train_acc` and the test set in `final_test_acc`. Since running the full codes takes quite some time, we store these two variables in text files via the `pickle` package so that we do not need to start from scratch if the running gets disrupted.