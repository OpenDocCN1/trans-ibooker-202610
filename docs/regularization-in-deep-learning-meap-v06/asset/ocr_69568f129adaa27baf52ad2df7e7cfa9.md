The model performance is reasonable given a limited amount of training data. However, overfitting occurred halfway, after which the model loss started to deteriorate. Let us see if the model performance could be improved when using data augmentation.

# ## 5.2.2 Training with data augmentation

Data augmentation is a natural technique when working with image data as it can introduce a small number of transformations to the input image without changing the target label. These geometric transformations allow the model to iterate over more training images of the same class, which is especially helpful when the original dataset is small. The common transformations are also easy to implement when training convolutional neural networks in computer vision tasks.

In order to train a model using data augmentation, all we have to do is to build a new data loader for the training set that involves different forms of transformations to generate new images. See the code listing below for the series of transformations used, including flipping, rotation, zooming, shearing, and changing colors. We omit the full codes as it largely remains the same as before. For the full training process, please refer to the accompanying notebook at https://github.com/jackliu333/regularization_in_deep_learning/blob/main/Chapter_5.0.ipynb#b.

```
Listing 5.8 Training with data augmentation

transform_aug = transforms.Compose([
    transforms.RandomHorizontalFlip(), #A
    transforms.RandomRotation(10), #B
    transforms.RandomAffine(0, shear=10,
                            scale=(0.8,1.2)), #C
    transforms.ColorJitter(brightness=0.2,
                           contrast=0.2,
                           saturation=0.2), #D
    transforms.ToTensor(),
    transforms.Normalize((0.5, 0.5, 0.5),
                         (0.5, 0.5, 0.5))
])

training_dataset_aug = datasets.CIFAR10(root='./data_aug', train=True,
                                        download=True, transform=transform_aug)
training_loader_aug = torch.utils.data.DataLoader(training_dataset_aug,
                                                  batch_size=100, shuffle=True)
```

+   - #A flip the image w.r.t horizontal axis
+   - #B rotate the image to a specified angel
+   - #C zoom and shear
+   - #D change color

Figure 5.8 shows the resulting performance plots for the model trained using data augmentation for 100 epochs. The result shows that the overfitting phenomenon is clearly mitigated as indicated by the uniformly decreasing loss and increasing accuracy for the validation set. The final validation accuracy reaches 74.4% at the end of the 100th epoch, which is over 4% higher than the case when data augmentation is not used. The plots also