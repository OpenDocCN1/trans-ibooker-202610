
look at a concrete example. Suppose we have an entry on the metadata of a car called “Audi RS 4 Convertible 2008”. The three target labels would then be derived based on this string, generating “Audi”, “Convertible”, and “0”, where the last label is 0 if the car is produced before the 2009 threshold. We would then map the first two labels to integer-based indexes to facilitate model training, producing a final set output as [0, 0, 0], where both “Audi” and “Convertible” are the first in line of the respective mapping dictionaries.

In the following code listing, we define a class called MetaParsing to process the metadata and extract the intended labels from the car names using string matching. Note that one of the input arguments is specified as *args, which is used to hold two mapping dictionaries.

### Listing 6.7 Parsing the metadata

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

- #A load metadata in matlab format
- #B used to create cohort flag
- #C hold two mapping dictionary(s)
- #D extract the file name for each sample
- #E extract the index of the label for each sample
- #F extract the car names as strings
- #G create a list with car names for each sample
- #H perform string-based matching
- #I retrieve the indexes of brand and type of vehicle
- #J retrieve the cohort tagging flag

We can then instantiate this class and parse our metadata as desired, after creating two customized mapping dictionaries as shown in the following code snippets.

```
>>> brand_dict = {0: 'Audi', 1: 'BMW', 2: 'Chevrolet', 3: 'Dodge', 4: 'Ford', 5: 'Other'}
>>> vehicle_types_dict = {0: 'Convertible', 1: 'Coupe', 2: 'SUV', 3: 'Van', 4: 'Other'}
>>> results, file_names, translated_car_names = MetaParsing("./car_metadata.mat",
brand_dict, vehicle_types_dict, year=2009).parsing()
```

Let us do a bit of sense check on the count of brand and type of vehicles. In the following code snippet, we define a function to iterate through different keys of the two mapping dictionaries and count the occurrence in the parsed `results` from earlier.

```
def count_classes(base_dict, base_list):
    for i in range(len(list(base_dict.keys()))):
        print("{}: {}".format(base_dict[i], str(base_list.count(i))))
>>> count_classes(brand_dict, results[0]) #A count of brand names
>>> count_classes(vehicle_types_dict, results[1]) #B count of type of vehicle
```

The result, as shown below, suggests that almost all brands and types of vehicle occupy more than 1000 images in the dataset.

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

Next, we will create a customized `Dataset` class and pass to the PyTorch’s `DataLoader` function for consistent data serving in batch, which follows the same pipeline as we did in the previous chapter. In the following code listing, we define a class called `CarDataset` to piece together the image data, the parsed metadata, and the data transformations, if any.

### Listing 6.8 Defining the customized dataset class

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

- #A retrieve the corresponding labels

This class can then be instantiated by passing in the directory of the image data, the transformations (resizing, converting to tensor, and normalization), and the overall mapping dictionary `mapping_dict` that maps each file name to the three target labels. The dataset is also split into training (80%) and test (20%) sets, each passed to the DataLoader function with a batch size of 16 when serving to the model. Note that the `CarDataset` class needs to return all three labels together with input image when providing samples to the model. We also drop the last batch if the number of the remaining samples is less than the pre-specified batch size. See the code snippet below.

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

- #A create the overall mapping dictionary
- #B define a chain of pre-processing transformations
- #C create the customized dataset instance
- #D split the data in training and testing
- #E create the dataloader for each dataset

Let us grab one example from the training data loader to examine its contents. In the following code snippet, we fetch one batch of data in `sample` and print out its keys and relevant contents on the brand.

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

Now let us define the model architecture. This time, we will leverage on a pre-trained model called resnet, which is a special type of architecture that is particularly good at modeling sequence data. Since typical resnet-based architectures are usually big in model size, manually defining the model structure from scratch would be a relatively big undertaking. To bypass the heavy lifting, we will resort to the `torchvision.models` subpackage, which offers pre-trained weights for many popular model architectures, including resnet. In the following code snippet, we first download the pre-trained model by setting `pretrained=True`, and then print out the last three layers of the model architecture.
