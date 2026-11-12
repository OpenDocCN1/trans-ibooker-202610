
```
def fit(self, trainset, testset, verbose=True, test_score_freq=5):
    trainloader = torch.utils.data.DataLoader(trainset, batch_size=self.batch_size,
    shuffle=True) #A
    testloader = torch.utils.data.DataLoader(testset, batch_size=len(testset),
    shuffle=False) #A
    X_test, y_test = iter(testloader).next() #A
    X_test = X_test.cuda() #A
    for epoch in range(self.max_epoch): #B
        running_loss = 0
        for i, training_batch in enumerate(trainloader, 0):
            X_train, y_train = training_batch
            X_train, y_train = Variable(X_train).cuda(), Variable(y_train).cuda()
            self.optimizer.zero_grad() #C
            y_pred = self.model(X_train) #D
            loss = self.criterion(y_pred, y_train) #E
            loss.backward() #F
            self.optimizer.step() #G
            running_loss += loss.item() #H
    self.loss.append(running_loss / len(trainloader)) #I
    if verbose:
        print('{Epoch {} loss: {}}'.format(epoch+1, self.loss[-1]))
    if (epoch+1) % test_score_freq == 0:
        y_test_pred = self.predict(X_test).cpu() #J
        self.y_test_pred = y_test_pred
        self.y_test = y_test
        self.test_accuracy.append(np.mean((y_test == y_test_pred).numpy())) #K
        if verbose:
            print('Test accuracy: {}'.format(self.test_accuracy[-1]))
    return self
```

- #A prepare training and test sets
- #B start training procedure, best run on GPU
- #C clear gradients
- #D make prediction
- #E calculate loss of current batch
- #F perform back propagation to calculate gradients automatically
- #G update weights
- #H add loss of different batches within the same epoch
- #I store average loss per batch for all epochs
- #J get predictions for the test set
- #K store test set accuracy

While making predictions for the whole test set, we used the predict() method from the class. This is a self-defined prediction function that converts the probabilistic model output into class assignments, as shown in the following code snippet.

```
def predict(self, x):
    model = self.model.eval()
    outputs = model(Variable(x))
    _, pred = torch.max(outputs.data, 1)
    model = self.model.train()
    return pred
```

Finally, we can test the classifier on the MNIST dataset. In the following code listing, we test out three different models: the original FCN without any dropout, the FCN with 50% dropout in the hidden layers, and the FCN with 50% dropout in the hidden layers plus 20% dropout in the input layer. The FCN has two hidden layers, each consisting of 300 hidden nodes. We train all three models for a total of 500 epochs, and record the test set accuracy every 5 epochs. All models are contained in `model_list`. In addition, since running this code listing takes around 5 hours using a single GPU, we save the models and performance metrics offline for plotting later.

## Listing 6.4 Training three FCNs on the MNIST dataset

```
transform = transforms.Compose([transforms.ToTensor(),
 transforms.Normalize([0.5], [0.5])]) #A
trainset = datasets.MNIST(root='data/', train=True, download=True, transform=transform) #A
testset = datasets.MNIST(root='data/', train=False, transform=transform) #A
hidden_layers = [300, 300] #B
model_list = [MLPClassifier(hidden_layers, dropout_rates=[0, 0], max_epoch=500),
 MLPClassifier(hidden_layers, dropout_rates=[0, 0.5], max_epoch=500),
 MLPClassifier(hidden_layers, dropout_rates=[0.2, 0.5], max_epoch=500)] #B
[tmp_model.fit(trainset, testset, verbose=True) for tmp_model in model_list] #C
for ind, tmp_model in enumerate(model_list): #D
    torch.save(tmp_model, 'mnist_mlp_'+str(ind)+'.pth')
    tmp_model.test_accuracy = list(map(str, tmp_model.test_accuracy))
from google.colab import files
open("mlp_test_accuracy.txt", "w").write('\\n'.join(['.'.join(tmp_model.test_accuracy) for tmp_model in model_list])) #E
files.download('mlp_test_accuracy.txt')
```

- # A load and transform MNIST
- # B define three FCN models
- # C start training
- # D save torch models
- # E save test accuracy

When the training completes, we can load the saved file on test set accuracy and plot it as a function of training iterations at a step size of 5. See the following code snippet for the plotting details.

```
import matplotlib.pyplot as plt
mlp_test_accuracy = [accuracy_array.split(',') for accuracy_array in
 open("mlp_test_accuracy.txt", "r").read().split('\\n')]
mlp_test_accuracy = np.array(mlp_test_accuracy, dtype='f')
labels = ['MLP no dropout',
          'MLP 50% dropout in hidden layers',
          'MLP 50% dropout in hidden layers + 20% in input layer']
plt.figure(figsize=(8, 7))
for i, r in enumerate(mlp_test_accuracy):
    plt.plot(np.array(range(1, len(r)+1))*5, r, '.-', label=labels[i], alpha=0.6);
plt.legend(loc=4);
plt.xlabel('Epochs');
plt.ylabel('Accuracy in test set');
plt.show()
```

Running the codes above will generate figure 6.12. In this case, adding dropout in the second and third models clearly improves the test set performance. In practice, it is often observed that dropout brings a marked contribution to the generalization performance of the model, which is even more obvious than explicitly regularizing the norm of the weights. In addition, applying dropout to the input layer (to a small extent) further boosts the test set performance.

![](img/ee98e91e6845f41077b163bec833ee40_199_0.png)

Figure 6.12 Comparing the test set accuracy of three different models. Adding dropout as a regularization on the model architecture helps improve the generalization performance on the test set, as shown by a higher test set accuracy for the latter two models. Further applying the dropout layer after the input layer further helps improve the test performance as indicated by the marginally higher curve of the third model.

In the next section, we will apply dropout to CNN using LeNet, a typical model architecture we encountered in chapter 5. When working with CNN, each input image is a three-dimensional tensor object and will be treated using the convolution operation. We will use the nn.Dropout2d() function, which is equivalent to our Dropout layer defined earlier, except that it is specialized for two-dimensional dropout operation (along the width and height of an image) and provides better processing efficiency.

## 6.2.4 Applying dropout in CNN

Applying dropout to LeNet follows a similar pipeline as before. First, we define the model architecture. As shown in the following code listing, the model consists of two blocks of CONV + DROPOUT + MAX POOLING operations, followed by two levels of FC + RELU operations before reaching the final output layer. We use the default 50% dropout probability for all dropout layers.

## Listing 6.5 Defining LeNet

```
class LeNet(nn.Module):
    def __init__(self, dropout_rate=0.5):
        super(LeNet, self).__init__()
        self.model = nn.Sequential()
        self.model.add_module('conv1', nn.Conv2d(1, 20, kernel_size=5, padding=2))
        self.model.add_module('dropout1', nn.Dropout2d(p=dropout_rate))
        self.model.add_module('maxpool1', nn.MaxPool2d(2, stride=2))
        self.model.add_module('conv2', nn.Conv2d(20, 50, kernel_size=5, padding=2))
        self.model.add_module('dropout2', nn.Dropout2d(p=dropout_rate))
        self.model.add_module('maxpool2', nn.MaxPool2d(2, stride=2))
        self.model.add_module('flatten', Flatten())
        self.model.add_module('dense3', nn.Linear(50*7*7, 500))
        self.model.add_module('relu3', nn.ReLU())
        self.model.add_module('dropout3', nn.Dropout(p=dropout_rate))
        self.model.add_module('final', nn.Linear(500, 10))

    def forward(self, x):
        return self.model(x)
```

Note the two-dimensional feature map as a result of the convolution operation needs to be converted or flattened into a one-dimensional array in order to connect with another linear layer. This is achieved via another Flatten class defined in the following code snippet.

```
class Flatten(nn.Module):
    def __init__(self):
        super(Flatten, self).__init__()

    def forward(self, x):
        x = x.view(x.size(0), -1)
        return x
```

We can also print out the model architecture to get a sense of the different types of layers involved.

```
>>> model = LeNet()
>>> print(model)
LeNet(
  (model): Sequential(
    (conv1): Conv2d(1, 20, kernel_size=(5, 5), stride=(1, 1), padding=(2, 2))
    (dropout1): Dropout2d(p=0.5, inplace=False)
    (maxpool1): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
    (conv2): Conv2d(20, 50, kernel_size=(5, 5), stride=(1, 1), padding=(2, 2))
    (dropout2): Dropout2d(p=0.5, inplace=False)
    (maxpool2): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
    (flatten): Flatten()
    (dense3): Linear(in_features=2450, out_features=500, bias=True)
    (relu3): ReLU()
    (dropout3): Dropout(p=0.5, inplace=False)
    (final): Linear(in_features=500, out_features=10, bias=True)
  )
)
```
