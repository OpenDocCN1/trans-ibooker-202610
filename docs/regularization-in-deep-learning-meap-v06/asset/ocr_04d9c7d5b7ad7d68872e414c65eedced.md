# Listing 7.11 Class method on model fitting

```
def fit(self, trainloader, testloader):
    for epoch in range(self.num_epochs): #A
    print(f'Starting epoch {epoch+1}')
    for i, data in enumerate(trainloader, 0):
    X_train, y_train = data #B
    X_train, y_train = Variable(X_train).cuda(), Variable(y_train).cuda() #C
    self.optimizer.zero_grad() #D
    y_predicted = self.model(X_train) #E
    loss_train = self.criterion(y_predicted, y_train) #F

    if self.l1_weight != 0: #G
    l1_parameters = []
    for parameter in self.model.parameters():
    l1_parameters.append(parameter.view(-1))
    l1 = self.l1_weight * self.compute_l1_loss(torch.cat(l1_parameters))
    loss_train += l1

    if self.l2_weight != 0: #H
    l2_parameters = []
    for parameter in self.model.parameters():
    l2_parameters.append(parameter.view(-1))
    l2 = self.l2_weight * self.compute_l2_loss(torch.cat(l2_parameters))
    loss_train += l2

    loss_train.backward() #I
    self.optimizer.step() #J
    self.train_loss.append(loss_train.item()) #K

    with torch.no_grad(): #L
    X_test, y_test = iter(testloader).next()
    X_test = X_test.cuda()
    y_test_pred = self.predict(X_test.cpu()) #M
    self.test_accuracy.append(np.mean((y_test == y_test_pred).numpy())) #N
    print('Test accuracy: {}'.format(self.test_accuracy[-1]))

    return self
```

+   - #A Iterate over the DataLoader for training data
+   - #B get inputs
+   - #C send to GPU if available
+   - #D zero the gradients
+   - #E perform forward pass
+   - #F compute loss
+   - #G add L1 loss component
+   - #H add L2 loss component
+   - #I perform backward pass
+   - #J perform optimization
+   - #K add training loss
+   - #L get test set performance
+   - #M get predictions for the test set
+   - #N Store test set accuracy

We have a few more functions used within the class. First, the `predict()` function is used to score the test set data in the evaluation mode by setting `self.model.eval()`, and the final