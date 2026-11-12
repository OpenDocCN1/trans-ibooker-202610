```
shuffle=False)
test_loader = DataLoader(test_dataset, batch_size=64, \
shuffle=False)
optimizer = optim.SGD(model.parameters(), lr=0.005, momentum=0.7)
train_epochs(model, device, train_loader, valid_loader, optimizer)
```

11. Define the following function that will run the actual training epochs (let's run for 20 epochs). For each epoch, run a forward pass on the model first with the training data loader and then with the validation data loader. Switch the mode of the model from train to eval to use the input training and validation dataset, respectively:

```
def train_epochs(model, device, train_data_loader,
valid_data_loader, optimizer):
    liveloss = PlotLosses()
    loss_function = nn.CrossEntropyLoss()
    running_loss = 0.0
    running_corrects = 0
    data_loaders = {'train': train_data_loader,
    'validation':valid_data_loader}

    for epoch in range(20):
    logs = {}
    for phase in ['train', 'validation']:
    if phase == 'train': model.train()
    else: model.eval()
    running_loss = 0.0
    running_corrects = 0
    total = 0
```