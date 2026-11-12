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
    print("{}": {}").format(base_dict[i], str(base_list.count(i))))
>>> count_classes(base_dict, results[0]) #A
>>> count_classes(vehicle_types_dict, results[1])
```

#A count of brand names
#B count of type of vehicle

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