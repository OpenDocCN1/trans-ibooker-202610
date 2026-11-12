```
figure = plt.figure(figsize=(10, 8))
cols, rows = 5, 5
for i in range(1, cols * rows + 1): #A
    sample_idx = torch.randint(len(train_data), size=(1,)).item() #B #C
    img, label = train_data[sample_idx] #D
    figure.add_subplot(rows, cols, i)
    plt.title(label)
    plt.axis("off")
plt.imshow(img.squeeze(), cmap="gray") #E
plt.show()
```

#A Loop over 25 places to plot the images
#B Generate a random index to select an image
#C The item function converts the Tensor object into a scalar value
#D Extract the image data and target label
#E Squeeze the image to convert the image shape from [1,28,28] to [28,28]

Figure 4.16 shows the output of the 25 digits. Note that each image has a shape of 28 by 28, thus having a total of 784 pixels, i.e., features per image.