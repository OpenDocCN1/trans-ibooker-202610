# Listing 5.9 Build a custom tensor dataset that supports transform operations

```
class CustomTensorDataset(torch.utils.data.Dataset):
    def __init__(self, tensors, transform=None):
        self.tensors = tensors
        self.transform = transform

    def __getitem__(self, index):
        x = self.tensors[0][index]
        if self.transform:
            x = self.transform(x)
        y = self.tensors[1][index]
        return x, y

    def __len__(self):
        return self.tensors[0].size(0)
```

Since the files end with `.npz` and are stored as `numpy` format, we can load them and convert
to `tensor` format so as to build the data loader. Each input image tensor is changed
from channel-last to channel-first format via the `permute` function before passing into
`CustomTensorDataset`. We will also maintain the same configurations such as the batch
size, the learning rate, and basic transformations as in the previous example, provided that
the input images are first converted into PIL image format. The code snippet below shows an
example of how to build a data loader based on the first downloaded CIFAR-5m file.

```
transform = transforms.Compose([transforms.ToPILImage(),
                                transforms.ToTensor(),
                                transforms.Normalize(mean=(0.5, 0.5, 0.5),
                                                     std=(0.5, 0.5, 0.5))]) #A

train_X = np.load("part0.npz")["X"]
train_Y = np.load("part0.npz")["Y"]
train_X = torch.Tensor(train_X) #B
train_Y = torch.Tensor(train_Y) #B
training_dataset = CustomTensorDataset(tensors=(train_X.permute(0, 3, 1, 2), train_Y),
                                       transform=transform)
training_loader = torch.utils.data.DataLoader(training_dataset, batch_size=100)
del train_X, train_Y, training_dataset #C
```

#A transformations to be applied to the input image tensors
#B tensor to torch tensor
#C save some RAM space by deleting unnecessary objects

Note that we need to delete the unnecessary objects to save some RAM space as these and
follow-up operations are every RAM intensive. Without this step, the code would trigger an
error and restart the session due to insufficient RAM when running the notebook via a Colab
Pro account with 32GB of RAM.

In addition, the performance metric used to measure the bootstrap error is the _soft_
_classification error_, defined as one minus the softmax probability on the correct target label.
On the other hand, the hard classification error measures the percentage of wrong
classifications, and the soft classification error thus better quantifies the absolute distance
with the true target.

Next, we will use the same `LeNet` architecture to train a model using CIFAR-5m dataset.