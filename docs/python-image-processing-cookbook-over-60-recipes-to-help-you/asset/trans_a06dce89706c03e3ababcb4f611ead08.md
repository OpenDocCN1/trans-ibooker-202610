

## 6. 如果你从训练图片中选取少量样本，并将它们与标签一起绘制，你将得到类似于下面的图表：

| 限速 (20km/h) | 限速 (30km/h) | 限速 (50km/h) | 限速 (60km/h) | 限速 (70km/h) | 限速 (80km/h) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 限速结束 (80km/h) | 限速 (100km/h) | 限速 (120km/h) | 禁止超车 | 3.5公吨以上车辆禁止超车 | 下一路口优先通行 |
| 优先道路 | 让行 | 停车 | 禁止车辆通行 | 禁止3.5公吨以上车辆通行 | 禁止驶入 |
| 一般警告 | 左侧危险弯道 | 右侧危险弯道 | 连续弯道 | 颠簸路面 | 路面滑湿 |
| 右侧路面变窄 | 道路施工 | 交通信号灯 | 行人 | 儿童过马路 | 自行车过道 |
| 结冰/积雪警告 | 野生动物穿越 | 所有限速和禁超结束 | 前方右转 | 前方左转 | 仅限直行 |
| 直行或右转 | 直行或左转 | 靠右行驶 | 靠左行驶 | 前方环形交叉路口 | 禁超结束 |
| 3.5公吨以上车辆禁超结束 | | | | | |

## 7. 通过定义TrafficNet类来实现IDSIA MCDNN模型
（继承自nn.Module）。使用构造函数（__init__()方法）
来定义神经网络（包含卷积、池化和全连接层），并使用forward()
方法在神经网络中实现前向传播，使用如下代码片段：

```
class TrafficNet(nn.Module):

    def __init__(self, gray=False):
        super(TrafficNet, self).__init__()
        input_chan = 1 if gray else 3
        self.conv1 = nn.Conv2d(input_chan, 6, 5)
        self.pool = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 43)

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = x.view(-1, 16 * 5 * 5)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x
```

## 8. 输入图像具有高对比度变化，并使用CLAHE变换进行对比度归一化
（即，使用opencv-python函数对图像应用自适应直方图均衡化）：

```
class ClaheTransform:
    def __init__(self, clipLimit=2.5, tileGridSize=(4, 4)):
        self.clipLimit = clipLimit
        self.tileGridSize = tileGridSize

    def __call__(self, im):
        img_y = cv2.cvtColor(im, cv2.COLOR_RGB2YCrCb)[:, :, 0]
        clahe = cv2.createCLAHE(clipLimit=self.clipLimit,
                                tileGridSize=self.tileGridSize)

        img_y = clahe.apply(img_y)
        img_output = img_y.reshape(img_y.shape + (1,))
        return img_output
```

## 9. 你下载的输入数据集使用pickle进行了序列化。让我们通过从PyTorch数据集类继承来定义PickledTrafficSignsDataset类：

```
class PickledTrafficSignsDataset(Dataset):
    def __init__(self, file_path, transform=None):
        with open(file_path, mode='rb') as f:
            data = pickle.load(f)
            self.features = data['features']
            self.labels = data['labels']
            self.count = len(self.labels)
            self.transform = transform

    def __getitem__(self, index):
        feature = self.features[index]
        if self.transform is not None:
            feature = self.transform(feature)
        return (feature, self.labels[index])

    def __len__(self):
        return self.count
```

## 10. 定义train()函数以实现整个训练过程，从加载数据、应用变换开始，然后运行训练轮次：

```
def train(model, device):

    data_transforms = transforms.Compose([
            ClaheTransform(),
            transforms.ToTensor()
        ])
    liveloss = PlotLosses()
    torch.manual_seed(1)
    train_dataset = PickledTrafficSignsDataset(training_file,
                                               transform=data_transforms)
    valid_dataset = PickledTrafficSignsDataset(validation_file,
                                               transform=data_transforms)
    test_dataset = PickledTrafficSignsDataset(testing_file,
                                               transform=data_transforms)

    class_sample_count = np.bincount(train_dataset.labels)
    weights = 1 / np.array([class_sample_count[y] for y in
                                train_dataset.labels])

    samp = sampler.WeightedRandomSampler(weights, 43 * 2000)
    train_loader = DataLoader(train_dataset, batch_size=64,
                              sampler=samp)
    valid_loader = DataLoader(valid_dataset, batch_size=64,
                              shuffle=False)
    test_loader = DataLoader(test_dataset, batch_size=64,
                             shuffle=False)
    optimizer = optim.SGD(model.parameters(), lr=0.005, momentum=0.7)
    train_epochs(model, device, train_loader, valid_loader, optimizer)
```

## 11. 定义以下函数以运行实际的训练轮次（让我们运行20个轮次）。对于每个轮次，先使用训练数据加载器对模型运行前向传播，然后使用验证数据加载器。分别将模型模式从train切换到eval，以使用输入的训练数据集和验证数据集：

```
def train_epochs(model, device, train_data_loader,
                 valid_data_loader, optimizer):
    liveloss = PlotLosses()
    loss_function = nn.CrossEntropyLoss()
    running_loss = 0.0
    running_corrects = 0
    data_loaders = {'train': train_data_loader,
                    'validation': valid_data_loader}

    for epoch in range(20):
        logs = {}
        for phase in ['train', 'validation']:
            if phase == 'train':
                model.train()
            else:
                model.eval()
            running_loss = 0.0
            running_corrects = 0
            total = 0
```

## 12. 对于训练阶段，计算训练损失并运行反向传播（back propagation）以更新模型权重。对于验证阶段，只需计算验证损失（使用`loss_function()`，使用交通信号实例的实际类标签和预测类标签）：

```
for batch_idx, (data, target) in enumerate(data_loaders[phase]):
    if phase == 'train':
        output = model(data.to(device))
        target = target.long().to(device)
        loss = loss_function(output, target)
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
    else:
        with torch.no_grad():
            output = model(data.to(device))
            target = target.long().to(device)
            loss = loss_function(output, target)
    if batch_idx % 100 == 0:
        print('Train Epoch: {} [{}/{} ({:.0f}%)]\tLoss: {:.6f}'.format(
            epoch, batch_idx * len(data), len(data_loaders[phase].dataset),
            100. * batch_idx / len(data_loaders[phase]), phase,
            loss.item()))
```

## 13. 最后，比较模型预测的类和地面真实标签，以计算正确预测的示例数量。跟踪运行损失，并使用`livelossplot`自动绘制训练和验证损失：

```
pred = torch.argmax(output, dim=1)
running_loss += loss.detach()
running_corrects += torch.sum(pred == target).sum().item()
total += target.size(0)
epoch_loss = running_loss / len(data_loaders[phase].dataset)
epoch_acc = running_corrects / total

prefix = ''
if phase == 'validation': prefix = 'val_'
logs[prefix + 'log loss'] = epoch_loss.item()
logs[prefix + 'accuracy'] = epoch_acc.item()
liveloss.update(logs)
liveloss.draw()
```

如果运行前面的代码块，并使用`livelossplot`在训练期间自动绘制日志损失/准确率，你将得到类似于下面的输出：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_322_0.png)

```
log loss:
training (min: 0.000, max: 0.145, cur: 0.000)
validation (min: 0.005, max: 0.058, cur: 0.005)
accuracy:
training (min: 0.028, max: 0.997, cur: 0.997)
validation (min: 0.035, max: 0.936, cur: 0.934)
Train Epoch: 19 [0/34799 (0%)] train Loss: 0.002379
Train Epoch: 19 [6400/34799 (7%)] train Loss: 0.002805
Train Epoch: 19 [12800/34799 (15%)] train Loss: 0.003652
```

