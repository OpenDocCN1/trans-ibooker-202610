
看一个具体示例。假设我们有一个关于汽车元数据的条目，名为“Audi RS 4 Convertible 2008”。然后将根据该字符串派生出三个目标标签，生成“Audi”、“Convertible”和“0”，其中最后一个标签为 0 表示汽车在 2009 年阈值之前生产。然后我们将前两个标签映射为基于整数的索引以方便模型训练，生成最终输出集合为 [0, 0, 0]，其中“Audi”和“Convertible”分别是各自映射字典中的第一个条目。

在以下代码清单中，我们定义一个名为 MetaParsing 的类来处理元数据，并使用字符串匹配从汽车名称中提取所需的标签。请注意，一个输入参数指定为 *args，用于保存两个映射字典。

### 清单 6.7 解析元数据

```
class MetaParsing():
    def __init__(self, path, *args, year=None):
        self.mat = scipy.io.loadmat(path) #A
        self.year = year #B
        self.args = args #C
        self.annotations = np.transpose(self.mat['annotations'])
        self.file_names = [annotation[0][0][0].split('/')[-1] for annotation in
                           self.annotations] #D
        self.label_indices = [annotation[0][5][0][0] for annotation in self.annotations] #E
        self.car_names = [x[0] for x in self.mat['class_names'][0]] #F
        self.translated_car_names = [self.car_names[x-1] for x in self.label_indices] #G

    def brand_types(self, base_dict, x):
        y = list(base_dict.keys())[-1]
        for k,v in base_dict.items(): #H
            if v in x:
                y=k
        return y

    def parsing(self):
        result = []
        for arg in self.args: #I
            temp_list = [self.brand_types(arg, x) for x in self.translated_car_names]
            result.append(temp_list)
        if self.year != None: #J
            years_list = [0 if int(x.split(" ")[-1]) <= self.year else 1 for x in
                          self.translated_car_names]
            result.append(years_list)
        return result, self.file_names, self.translated_car_names
```

- #A 加载 MATLAB 格式的元数据
- #B 用于创建队列标志
- #C 保存两个映射字典
- #D 提取每个样本的文件名
- #E 提取每个样本的标签索引
- #F 将汽车名称提取为字符串
- #G 为每个样本创建汽车名称列表
- #H 执行基于字符串的匹配
- #I 检索车辆品牌和类型的索引
- #J 检索队列标记标志

然后我们可以创建两个自定义映射字典，如以下代码片段所示，实例化此类并按需解析元数据。

```
>>> brand_dict = {0: 'Audi', 1: 'BMW', 2: 'Chevrolet', 3: 'Dodge', 4: 'Ford', 5: 'Other'}
>>> vehicle_types_dict = {0: 'Convertible', 1: 'Coupe', 2: 'SUV', 3: 'Van', 4: 'Other'}
>>> results, file_names, translated_car_names = MetaParsing("./car_metadata.mat",
brand_dict, vehicle_types_dict, year=2009).parsing()
```

让我们对品牌和车辆类型的计数进行一些合理性检查。在以下代码片段中，我们定义一个函数来遍历两个映射字典的不同键，并统计早先解析的 `results` 中的出现次数。

```
def count_classes(base_dict, base_list):
    for i in range(len(list(base_dict.keys()))):
        print("{}: {}".format(base_dict[i], str(base_list.count(i))))
>>> count_classes(brand_dict, results[0]) #A count of brand names
>>> count_classes(vehicle_types_dict, results[1]) #B count of type of vehicle
```

如下结果所示，几乎所有品牌和车辆类型在数据集中均占用超过 1000 张图像。

```
Audi: 1169
BMW: 1055
Chevrolet: 1799
Dodge: 1253
Ford: 1035
Other: 9874
Convertible: 1907
Coupe: 2143
SUV: 2855
Van: 832
Other: 8448
```

接下来，我们将创建一个自定义 `Dataset` 类并将其传递给 PyTorch 的 `DataLoader` 函数，以便按批次一致地提供数据，这遵循与前一章相同的流水线。在以下代码清单中，我们定义一个名为 `CarDataset` 的类，将图像数据、解析后的元数据以及数据转换（如果有）组合在一起。

### 清单 6.8 定义自定义数据集类

```
from torch.utils.data import Dataset, DataLoader
class CarDataset(Dataset):
    def __init__(self, car_path, transform, mapping_dict):
        self.path = car_path
        self.folder = [x for x in listdir(car_path)]
        self.transform = transform
        self.mapping_dict = mapping_dict

    def __len__(self):
        return len(self.folder)

    def __getitem__(self, idx):
        img_loc = os.path.join(self.path, self.folder[idx])
        image = Image.open(img_loc).convert('RGB')
        single_img = self.transform(image)
        label1 = mapping_dict[self.folder[idx]][0] #A
        label2 = mapping_dict[self.folder[idx]][1] #A
        label3 = mapping_dict[self.folder[idx]][2] #A
        sample = {'image':single_img, 'labels': {'label_brand':label1,
        'label_vehicle_type':label2, 'label_cohort':label3}}
        return sample
```

- #A 检索对应的标签

然后可以通过传入图像数据目录、转换操作（调整大小、转换为张量和归一化）以及将每个文件名映射到三个目标标签的总体映射字典 `mapping_dict` 来实例化此类。数据集还被分为训练集（80%）和测试集（20%），在提供给模型时，每个数据集的批次大小均为 16 并传递给 DataLoader 函数。请注意，`CarDataset` 类在向模型提供样本时需要将三个标签与输入图像一起返回。如果剩余样本数少于预设的批次大小，我们还会丢弃最后一个批次。请参见下面的代码片段。

```
mapping_dict = dict(zip(file_names,list(zip(results[0],results[1],results[2])))) #A
data_transforms = transforms.Compose([ #B
    transforms.Resize((224,224)),
    transforms.ToTensor(),
    transforms.Normalize((0.5,0.5,0.5), (0.5,0.5,0.5))
])
cardata = CarDataset("./car_ims", transform=data_transforms, mapping_dict=mapping_dict) #C
train_len = int(cardata.__len__()*0.8) #D
test_len = int(cardata.__len__()*0.2) #D
train_set, val_set = torch.utils.data.random_split(cardata, [train_len, test_len])
train_loader = DataLoader(train_set, batch_size=16, shuffle=True, num_workers=2,
    drop_last=True) #E
test_loader = DataLoader(val_set, batch_size=16, shuffle=False, num_workers=2,
    drop_last=True) #E
```

- #A 创建总体映射字典
- #B 定义预处理转换链
- #C 创建自定义数据集实例
- #D 将数据分为训练集和测试集
- #E 为每个数据集创建数据加载器

让我们从训练数据加载器中获取一个示例以检查其内容。在以下代码片段中，我们获取一个批次的数据存储在 `sample` 中，并打印出其键和与品牌相关的内容。

```
>>> sample = next(iter(train_loader))
>>> print("Keys in the current batch: {}".format(sample.keys()))
>>> print("Size for the images in the current batch: {}".format(sample['image'].shape))
>>> print("Size for the brand target in the current batch: {}".format(sample['labels']['label_brand'].shape))
>>> print("Brand indexes in the current batch: {}".format(sample['labels']['label_brand']))
Keys in the current batch: dict_keys(['image', 'labels'])
Size for the images in the current batch: torch.Size([16, 3, 224, 224])
Size for the brand target in the current batch: torch.Size([16])
Brand indexes in the current batch: tensor([2, 4, 5, 5, 5, 3, 5, 3, 2, 5, 5, 3, 5, 1, 5, 5])
```

现在让我们定义模型架构。这次，我们将利用一个名为 resnet 的预训练模型，这是一种特殊的架构，特别擅长建模序列数据。由于典型的基于 resnet 的架构通常模型体积较大，从头开始手动定义模型结构将是一项相当艰巨的工作。为了避免繁重的工作，我们将求助于 `torchvision.models` 子包，它为包括 resnet 在内的许多流行模型架构提供了预训练权重。在以下代码片段中，我们首先通过设置 `pretrained=True` 下载预训练模型，然后打印出模型架构的最后三层。
