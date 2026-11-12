
In the following sections, we will first show how to construct the ideal world with a constant new data feed, followed by an empirical study on the faster convergence speed of CNN in the ideal world.

## 5.3.4 Constructing the ideal world with CIFAR-5m

How could we construct the ideal world with infinite data feed? Recall that the earlier example used 10,000 training images from the CIFAR10 dataset and trained for 100 epochs, totalling 5 million images used for training, along with 10,000 images allocated for the test set. If we were to simulate an ideal world while maintaining the same training-test ratio, it would require a total of 5 million different images for training and 1 million different images for testing. These 5 million images will serve as the fresh samples for the model to learn in the ideal world in one epoch.

To this end, the authors in the paper constructed a synthetic CIFAR10-like dataset called CIFAR-5m that contains 6 million images in total. The dataset is produced by sampling from the CIFAR10 dataset using a generative model and is released at `gs://gresearch/cifar5m/part{i}.npz` for `i` in `(0,..,5)`, with each file occupying around 3GB disk space; we are now living in the big data regime. These files are downloaded via the `gsutil` command to the current directory of the active Google Colab instance as shown below.

```
>>> for i in range(0,6):
>>>     !gsutil cp -r gs://gresearch/cifar5m/part{i}.npz .
Copying gs://gresearch/cifar5m/part0.npz...
- [1 files][ 2.9 GiB/ 2.9 GiB]                                             42.3 MiB/s
Operation completed over 1 objects/2.9 GiB.
Copying gs://gresearch/cifar5m/part1.npz...
- [1 files][ 2.9 GiB/ 2.9 GiB]                                             47.9 MiB/s
Operation completed over 1 objects/2.9 GiB.
Copying gs://gresearch/cifar5m/part2.npz...
/ [1 files][ 2.9 GiB/ 2.9 GiB]                                             45.5 MiB/s
Operation completed over 1 objects/2.9 GiB.
Copying gs://gresearch/cifar5m/part3.npz...
\ [1 files][ 2.9 GiB/ 2.9 GiB]                                             50.0 MiB/s
Operation completed over 1 objects/2.9 GiB.
Copying gs://gresearch/cifar5m/part4.npz...
/ [1 files][ 2.9 GiB/ 2.9 GiB]                                             41.6 MiB/s
Operation completed over 1 objects/2.9 GiB.
Copying gs://gresearch/cifar5m/part5.npz...
- [1 files][ 2.9 GiB/ 2.9 GiB]                                             43.2 MiB/s
Operation completed over 1 objects/2.9 GiB.
```

Next, we would need to build a data loader that loads the data from the current directory and performs transformations as needed. The way to achieve this is to build a custom `Dataset` class that can accept the input tensors and transformations as parameters upon instantiation. In the coding listing below, we define a class named `CustomTensorDataset`, which inherits the `torch.utils.data.Dataset` class and contains three built-in functions: the `__init__` function to store the input data and transformations upon instantiation, the `__getitem__` to retrieve sample via index, and the `__len__` function that returns the size of the dataset.

### Listing 5.9 Build a custom tensor dataset that supports transform operations

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

Since the files end with `.npz` and are stored as `numpy` format, we can load them and convert to `tensor` format so as to build the data loader. Each input image tensor is changed from channel-last to channel-first format via the `permute` function before passing into `CustomTensorDataset`. We will also maintain the same configurations such as the batch size, the learning rate, and basic transformations as in the previous example, provided that the input images are first converted into PIL image format. The code snippet below shows an example of how to build a data loader based on the first downloaded CIFAR-5m file.

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

Note that we need to delete the unnecessary objects to save some RAM space as these and follow-up operations are extremely RAM intensive. Without this step, the code would trigger an error and restart the session due to insufficient RAM when running the notebook via a Colab Pro account with 32GB of RAM.

In addition, the performance metric used to measure the bootstrap error is the _soft classification error_, defined as one minus the softmax probability on the correct target label. On the other hand, the hard classification error measures the percentage of wrong classifications, and the soft classification error thus better quantifies the absolute distance with the true target.

Next, we will use the same `LeNet` architecture to train a model using CIFAR-5m dataset.

## 5.3.5 Model training in the ideal world

Since the training set consists of five files, we will loop through each and use all training images exactly once. This requires refreshing the training data loader upon exhausting a specific file. We also need to keep track of performance metrics such as loss, classification error, and soft classification error after processing every 50000 images during the training stage; this corresponds to an entire epoch in the offline training setting in the real world. Upon training completes, a total of 100 performance checkpoints will be stored in the master list for each metric and used for comparison with the offline setting.

These 100 checkpoints correspond to 100 different models, each to be scored across the whole test set. Since storing all these intermediate datasets and models is not an option, we can download them to the disk and sequentially reload them at the test stage. The following code listing provides the whole training procedure using CIFAR-5m.

```
Listing 5.10 Training with CIFAR-5m
training_loss_history_5m = []
training_error_history_5m = []
training_softerror_history_5m = []
test_loss_history_5m = []
test_error_history_5m = []
test_softerror_history_5m = []
if not os.path.exists('models'): #A
    os.makedirs('models')

for i in range(5):
    print(f"======= Dataset {i+1} =======")
    train_X = np.load(f"part{i}.npz")["X"] #B
    train_Y = np.load(f"part{i}.npz")["Y"] #B
    train_X = torch.Tensor(train_X) #C
    train_Y = torch.Tensor(train_Y) #C
    training_dataset = CustomTensorDataset(tensors=(
        train_X.permute(0, 3, 1, 2), train_Y), transform=transform)
    training_loader = torch.utils.data.DataLoader(training_dataset, batch_size=100)
    del train_X, train_Y, training_dataset #D

training_loss = 0.0
training_accuracy = 0.0
training_error = 0.0
training_softerror = 0.0
test_loss = 0.0
test_accuracy = 0.0
test_error = 0.0
test_softerror = 0.0
for idx, (inputs, labels) in enumerate(training_loader):
    inputs = inputs.to(device)
    labels = labels.to(torch.int64).to(device)
    outputs = model(inputs).to(device)
    loss = criterion(outputs, labels)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
    _, preds = torch.max(outputs, 1)
    correct_class_prob = torch.exp(outputs[torch.arange(len(outputs)),
        labels.data]) / \\
```
